# RevenueCat Test Store — Empty Offerings Issue

## Symptom

After integrating RevenueCat into an Expo React Native app and opening the paywall, the SDK logged:

```
DEBUG [RevenueCat] [Purchases] Initializing Purchases SDK with RC Test store API Key.
DEBUG [RevenueCat] [Purchases] Empty offerings. Please make sure you've configured offerings correctly in the RevenueCat dashboard and that the products are properly configured.
```

The paywall displayed: **"No subscription plans available yet. Please check back later."**

---

## Root Cause (Three Issues)

### Issue 1: RevenueCatProvider was skipped on web

The app's `_layout.tsx` had a platform check that bypassed the `RevenueCatProvider` entirely on web:

```tsx
export default function RootLayout() {
  if (Platform.OS === "web") {
    return <AppContent />; // No RevenueCatProvider wrapping!
  }

  return (
    <RevenueCatProvider>
      <AppContent />
    </RevenueCatProvider>
  );
}
```

Since the app was being previewed on web, `Purchases.configure()` never ran. The `useRevenueCat()` hook returned default context values, and `useOfferings()` called `Purchases.getOfferings()` on an unconfigured SDK, which silently failed and returned null.

**Fix:** Remove the platform guard. The `react-native-purchases` v9 SDK supports web via a simulated store, so the provider should wrap the app on all platforms:

```tsx
export default function RootLayout() {
  return (
    <RevenueCatProvider>
      <AppContent />
    </RevenueCatProvider>
  );
}
```

### Issue 2: RevenueCatProvider only configured on iOS/Android

Even after fixing the layout, the provider itself had platform-specific `configure()` calls:

```tsx
if (Platform.OS === "ios") {
  await Purchases.configure({ apiKey });
} else if (Platform.OS === "android") {
  await Purchases.configure({ apiKey });
}
```

On web, neither branch executed, so the SDK was never configured.

**Fix:** Call `configure()` unconditionally and wrap in try/catch:

```tsx
try {
  await Purchases.configure({ apiKey });
  // ...
} catch (e) {
  console.warn("RevenueCat init failed:", e);
  setIsReady(true);
}
```

### Issue 3: Products from wrong store type attached to offering packages

After fixing the provider, the SDK initialized correctly but still returned empty offerings. The debug log confirmed:

```
Empty offerings. Please make sure you've configured offerings correctly...
```

The "Default Offering" (`ofrngcc9569fbec`) had packages with **App Store** products attached (e.g., `prodc0a32d7f6e` belonging to app `app96c7c04c94` of type `app_store`). However, the SDK was configured with a **test store** API key (`test_QAYKjRCbIXDNejFWvQSjMIugtXi`) belonging to a test store app (`appdb89b8737b` of type `test_store`).

**RevenueCat's SDK filters offerings by the app type matching the API key.** When the SDK sees a test store API key, it only looks for products belonging to the test store app. Since the packages in the Default Offering only had App Store products, the SDK treated the offering as empty.

Attempting to attach test store products to the existing packages failed with:

```
Cannot attach product `prodc0a32d7f6e` to this package because there is
already another incompatible product from the same app `app96c7c04c94`
attached to it.
```

This error was misleading — it actually meant the package already had a product from the same store type. You cannot have two products from the same app on one package.

**Fix:** Create a brand new offering with fresh packages that only have test store products:

1. Created a new offering "Timer Pro" (`ofrng7d194dd2bf`)
2. Created three new packages: `$rc_monthly`, `$rc_annual`, `$rc_lifetime`
3. Attached the **test store** products (with prices) to these new packages
4. Set the new offering as `is_current: true`

---

## Timeline of Debugging Steps

### Step 1: Initial integration

- Installed `react-native-purchases` and `expo-build-properties`
- Created `RevenueCatProvider`, `useOfferings`, `usePurchases` hooks
- Created paywall screen at `app/paywall.tsx`
- Found existing "Timer Pro iOS" app (`app96c7c04c94`) in RevenueCat with matching bundle ID `com.bfloat.app`
- Set `.env.local` to the App Store API key: `appl_eGLCYSRagrVziYbePXWqNIjSjxt`
- Result: **"No subscription plans available"** — but the user was testing via the test store, not App Store

