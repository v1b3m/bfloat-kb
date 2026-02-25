# Configure Northflank for Web Deployment

This guide covers the complete setup of Northflank for deploying Bfloat web applications with wildcard subdomain routing.

## Overview

The deployment flow:
```
Gitea Repository → Cloud Build → Artifact Registry → Northflank → https://<app-id>.bfloat.app
```

## Prerequisites

- Google Cloud project with Artifact Registry enabled
- Northflank account with team access
- Domain configured to point to Northflank
- GCP service account with appropriate permissions

---

## Step 1: Configure GCP Service Accounts

### Cloud Build Service Account

Create a service account for Cloud Build with permission to impersonate:

```bash
# Create the service account
gcloud iam service-accounts create cloudbuild-sa \
  --project=endless-apogee-462504-q5 \
  --display-name="Cloud Build Service Account"

# Grant Cloud Build roles
gcloud projects add-iam-policy-binding endless-apogee-462504-q5 \
  --member="serviceAccount:cloudbuild-sa@endless-apogee-462504-q5.iam.gserviceaccount.com" \
  --role="roles/cloudbuild.builds.builder"

# Grant permission to impersonate
gcloud iam service-accounts add-iam-policy-binding cloudbuild-sa@endless-apogee-462504-q5.iam.gserviceaccount.com \
  --project=endless-apogee-462504-q5 \
  --member="serviceAccount:549280315750-compute@developer.gserviceaccount.com" \
  --role="roles/iam.serviceAccountUser"
```

### Artifact Registry Access

Ensure the Cloud Build service account can access Artifact Registry:

```bash
gcloud projects add-iam-policy-binding endless-apogee-462504-q5 \
  --member="serviceAccount:cloudbuild-sa@endless-apogee-462504-q5.iam.gserviceaccount.com" \
  --role="roles/artifactregistry.reader"
```

---

## Step 2: Configure Wildcard Domain in Northflank

### 2.1 Add Domain with Wildcard Routing

