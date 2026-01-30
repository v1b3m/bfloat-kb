# Connect Northflank for Web Deployment

This guide is for developers setting up Northflank web deployment locally.

## Environment Variables

### Backend (bfloat-app-engineer)

Add to your `.env` file:

````bash
# Northflank Configuration
NORTHFLANK_API_TOKEN=nf-eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJ1dWlkIjoiOGVhOTQxMjYtMDM0NC00NTI4LWFhYTctNzRlMjNhYjExOGQ3IiwiZW50aXR5SWQiOiI2OTc0YmFlODk2YmMzZWIzMTA1MDQzMWUiLCJlbnRpdHlUeXBlIjoidGVhbSIsInRva2VuSWQiOiI2OTc3YmQxNDRiYTkxYWIxZWU3YjMwYTMiLCJ0b2tlbkludGVybmFsSWQiOiJkZXBsb3kiLCJyb2xlSWQiOiI2OTc3YjkzOWE1MWU3M2Q2ZmRiOGUxNmEiLCJyb2xlSW50ZXJuYWxJZCI6Im9yZy9kZXBsb3ktYXBwcyIsInR5cGUiOiJ0ZW1wbGF0ZSIsImlhdCI6MTc2OTQ1NDg2OH0.GjeQFDb3IrGVkzRd4dEw6hAChbOSBBmNTRLVzdBjbJU
NORTHFLANK_PROJECT_ID=bfloat-apps
NORTHFLANK_BASE_DOMAIN=bfloat.app
NORTHFLANK_REGISTRY_CREDENTIALS_ID=your_credential_id_from_northflank

# GCP Configuration
GCP_PROJECT_ID=endless-apogee-462504-q5
GCP_REGION=us-central1
ARTIFACT_REGISTRY_REPO=bfloat-apps
CLOUDBUILD_SERVICE_ACCOUNT=cloudbuild-sa@endless-apogee-462504-q5.iam.gserviceaccount.com```

### IDE (bfloat-ide)

The IDE primarily needs Git access token for cloning repos:

```bash
# Git Access Token (for cloning/pushing to Gitea)
GIT_ACCESS_TOKEN=50d323eb8b90b1539c3ac432a070e86d0d7d7b56```

> *Note:* The IDE doesn't directly call Northflank - it makes requests to the backend which handles all Northflank/GCP operations.

---

## Getting Required Values

### Northflank API Token

1. Go to [Northflank](https://app.northflank.com)
2. *Account Settings* → *API access*
3. Create a new token
4. Copy and add to `.env`

### Northflank Project ID

1. Go to your project in Northflank
2. The project ID is in the URL: `app.northflank.com/projects/{PROJECT_ID}/...`
3. For Bfloat: `bfloat-apps`

### Registry Credentials ID

1. *Account Settings* → *Integrations* → *Registries*
2. Add your GCP Artifact Registry credentials (see [configure-northflank.md](./configure-northflank.md))
3. After saving, copy the *Credential ID* shown in the list

### GCP Values

Already configured in the project. Use the values shown above.

---

## Common Issues & Resolutions

### Issue 1: "403 Permission Denied" from Cloud Build

*Error:*
```[CloudBuild] API error: 403 - Permission denied```

*Cause:* Your local GCP credentials lack Cloud Build permissions.

*Resolution:*
You have two options:

*Option A:* Use the Cloud Build service account (recommended)
```bash
# Authenticate as the Cloud Build SA
gcloud auth activate-service-account cloudbuild-sa@endless-apogee-462504-q5.iam.gserviceaccount.com --key-file=path/to/key.json```

*Option B:* Grant your user account Cloud Build roles
```bash
gcloud projects add-iam-policy-binding endless-apogee-462504-q5 \
  --member="user:your-email@example.com" \
  --role="roles/cloudbuild.builds.builder"```

### Issue 2: Git Clone Prompts for Password

*Error:* Git operations ask for username/password when cloning from Gitea.

*Cause:* `GIT_ACCESS_TOKEN` not available in the IDE's main process.

*Resolution:*
1. Make sure `.env` file exists in the bfloat-ide root directory
2. Add `GIT_ACCESS_TOKEN=your_token` to `.env`
3. Restart the IDE

*Note:* The IDE loads `.env` in the main process via `dotenv.config()` in `lib/main/main.ts`

### Issue 3: "Subdomain not found" when deploying

*Error:*
```Subdomain 'xxx.bfloat.app' not found, and no wildcard parent domain present```

*Cause:* Wildcard domain not configured in Northflank.

*Resolution:* Follow the wildcard domain setup in [configure-northflank.md](./configure-northflank.md#step-2-configure-wildcard-domain-in-northflank)

### Issue 4: "Service must be deployed in the same cluster as the domain's redirect target"

*Error:*
```Subdomain cannot be created because its parent domain is configured to redirect to a different cluster```

*Cause:* Domain wildcard routing points to a different region/cluster than your project.

*Resolution:*
* Check your project's cluster (e.g., `gcp/us-west1`)
* Reconfigure the domain with wildcard routing to the *same cluster*
* Note: `us-west` (Northflank cloud) ≠ `gcp/us-west1` (your GCP cluster)

### Issue 5: "NORTHFLANK_REGISTRY_CREDENTIALS_ID not set - private registry will fail"

*Warning in logs:*
```[Northflank] NOTE: NORTHFLANK_REGISTRY_CREDENTIALS_ID not set - private registry will fail```

*Cause:* Artifact Registry credentials not configured in Northflank.

*Resolution:*
1. Follow [configure-northflank.md](./configure-northflank.md#step-3-add-artifact-registry-credentials-to-northflank) Step 3
2. Add the credential ID to your `.env`

### Issue 6: "lstat /workspace/app/Dockerfile: no such file or directory"

*Error in Cloud Build logs:*
```lstat /workspace/app/Dockerfile: no such file or directory```

*Cause:* Repository doesn't have a Dockerfile.

*Resolution:* The Cloud Build config auto-generates a Dockerfile for Node.js apps. Ensure your repo has a `package.json` file at the root.

### Issue 7: "405 Method Not Allowed" when updating existing service

*Error:*
```[Northflank] API error: 405 - Method Not Allowed```

*Cause:* Using deprecated API endpoint/method.

*Resolution:* This was fixed in a recent update. Make sure you have the latest `northflank/client.ts`.

---

## Quick Start Checklist

* [ ] Backend `.env` has all Northflank variables
* [ ] IDE `.env` has `GIT_ACCESS_TOKEN`
* [ ] You have authenticated with GCP (or using service account key)
* [ ] Wildcard domain `bfloat.app` is configured in Northflank
* [ ] Artifact Registry credentials are added to Northflank

---

## Testing Your Setup

1. Create a simple web project in the IDE
2. Click *Deploy to Web*
3. Check backend logs for `[Northflank]` messages
4. If successful, you'll see:

```   [Northflank] Service created: bfloat-xxxxx
   [Northflank] URL: https://xxxxx.bfloat.app
````

---

## Related Documentation

- [configure-northflank.md](./configure-northflank.md) - Complete setup guide
- [northflank-keep-in-mind.md](./northflank-keep-in-mind.md) - Scalability considerations

---

_Last updated: January 2025_
