# Integrations Policy

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This document defines the integration policy for the claude-code-showcase foundation, including MCP server inventory, default integration postures, and explicit non-defaults for GitHub and JIRA.

---

## Integration Posture: Dormant by Default

**Core Principle:** All external integrations are **dormant by default**.

Integrations require:
1. **Explicit enablement** - User must opt-in
2. **Credential provision** - User must supply authentication
3. **Capability verification** - System must confirm integration is functional
4. **Audit trail** - Usage must be logged

**Why:** Prevents accidental external system modifications, protects credentials, allows cost control.

---

## MCP Server Inventory

### Configured Servers

The following MCP servers are configured in `.mcp.json`:

| Server | Purpose | Status | Credentials Required |
|--------|---------|--------|---------------------|
| `jira` | JIRA issue tracking | Dormant | JIRA_HOST, JIRA_EMAIL, JIRA_API_TOKEN |
| `github` | GitHub API operations | Dormant | GITHUB_TOKEN |
| `linear` | Linear issue tracking | Dormant | LINEAR_API_KEY |
| `sentry` | Error monitoring | Dormant | SENTRY_AUTH_TOKEN, SENTRY_ORG |
| `postgres` | PostgreSQL database | Dormant | DATABASE_URL |
| `slack` | Slack messaging | Dormant | SLACK_BOT_TOKEN, SLACK_TEAM_ID |
| `notion` | Notion workspace | Dormant | NOTION_API_KEY |
| `memory` | Persistent memory | Dormant | None (local storage) |

### Server Status Definitions

| Status | Definition | Enablement Method |
|--------|------------|-------------------|
| **Dormant** | Configuration present, server disabled | Explicit enablement + credentials |
| **Enabled** | Configuration present, explicitly enabled | Set in settings.json + credentials |
| **Active** | Enabled and successfully connected | Runtime verification |
| **Failed** | Enabled but connection failed | Check credentials and network |

### Enablement Methods

#### Method 1: Enable All Project MCP Servers

```json
// .claude/settings.json
{
  "enableAllProjectMcpServers": true
}
```

**Effect:** Enables all servers in `.mcp.json` (credentials still required)

**Risk:** High - Enables all integrations without selective control

**Recommended For:** Development environments, trusted projects

---

#### Method 2: Enable Specific Servers (Recommended)

```json
// .claude/settings.json
{
  "enabledMcpjsonServers": ["jira", "github"]
}
```

**Effect:** Enables only specified servers

**Risk:** Medium - Controlled enablement

**Recommended For:** Production environments, selective integration

---

#### Method 3: Disable All (Default)

```json
// .claude/settings.json (or omit MCP configuration)
{
  // No MCP enablement
}
```

**Effect:** All MCP servers remain dormant

**Risk:** None - No external access

**Recommended For:** Initial setup, security-first environments

---

## Integration-Specific Policies

### JIRA Integration

**Status:** Dormant by Default

**Configuration:**
```json
{
  "mcpServers": {
    "jira": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-jira"],
      "env": {
        "JIRA_HOST": "${JIRA_HOST}",
        "JIRA_EMAIL": "${JIRA_EMAIL}",
        "JIRA_API_TOKEN": "${JIRA_API_TOKEN}"
      }
    }
  }
}
```

**Capabilities When Enabled:**
- Read issue details, comments, attachments
- Update issue status, assignee, priority
- Add comments to issues
- Create new issues
- Search issues with JQL
- Link issues, create subtasks

**Authorization Requirements:**
- JIRA API token with appropriate project permissions
- Email address associated with JIRA account
- Host URL for JIRA instance

**Risks:**
- **HIGH:** Can modify production tickets
- **MEDIUM:** Can create spam issues if misconfigured
- **LOW:** Can read sensitive ticket data

**Posture: Retained but Dormant**

JIRA is **NOT** the primary workflow for this foundation. GitHub Issues and GitHub Projects are the intended defaults.

However, JIRA integration is **retained in canon** because:
1. Many organizations use JIRA as system of record
2. Provides reference for enterprise issue tracking
3. Demonstrates MCP server patterns
4. Supports `/ticket` command workflow

