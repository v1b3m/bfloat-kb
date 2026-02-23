Production deployments are failing with the error below:

```
[main 1d871ad] Configure for deployment6 files changed, 14764 insertions(+), 244 deletions(-)
create mode 100644 package-lock.jsonnpm warn Unknown env config "shamefully-hoist". This will stop working in the next major version of npm.npm warn Unknown env config "npm-globalconfig". This will stop working in the next major version of npm.
npm warn Unknown env config "verify-deps-before-run". This will stop working in the next major version of npm.
npm warn Unknown env config "_jsr-registry". This will stop working in the next major version of npm.
npm warn Unknown env config "recursive". This will stop working in the next major version of npm.
npm warn Unknown env config "node-linker". This will stop working in the next major version of npm.
npm warn Unknown env config "python". This will stop working in the next major version of npm.
npm warn Unknown user config "python". This will stop working in the next major version of npm.Using EAS CLI without version control system is not recommended, use this mode only if you know what you are doing.eas.json is not valid.
- "submit.production.android.serviceAccountKeyPath" is not allowed to be empty
- "submit.production.ios.ascApiKeyPath" is not allowed to be empty
- "submit.production.ios.ascApiKeyId" is not allowed to be empty
- "submit.production.ios.ascApiKeyIssuerId" is not allowed to be emptyError: build command failed.
```

[[production-deploys-triage]]

