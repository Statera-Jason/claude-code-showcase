# Capability Register

**Snapshot Date:** 2026-01-09

This document catalogs all capabilities present in the repository, organized by feature type.

## 1. Core Configuration System

### CLAUDE.md - Project Memory
- **Location:** `/CLAUDE.md`
- **Trigger:** Automatic (loaded at Claude Code session start)
- **External Dependencies:** None
- **Safe to Ignore:** No (fundamental feature)
- **Extension Path:** Edit file to add project-specific information, conventions, and rules

### settings.json - Hook Configuration
- **Location:** `.claude/settings.json`
- **Trigger:** Automatic (loaded at Claude Code session start)
- **External Dependencies:** Bash/sh shell, external tools called by hooks (npm, prettier, tsc, etc.)
- **Safe to Ignore:** Partially (can disable individual hooks)
- **Extension Path:**
  - Add new hooks to PreToolUse, PostToolUse, UserPromptSubmit, Stop events
  - Modify environment variables
  - Configure permissions and tool restrictions

### settings.local.json - Personal Overrides
- **Location:** `.claude/settings.local.json` (not present, gitignored)
- **Trigger:** Automatic (if present, overrides settings.json)
- **External Dependencies:** None
- **Safe to Ignore:** Yes (optional personal configuration)
- **Extension Path:** Create file for user-specific hook overrides

## 2. Skill System

Skills provide domain-specific knowledge and patterns. All skills are automatically suggested based on context matching.

### testing-patterns
- **Location:** `.claude/skills/testing-patterns/SKILL.md`
- **Trigger:** Automatic suggestion when keywords match (test, jest, spec, tdd, mock) or test file paths detected
- **External Dependencies:** Jest (for actual test execution)
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Modify SKILL.md to reflect team's testing conventions
  - Update skill-rules.json triggers for custom test file patterns

### systematic-debugging
- **Location:** `.claude/skills/systematic-debugging/SKILL.md`
- **Trigger:** Automatic suggestion when debugging keywords detected (bug, fix, debug, error, investigate)
- **External Dependencies:** None
- **Safe to Ignore:** Yes
- **Extension Path:** Add project-specific debugging tools and procedures

### react-ui-patterns
- **Location:** `.claude/skills/react-ui-patterns/SKILL.md`
- **Trigger:** Automatic suggestion for React files, component keywords, UI state patterns
- **External Dependencies:** React, GraphQL/Apollo Client
- **Safe to Ignore:** Yes (can ignore if not using React)
- **Extension Path:** Replace with team's actual React patterns and component conventions

### graphql-schema
- **Location:** `.claude/skills/graphql-schema/SKILL.md`
- **Trigger:** Automatic suggestion for GraphQL keywords, .graphql files, query/mutation patterns
- **External Dependencies:** GraphQL, Apollo Client, graphql-codegen
- **Safe to Ignore:** Yes (can ignore if not using GraphQL)
- **Extension Path:** Add project's actual schema patterns and codegen configuration

### core-components
- **Location:** `.claude/skills/core-components/SKILL.md`
- **Trigger:** Automatic suggestion for component files, design system keywords
- **External Dependencies:** Project's component library
- **Safe to Ignore:** Yes
- **Extension Path:** Replace with actual design system documentation and component API reference

### formik-patterns
- **Location:** `.claude/skills/formik-patterns/SKILL.md`
- **Trigger:** Automatic suggestion for form keywords, Formik usage
- **External Dependencies:** Formik, Yup
- **Safe to Ignore:** Yes (can ignore if not using Formik)
- **Extension Path:** Add team's form validation patterns and submission workflows

## 3. Agent System

Agents are specialized AI assistants with focused purposes. All agents require manual invocation or proactive triggering.

### code-reviewer
- **Location:** `.claude/agents/code-reviewer.md`
- **Trigger:**
  - Manual: User explicitly invokes
  - Proactive: Automatically invoked after significant code changes (if configured)
- **External Dependencies:** git (for diffs)
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Modify review checklist to match team standards
  - Add language-specific checks
  - Integrate with team's code quality tools

### github-workflow
- **Location:** `.claude/agents/github-workflow.md`
- **Trigger:** Manual invocation for git operations
- **External Dependencies:** git, gh CLI (GitHub CLI)
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Customize commit message format
  - Add team-specific branching strategies
  - Modify PR template and review process

## 4. Command System (Slash Commands)

All commands are manually triggered via `/command-name` syntax.

### /onboard
- **Location:** `.claude/commands/onboard.md`
- **Trigger:** Manual (`/onboard <task-description>`)
- **External Dependencies:** None
- **Safe to Ignore:** Yes
- **Extension Path:** Customize exploration depth and onboarding questions

### /pr-review
- **Location:** `.claude/commands/pr-review.md`
- **Trigger:** Manual (`/pr-review <pr-number>`)
- **External Dependencies:** gh CLI, git
- **Safe to Ignore:** Yes
- **Extension Path:** Add team-specific review criteria and automation steps

