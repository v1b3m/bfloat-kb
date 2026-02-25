# Local Gitea Setup

Complete guide for setting up local Gitea for Bfloat project storage.

## Prerequisites

- Docker installed
- git installed

## Quick Start

### 1. Start Gitea

```bash
docker-compose -f docker-compose.gitea.yml up -d
```

Wait a few seconds for Gitea to start, then verify:

```bash
curl http://localhost:3001/api/v1/version
```

Should return: `{"version":"1.21.11"}`

### 2. Create Admin User

```bash
docker exec bfloat-gitea gitea admin user create \
  --username bfloat \
  --password bfloat123 \
  --email bfloat@local \
  --admin \
  --must-change-password=false
```

### 3. Generate Access Token

```bash
docker exec bfloat-gitea gitea admin user generate-access-token \
  --username bfloat \
  --scopes 'all'
```

Copy the returned token.

### 4. Configure Environment

Add to `.env`:

```bash
# Git Storage (Gitea)
USE_GIT_STORAGE=true
GIT_BASE_URL=http://localhost:3001
GIT_ORGANIZATION=bfloat-projects
GIT_DEFAULT_BRANCH=main
GIT_ACCESS_TOKEN=<your-token-from-step-3>
```

### 5. Setup Template Repositories

```bash
GIT_BASE_URL=http://localhost:3001 GIT_ACCESS_TOKEN=<your-token> \
  npx tsx app/scripts/setup-templates.ts
```

### 6. Configure Permissions

Add the `bfloat` user to the Owners team with repo creation permission:

```bash
# Get team ID
curl -s -H "Authorization: token <your-token>" \
  "http://localhost:3001/api/v1/orgs/bfloat-projects/teams" | jq '.[] | {id, name}'

# Add user to Owners team (ID is usually 2)
curl -s -X PUT -H "Authorization: token <your-token>" \
  "http://localhost:3001/api/v1/teams/2/members/bfloat"

# Enable repo creation for the team
curl -s -X PATCH -H "Authorization: token <your-token>" \
  -H "Content-Type: application/json" \
  "http://localhost:3001/api/v1/teams/2" \
  -d '{"can_create_org_repo":true}'
```

## Verify Setup

Check that everything is accessible:

```bash
# Gitea version
curl http://localhost:3001/api/v1/version

# Templates org
curl -H "Authorization: token <your-token>" \
  http://localhost:3001/api/v1/orgs/bfloat-templates

# Projects org
curl -H "Authorization: token <your-token>" \
  http://localhost:3001/api/v1/orgs/bfloat-projects

# Template repos
curl -H "Authorization: token <your-token>" \
  http://localhost:3001/api/v1/repos/bfloat-templates/expo-default
```

## Access Points

| Service | URL |
|---------|-----|
| Gitea Web UI | http://localhost:3001 |
| Templates Org | http://localhost:3001/bfloat-templates |
| Projects Org | http://localhost:3001/bfloat-projects |
| Expo Template | http://localhost:3001/bfloat-templates/expo-default |
| Next.js Template | http://localhost:3001/bfloat-templates/nextjs-default |

## Default Credentials

- **Username:** `bfloat`
- **Password:** `bfloat123`
- **Email:** `bfloat@local`

## Troubleshooting

### Gitea won't start

```bash
# Check logs
docker logs bfloat-gitea

# Restart
docker-compose -f docker-compose.gitea.yml restart
```

### Permission denied errors

Ensure the `bfloat` user is in the Owners team with `can_create_org_repo: true`:

```bash
curl -s -H "Authorization: token <your-token>" \
  "http://localhost:3001/api/v1/teams/2" | jq '{name, can_create_org_repo}'
```

### Template repos empty

Re-run the setup script:

```bash
GIT_BASE_URL=http://localhost:3001 GIT_ACCESS_TOKEN=<your-token> \
  npx tsx app/scripts/setup-templates.ts
```

### Reset everything

```bash
# Stop Gitea
docker-compose -f docker-compose.gitea.yml down

# Remove volumes
docker volume rm bfloat-app-engineer_gitea_data bfloat-app-engineer_gitea_config

# Start fresh
docker-compose -f docker-compose.gitea.yml up -d
```

## API Examples

### Create a repo manually

```bash
curl -X POST \
  -H "Authorization: token <your-token>" \
  -H "Content-Type: application/json" \
  "http://localhost:3001/api/v1/orgs/bfloat-projects/repos" \
  -d '{
    "name": "my-test-repo",
    "description": "Test repository",
    "private": true,
    "auto_init": true,
    "default_branch": "main"
  }'
```

### Generate from template

```bash
curl -X POST \
  -H "Authorization: token <your-token>" \
  -H "Content-Type: application/json" \
  "http://localhost:3001/api/v1/repos/bfloat-templates/expo-default/generate" \
  -d '{
    "name": "my-new-project",
    "owner": "bfloat-projects",
    "git_content": true,
    "private": true
  }'
```
