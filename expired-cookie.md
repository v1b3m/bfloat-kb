We support deployments to testflight via expo.

I tried running into today and noticed in the log that the cookie had expired.

```
See usage in billing.: https://expo.dev/accounts/bfloat/settings/billingResolved "production" environment for the build. Learn more: https://docs.expo.dev/eas/environment-variables/#setting-the-environment-for-your-buildsNo environment variables with visibility "Plain text" and "Sensitive" found for the "production" environment on EAS.No remote versions are configured for this project, buildNumber will be initialized based on the value from the local project.
Initializing buildNumber with 1.

Using remote iOS credentials (Expo server)If you provide your Apple account credentials we will be able to generate all necessary build credentials and fully validate them.This is optional, but without Apple account access you will need to provide all the missing values manually and we can only run minimal validation on them.? Do you want to log in to your Apple account? › (Y/n)Do you want to log in to your Apple account? … yes› Restoring session /Users/v1b3m/.app-store/auth/ben@bfloat.ai/cookie› Session expired Local session› The password is only used to authenticate with Apple and never stored on EAS servers\n\n  Learn more: https://bit.ly/2VtGWhU? Password (for ben@bfloat.ai): ›
```

We need to handle this gracefully.

1. Can we tell from the cookie itself if it's expired or not such that we prompt users to log in again with a password before we get to the deployment step
2. If for some reason, we cannot, what's the best way forward?

---
