## Console logs

```
Failed to load resource: the server responded with a status of 500 ()
teams.ts:102 [TeamsStore] createTeam error: Error: Failed to create team
    at createTeam (teams-api.ts:95:11)
    at async createTeam (teams.ts:95:18)
    at async handleCreateTeam (TeamsSection.tsx:64:18)
createTeam @ teams.ts:102
```

## Backend logs

```
[2026-01-30 15:31:03.163 +0300] ERROR: Error creating team
    host: "MacBook-Pro.local"
    err: {
      "type": "UnprocessableEntityException",
      "message": "The role is invalid.",
      "stack":
          UnprocessableEntityException: The role is invalid.
              at WorkOSNode.handleHttpError (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@workos-inc/node/lib/workos.js:252:27)
              at WorkOSNode.<anonymous> (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@workos-inc/node/lib/workos.js:133:22)
              at Generator.throw (<anonymous>)
              at rejected (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@workos-inc/node/lib/workos.js:6:65)
              at processTicksAndRejections (node:internal/process/task_queues:105:5)
      "status": 422,
      "name": "UnprocessableEntityException",
      "requestID": "",
      "code": "invalid_role"
    }
```