### /pr-summary
- **Location:** `.claude/commands/pr-summary.md`
- **Trigger:** Manual (`/pr-summary`)
- **External Dependencies:** git
- **Safe to Ignore:** Yes
- **Extension Path:** Customize PR description template and changelog format

### /code-quality
- **Location:** `.claude/commands/code-quality.md`
- **Trigger:** Manual (`/code-quality <directory>`)
- **External Dependencies:** None
- **Safe to Ignore:** Yes
- **Extension Path:** Add project-specific quality checks and linting rules

### /docs-sync
- **Location:** `.claude/commands/docs-sync.md`
- **Trigger:** Manual (`/docs-sync`)
- **External Dependencies:** git
- **Safe to Ignore:** Yes
- **Extension Path:** Define documentation structure and sync rules

### /ticket
- **Location:** `.claude/commands/ticket.md`
- **Trigger:** Manual (`/ticket <ticket-id>`)
- **External Dependencies:** MCP server (JIRA or Linear), git, gh CLI
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Configure for team's ticket system (JIRA, Linear, etc.)
  - Customize workflow states and transitions
  - Add team-specific acceptance criteria parsing

## 5. Hook System

Hooks automate actions at specific trigger points.

### UserPromptSubmit Hook - Skill Evaluation
- **Location:** `.claude/hooks/skill-eval.sh` + `.claude/hooks/skill-eval.js`
- **Trigger:** Automatic on every user prompt submission
- **External Dependencies:** node (for JavaScript execution)
- **Safe to Ignore:** Yes (can disable in settings.json)
- **Extension Path:**
  - Modify skill-rules.json to add/change skill matching patterns
  - Adjust confidence thresholds
  - Add custom skill categories

### PreToolUse Hook - Main Branch Protection
- **Location:** Inline in `.claude/settings.json`
- **Trigger:** Automatic before Edit, MultiEdit, or Write operations
- **External Dependencies:** git
- **Safe to Ignore:** No (protects main branch)
- **Extension Path:**
  - Change protected branch names
  - Add additional pre-edit validations
  - Customize protection rules

### PostToolUse Hook - Code Formatting
- **Location:** Inline in `.claude/settings.json`
- **Trigger:** Automatic after editing JS/TS files
- **External Dependencies:** npx, prettier (or biome)
- **Safe to Ignore:** Yes (can disable auto-formatting)
- **Extension Path:**
  - Switch to different formatter (biome, eslint --fix)
  - Adjust file pattern matching
  - Configure formatter options

### PostToolUse Hook - Dependency Installation
- **Location:** Inline in `.claude/settings.json`
- **Trigger:** Automatic after editing package.json
- **External Dependencies:** npm
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Switch to yarn, pnpm, or other package manager
  - Add additional post-install steps

### PostToolUse Hook - Test Runner
- **Location:** Inline in `.claude/settings.json`
- **Trigger:** Automatic after editing test files
- **External Dependencies:** npm, Jest (via npm test)
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Configure different test runner
  - Adjust test file patterns
  - Add coverage thresholds

### PostToolUse Hook - TypeScript Type Checking
- **Location:** Inline in `.claude/settings.json`
- **Trigger:** Automatic after editing TS/TSX files
- **External Dependencies:** npx, typescript (tsc)
- **Safe to Ignore:** Yes (non-blocking)
- **Extension Path:**
  - Adjust error reporting
  - Make blocking instead of non-blocking
  - Add custom type checking rules

## 6. MCP Integration System

MCP servers enable external tool integrations. All servers are configured but require API credentials to activate.

### JIRA MCP Server
- **Location:** `.mcp.json` (mcpServers.jira)
- **Trigger:** Automatic when MCP is enabled and credentials provided
- **External Dependencies:**
  - Environment variables: JIRA_HOST, JIRA_EMAIL, JIRA_API_TOKEN
  - Internet connectivity to JIRA instance
- **Safe to Ignore:** Yes (opt-in integration)
- **Extension Path:** Set environment variables to activate

### GitHub MCP Server
- **Location:** `.mcp.json` (mcpServers.github)
- **Trigger:** Automatic when enabled with GITHUB_TOKEN
- **External Dependencies:** GITHUB_TOKEN environment variable
- **Safe to Ignore:** Yes
- **Extension Path:** Set GITHUB_TOKEN to enable GitHub API access

### Linear MCP Server
- **Location:** `.mcp.json` (mcpServers.linear)
- **Trigger:** Automatic when enabled with LINEAR_API_KEY
- **External Dependencies:** LINEAR_API_KEY environment variable
- **Safe to Ignore:** Yes
- **Extension Path:** Set LINEAR_API_KEY to activate

