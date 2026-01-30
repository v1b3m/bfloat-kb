I am getting this error when creating a new project

```
[2026-01-28 21:35:54.891 +0300] ERROR: Error fetching project:
    host: "MacBook-Pro.local"
    err: {
      "type": "PrismaClientKnownRequestError",
      "message": "\nInvalid `prisma.deployment.findUnique()` invocation:\n\n\nThe column `Deployment.deploymentProvider` does not exist in the current database.",
      "stack":
          PrismaClientKnownRequestError:
          Invalid `prisma.deployment.findUnique()` invocation:


          The column `Deployment.deploymentProvider` does not exist in the current database.
              at ei.handleRequestError (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@prisma/client/src/runtime/RequestHandler.ts:228:13)
              at ei.handleAndLogRequestError (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@prisma/client/src/runtime/RequestHandler.ts:174:12)
              at ei.request (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@prisma/client/src/runtime/RequestHandler.ts:143:12)
              at a (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@prisma/client/src/runtime/getPrismaClient.ts:833:24)
              at eval (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/app/routes/api.projects.$id.ts:299:27)
              at handleAuthLoader (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@workos-inc/authkit-remix/src/session.ts:441:20)
              at Module.authkitLoader (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@workos-inc/authkit-remix/src/session.ts:338:14)
              at callRouteHandler (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:508:16)
              at commonRoute.loader (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:658:19)
              at file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4762:19
              at callLoaderOrAction (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4814:16)
              at async Promise.all (index 0)
              at defaultDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4439:17)
              at callDataStrategyImpl (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:4708:17)
              at callDataStrategy (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:3748:19)
              at loadRouteData (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:3725:19)
              at queryImpl (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:3500:20)
              at Object.queryRoute (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-JMJ3UQ3L.mjs:3451:18)
              at handleResourceRequest (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:1441:18)
              at requestHandler (file:///Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/react-router/dist/development/chunk-YNUBSHFH.mjs:1161:18)
              at nodeHandler (/Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@react-router/dev/dist/vite.js:3523:30)
              at /Users/v1b3m/Dev/bfloat/bfloat-app-engineer/node_modules/@react-router/dev/dist/vite.js:3529:17
      "code": "P2022",
      "meta": {
        "modelName": "Deployment",
        "column": "Deployment.deploymentProvider"
      },
      "clientVersion": "6.19.1",
      "name": "PrismaClientKnownRequestError"
    }
```

The last migration was supposed to fix this

`prisma/migrations/20260126151329_add_deployment_table/migration.sql`

