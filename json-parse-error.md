I am seeing a JSON parse error on the payments tab of the project page. 

```html
<div class="flex flex-col items-center justify-center h-full gap-4"><p class="text-sm text-destructive">Failed to parse response as JSON</p><button class="h-8 px-3 text-sm font-medium bg-primary hover:bg-primary/90 text-primary-foreground rounded-md flex items-center gap-2 transition-colors"><svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="lucide lucide-refresh-cw h-3.5 w-3.5" aria-hidden="true"><path d="M3 12a9 9 0 0 1 9-9 9.75 9.75 0 0 1 6.74 2.74L21 8"></path><path d="M21 3v5h-5"></path><path d="M21 12a9 9 0 0 1-9 9 9.75 9.75 0 0 1-6.74-2.74L3 16"></path><path d="M8 16H3v5"></path></svg>Retry</button></div>
```

let's figure out what's going on and resolve this.

---

I believe this is hitting a non-existent overview endpoint

```ts
fetch("http://localhost:3000/api/payments/stripe/overview", {
  "headers": {
    "accept": "application/json",
    "accept-language": "en-US",
    "authorization": "Bearer eyJhbGciOiJSUzI1NiIsImtpZCI6InNzb19vaWRjX2tleV9wYWlyXzAxS0cwODNRMUFCS01ZNllaRjlHRDk5WTdZIn0.eyJpc3MiOiJodHRwczovL2FwaS53b3Jrb3MuY29tL3VzZXJfbWFuYWdlbWVudC9jbGllbnRfMDFLRzA4M1FFTkhNQ0ZIQVJTUzJOWkFYRzYiLCJzdWIiOiJ1c2VyXzAxS0cyNTNUOFdBUjkzRzlSTlFNMzRGWFhNIiwic2lkIjoic2Vzc2lvbl8wMUtIMThSNFgxRTI2WTlHRTNUSFdLOFBGQSIsImp0aSI6IjAxS0gxOVcyM05KN1BLMjgxTjEwOFEwMlYyIiwib3JnX2lkIjoib3JnXzAxS0c3RUVYUjRROVM3WVlBR1oyWEtaWjhUIiwicm9sZSI6Im93bmVyIiwicm9sZXMiOlsib3duZXIiXSwicGVybWlzc2lvbnMiOltdLCJleHAiOjE3NzA2NDQ0NzksImlhdCI6MTc3MDY0NDE3OX0.bpYPpct1hgoGC80VqWyrj98tZgBQ2pLl8nfo-yq2vvFTzNgipYBmKVSzMe8bxLYIbCYWpgBpyD7iH-S1Sv76grS4vfy9yBp1_Adp2aj9xDPwCP1D22ECGxy1IC5J3rk7Yg3ezGPyoLrusdaYfLM5smgmCBecyVTAx9pI8VX763w_MAWA4mrOyP6Hy4CK1zqtOqKv4bXbYsw6ql_wI9RW2BV-r1qiq2fOHXY_ZsybNE3HyTCVV2IPAZLF34ZGyMKKLIWN7wyoc0B0QoT2EKq_ydXMxQuE9cXsEr20D3AxZcr7RV8KXY8KjFOS6xxuK7B2mJh0f9ihlOCAEaD3J_5yqw",
    "content-type": "application/json",
    "sec-ch-ua": "\"Not)A;Brand\";v=\"8\", \"Chromium\";v=\"138\"",
    "sec-ch-ua-mobile": "?0",
    "sec-ch-ua-platform": "\"macOS\"",
    "sec-fetch-dest": "empty",
    "sec-fetch-mode": "cors",
    "sec-fetch-site": "same-site",
    "x-desktop-app": "true",
    "x-user-id": "user_01KG253T8WAR93G9RNQM34FXXM",
    "Referer": "http://localhost:5173/"
  },
  "body": null,
  "method": "GET"
});
```

Which then falls through to the default html response.

Check the `/Users/v1b3m/Dev/bfloat/app-engineer` repo for this endpoint and import it onto this side. check for any other missing routes.

---