### Sentry MCP Server
- **Location:** `.mcp.json` (mcpServers.sentry)
- **Trigger:** Automatic when enabled with credentials
- **External Dependencies:** SENTRY_AUTH_TOKEN, SENTRY_ORG environment variables
- **Safe to Ignore:** Yes
- **Extension Path:** Set Sentry credentials to enable error tracking integration

### PostgreSQL MCP Server
- **Location:** `.mcp.json` (mcpServers.postgres)
- **Trigger:** Automatic when enabled with DATABASE_URL
- **External Dependencies:** DATABASE_URL environment variable
- **Safe to Ignore:** Yes
- **Extension Path:** Set DATABASE_URL to enable database queries

### Slack MCP Server
- **Location:** `.mcp.json` (mcpServers.slack)
- **Trigger:** Automatic when enabled with credentials
- **External Dependencies:** SLACK_BOT_TOKEN, SLACK_TEAM_ID environment variables
- **Safe to Ignore:** Yes
- **Extension Path:** Set Slack credentials to enable messaging integration

### Notion MCP Server
- **Location:** `.mcp.json` (mcpServers.notion)
- **Trigger:** Automatic when enabled with NOTION_API_KEY
- **External Dependencies:** NOTION_API_KEY environment variable
- **Safe to Ignore:** Yes
- **Extension Path:** Set NOTION_API_KEY to enable Notion integration

### Memory MCP Server
- **Location:** `.mcp.json` (mcpServers.memory)
- **Trigger:** Automatic (no credentials required)
- **External Dependencies:** None (local storage)
- **Safe to Ignore:** Yes
- **Extension Path:** No configuration needed, works out of the box

## 7. GitHub Actions Automation

All workflows are opt-in and require ANTHROPIC_API_KEY secret configuration.

### PR Code Review Workflow
- **Location:** `.github/workflows/pr-claude-code-review.yml`
- **Trigger:**
  - Automatic on PR open/sync/reopen
  - Manual via @claude mention in PR comments
- **External Dependencies:**
  - GitHub Actions
  - ANTHROPIC_API_KEY repository secret
  - gh CLI
- **Safe to Ignore:** Yes (CI/CD is opt-in)
- **Extension Path:**
  - Modify review criteria
  - Adjust model selection
  - Customize comment format

### Scheduled Documentation Sync
- **Location:** `.github/workflows/scheduled-claude-code-docs-sync.yml`
- **Trigger:**
  - Scheduled: 1st of every month at 9 AM UTC
  - Manual: workflow_dispatch
- **External Dependencies:**
  - GitHub Actions
  - ANTHROPIC_API_KEY repository secret
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Adjust schedule frequency
  - Modify sync scope and rules
  - Customize documentation patterns

### Scheduled Code Quality Review
- **Location:** `.github/workflows/scheduled-claude-code-quality.yml`
- **Trigger:**
  - Scheduled: Weekly on Sunday
  - Manual: workflow_dispatch
- **External Dependencies:**
  - GitHub Actions
  - ANTHROPIC_API_KEY repository secret
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Change schedule
  - Define quality criteria
  - Add auto-fix capabilities

### Scheduled Dependency Audit
- **Location:** `.github/workflows/scheduled-claude-code-dependency-audit.yml`
- **Trigger:**
  - Scheduled: Biweekly (1st and 15th of month)
  - Manual: workflow_dispatch
- **External Dependencies:**
  - GitHub Actions
  - ANTHROPIC_API_KEY repository secret
  - npm (or other package manager)
- **Safe to Ignore:** Yes
- **Extension Path:**
  - Adjust update strategy
  - Configure test verification
  - Customize dependency selection

## 8. LSP Support (Documented)

Note: LSP integration is documented in the README but not actively configured in this repository.

### TypeScript LSP
- **Location:** Documentation only (not configured)
- **Trigger:** Would be automatic if enabled via settings.json plugins
- **External Dependencies:** typescript-language-server, typescript
- **Safe to Ignore:** Yes (not currently active)
- **Extension Path:**
  - Enable in settings.json with enabledPlugins
  - Install typescript-language-server
  - Optionally create .lsp.json for custom configuration

### Python LSP (Pyright)
- **Location:** Documentation only (not configured)
- **Trigger:** Would be automatic if enabled
- **External Dependencies:** pyright
- **Safe to Ignore:** Yes (not currently active)
- **Extension Path:** Enable in settings.json and install pyright

## Summary Table

| Capability Type | Total Count | Active by Default | Require External Setup |
|----------------|-------------|-------------------|------------------------|
| Core Config | 2 | Yes | No |
| Skills | 6 | Auto-suggested | No |
| Agents | 2 | No (manual) | No |
| Commands | 6 | No (manual) | Varies |
| Hooks | 6 instances | Yes (configurable) | Some |
| MCP Servers | 8 | No | Yes (API keys) |
| GitHub Workflows | 4 | No | Yes (secrets) |
| LSP Support | Documented | No | Yes (installation) |

Total distinct capabilities: 34