1. Navigate to **Team → Domains** in Northflank
2. Click **Add domain**
3. Enter your domain: `bfloat.app`
4. Expand **Advanced options**
5. Configure:
   - **Domain routing:** `wildcard redirect`
   - **Region:** Select your cluster (e.g., `gcp/us-west1` - must match your project's cluster)
   - **Certificate generation:** `wildcard via DCV`
6. Click **Add domain**

> **Important:** Wildcard routing restricts the domain to a single region. All services using this domain must be in the same cluster.

> **Critical:** Select the correct cluster type. `us-west` (Northflank's cloud) is different from `gcp/us-west1` (your GCP BYO cloud). The region must match your project's cluster.

### 2.2 Verify Domain Ownership

Northflank will provide a TXT record for verification:

| Field | Value |
|-------|-------|
| **Type** | `TXT` |
| **Host** | `verify-xxxxx` (provided by Northflank) |
| **Value** | (verification code provided by Northflank) |

Add this in your DNS provider (Namecheap, Cloudflare, etc.) and click **Verify** in Northflank.

### 2.3 Add CNAME for Wildcard Routing

Northflank will provide a CNAME record for wildcard routing:

| Field | Value |
|-------|-------|
| **Type** | `CNAME` |
| **Host** | `*` |
| **Value** | `*.bfloat.app.depl-xxx.dns.northflank.app` (provided by Northflank) |

Add this in your DNS provider and click **Verify** in Northflank.

---

## Step 3: Add Artifact Registry Credentials to Northflank

### 3.1 Create Service Account Key

1. Go to **Google Cloud → IAM & Admin → Service Accounts**
2. Create or select a service account with `Storage Object Viewer` role
3. Go to **Keys** tab
4. Click **Add Key → Create new key**
5. Select **JSON** and download

### 3.2 Add Credentials in Northflank

1. Navigate to **Account Settings → Integrations → Registries**
2. Click **Add registry credentials**
3. Select **Google Container Registry** from the dropdown
4. For GCP Artifact Registry, use this format:
   ```
   us-central1-docker.pkg.dev/endless-apogee-462504-q5/bfloat-apps
   ```
5. Upload the JSON keyfile
6. Save and copy the **Credential ID**

---

## Step 4: Configure Environment Variables

Set these environment variables in your backend:

```bash
# Northflank Configuration
NORTHFLANK_API_TOKEN=your_northflank_api_token
NORTHFLANK_PROJECT_ID=bfloat-apps
NORTHFLANK_BASE_DOMAIN=bfloat.app
NORTHFLANK_REGISTRY_CREDENTIALS_ID=<credential-id-from-step-3>

# GCP Configuration
GCP_PROJECT_ID=endless-apogee-462504-q5
GCP_REGION=us-central1
ARTIFACT_REGISTRY_REPO=bfloat-apps
CLOUDBUILD_SERVICE_ACCOUNT=cloudbuild-sa@endless-apogee-462504-q5.iam.gserviceaccount.com
```

### Getting the Northflank API Token

1. Go to **Account Settings → API access**
2. Create a new token
3. Copy and save securely

---

## Step 5: DNS Record Summary

Your DNS should have these records:

| Type | Host | Value |
|------|------|-------|
| CNAME | `@` | `bfloat.app.depl-xxx.dns.northflank.app` |
| CNAME | `*` | `*.bfloat.app.depl-xxx.dns.northflank.app` |
| TXT | `verify-xxxxx` | (verification code) |

---

## How It Works

### Service Naming

- **Service name:** `bfloat-{short-project-id}` (e.g., `bfloat-0b306b72`)
- **Subdomain:** `{short-project-id}.bfloat.app` (e.g., `0b306b72f7ad.bfloat.app`)
- **Image:** `us-central1-docker.pkg.dev/endless-apogee-462504-q5/bfloat-apps/{short-project-id}:latest`

### Deployment Flow

1. User clicks **Deploy to Web** in Bfloat IDE
2. Backend triggers Cloud Build with Gitea repo URL
3. Cloud Build clones repo, builds Docker image, pushes to Artifact Registry
4. Backend calls Northflank API to create/update service
5. Northflank pulls image from private registry (using credentials)
6. Service is deployed and accessible at `https://{app-id}.bfloat.app`

### Updating Existing Services

If a service already exists for a project, the deployment:
1. Updates the Docker image
2. Triggers a redeployment
3. URL remains the same

---

## Troubleshooting

### Error: "Subdomain not found, and no wildcard parent domain present"

**Cause:** Wildcard domain not configured or verified in Northflank.

**Solution:** Complete Step 2 to add and verify the wildcard domain.

### Error: "Service must be deployed in the same cluster as the domain's redirect target"

**Cause:** Domain configured for one region/cluster, but project is in another.

**Solution:** Ensure the wildcard domain region matches your project's cluster.
- If project is on `gcp/us-west1`, configure domain for `gcp/us-west1`
- NOT `us-west` (that's Northflank's own cloud, different from GCP BYO cloud)

### Error: "NORTHFLANK_REGISTRY_CREDENTIALS_ID not set - private registry will fail"

**Cause:** Registry credentials not configured.

**Solution:** Complete Step 3 and set the environment variable.

### Build Failures

**Error:** "lstat /workspace/app/Dockerfile: no such file or directory"

**Solution:** The Cloud Build config auto-generates a Dockerfile for Node.js apps. Ensure your repo has a `package.json`.

**Error:** "npm ci failed"

**Solution:** Changed to `npm install` in build config (no package-lock.json required).

---

## Resources

- [Northflank: Wildcard domains and certificates](https://northflank.com/docs/v1/application/domains/wildcard-domains-and-certificates)
- [Northflank: Save registry credentials](https://northflank.com/docs/v1/application/run/save-registry-credentials)
- [Northflank: Use path-based routing](https://northflank.com/docs/v1/application/domains/use-path-based-routing)
- [Northflank: Domains on Northflank](https://northflank.com/docs/v1/application/domains/domains-on-northflank)
- [Google: Configure authentication to Artifact Registry for Docker](https://docs.cloud.google.com/artifact-registry/docs/docker/authentication)

---

## Quick Reference

### Northflank API Endpoints

```
POST /projects/{projectId}/services/deployment  # Create service
PATCH /projects/{projectId}/services/{serviceId} # Update service
POST /projects/{projectId}/services/{serviceId}/deployments # Trigger deployment
GET /projects/{projectId}/services/{serviceId}  # Get service status
DELETE /projects/{projectId}/services/{serviceId} # Delete service
```

### Key Files

- `/app/lib/northflank/client.ts` - Northflank API client
- `/app/lib/cloudbuild/client.ts` - Cloud Build integration
- `.env` - Environment variables

---

*Last updated: January 2025*
