# Gitea Integration in Bfloat IDE

## Overview

Gitea is integrated into the Bfloat IDE system as a **self-hosted Git repository backend** for storing and managing project files. Instead of storing project files directly in a database, the system uses a Git-based approach where each project is a Git repository hosted on Gitea.

## Architecture

```
┌─────────────────────────────────────────────────────────────────┐
│                         Bfloat IDE (Electron)                    │
│                                                                   │
│  ┌──────────────────┐        ┌────────────────────────────┐    │
│  │  Frontend (React) │ ◄────► │  Main Process (Electron)   │    │
│  │                   │        │                             │    │
│  │  - Project UI     │        │  - Project Handler         │    │
│  │  - Code Editor    │        │  - Local Agent             │    │
│  │  - Terminal       │        │  - Git Operations          │    │
│  └──────────────────┘        └────────────┬───────────────┘    │
│                                            │                     │
└────────────────────────────────────────────┼─────────────────────┘
                                             │
                                             │ Git Clone/Pull/Push
                                             │ HTTP/HTTPS Protocol
                                             ▼
                              ┌─────────────────────────┐
                              │   Gitea Server          │
                              │   (localhost:3001)      │
                              │                         │
                              │  Organization:          │
                              │  bfloat-projects        │
                              │                         │
                              │  - Template repos       │
                              │  - User project repos   │
                              └─────────────────────────┘
                                             ▲
                                             │
                                             │ HTTP REST API
                                             │
                              ┌─────────────────────────┐
                              │   Backend Server        │
                              │   (localhost:3000)      │
                              │                         │
                              │  - Project creation     │
                              │  - Repository forking   │
                              │  - User management      │
                              └─────────────────────────┘
```

## Key Components

### 1. **Git Operations Module** (`lib/main/git-ops/`)

This module provides the core Git functionality:

#### `index.ts` - GitOps Interface

- `clone()` - Clone repository from Gitea to local directory
- `pull()` - Pull latest changes with rebase
- `writeFile()` - Write files to project directory
- `readFile()` - Read files from project directory
- `deleteFile()` - Delete files from project
- `commitAndPush()` - Stage all changes, commit, and push to Gitea
- `tag()` - Create and push Git tags
- `listFiles()` - List all tracked files
- `isCloned()` - Check if repository is already cloned
- `getCurrentBranch()` - Get current branch name
- `hasChanges()` - Check for uncommitted changes

**Implementation**: Uses Node.js `child_process` to execute Git commands via shell.

#### `local-agent.ts` - LocalAgent Class

The LocalAgent manages project synchronization between the IDE and Gitea:

- **File Watching**: Uses `chokidar` to watch for file changes in the project directory
- **Auto-Sync**: Automatically commits and pushes changes to Gitea
- **Project Management**: Handles multiple projects through an agent registry
- **Change Execution**: Applies AI-generated file changes and syncs them

**Key Features**:

- Clones repository on first start
- Pulls latest changes when starting
- Watches for local file changes
- Auto-commits changes to Gitea with message: "AI: Updated N file(s)"
- Maintains uncommitted change status

**Storage Location**: Projects are cloned to `~/.bfloat/projects/{projectId}/`

### 2. **Project Handler** (`lib/conveyor/handlers/project-handler.ts`)

Communicates with the backend server to manage projects:

- **Backend URL**: Configurable via `BFLOAT_WEB_BACKEND_URL` (default: `http://localhost:3000`)
- **Project Creation**: Backend creates projects by forking from Git template repositories
- **Git Info**: Backend returns `gitInfo` containing:
  - `cloneUrl` - URL for cloning the repository
  - `htmlUrl` - Web URL for viewing the repository
  - `defaultBranch` - Default branch name (e.g., "main")

**Project Types**:

- `expo` - Mobile apps (React Native + Expo)
- `nextjs` - Web apps (Next.js)

### 3. **Agent Handler** (`lib/conveyor/handlers/agent-handler.ts`)

Manages LocalAgent instances for each project:

- `agent-start` - Start agent for a project (clone/pull + watch)
- `agent-stop` - Stop agent for a project
- `agent-execute` - Execute AI-generated file changes
- `agent-commit` - Manually commit and push changes
- `agent-get-files` - Get all project files
- `agent-read-file` - Read a specific file
- `agent-pull` - Pull latest changes from Gitea
- `agent-status` - Check agent running status
- `agent-get-project-path` - Get local project path

## Configuration

### Environment Variables

The system uses these environment variables for Gitea integration:

```bash
# From .claude/settings.local.json (used in development/testing)
GIT_PROVIDER=gitea
GIT_BASE_URL=http://localhost:3001
GIT_ORGANIZATION=bfloat-projects
GIT_ACCESS_TOKEN=f5da732ddd002cdc540ec4258ce5533fc3cd4469
GIT_DEFAULT_BRANCH=main
```

These are configured on the backend server, not directly in the IDE. The IDE receives the `remoteUrl` from the backend, which includes authentication.

### Backend Configuration

The backend server (separate from the IDE) handles:

- Gitea API calls
- Repository creation and forking
- Access token management
- Project-to-repository mapping

## Data Flow

### Project Creation Flow

