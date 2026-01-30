# Shared Projects Feature

## Overview

Projects can be shared with team members through WorkOS Organizations. When a project is assigned to a team, all members of that team can access and work on the project.

## Architecture

### Backend

**Endpoint:** `GET /api/projects/shared` (`app/routes/api.projects.shared.ts`)

Fetches projects shared with the current user through their team memberships (excludes projects the user owns).

```typescript
// Flow:
1. Get user's team memberships from WorkOS
2. Query projects where teamId is in user's teams AND userId is NOT the current user
3. Enrich projects with team names from WorkOS
4. Return shared projects list
```

**Project Model:** Projects have a `teamId` field that links them to a WorkOS Organization.

### Frontend

**API:** `projectApi.listShared()` in `lib/api/project.ts`

**UI:** "Shared with you" section on HomePage (`app/components/home/HomePage.tsx`)
- Displays below "Open existing project" section
- Shows project cards with team name badges
- Respects the same grid/list view toggle and search filter

**Project Settings:** Team sharing dropdown in `ProjectSettings.tsx`
- Only shows teams where user is owner or admin
- Allows assigning/removing project from team

## What Gets Shared

| Data | Shared? | Storage |
|------|---------|---------|
| Project files | Yes | Git (Gitea) |
| Project metadata (title, description, slug) | Yes | PostgreSQL |
| App icons | Yes | S3 |
| Project settings (bundle IDs, etc.) | Yes | PostgreSQL |
| **Chat/conversation history** | **No** | Local (Claude Code CLI) |
| Agent sessions | No | Local per-user |

## Current Limitations

### 1. Chat History Not Shared

Conversation history with Claude/Codex is stored locally via CLI sessions, not in the cloud. When a team member opens a shared project, they:
- See all project files
- Start with a fresh conversation (no history from the project owner)
- Have their own separate agent sessions

**Technical reason:** The `messages` field on projects is deprecated. Sessions are managed locally by Claude Code CLI, with only metadata (sessionId, provider) stored in the `AgentSession` table.

**Potential solutions:**
1. Store messages in cloud database (revert from local-only)
2. Build sync mechanism between local sessions and server
3. Export/import conversation logs

### 2. WorkOS Organizations = Teams

WorkOS only provides "Organizations" - there's no separate "Team" concept. We rebrand Organizations as "Teams" in the UI for user-friendliness.

Mapping:
- WorkOS Organization = Team
- WorkOS Organization Membership = Team member
- WorkOS Invitation = Team invitation

### 3. Role-Based Sharing

Currently, only team owners and admins can share projects with a team. Regular members can view shared projects but cannot share their own projects to the team.

### 4. No Granular Permissions

All team members have the same access level to shared projects. There's no read-only vs edit distinction - if you can see it, you can edit it.

## Database Schema

```prisma
model Project {
  // ... other fields
  teamId    String?

  @@index([teamId])
}
```

The `teamId` references a WorkOS Organization ID, not a local database table.

## API Responses

### GET /api/projects/shared

```json
{
  "success": true,
  "projects": [
    {
      "id": "project-uuid",
      "title": "Shared App",
      "description": "...",
      "appType": "mobile",
      "teamId": "org_xxx",
      "teamName": "My Team",
      "userId": "user_xxx",
      "createdAt": "2024-01-15T...",
      "updatedAt": "2024-01-20T..."
    }
  ]
}
```

### PATCH /api/projects/:id (to share/unshare)

```json
// Request
{ "teamId": "org_xxx" }  // or null to unshare

// Response
{
  "success": true,
  "data": { /* updated project */ }
}
```

## Future Considerations

1. **Shared conversation context** - Allow team members to see conversation history
2. **Activity feed** - Show who made changes to shared projects
3. **Notifications** - Alert when someone shares a project with your team
4. **Granular permissions** - Read-only access, per-project roles
5. **Transfer ownership** - Allow transferring project ownership to another user