**Default Workflow Priority:**
1. GitHub Issues (intended default, implementation deferred to Phase B)
2. GitHub Projects (intended default, implementation deferred to Phase B)
3. JIRA (available but dormant)
4. Linear (available but dormant)

**Adoption Guidance:**
- Projects using JIRA: Enable `jira` MCP server explicitly
- Projects using GitHub only: Leave JIRA dormant, remove from `.mcp.json` if desired
- Projects using Linear: Enable `linear` MCP server instead

---

### GitHub Integration

**Status:** Dormant by Default

**Configuration:**
```json
{
  "mcpServers": {
    "github": {
      "type": "stdio",
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-github"],
      "env": {
        "GITHUB_TOKEN": "${GITHUB_TOKEN}"
      }
    }
  }
}
```

**Capabilities When Enabled:**
- Read repository information
- List and read issues
- Create and update issues
- Manage pull requests
- Create releases
- Manage labels, milestones

**Authorization Requirements:**
- GitHub personal access token or app token
- Token scopes: `repo`, `issues`, `pull_requests` (minimum)

**Risks:**
- **HIGH:** Can create/modify issues and PRs
- **MEDIUM:** Can manage repository settings (if token permits)
- **LOW:** Can read repository data

**Intended Default (Implementation Deferred):**

GitHub is the **intended primary workflow** for this foundation. However:

- Phase A: Configuration present but dormant
- Phase B+: Implementation of GitHub-first workflow
- Requires: Explicit user request to enable

**Why Deferred:**
- Phase A is documentation only, not implementation
- GitHub workflow requires runtime behavior definition
- Cost and governance gates not yet defined

**Adoption Guidance:**
- Enable `github` MCP server for full GitHub integration
- Use `gh` CLI as alternative for some operations
- GitHub Actions workflows are separate (see CAP-008)

---

### Other Integrations

#### Linear

**Status:** Dormant by Default

**Use Case:** Alternative to JIRA for modern issue tracking

**Enablement:** Set `enabledMcpjsonServers: ["linear"]` and provide `LINEAR_API_KEY`

---

#### Slack

**Status:** Dormant by Default

**Use Case:** Notifications, status updates, team communication

**Risks:** HIGH - Can send messages to channels, potential for spam

**Enablement:** Requires bot token with explicit channel permissions

---

#### Sentry

**Status:** Dormant by Default

**Use Case:** Error monitoring, reading error reports, creating issue links

**Risks:** LOW - Primarily read-only, can link errors to issues

---

#### Postgres

**Status:** Dormant by Default

**Use Case:** Database query for debugging, schema inspection

**Risks:** VERY HIGH - Direct database access, potential for data loss

**Recommendation:** Only enable in isolated development environments, never production

---

#### Notion

**Status:** Dormant by Default

**Use Case:** Documentation reading/writing, knowledge base integration

**Risks:** MEDIUM - Can modify documentation

---

#### Memory

**Status:** Dormant by Default

**Use Case:** Persistent storage for context across sessions

**Risks:** LOW - Local storage only

---

## Integration Security Requirements

### Credential Management

**MUST:**
- ✓ Store credentials in environment variables
- ✓ Use `.env` files (gitignored) or system environment
- ✓ Document required credential scopes/permissions
- ✓ Rotate credentials periodically

**MUST NOT:**
- ✗ Commit credentials to version control
- ✗ Store credentials in `.mcp.json` directly
- ✗ Share credentials across environments (dev/staging/prod)
- ✗ Use overly permissive tokens (principle of least privilege)

### Example: Secure Credential Setup

```bash
# .env (gitignored)
JIRA_HOST=https://yourcompany.atlassian.net
JIRA_EMAIL=bot@yourcompany.com
JIRA_API_TOKEN=ATATT3xFfGF0...

GITHUB_TOKEN=ghp_abc123...

# Load in shell profile
export $(cat .env | xargs)

# Or use direnv
# echo 'dotenv' > .envrc
# direnv allow
```

### Token Scopes

Minimum required scopes for each integration:

