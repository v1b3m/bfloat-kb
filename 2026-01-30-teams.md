# Teams Feature Implementation (WorkOS Organizations)

## Overview

Add team support to bfloat IDE using WorkOS Organizations. Users can create teams (organizations), invite members via email, and share projects with their team.

**Repositories:**
- IDE: `/Users/v1b3m/Dev/bfloat/bfloat-ide`
- Backend: `/Users/v1b3m/Dev/bfloat/bfloat-app-engineer`

**References:**
- [WorkOS Organizations API](https://workos.com/docs/reference/organization)
- [WorkOS Organization Memberships](https://workos.com/docs/reference/user-management/organization-membership)
- [WorkOS Invitations](https://workos.com/docs/user-management/invitations)

---

## Design Decisions

### 1. WorkOS Organizations as Teams

**Decision:** Use WorkOS Organizations to manage teams instead of local database tables.

**Rationale:**
- WorkOS was specifically chosen over Clerk for its Organizations feature
- Enterprise-ready: SSO, SCIM directory sync, audit logs per organization
- Managed invitations with email delivery handled by WorkOS
- Built-in role management with custom roles support
- Future-proof for B2B features (per-org billing, SSO enforcement)

**Implementation:**
- Teams = WorkOS Organizations
- Team memberships = WorkOS Organization Memberships
- Team invites = WorkOS Invitations with `organizationId`

### 2. Multi-team Membership

**Decision:** Users can belong to multiple teams (organizations).

**Rationale:**
- WorkOS natively supports multi-organization membership
- Common pattern in collaboration tools (Slack, Discord, GitHub)
- Matches real-world usage (freelancers, consultants)

**Implementation:**
- Use `listOrganizationMemberships({ userId })` to get all teams
- Frontend shows team switcher/selector

### 3. WorkOS Email Invitations

**Decision:** Use WorkOS Invitations API which automatically sends emails.

**Rationale:**
- No need to set up email service (SES, SendGrid)
- Professional email templates managed by WorkOS
- Built-in invitation tracking (pending, accepted, expired)
- Secure token-based acceptance flow

**Implementation:**
- `workos.userManagement.sendInvitation({ email, organizationId, roleSlug })`
- Invitation emails sent automatically by WorkOS
- Users click link → authenticate → join organization

### 4. Role-based Permissions

**Decision:** Three roles using WorkOS role slugs: `owner`, `admin`, `member`

**WorkOS Roles:**
- Define custom roles in WorkOS Dashboard
- Assign via `roleSlug` parameter when creating membership/invitation

**Permissions Matrix:**
| Action | Owner | Admin | Member |
|--------|-------|-------|--------|
| View team projects | Yes | Yes | Yes |
| Edit team projects | Yes | Yes | Yes |
| Invite members | Yes | Yes | No |
| Remove members | Yes | Yes (not owner) | No |
| Change member roles | Yes | Yes (not to owner) | No |
| Edit team settings | Yes | Yes | No |
| Delete team | Yes | No | No |
| Transfer ownership | Yes | No | No |

### 5. Project Sharing Model (Explicit)

**Decision:** Projects must be explicitly assigned to a team via `teamId`. No automatic sharing.

**Behavior:**
- `teamId = null` → Personal project (only owner can access)
- `teamId = "org_xxx"` → Team project (all team members can access)

**User Flow:**
1. User creates projects → Personal by default
2. User creates team, invites members
3. User explicitly assigns specific projects to team ("Move to Team" action)
4. Team members see only those assigned projects

**Implementation:**
- Add `teamId` (nullable) to `Project` model in Prisma
- Store WorkOS Organization ID in `teamId`
- Project list query: `WHERE userId = ? OR teamId IN (user's team IDs)`
- Authorization: check ownership OR organization membership before access

---

## WorkOS SDK Methods

### Organizations (Teams)

```typescript
import { workos } from '~/lib/workos/workos.server'

// Create organization (team)
const org = await workos.organizations.createOrganization({
  name: 'My Team',
  externalId: 'optional-external-id', // For mapping to external systems
  metadata: { tier: 'free' }, // Custom metadata
})

// Get organization
const org = await workos.organizations.getOrganization(orgId)

// List organizations (for admin purposes)
const orgs = await workos.organizations.listOrganizations({ limit: 100 })

// Update organization
await workos.organizations.updateOrganization({
  organizationId: orgId,
  name: 'New Team Name',
})

// Delete organization
await workos.organizations.deleteOrganization(orgId)
```

### Organization Memberships

```typescript
// Create membership (add user to team directly)
const membership = await workos.userManagement.createOrganizationMembership({
  organizationId: orgId,
  userId: userId,
  roleSlug: 'member', // or 'admin', 'owner'
})

// List user's memberships (get all teams for user)
const memberships = await workos.userManagement.listOrganizationMemberships({
  userId: userId,
})

// List organization's members
const members = await workos.userManagement.listOrganizationMemberships({
  organizationId: orgId,
})

// Update membership role
await workos.userManagement.updateOrganizationMembership(membershipId, {
  roleSlug: 'admin',
})

// Deactivate membership (soft delete - keeps history)
await workos.userManagement.deactivateOrganizationMembership(membershipId)

// Delete membership (hard delete)
await workos.userManagement.deleteOrganizationMembership(membershipId)
```

### Invitations

```typescript
// Send invitation to join organization
const invitation = await workos.userManagement.sendInvitation({
  email: 'user@example.com',
  organizationId: orgId,
  roleSlug: 'member',
  inviterUserId: currentUserId, // Shows who invited them
  expiresInDays: 7, // Optional expiration
})

// List pending invitations
const invitations = await workos.userManagement.listInvitations({
  organizationId: orgId,
})

// Revoke invitation
await workos.userManagement.revokeInvitation(invitationId)

// Get invitation by token (for accept page)
const invitation = await workos.userManagement.getInvitation(invitationId)
```

---

## Database Schema

### Modified Models Only

Since teams are managed by WorkOS, we only need to add `teamId` to projects:

```prisma
model Project {
  // ... existing fields
  teamId    String?  // WorkOS Organization ID

  @@index([teamId])
}
```

**No new tables needed** - WorkOS handles:
- Team (Organization) storage
- Membership storage
- Invitation storage

---

## API Design

### Teams Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/teams` | List user's teams | Yes |
| POST | `/api/teams` | Create new team | Yes |
| GET | `/api/teams/:id` | Get team details with members | Yes (member) |
| PATCH | `/api/teams/:id` | Update team name | Yes (admin+) |
| DELETE | `/api/teams/:id` | Delete team | Yes (owner) |

### Members Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/teams/:id/members` | List team members | Yes (member) |
| DELETE | `/api/teams/:id/members/:membershipId` | Remove member | Yes (admin+) |
| PATCH | `/api/teams/:id/members/:membershipId` | Update role | Yes (admin+) |

### Invitations Endpoints

| Method | Endpoint | Description | Auth Required |
|--------|----------|-------------|---------------|
| GET | `/api/teams/:id/invitations` | List pending invitations | Yes (admin+) |
| POST | `/api/teams/:id/invitations` | Send invitation email | Yes (admin+) |
| DELETE | `/api/teams/:id/invitations/:inviteId` | Revoke invitation | Yes (admin+) |

### Response Types

```typescript
interface Team {
  id: string              // WorkOS Organization ID
  name: string
  role: 'owner' | 'admin' | 'member'  // Current user's role
  memberCount: number
  createdAt: string
  updatedAt: string
}

interface TeamMember {
  membershipId: string    // WorkOS Membership ID
  userId: string
  email: string
  name: string
  profilePictureUrl: string | null
  role: 'owner' | 'admin' | 'member'
  status: 'active' | 'inactive' | 'pending'
  createdAt: string
}

interface TeamInvitation {
  id: string
  email: string
  role: string
  state: 'pending' | 'accepted' | 'revoked' | 'expired'
  inviterName: string
  expiresAt: string
  createdAt: string
}
```

---

## Backend Implementation

### Files to Create/Modify

**`app/lib/workos/workos.server.ts`** - Already exists, exports `workos`

**`app/dao/teams.server.ts`** - Teams data access layer:
```typescript
import { workos } from '~/lib/workos/workos.server'

// Get all teams for a user
export async function getTeamsForUser(userId: string) {
  const memberships = await workos.userManagement.listOrganizationMemberships({
    userId,
  })

  // Enrich with organization details
  const teams = await Promise.all(
    memberships.data.map(async (m) => {
      const org = await workos.organizations.getOrganization(m.organizationId)
      return {
        id: org.id,
        name: org.name,
        role: m.role?.slug || 'member',
        membershipId: m.id,
        createdAt: org.createdAt,
        updatedAt: org.updatedAt,
      }
    })
  )

  return teams
}

// Create team with user as owner
export async function createTeam(name: string, userId: string) {
  const org = await workos.organizations.createOrganization({ name })

  // Add creator as owner
  await workos.userManagement.createOrganizationMembership({
    organizationId: org.id,
    userId,
    roleSlug: 'owner',
  })

  return org
}

// Check if user is member of team
export async function getMembership(teamId: string, userId: string) {
  const memberships = await workos.userManagement.listOrganizationMemberships({
    userId,
    organizationId: teamId,
  })
  return memberships.data[0] || null
}

// Get team members with user details
export async function getTeamMembers(teamId: string) {
  const memberships = await workos.userManagement.listOrganizationMemberships({
    organizationId: teamId,
  })

  const members = await Promise.all(
    memberships.data.map(async (m) => {
      const user = await workos.userManagement.getUser(m.userId)
      return {
        membershipId: m.id,
        userId: user.id,
        email: user.email,
        name: `${user.firstName || ''} ${user.lastName || ''}`.trim() || user.email,
        profilePictureUrl: user.profilePictureUrl,
        role: m.role?.slug || 'member',
        status: m.status,
        createdAt: m.createdAt,
      }
    })
  )

  return members
}

// Send invitation
export async function sendInvitation(
  teamId: string,
  email: string,
  roleSlug: string,
  inviterUserId: string
) {
  return workos.userManagement.sendInvitation({
    email,
    organizationId: teamId,
    roleSlug,
    inviterUserId,
  })
}
```

**API Routes:**
- `app/routes/api.teams.ts` - List/create teams
- `app/routes/api.teams.$id.ts` - Team CRUD
- `app/routes/api.teams.$id.members.ts` - Member management (listing now included)
- `app/routes/api.teams.$id.invitations.ts` - Invitation management

---

## Frontend Architecture

### Components Structure

```
app/components/
├── settings/
│   └── sections/
│       └── TeamsSection.tsx       # Teams list in settings
├── teams/
│   ├── TeamDetailView.tsx         # Team management view
│   ├── TeamMembersList.tsx        # Members list component
│   ├── TeamInvitesList.tsx        # Pending invitations list
│   └── dialogs/
│       ├── CreateTeamDialog.tsx   # Create new team
│       ├── InviteMemberDialog.tsx # Send email invitation
│       └── ManageMemberDialog.tsx # Change role / remove
```

### UI Flow

1. **Settings > Teams**: Shows list of teams user belongs to
2. **Create Team**: Modal with name input → creates WorkOS Organization
3. **Team Detail**: Shows members, pending invitations, team settings
4. **Invite Flow**: Enter email → WorkOS sends invitation → User receives email → Clicks link → Joins team
5. **Accept Invite**: Handled by WorkOS authentication flow (automatic)

---

## Authorization Logic

```typescript
// Helper functions for permission checks
function canManageTeam(role: string): boolean {
  return role === 'owner' || role === 'admin'
}

function canDeleteTeam(role: string): boolean {
  return role === 'owner'
}

function canRemoveMember(actorRole: string, targetRole: string): boolean {
  if (actorRole === 'owner') return true
  if (actorRole === 'admin' && targetRole !== 'owner') return true
  return false
}

// Route protection pattern
async function requireTeamAdmin(teamId: string, userId: string) {
  const membership = await getMembership(teamId, userId)
  if (!membership) {
    throw new Response('Not a team member', { status: 403 })
  }
  if (!canManageTeam(membership.role?.slug || 'member')) {
    throw new Response('Insufficient permissions', { status: 403 })
  }
  return membership
}
```

---

## File Summary

### Backend (bfloat-app-engineer)

| File | Action | Description |
|------|--------|-------------|
| `prisma/schema.prisma` | Modify | Add `teamId` to Project model |
| `app/dao/teams.server.ts` | Create | Teams data access layer (wraps WorkOS) |
| `app/routes/api.teams.ts` | Create | List/create teams |
| `app/routes/api.teams.$id.ts` | Create | Team CRUD |
| `app/routes/api.teams.$id.members.ts` | Create | Member management |
| `app/routes/api.teams.$id.invitations.ts` | Create | Invitation management |

### Frontend (bfloat-ide)

| File | Action | Description |
|------|--------|-------------|
| `app/utils/teams-api.ts` | Create | API wrapper functions |
| `app/stores/teams.ts` | Create | Teams state store |
| `app/components/settings/sections/TeamsSection.tsx` | Create | Teams settings section |
| `app/components/teams/TeamDetailView.tsx` | Create | Team management view |
| `app/components/teams/dialogs/CreateTeamDialog.tsx` | Create | Create team dialog |
| `app/components/teams/dialogs/InviteMemberDialog.tsx` | Create | Invite dialog (email input) |
| `app/components/teams/dialogs/ManageMemberDialog.tsx` | Create | Manage member dialog |
| `app/components/settings/SettingsPage.tsx` | Modify | Add teams navigation |

---

## WorkOS Dashboard Setup

Before implementation, configure in WorkOS Dashboard:

1. **Create Custom Roles:**
   - `owner` - Full control
   - `admin` - Manage members and settings
   - `member` - Basic access (default)

2. **Configure Invitation Emails:**
   - Customize email templates if needed
   - Set default expiration

3. **Enable Organizations:**
   - Ensure Organizations feature is enabled for your environment

---

## Security Considerations

1. **Authentication**: All endpoints require authenticated user
2. **Authorization**: Check membership and role before operations
3. **Role escalation**: Prevent admins from promoting to owner
4. **Self-removal**: Users can leave teams (except last owner)
5. **Invitation expiry**: WorkOS handles automatic expiration
6. **Email validation**: WorkOS validates email format

---

## Implementation Order

1. Add `teamId` to Project schema + migration
2. Create teams DAO layer (wrapping WorkOS SDK)
3. Create API routes (teams, members, invitations)
4. Create frontend API wrapper
5. Create TeamsSection in settings
6. Create team detail view with members list
7. Create dialogs (create, invite, manage)
8. (Optional) Project sharing enhancements

---

## Verification Plan

1. **WorkOS Setup**: Verify roles exist in dashboard
2. **Backend API**: Test with curl:
   - Create team → verify org created in WorkOS
   - Send invitation → verify email received
   - List members → verify data from WorkOS
3. **Frontend**:
   - Create team from Settings > Teams
   - Send invitation, verify email
   - Accept invite (separate user), verify member appears
   - Change roles, remove members
4. **Integration**:
   - Full flow: create team → invite → accept → manage
