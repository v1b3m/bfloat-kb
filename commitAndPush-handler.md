My Mac keeps asking me to input my osx-keychain password
## Electron backend logs

```
Error occurred in handler for 'project:commitAndPush': Error: Command failed: git -C "/Users/v1b3m/.bfloat/projects/a77a58ad-81f4-4ff4-9ffe-d074b52871d4" push
remote: Invalid username or token. Password authentication is not supported for Git operations.
fatal: Authentication failed for 'https://github.com/bfloat-projects/a77a58ad-81f4-4ff4-9ffe-d074b52871d4.git/'

    at genericNodeError (node:internal/errors:983:15)
    at wrappedFn (node:internal/errors:537:14)
    at ChildProcess.exithandler (node:child_process:417:12)
    at ChildProcess.emit (node:events:519:28)
    at maybeClose (node:internal/child_process:1101:16)
    at Socket.<anonymous> (node:internal/child_process:456:11)
    at Socket.emit (node:events:519:28)
    at Pipe.<anonymous> (node:net:346:12) {
  code: 128,
  killed: false,
  signal: null,
  cmd: 'git -C "/Users/v1b3m/.bfloat/projects/a77a58ad-81f4-4ff4-9ffe-d074b52871d4" push',
  stdout: '',
  stderr: 'remote: Invalid username or token. Password authentication is not supported for Git operations.\n' +
    "fatal: Authentication failed for 'https://github.com/bfloat-projects/a77a58ad-81f4-4ff4-9ffe-d074b52871d4.git/'\n"
}
```

These projects are stored in the bfloat-dev-projects organization. A user should not have to input their password, unless I am mistaken.

