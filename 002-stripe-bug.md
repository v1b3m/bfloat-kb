After connecting stripe, the "Set up Stripe" banner should stop showing up in the chat when a user sends a stripe-related message.
## Request dumped to fetch

```ts
fetch("http://localhost:3000/api/projects/0c0482a5-38c5-473c-a29c-c156aaa6d5e4/setup-stripe", {
  "headers": {
    "accept": "application/json",
    "accept-language": "en-US",
    "authorization": "Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6InNzb19vaWRjX2tleV9wYWlyXzAxS0cwODNRMUFCS01ZNllaRjlHRDk5WTdZIn0.eyJpc3MiOiJodHRwczovL2FwaS53b3Jrb3MuY29tL3VzZXJfbWFuYWdlbWVudC9jbGllbnRfMDFLRzA4M1FFTkhNQ0ZIQVJTUzJOWkFYRzYiLCJzdWIiOiJ1c2VyXzAxS0cyNTNUOFdBUjkzRzlSTlFNMzRGWFhNIiwic2lkIjoic2Vzc2lvbl8wMUtIMThSNFgxRTI2WTlHRTNUSFdLOFBGQSIsImp0aSI6IjAxS0hHVzFKQlhTS0owOVYyRVpXQkpXNlo2Iiwib3JnX2lkIjoib3JnXzAxS0c3RUVYUjRROVM3WVlBR1oyWEtaWjhUIiwicm9sZSI6Im93bmVyIiwicm9sZXMiOlsib3duZXIiXSwicGVybWlzc2lvbnMiOltdLCJleHAiOjE3NzExNjY4NTAsImlhdCI6MTc3MTE2NjU1MH0.fP1our3vdCHeneNl0rwVqdU2d_RrxajEWtfjp5CW_b05Trw3U-Jx90tenOylWhaxfepa93iu9UHXbYsjTO_LRc58YEnu9PlqL8K_F5oylNCr99WLY3gVsyFrnODyYQoTJt3Z1ZYyoXQD4q1NRvuPtn1l9HblMYFU0gThBQsXG1xWHwTkBBKLZ6ZrwxvkVUGIWo4IZYQNqsImKQ0TO_Df3s13pyUhB6yBw44l9qAO-ExNSXdjpgFgfLtL9hlDbe8WdW6RNyKy7JpcSBd-fDRzDTyTc1oboXQQ5SHABvK43ZyQ--oYxJ53ax2BtS_61OAoc1XCvOOP2jKdVwOCQxCX-A",
    "content-type": "application/json",
    "sec-ch-ua": "\"Not)A;Brand\";v=\"8\", \"Chromium\";v=\"138\"",
    "sec-ch-ua-mobile": "?0",
    "sec-ch-ua-platform": "\"macOS\"",
    "sec-fetch-dest": "empty",
    "sec-fetch-mode": "cors",
    "sec-fetch-site": "same-site",
    "x-desktop-app": "true",
    "x-user-id": "user_01KG253T8WAR93G9RNQM34FXXM",
    "Referer": "http://localhost:5176/"
  },
  "body": null,
  "method": "POST"
});
```

### Response

```json
{
    "success": true,
    "stripePublishableKey": "pk_test_51Sx310FVCoTu07s0n2IiTJgJY8mIKdPTNfkEJwX8NxbTJzVfiKg4e6am4Ge7xxMqzwKtOE8G48WMiCGj2gcz1nX800wzNtBNwH",
    "stripeSecretKey": "sk_test_<REDACTED>",
    "stripeAccountId": "acct_1Sx310FVCoTu07s0",
    "devEnvVars": {
        "NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY": "pk_test_51Sx310FVCoTu07s0n2IiTJgJY8mIKdPTNfkEJwX8NxbTJzVfiKg4e6am4Ge7xxMqzwKtOE8G48WMiCGj2gcz1nX800wzNtBNwH",
        "STRIPE_SECRET_KEY": "sk_test_<REDACTED>"
    }
}
```

The chat should know that stripe has been successfully connected and proceed to using the skill to connect stripe into the code.

---

### The banner in question

```html
<div style="display: flex; flex-direction: column; gap: 12px; padding: 14px 16px; border-radius: 10px; background-color: var(--bfloat-bg-secondary, #1a1a2e); border: 1px solid var(--bfloat-border, rgba(255,255,255,0.08));"><div style="display: flex; align-items: center; gap: 8px;"><svg width="20" height="20" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" clip-rule="evenodd" d="M3 4C3 3.44772 3.44772 3 4 3H20C20.5523 3 21 3.44772 21 4V20C21 20.5523 20.5523 21 20 21H4C3.44772 21 3 20.5523 3 20V4Z" fill="#635BFF"></path><path fill-rule="evenodd" clip-rule="evenodd" d="M11.1 10.2C11.1 9.6 11.6 9.35 12.4 9.35C13.5 9.35 14.9 9.7 16 10.3V7.3C14.8 6.8 13.6 6.6 12.4 6.6C9.8 6.6 8.1 8 8.1 10.35C8.1 14.05 13.3 13.45 13.3 15.05C13.3 15.75 12.7 16 11.85 16C10.65 16 9.1 15.5 7.9 14.8V17.85C9.2 18.45 10.55 18.7 11.85 18.7C14.5 18.7 16.3 17.35 16.3 14.95C16.3 10.95 11.1 11.7 11.1 10.2Z" fill="white"></path></svg><span style="font-size: 13px; color: var(--bfloat-text-secondary, #a0a0b8);">To add Stripe payments, let's get it connected first.</span></div><button style="padding: 8px 16px; border-radius: 8px; border: none; background-color: var(--bfloat-accent, #6c5ce7); color: rgb(255, 255, 255); font-size: 13px; font-weight: 500; cursor: pointer; align-self: flex-start;">Set up Stripe</button></div>
```