### Step 2: Discovered existing test store products needed

- Listed apps and found existing test store app (`appdb89b8737b`) with key `test_QAYKjRCbIXDNejFWvQSjMIugtXi`
- `.env.local` was already updated to the test store key (by the platform)
- Confirmed the "Default Offering" existed with 3 packages (Monthly, Annual, Lifetime)
- Found existing App Store products were attached to those packages
- Created 3 new **test store** products with prices:
  - `proda093685728` — Monthly, $4.99, duration P1M
  - `prod4e56254ba2` — Annual, $29.99, duration P1Y
  - `prodd3681955ad` — Lifetime, $49.99, non-consumable
- Tried attaching test store products to existing packages — **failed** with "incompatible product" error
- Attached test store products to the "Pro Access" entitlement — **succeeded**
- Result: Still **"No subscription plans available"**

### Step 3: Fixed provider platform issues

- Discovered `_layout.tsx` was skipping `RevenueCatProvider` on web entirely
- Discovered `RevenueCatProvider.tsx` had `if (Platform.OS === "ios")` / `else if (Platform.OS === "android")` guards on `configure()`
- Fixed both: removed web bypass, made `configure()` unconditional
- Result: SDK now initialized but still logged **"Empty offerings"**

### Step 4: Created new offering with test store products

- Realized the Default Offering's packages only had App Store products attached
- The test store SDK key could not resolve App Store products → empty offerings
- Could not attach test store products to existing packages (already had App Store products)
- Created brand new offering "Timer Pro" (`ofrng7d194dd2bf`)
- Created 3 fresh packages in the new offering
- Attached test store products to the new packages
- Set "Timer Pro" as `is_current: true`
- Result: **Offerings now load correctly**

---

## RevenueCat Dashboard State (Before Fix)

### Apps in the project (`proj9fbe92b9`)

| App | ID | Type |
|---|---|---|
| Timer Pro iOS | `app96c7c04c94` | `app_store` (bundle: `com.bfloat.app`) |
| Test Store | `appdb89b8737b` | `test_store` |
| MCP Test App | `app810f653c39` | `play_store` |
| MCP Test App 2 | `app11c2d5709c` | `play_store` |
| ConcertTix iOS | `appb6aa361f22` | `app_store` |

### Products

**App Store products** (belonged to `app96c7c04c94`):
- `prodc0a32d7f6e` — Timer Pro Monthly (`com.bfloat.app.pro.monthly`) — no duration, no price
- `prodbac151d69c` — Timer Pro Annual (`com.bfloat.app.pro.annual`) — no duration, no price
- `prod20f0b7bf6b` — Timer Pro Lifetime (`com.bfloat.app.pro.lifetime`) — non_consumable, no price

**Test Store products** (belonged to `appdb89b8737b`) — created during debugging:
- `proda093685728` — Timer Pro Monthly (`com.bfloat.app.pro.monthly`, duration: P1M, price: $4.99 USD)
- `prod4e56254ba2` — Timer Pro Annual (`com.bfloat.app.pro.annual`, duration: P1Y, price: $29.99 USD)
- `prodd3681955ad` — Timer Pro Lifetime (`com.bfloat.app.pro.lifetime`, one_time, price: $49.99 USD)

### Offerings (Before Fix)

**Default Offering** (`ofrngcc9569fbec`, `is_current: true`):
- Package `$rc_monthly` (`pkged795201806`) — had App Store product `prodc0a32d7f6e` attached
- Package `$rc_annual` (`pkge5605a116e4`) — had App Store product `prodbac151d69c` attached
- Package `$rc_lifetime` (`pkgecec7f3ae1e`) — had App Store product `prod20f0b7bf6b` attached

SDK with test store key could not resolve App Store products → **empty offerings**

### API Keys

