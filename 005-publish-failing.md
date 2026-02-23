
The publish to web is failing with

```
Publish to Web

FailedFix with AI

Publish

Vercel API error: 400 - {"error":{"code":"bad\_request","message":"To link a GitHub repository, you need to install the GitHub integration first. Make sure there aren't any typos and that you have access to the repository if it's private.","action":"Install GitHub App","link":"https://github.com/apps/vercel","repo":"0c0482a5-38c5-473c-a29c-c156aaa6d5e4"}}
```

What does this mean?

## Backend logs

```
[Vercel] API error: 400 - {"error":{"code":"bad_request","message":"To link a GitHub repository, you need to install the GitHub integration first. Make sure there aren't any typos and that you have access to the repository if it's private.","action":"Install GitHub App","link":"https://github.com/apps/vercel","repo":"0c0482a5-38c5-473c-a29c-c156aaa6d5e4"}}
[Vercel] Failed to create deployment: Error: Vercel API error: 400 - {"error":{"code":"bad_request","message":"To link a GitHub repository, you need to install the GitHub integration first. Make sure there aren't any typos and that you have access to the repository if it's private.","action":"Install GitHub App","link":"https://github.com/apps/vercel","repo":"0c0482a5-38c5-473c-a29c-c156aaa6d5e4"}}
    at vercelRequest (/Users/v1b3m/Dev/bfloat/bfloat-workbench/apps/web/app/lib/vercel/client.ts:44:11)
    at processTicksAndRejections (node:internal/process/task_queues:105:5)
    at ensureVercelProject (/Users/v1b3m/Dev/bfloat/bfloat-workbench/apps/web/app/lib/vercel/client.ts:85:19)
    at createVercelDeployment (/Users/v1b3m/Dev/bfloat/bfloat-workbench/apps/web/app/lib/vercel/client.ts:105:27)
    at deployViaVercel (/Users/v1b3m/Dev/bfloat/bfloat-workbench/apps/web/app/routes/api.deploy.web.ts:279:18)
    at action (/Users/v1b3m/Dev/bfloat/bfloat-workbench/apps/web/app/routes/api.deploy.web.ts:510:16)
    at callRouteHandler (file:///Users/v1b3m/Dev/bfloat/bfloat-workbench/node_modules/react-router/dist/development/chunk-4LKRSAEJ.mjs:509:16)
    at file:///Users/v1b3m/Dev/bfloat/bfloat-workbench/node_modules/react-router/dist/development/chunk-JZWAC4HX.mjs:4756:19
    at callLoaderOrAction (file:///Users/v1b3m/Dev/bfloat/bfloat-workbench/node_modules/react-router/dist/development/chunk-JZWAC4HX.mjs:4808:16)
    at async Promise.all (index 0)
```

