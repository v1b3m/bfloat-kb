I get the error below when I try to publish an app to web

```
Sync failed: Error invoking remote method 'project:syncToRemote': Error: Command failed: git -C "/Users/v1b3m/.bfloat/projects/39143021-1d52-4c2e-91b3-686132c571c6" commit -m "Deploy sync - 2026-02-15 15:54:17" error: cannot run gpg: No such file or directory error: gpg failed to sign the data: (no gpg output) fatal: failed to write commit object
```

---

## Important to note

- Local deployments are working fine when I run the app in dev (`pnpm dev:desktop`)
