Run the build locally, push it to s3 directly, you could simply change a version number, build and push to s3, then ensure the local build should have a less number, this way it should be able to detect the new version.

## Credentials to s3

```sh
export AWS_ACCESS_KEY_ID=<YOUR_AWS_ACCESS_KEY_ID>
export AWS_SECRET_ACCESS_KEY=<YOUR_AWS_SECRET_ACCESS_KEY>
```

---

`REVENUECAT_PUBLIC_SDK_API_KEY`
- this way revenuecat knows if you're in dev or prod

---

- `stripe_connect_fields`

Go to app-engineer and look at these APIs
- `generate-dev`
- `/generate-prod`

We need these in the workbench

Also, look at migrations after `stripe_connect_fields`, we need to have these in the workbench too.

Also important to look at the `/services` subfolder. We don't have this in workbench.

	- `dev-env-generator.server.ts`
	- `prod-env-generator.server.ts`
	- 

I think we lost stuff during the merging of codebases.

---