1. **User creates project** in IDE with title and description
2. **IDE sends request** to backend via `project:create` handler
3. **Backend**:
   - Creates project record in database
   - Forks appropriate template repository on Gitea (expo or nextjs)
   - Returns project info with `gitInfo.cloneUrl`
4. **IDE receives** project with Git URL
5. **LocalAgent starts**:
   - Clones repository to `~/.bfloat/projects/{projectId}/`
   - Starts file watcher
   - Project is ready for editing

### AI Code Generation Flow

1. **User chats** with AI in the IDE
2. **AI generates** file changes (create/update/delete)
3. **IDE executes** changes via `agent-execute`
4. **LocalAgent**:
   - Writes files to local project directory
   - Auto-commits with message
   - Pushes to Gitea
5. **Chokidar detects** file changes
6. **IDE updates** preview/UI

### File Sync Flow

```
User Action → LocalAgent → Git Commands → Gitea Server
                ↓
        Chokidar Watches
                ↓
        File Change Event
                ↓
        Update Preview
```

## Benefits of Git-Based Storage

### 1. **Version Control**

- Full Git history for every project
- Ability to tag versions (e.g., by message ID)
- Easy rollback to previous states
- Branch-based workflows possible

### 2. **Collaboration**

- Multiple developers can work on same project
- Standard Git merge workflows
- Pull requests and code review possible

### 3. **Self-Hosted**

- Full control over data
- No dependency on external Git hosting
- Can be deployed in private networks

### 4. **Scalability**

- Git repositories handle large codebases efficiently
- Binary file support (images, assets, etc.)
- Efficient storage with Git compression

### 5. **Tooling Compatibility**

- Projects can be cloned and worked on with any Git client
- Compatible with CI/CD pipelines
- Works with standard development tools

## Local Storage

Projects are stored locally at:

```
~/.bfloat/projects/
├── project-id-1/
│   ├── .git/
│   ├── app/
│   ├── package.json
│   └── ...
├── project-id-2/
│   ├── .git/
│   ├── app/
│   └── ...
```

Each project is a complete Git repository that can be:

- Opened in any code editor
- Committed to with any Git client
- Backed up independently
- Shared via Gitea's web interface

## Security Considerations

### Authentication

- Git operations use access tokens for authentication
- Tokens are managed by the backend server
- Clone URLs include embedded authentication

### Access Control

- Gitea manages repository permissions
- Projects are organized under `bfloat-projects` organization
- User access controlled at Gitea level

### Local Security

- Projects stored in user's home directory
- File system permissions apply
- No additional encryption (relies on OS security)

## Template System

### Template Repositories

The system uses Git template repositories for different project types:

**Expo Template** (Mobile):

- Expo SDK 53
- React Native 0.76
- Expo Router (file-based routing)
- NativeWind (Tailwind CSS)
- TypeScript
- Convex integration (optional)

**Next.js Template** (Web):

- Next.js 15
- React 19
- App Router
- Tailwind CSS
- TypeScript
- Simple, no Edge runtime issues

### Project Creation

When creating a new project:

1. Backend forks the appropriate template repository
2. Renames it to the project title
3. Updates configuration (package.json, app.json, etc.)
4. Returns the clone URL to the IDE
5. IDE clones and starts the LocalAgent

## Code References

### Key Files

- `lib/main/git-ops/index.ts:56` - GitOps implementation using git commands
- `lib/main/git-ops/local-agent.ts:114` - Auto-commit and push to Gitea
- `lib/conveyor/handlers/project-handler.ts:134` - Git info handling
- `lib/conveyor/handlers/agent-handler.ts:28` - Agent start handler
- `app/types/project.ts:21` - GitInfo interface definition

### Environment Configuration

- `.claude/settings.local.json:60` - Gitea environment variables
- `.env.example` - Backend configuration template

## Troubleshooting

### Common Issues

1. **Clone fails**
   
   - Check Gitea server is running (localhost:3001)
   - Verify access token is valid
   - Ensure network connectivity

2. **Push fails**
   
   - Check for uncommitted changes
   - Verify repository permissions
   - Check Gitea storage quota

3. **File sync not working**
   
   - Verify LocalAgent is started
   - Check chokidar is watching directory
   - Inspect file system permissions

4. **Project not loading**
   
   - Check if repository exists on Gitea
   - Verify clone URL is correct
   - Check local storage directory exists

## Future Enhancements

Potential improvements to the Gitea integration:

- **Branch Management**: Support for creating and switching branches
- **Conflict Resolution**: UI for handling merge conflicts
- **Diff Viewer**: Visual diff for uncommitted changes
- **History Browser**: Browse Git history in the IDE
- **Multi-Remote**: Support for multiple Git remotes
- **SSH Authentication**: Alternative to HTTPS with access tokens
- **Webhook Integration**: Real-time notifications from Gitea
- **Backup/Restore**: Automated backup of projects
- **Migration Tools**: Import from GitHub/GitLab

## Summary

Gitea serves as the **version control backbone** for Bfloat IDE, providing:

- ✅ Self-hosted Git repository hosting
- ✅ Project file storage and versioning
- ✅ Template-based project creation
- ✅ Real-time synchronization with local IDE
- ✅ Full Git history and version control
- ✅ Standard Git workflows and tooling compatibility

The integration is seamless - users don't need to interact with Git commands directly. The LocalAgent and handlers manage all Git operations automatically while providing the full power of version control under the hood.