| Key | App | Type |
|---|---|---|
| `appl_eGLCYSRagrVziYbePXWqNIjSjxt` | Timer Pro iOS (`app96c7c04c94`) | App Store |
| `test_QAYKjRCbIXDNejFWvQSjMIugtXi` | Test Store (`appdb89b8737b`) | Test Store |

The `.env.local` was set to the test store key.

---

## RevenueCat Dashboard State (After Fix)

### New Offering (set as current)

**Timer Pro** (`ofrng7d194dd2bf`, `is_current: true`):

| Package | ID | Lookup Key | Product (Test Store) | Product ID | Price |
|---|---|---|---|---|---|
| Monthly | `pkgef84003c09b` | `$rc_monthly` | Timer Pro Monthly | `proda093685728` | $4.99/mo |
| Annual | `pkge7c902ebb31` | `$rc_annual` | Timer Pro Annual | `prod4e56254ba2` | $29.99/yr |
| Lifetime | `pkge5e3a9ef1c8` | `$rc_lifetime` | Timer Pro Lifetime | `prodd3681955ad` | $49.99 |

### Old Offering (no longer current)

**Default Offering** (`ofrngcc9569fbec`, `is_current: false`):
- Still exists with App Store products attached — will be used when switching to production App Store key

### Entitlement

**Pro Access** (`entl051e28e4cd`, lookup_key: `pro`):
- All 6 products attached (both App Store and Test Store variants):
  - `prodc0a32d7f6e`, `prodbac151d69c`, `prod20f0b7bf6b` (App Store)
  - `proda093685728`, `prod4e56254ba2`, `prodd3681955ad` (Test Store)
- Provider checks `customerInfo?.entitlements.active["pro"]`

Note: The entitlement key in the provider was also changed from `"premium"` to `"pro"` to match the actual RevenueCat entitlement lookup_key.

---

## Files Changed

| File | Change |
|---|---|
| `app/_layout.tsx` | Removed `Platform.OS === "web"` guard; `RevenueCatProvider` now wraps app on all platforms |
| `providers/RevenueCatProvider.tsx` | Removed platform-specific `if (iOS) / else if (android)` configure branches; now calls `Purchases.configure({ apiKey })` unconditionally; added try/catch around init; changed entitlement check from `"premium"` to `"pro"` |
| `.env.local` | Changed from App Store key (`appl_eGLCYSRagrVziYbePXWqNIjSjxt`) to Test Store key (`test_QAYKjRCbIXDNejFWvQSjMIugtXi`) |

---

## Key Takeaways

1. **RevenueCat SDK v9 supports web** via a simulated store. Don't skip the provider on web if you're using the test store for development.

2. **API key type must match product app type.** If you use a test store API key (`test_...`), the SDK will only resolve products that belong to the test store app. App Store products (`appl_...`) in the same offering will be invisible to the SDK, making the offering appear empty.

3. **Each package can only have one product per app.** You cannot attach a second product from the same app to a package. If you need different store types, you attach one product per store type to the same package — or create a separate offering entirely.

4. **Test store products require explicit price configuration.** Use the `create_price` API (or dashboard) to set prices. Without prices, products exist but the SDK cannot display them.

5. **Test store products require explicit duration.** When creating subscription products for the test store, you must pass the `duration` field (e.g., `P1M`, `P1Y`). App Store products get duration from Apple; test store products have no external source.

6. **The "Empty offerings" error is a catch-all.** It can mean any of: no current offering set, packages have no products, products don't match the API key's app type, products have no prices, or products have no duration. The SDK doesn't distinguish between these cases in its debug log.

7. **Creating a fresh offering is the cleanest fix** when packages have mixed store-type products that can't coexist. Rather than trying to detach and reattach, create a new offering with clean packages linked to the correct store type.

8. **When switching from test to production**, you'll need to: change the API key in `.env.local` back to the App Store key, and set the old "Default Offering" (with App Store products) back to `is_current: true` — or update the "Timer Pro" offering's packages to also include App Store products.
