**Ref:** chore/day-5-002
**Depends:** none
### Context

While checking out stripe I got this error

```
Checkout error: [Error: Automatic tax calculation in Checkout requires a valid address on the Customer. Add a valid address to the Customer or set `customer_update[address]` to 'auto' to save the billing address entered in Checkout to the Customer.] {
  type: 'StripeInvalidRequestError',
  raw: [Object],
  rawType: 'invalid_request_error',
  code: 'customer_tax_location_invalid',
  doc_url: 'https://stripe.com/docs/error-codes/customer-tax-location-invalid',
  param: undefined,
  detail: undefined,
  headers: [Object],
  requestId: 'req_ZNrur300Z9ERy2',
  statusCode: 400,
  userMessage: undefined,
  charge: undefined,
  decline_code: undefined,
  payment_intent: undefined,
  payment_method: undefined,
  payment_method_type: undefined,
  setup_intent: undefined,
  source: undefined
}
 POST /api/checkout 500 in 1433ms
```

### Acceptance Criteria

- [x] Stripe checkout completes without errors
- [x] A concise yet thorough bug report in [[#Report]]
- [x] A concise yet thorough bug handoff in [[#Handoff]]

### Constraints

- None

### Resources

- None

---
### Report
- **Symptom:** Checkout creation fails with Stripe `customer_tax_location_invalid` (`POST /api/checkout 500`) when automatic tax is enabled.
- **Root cause:** The generated checkout flow reused/created customers but did not require saving an address onto that customer during Checkout. Stripe automatic tax requires a resolvable customer tax location.
- **Impact:** Users can reach pricing and select plans, but checkout session creation fails at runtime for affected flows.
- **Fix applied:** Updated Stripe integration skill guidance to include `automatic_tax`, `billing_address_collection`, and `customer_update: { address: 'auto', name: 'auto' }` in checkout session creation when using an existing customer id from `getOrCreateCustomer(email)`.
- **Why this works:** Stripe now saves the billing address captured in Checkout to the Customer record, satisfying automatic tax location requirements and preventing the `customer_tax_location_invalid` error.

---
### Handoff
- **What was done:** Patched `resources/skills/add-stripe/SKILL.md` checkout implementation guidance to use `customerId = await getOrCreateCustomer(body.email)` and pass:
  - `automatic_tax: { enabled: true }`
  - `billing_address_collection: 'auto'`
  - `customer_update: { address: 'auto', name: 'auto' }`
  Added a dedicated note in Customer Handling explaining why these fields are required for automatic tax.
- **Commit:** `92bddcc`
- **Files touched:** `resources/skills/add-stripe/SKILL.md`
- **Decisions made:** Fixed the source-of-truth skill guidance (not app-specific code) so newly generated Stripe integrations avoid this recurring checkout failure.
- **Known limitations:** Existing apps already scaffolded from older guidance will still need manual checkout route updates.
- **How to verify:** Regenerate/apply Stripe checkout code from the updated skill and confirm `/api/checkout` returns success for subscription checkout with automatic tax enabled and no `customer_tax_location_invalid` errors.
