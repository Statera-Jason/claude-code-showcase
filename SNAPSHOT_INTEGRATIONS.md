# External Integration Surfaces

**Snapshot Date:** 2026-01-09

This document catalogs all external integration points, dependencies, and implicit assumptions about external tooling.

## MCP (Model Context Protocol) Server Integrations

MCP servers provide Claude Code with tools to interact with external services. All servers are configured in `.mcp.json` but require credentials to activate.

### JIRA Integration
- **Configuration:** `.mcp.json` (mcpServers.jira)
- **Type:** stdio (local bridge process)
- **Command:** `npx -y @anthropic/mcp-jira`
- **Required Environment Variables:**
  - `JIRA_HOST` - JIRA instance URL (e.g., https://company.atlassian.net)
  - `JIRA_EMAIL` - User email for authentication
  - `JIRA_API_TOKEN` - API token for authentication
- **Status:** Present but unused by default (requires credentials)
- **Purpose:** Read tickets, update status, add comments, create tickets
- **Usage Pattern:** Primarily via `/ticket` command
- **Network Requirements:** Internet access to JIRA instance

### Linear Integration
- **Configuration:** `.mcp.json` (mcpServers.linear)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-linear`
- **Required Environment Variables:**
  - `LINEAR_API_KEY` - Linear API key
- **Status:** Present but unused by default
- **Purpose:** Alternative to JIRA for ticket management
- **Usage Pattern:** Via `/ticket` command or direct invocation
- **Network Requirements:** Internet access to Linear API

### GitHub Integration
- **Configuration:** `.mcp.json` (mcpServers.github)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-github`
- **Required Environment Variables:**
  - `GITHUB_TOKEN` - GitHub personal access token
- **Status:** Present but unused by default
- **Purpose:** Enhanced GitHub API access beyond gh CLI
- **Usage Pattern:** Repository operations, issue management, PR operations
- **Network Requirements:** Internet access to github.com

### Sentry Integration
- **Configuration:** `.mcp.json` (mcpServers.sentry)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-sentry`
- **Required Environment Variables:**
  - `SENTRY_AUTH_TOKEN` - Sentry authentication token
  - `SENTRY_ORG` - Organization slug
- **Status:** Present but unused by default
- **Purpose:** Read error reports, debug production issues
- **Usage Pattern:** Error investigation workflows
- **Network Requirements:** Internet access to sentry.io

### PostgreSQL Integration
- **Configuration:** `.mcp.json` (mcpServers.postgres)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-postgres`
- **Required Environment Variables:**
  - `DATABASE_URL` - PostgreSQL connection string
- **Status:** Present but unused by default
- **Purpose:** Database queries, schema inspection
- **Usage Pattern:** Database exploration and migrations
- **Network Requirements:** Access to PostgreSQL server

### Slack Integration
- **Configuration:** `.mcp.json` (mcpServers.slack)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-slack`
- **Required Environment Variables:**
  - `SLACK_BOT_TOKEN` - Slack bot OAuth token
  - `SLACK_TEAM_ID` - Workspace team ID
- **Status:** Present but unused by default
- **Purpose:** Send notifications, read channels, post updates
- **Usage Pattern:** CI/CD notifications, team updates
- **Network Requirements:** Internet access to slack.com

### Notion Integration
- **Configuration:** `.mcp.json` (mcpServers.notion)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-notion`
- **Required Environment Variables:**
  - `NOTION_API_KEY` - Notion integration token
- **Status:** Present but unused by default
- **Purpose:** Read/write documentation, project planning
- **Usage Pattern:** Documentation sync, knowledge base queries
- **Network Requirements:** Internet access to notion.so

### Memory MCP Server
- **Configuration:** `.mcp.json` (mcpServers.memory)
- **Type:** stdio
- **Command:** `npx -y @anthropic/mcp-memory`
- **Required Environment Variables:** None
- **Status:** Present and usable by default (no credentials needed)
- **Purpose:** Persistent memory across Claude Code sessions
- **Usage Pattern:** Remembering user preferences, project context
- **Network Requirements:** None (local storage)

## GitHub Actions Integration

All workflows use the `anthropics/claude-code-action@beta` action and require repository configuration.

### Action: claude-code-action@beta
- **Provider:** Anthropic (official)
- **Version:** beta
- **Required Secrets:**
  - `ANTHROPIC_API_KEY` - Must be configured in repository settings
- **Required Permissions:**
  - `contents: write` - For creating commits and branches
  - `pull-requests: write` - For creating and commenting on PRs
  - `issues: read` - For reading issue comments
- **Network Requirements:** Internet access to api.anthropic.com

### Workflow: PR Code Review
- **File:** `.github/workflows/pr-claude-code-review.yml`
- **Trigger:** PR events or @claude mentions
- **Dependencies:**
  - GitHub Actions runner (ubuntu-latest)
  - git (for diffs)
  - gh CLI (pre-installed on GitHub runners)
- **Costs:** Anthropic API usage per PR review
- **Status:** Present but requires ANTHROPIC_API_KEY to activate

### Workflow: Scheduled Documentation Sync
- **File:** `.github/workflows/scheduled-claude-code-docs-sync.yml`
- **Schedule:** Cron schedule (monthly)
- **Dependencies:**
  - GitHub Actions runner
  - git (for change detection)
  - gh CLI (for PR creation)
- **Costs:** Anthropic API usage monthly
- **Status:** Present but requires ANTHROPIC_API_KEY

### Workflow: Scheduled Code Quality
- **File:** `.github/workflows/scheduled-claude-code-quality.yml`
- **Schedule:** Cron schedule (weekly)
- **Dependencies:**
  - GitHub Actions runner
  - Project's linting/testing tools
- **Costs:** Anthropic API usage weekly
- **Status:** Present but requires ANTHROPIC_API_KEY

### Workflow: Scheduled Dependency Audit
- **File:** `.github/workflows/scheduled-claude-code-dependency-audit.yml`
- **Schedule:** Cron schedule (biweekly)
- **Dependencies:**
  - GitHub Actions runner
  - npm (or project's package manager)
  - git
- **Costs:** Anthropic API usage biweekly
- **Status:** Present but requires ANTHROPIC_API_KEY

## Development Tool Integrations

### Git
- **Required For:** Branch protection hooks, GitHub workflows, PR commands
- **Usage:** Branch management, diff generation, commit history
- **Assumption:** Project is a git repository
- **Version Requirements:** Any modern git version

### GitHub CLI (gh)
- **Required For:** GitHub workflows, PR commands
- **Usage:** PR creation, issue management, repository operations
- **Assumption:** Authenticated with GitHub
- **Version Requirements:** gh 2.0+

### Node.js / npm
- **Required For:**
  - MCP server execution (all servers use npx)
  - Skill evaluation hooks (skill-eval.js)
  - PostToolUse hooks (npm install, npm test)
- **Usage:** Package management, script execution
- **Assumption:** Node.js installed on system
- **Version Requirements:** Node.js 16+

### TypeScript Compiler (tsc)
- **Required For:** PostToolUse type checking hook
- **Usage:** Type validation after TS/TSX edits
- **Assumption:** TypeScript is installed (via npm)
- **Status:** Hook present but may fail if not installed
- **Version Requirements:** TypeScript 4.0+

### Prettier
- **Required For:** PostToolUse formatting hook
- **Usage:** Auto-format JS/TS files after edits
- **Assumption:** Prettier available via npx
- **Status:** Hook present, uses npx (downloads if needed)
- **Alternative:** Can swap for Biome or other formatter

### Jest
- **Required For:**
  - PostToolUse test runner hook
  - testing-patterns skill
- **Usage:** Run tests after test file edits
- **Assumption:** npm test command runs Jest
- **Status:** Hook present but graceful if tests don't exist
- **Version Requirements:** Jest 27+

## LSP (Language Server Protocol) Integration

Note: LSP is documented but not configured in this repository.

### TypeScript Language Server
- **Status:** Documented but not enabled
- **Would Require:**
  - `typescript-language-server` installed globally or locally
  - `typescript` package installed
  - Enable in settings.json via enabledPlugins
- **Purpose:** Real-time type checking, intellisense
- **Network Requirements:** None (local)

### Pyright (Python LSP)
- **Status:** Documented but not enabled
- **Would Require:**
  - `pyright` installed via pip
  - Enable in settings.json
- **Purpose:** Python type checking and intellisense
- **Network Requirements:** None (local)

## Implicit Tooling Assumptions

### Shell Environment
- **Assumption:** Unix-like shell (bash) for hook execution
- **Issue Detected:** Hooks use bash syntax `[[` but may execute with `/bin/sh`
- **Impact:** Hook errors visible in this snapshot
- **Resolution Required:** Update hooks to use POSIX-compliant syntax or ensure bash execution

### Package Manager
- **Assumption:** npm is the package manager
- **Evidence:** package.json references, npm commands in hooks
- **Flexibility:** Can be swapped to yarn, pnpm with hook modifications

### React Ecosystem
- **Assumption:** Project uses React, TypeScript, GraphQL
- **Evidence:** Skills focused on React patterns, TypeScript checking, GraphQL
- **Reality:** This showcase has no actual React code, only example patterns
- **Impact:** Skills would need replacement for non-React projects

### Testing Framework
- **Assumption:** Jest for testing
- **Evidence:** testing-patterns skill, npm test command
- **Flexibility:** Can adapt to other test frameworks by updating hooks and skills

### GraphQL with Apollo Client
- **Assumption:** GraphQL with Apollo Client and codegen
- **Evidence:** graphql-schema skill content
- **Flexibility:** Can adapt to other GraphQL clients

### Form Library
- **Assumption:** Formik for forms
- **Evidence:** formik-patterns skill
- **Flexibility:** Can replace with other form libraries

## Network and API Requirements Summary

| Integration | Requires Internet | Requires API Key | Cost Implications |
|-------------|-------------------|------------------|-------------------|
| JIRA MCP | Yes | Yes | None (uses existing JIRA license) |
| Linear MCP | Yes | Yes | None (uses existing Linear license) |
| GitHub MCP | Yes | Yes | None (within GitHub API limits) |
| Sentry MCP | Yes | Yes | None (uses existing Sentry license) |
| PostgreSQL MCP | Depends | No | None (direct DB access) |
| Slack MCP | Yes | Yes | None (uses existing Slack workspace) |
| Notion MCP | Yes | Yes | None (uses existing Notion workspace) |
| Memory MCP | No | No | None (local storage) |
| GitHub Actions | Yes | Yes | Anthropic API costs (per run) |
| Prettier/npm | Depends | No | None (uses public npm registry) |

## Security Considerations

### API Keys and Secrets
- MCP server credentials should be stored as environment variables, never committed
- GitHub Actions requires ANTHROPIC_API_KEY as repository secret
- All MCP configurations use `${VAR}` expansion for safe credential management

### Tool Execution
- Hooks execute shell commands with elevated privileges
- MCP servers run as local processes with system access
- GitHub Actions run in isolated containers

### Network Exposure
- MCP servers make outbound connections to external services
- GitHub Actions workflows pull from public npm registry
- No inbound network access required

## Compatibility Notes

### Operating System
- Hooks assume Unix-like environment (Linux, macOS)
- Windows users may need WSL or Git Bash
- GitHub Actions use ubuntu-latest runners

### Claude Code Version
- Configuration format appears stable across Claude Code versions
- GitHub Action uses @beta tag (may have breaking changes)

### External Service Versions
- JIRA: Cloud and Server/Data Center supported
- Linear: Current API version
- GitHub: API v3 and v4 (GraphQL)
- Slack: Bot token API (current)

## Migration and Portability

### What Travels with Repository
- All configuration files (.claude/, .mcp.json, .github/)
- Documentation and examples
- Hook scripts and skill definitions

### What Requires Per-Environment Setup
- API keys and credentials (environment variables)
- GitHub repository secrets (ANTHROPIC_API_KEY)
- Local tool installation (node, git, gh CLI)

### What Must Be Customized Per-Project
- Skills (replace examples with actual patterns)
- Hook commands (adjust for project's tooling)
- MCP server selection (enable only needed integrations)
- GitHub workflow schedules and thresholds
