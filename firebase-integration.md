We have added convex integrations back to back

1. User connects it via settings in the IDE, we set up the OAuth flow, get tokens
2. Then it can now be connected to any project
	1. We hit our backend which setups necessary credentials
	2. claude code then handles the rest, schema, pushing etc

Use an agent to explore the codebases so you can fully trace and understand how this flow works.

1. Backend is at `/Users/v1b3m/Dev/bfloat/bfloat-app-engineer`
2. Frontend (IDE) is at `/Users/v1b3m/Dev/bfloat/bfloat-ide`

After studying, write to `/Users/v1b3m/Dev/KnowledgeBase/bfloat-kb/convex-integration.md` how the whole thing works, use ASCII or any other diagrams where necessary so this flow can be understood by anyone.

After that, lay out a plan because we need to add a firebase integration too.

## Note

1. The convex integration is complete and works end-to-end, so we'll borrow from it what we can
2. 