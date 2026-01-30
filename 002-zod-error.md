```
IPC Error in terminal-find-port: ZodError: [
  {
    "code": "too_big",
    "maximum": 1,
    "origin": "array",
    "path": [],
    "message": "Too big: expected array to have <1 items"
  }
]
    at validateArgs (/Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:872:35)
    at /Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:880:29
    at Session.<anonymous> (node:electron/js2c/browser_init:2:107269)
    at Session.emit (node:events:519:28)
Error occurred in handler for 'terminal-find-port': ZodError: [
  {
    "code": "too_big",
    "maximum": 1,
    "origin": "array",
    "path": [],
    "message": "Too big: expected array to have <1 items"
  }
]
    at validateArgs (/Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:872:35)
    at /Users/v1b3m/Dev/bfloat/bfloat-ide/out/main/main.js:880:29
    at Session.<anonymous> (node:electron/js2c/browser_init:2:107269)
    at Session.emit (node:events:519:28)
```

This error happens when I create a new project

The app continues to work just fine