| Integration | Minimum Scopes | Why |
|-------------|---------------|-----|
| JIRA | Read/Write issues | Ticket workflow |
| GitHub | `repo`, `issues` | Issue management, PR operations |
| Linear | Read/Write issues | Issue tracking |
| Slack | `chat:write`, `channels:read` | Send messages to authorized channels |
| Sentry | `project:read`, `event:read` | Read error data |
| Postgres | `CONNECT`, `SELECT` (read-only recommended) | Query execution |
| Notion | Read/Write pages | Documentation access |

---

## GitHub Actions Integration

**Status:** Disabled by Default

**Location:** `.github/workflows/`

**Workflows:**
- `pr-claude-code-review.yml` - Automated PR reviews
- `scheduled-claude-code-docs-sync.yml` - Monthly documentation sync
- `scheduled-claude-code-quality.yml` - Weekly code quality
- `scheduled-claude-code-dependency-audit.yml` - Biweekly dependency updates

**Why Disabled by Default:**

1. **Cost Concerns:**
   - Anthropic API usage: ~$10-50/month estimated
   - GitHub Actions minutes: Included in most plans
   - Unpredictable costs based on PR volume

2. **Governance Concerns:**
   - Automated commits require review
   - PR review quality needs validation
   - Scheduled jobs may conflict with team workflows

3. **Configuration Required:**
   - `ANTHROPIC_API_KEY` secret must be set
   - Workflows may need customization for project
   - Branch protection rules may need adjustment

**Enablement Requirements:**

1. Review each workflow for project fit
2. Set `ANTHROPIC_API_KEY` in repository secrets
3. Test on non-production branches first
4. Monitor costs and adjust schedules
5. Commit workflows to `.github/workflows/` to enable

**Adoption Guidance:**
- Start with `pr-claude-code-review.yml` only
- Monitor costs for 1 month before enabling scheduled workflows
- Disable or adjust schedules if costs exceed budget

---

## Integration Testing

### Verification Checklist

Before relying on an integration:

- [ ] Credentials configured correctly
- [ ] MCP server enabled in settings.json
- [ ] Server connects successfully (check logs)
- [ ] Test operations in development environment
- [ ] Audit trail configured (if applicable)
- [ ] Error handling tested (network failure, auth failure)
- [ ] Cost implications understood

### Test Commands

```bash
# Test JIRA connection
claude "Using JIRA MCP, read ticket PROJ-123"

# Test GitHub connection
claude "Using GitHub MCP, list issues in this repo"

# Verify MCP server status
# (Check Claude Code output for MCP connection logs)
```

---

## Integration Audit Trail

### Required Logging

For all integrations, log:
- Action taken (read, write, create, update, delete)
- Resource affected (ticket ID, issue number, etc.)
- Timestamp
- Result (success, failure, error)

### Example Log Format

```
[2026-01-09T12:34:56Z] MCP:JIRA:UPDATE ticket=PROJ-123 field=status value="In Review" result=success
[2026-01-09T12:35:10Z] MCP:GITHUB:CREATE_ISSUE repo=owner/repo title="Bug: ..." result=success
[2026-01-09T12:35:15Z] MCP:SLACK:SEND_MESSAGE channel=#engineering message="PR ready" result=success
```

---

## Explicit Non-Defaults

### GitHub Issues as Primary Workflow

**Status:** Intended default, implementation deferred

**Phase A:** Configuration and policy only
**Phase B+:** Runtime implementation when explicitly requested

**Reason for Deferral:**
- GitHub integration patterns need refinement
- Cost and governance gates need definition
- User workflow research required

### JIRA as Secondary Option

**Status:** Retained but not primary

**Reason:** Enterprise requirement support, but GitHub Issues preferred for open-source and GitHub-native workflows

---

## Acceptance Criteria

This policy is complete when:

1. ✓ MCP server inventory is complete
2. ✓ Default posture (dormant) is stated
3. ✓ Enablement methods are documented
4. ✓ Integration-specific policies are defined
5. ✓ Security requirements are explicit
6. ✓ GitHub Actions status is documented
7. ✓ Explicit non-defaults are stated (GitHub as intended default, JIRA retained but dormant)

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
