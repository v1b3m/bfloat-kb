The backend fails with the errors below when we try to create new projects

```
[2026-01-28 07:39:44.822 +0300] ERROR: Failed to create git repo for project
    host: "MacBook-Pro.local"
    projectId: "f743bc9b-ea19-4daa-bdcd-4f17f67b3a24"
    error: {}
[2026-01-28 07:39:44.885 +0300] ERROR: Failed to get files from git
    host: "MacBook-Pro.local"
    projectId: "f743bc9b-ea19-4daa-bdcd-4f17f67b3a24"
    error: {}
[2026-01-28 07:39:44.905 +0300] WARN: Could not get git repo info
    host: "MacBook-Pro.local"
    projectId: "f743bc9b-ea19-4daa-bdcd-4f17f67b3a24"
    error: {}
```

Check [[local-gitea]] for details on how this was resolved

