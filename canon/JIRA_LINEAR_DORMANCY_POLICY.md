# Jira and Linear Dormancy Policy

## Purpose

This document defines the canonical policy for Jira and Linear integration dormancy. It establishes that these integrations remain present in the system but inactive by default, with GitHub Issues and Projects as the primary work management system.

## Scope

This specification governs:

- Default state of Jira and Linear integrations
- Conditions under which dormant integrations may be activated
- Configuration management for dormant vs active integrations
- Migration paths between GitHub-native and external system workflows
- Coexistence constraints when external systems are active

## Non-goals

This specification does NOT:

- Remove Jira or Linear integration code from the codebase
- Prevent users from activating external integrations
- Define implementation details of Jira/Linear MCP servers
- Prescribe specific API clients or libraries
- Replace existing external system documentation

## Explicit Constraints

1. **Dormant by Default**: New repositories MUST default to GitHub-native workflow with Jira/Linear inactive
2. **Explicit Activation**: External integrations MUST be explicitly enabled via configuration
3. **No Dual Authority**: GitHub and external system MUST NOT both be active for same work items
4. **Opt-In Only**: No automatic activation of external integrations based on heuristics
5. **Fail-Closed**: When integration state ambiguous, system MUST refuse operation
6. **Preserve Code**: Integration code MUST remain in repository for users who need it

## Definitions

- **Dormant Integration**: Integration present in codebase but not actively syncing or reading data
- **Active Integration**: Integration configured with credentials and actively used
- **Dual Authority**: Anti-pattern where same work item managed in multiple systems simultaneously
- **GitHub-Native**: Work management using only GitHub Issues, Projects, and native features
- **External System**: Jira, Linear, or similar work management platform outside GitHub
- **Activation**: Process of enabling dormant integration with valid credentials and configuration

## Canonical Model

### Default Integration State

#### New Repositories

1. New repositories MUST default to:
   - GitHub Issues as canonical work item source
   - GitHub Projects as canonical workflow board
   - No Jira or Linear configuration present

2. Repository README or setup docs SHOULD state:
   ```markdown
   ## Work Management
   This repository uses GitHub Issues and Projects for work tracking.
   Jira and Linear integrations are available but dormant by default.
   See docs/canon/JIRA_LINEAR_DORMANCY_POLICY.md for activation.
   ```

3. Configuration files MUST NOT include Jira/Linear credentials by default:
   ```json
   // .claude/config.json (example)
   {
     "workManagement": {
       "provider": "github",
       "github": {
         "enabled": true,
         "defaultProject": "Main Board"
       },
       "jira": {
         "enabled": false
         // Credentials must be added manually
       },
       "linear": {
         "enabled": false
         // Credentials must be added manually
       }
     }
   }
   ```

#### Existing Repositories with Active Integrations

1. Repositories with existing Jira/Linear usage MUST NOT be automatically migrated

2. Configuration MUST remain as-is until team explicitly updates:
   ```json
   {
     "workManagement": {
       "provider": "jira",  // Preserved for existing users
       "jira": {
         "enabled": true,
         "instance": "https://company.atlassian.net"
       }
     }
   }
   ```

3. Teams MAY migrate to GitHub-native at their discretion following migration plan

### Integration States

#### Dormant State (Default)

**Characteristics**:
- Integration code present in codebase
- No credentials configured
- No API calls made
- Commands referencing integration return clear error
- Documentation available for future activation

**Behavior**:
```bash
# Example: /ticket command with dormant Jira
$ /ticket PROJ-123

Error: Jira integration is not configured.

This repository uses GitHub-native work management.
To work on an issue:
  1. Find or create GitHub issue
  2. See docs/canon/TICKET_COMMAND_REPLACEMENT_SPEC.md

To activate Jira integration:
  - See docs/canon/JIRA_LINEAR_DORMANCY_POLICY.md
```

**Configuration**:
```json
{
  "jira": {
    "enabled": false
  }
}
```

#### Active State (Opt-In)

**Characteristics**:
- Valid credentials configured
- API connectivity verified
- Commands function normally
- GitHub remains source of truth for git workflow
- External system is source of truth for work items

**Behavior**:
```bash
# Example: /ticket command with active Jira
$ /ticket PROJ-123

✓ Fetched PROJ-123: Implement user authentication
✓ Created branch: PROJ-123-implement-user-auth
✓ Updated ticket status to In Progress
```

**Configuration**:
```json
{
  "jira": {
    "enabled": true,
    "instance": "https://company.atlassian.net",
    "project": "PROJ",
    "credentials": {
      "email": "user@company.com",
      "apiToken": "${JIRA_API_TOKEN}"
    }
  }
}
```

#### Invalid State (Error)

**Characteristics**:
- Enabled flag set to true
- BUT credentials missing or invalid
- OR network unreachable
- OR project not accessible

**Behavior**:
System MUST fail-closed:
```bash
$ /ticket PROJ-123

Error: Jira integration enabled but not functional.
  - API token missing or expired
  - Check configuration: .claude/config.json

Falling back to GitHub-native workflow not safe.
Fix configuration or disable Jira integration.
```

System MUST NOT silently fall back to GitHub Issues. Explicit user action REQUIRED.

### Activation Process

#### Prerequisites

Before activating external integration:

1. **Team Agreement**: Team must agree to use external system as canonical source
2. **Migration Plan**: If migrating from GitHub-native, plan for existing issues
3. **Credentials**: Valid API tokens or OAuth credentials available
4. **Network Access**: Confirm firewall/VPN allows access to external system
5. **Training**: Team familiar with external system workflow

#### Activation Steps (Jira Example)

1. **Obtain Credentials**:
   ```bash
   # Create API token at https://id.atlassian.com/manage-profile/security/api-tokens
   export JIRA_API_TOKEN="your-token-here"
   ```

2. **Update Configuration**:
   ```json
   // .claude/config.json
   {
     "workManagement": {
       "provider": "jira",
       "jira": {
         "enabled": true,
         "instance": "https://yourcompany.atlassian.net",
         "project": "PROJ",
         "credentials": {
           "email": "your-email@company.com",
           "apiToken": "${JIRA_API_TOKEN}"
         }
       }
     }
   }
   ```

3. **Verify Connectivity**:
   ```bash
   # Test command (hypothetical, not implemented in Phase B)
   $ gh jira test-connection
   ✓ Connected to Jira instance
   ✓ Project PROJ accessible
   ✓ User has read/write permissions
   ```

4. **Update Documentation**:
   Update repository README:
   ```markdown
   ## Work Management
   This repository uses Jira for work tracking.
   - Jira Project: PROJ
   - Use `/ticket PROJ-123` to start work on tickets
   ```

5. **Train Team**:
   - Share Jira project link
   - Document workflow differences
   - Update PR templates to reference Jira tickets

#### Activation Steps (Linear Example)

Similar process for Linear:

1. **Obtain API Key**:
   ```bash
   # Create API key at https://linear.app/settings/api
   export LINEAR_API_KEY="lin_api_your-key-here"
   ```

2. **Update Configuration**:
   ```json
   {
     "workManagement": {
       "provider": "linear",
       "linear": {
         "enabled": true,
         "team": "ENG",
         "credentials": {
           "apiKey": "${LINEAR_API_KEY}"
         }
       }
     }
   }
   ```

3. **Verify and document** (same as Jira)

### Deactivation Process

To revert from external system to GitHub-native:

1. **Export Work Items**: Export open Jira/Linear items to GitHub Issues
2. **Update Configuration**: Set `enabled: false` for external integration
3. **Update Documentation**: Update README to reflect GitHub-native workflow
4. **Notify Team**: Communicate change and new workflow
5. **Archive External Project**: Mark Jira/Linear project as read-only

**CRITICAL**: Do NOT leave work items orphaned in external system. Migration MUST be complete before deactivation.

### Coexistence Rules

#### Prohibited: Dual Authority

The following is EXPLICITLY PROHIBITED:

- Same work item managed in both GitHub Issues and Jira/Linear
- Bidirectional sync between GitHub and external system
- Mirroring of work items across systems
- Using GitHub Issues for some work, Jira for other work in same repo

**Rationale**: Dual authority creates conflicts, data loss, and ambiguity about source of truth.

#### Allowed: Informational References

The following is PERMITTED:

- GitHub Issue body references external ticket for context
- External ticket references GitHub PR for implementation
- Historical migration notes linking old tickets to new issues

**Key Distinction**: References are informational only, not bidirectional sync.

Example:
```markdown
## GitHub Issue Body
Related to historical Jira ticket: PROJ-123
(for context only, this issue is source of truth)
```

#### Allowed: Phased Migration

During migration period, repository MAY:

- Have some features tracked in old system, new features in GitHub
- Use grace period (e.g., 1 sprint) for transition
- Maintain read-only access to old system for reference

**Requirements**:
- Clear documentation of which system is active
- Defined end date for migration period
- No new work in old system once migration starts

### Command Behavior by State

#### `/ticket` Command

| State | Behavior |
|-------|----------|
| Jira/Linear active | Fetch ticket from external system, create branch, update status |
| GitHub-native (default) | Error: "Jira/Linear not configured. See TICKET_COMMAND_REPLACEMENT_SPEC.md" |
| Config invalid | Error: "Integration enabled but not functional. Fix configuration." |

#### Future Commands (Phase C+)

| Command | Jira/Linear Active | GitHub-Native |
|---------|-------------------|---------------|
| `/issue-start` | Error: "Use /ticket for Jira/Linear" | Start GitHub issue workflow |
| `/pr-create` | Link to Jira/Linear ticket | Link to GitHub issue |
| `/work-status` | Show external system status | Show GitHub Project status |

### Configuration File Structure

#### Canonical Config Schema

```json
{
  "$schema": "https://example.com/claude-config-schema.json",
  "workManagement": {
    "provider": "github" | "jira" | "linear",
    "github": {
      "enabled": true,
      "defaultProject": "Main Board",
      "issueLabels": {
        "required": ["type", "priority"]
      }
    },
    "jira": {
      "enabled": false,
      "instance": "https://company.atlassian.net",
      "project": "PROJ",
      "credentials": {
        "email": "user@company.com",
        "apiToken": "${JIRA_API_TOKEN}"
      }
    },
    "linear": {
      "enabled": false,
      "team": "ENG",
      "credentials": {
        "apiKey": "${LINEAR_API_KEY}"
      }
    }
  }
}
```

#### Validation Rules

1. **Exactly One Provider**: `provider` field MUST match one enabled integration
2. **Credentials via Environment**: Sensitive values MUST use environment variable syntax
3. **No Plaintext Secrets**: API tokens MUST NOT be committed in plaintext
4. **Schema Validation**: Config MUST pass JSON schema validation before use

## Failure Modes

### Ambiguous Provider

**Scenario**: Both `github.enabled` and `jira.enabled` set to true.

**Failure Mode**: System MUST refuse to start. Error MUST identify conflict and require explicit choice.

```
Error: Multiple work management providers enabled.
  - GitHub: enabled
  - Jira: enabled

Only one provider may be active. Update .claude/config.json:
  Set workManagement.provider to "github" or "jira"
```

### Missing Credentials

**Scenario**: `jira.enabled: true` but `JIRA_API_TOKEN` environment variable not set.

**Failure Mode**: System MUST refuse Jira operations. Error MUST identify missing credential.

```
Error: Jira integration enabled but credentials missing.
  - Set environment variable: JIRA_API_TOKEN
  - Or disable Jira: set jira.enabled to false
```

### Stale Configuration

**Scenario**: Config references Jira project that no longer exists or user lost access.

**Failure Mode**: System MUST fail on first operation attempt. MUST NOT cache stale data.

```
Error: Cannot access Jira project PROJ.
  - Project may be deleted or renamed
  - User may lack permissions
  - Check credentials and project key
```

### Partial Migration

**Scenario**: Team starts migrating from Jira to GitHub but leaves some tickets in Jira.

**Failure Mode**: No automatic failure, but process failure. Mitigation: require migration plan with cutover date before starting.

### Silent Fallback Attempt

**Scenario**: Jira configured but unreachable; system tempted to fall back to GitHub.

**Failure Mode**: System MUST NOT silently change provider. MUST fail-closed and require user decision.

## Out-of-Scope Items

The following are explicitly OUT OF SCOPE:

1. **Automatic Migration**: No tool to automatically migrate Jira tickets to GitHub Issues
2. **Bidirectional Sync**: No syncing of state between GitHub and external systems
3. **Multi-Provider Support**: Cannot use Jira for some issues, Linear for others in same repo
4. **Provider Auto-Detection**: No guessing which provider to use based on branch names or commit messages
5. **Credential Management**: No built-in secrets vault; use environment variables
6. **SSO Integration**: External system SSO is external system's responsibility

## Change Control

### Amendment Process

1. Changes to dormancy policy MUST be proposed via PR to this document
2. PRs modifying policy MUST include:
   - Rationale for change
   - Impact on users with active integrations
   - Impact on users with dormant integrations
   - Migration plan if defaults change

3. Approval requirements:
   - Review from repository maintainer
   - Sign-off from at least one user of each integration type (GitHub, Jira, Linear)
   - Validation that change doesn't break existing configurations

### Versioning

This document follows semantic versioning:
- **Major**: Change to default state or removal of integration support
- **Minor**: New integration type or optional feature
- **Patch**: Clarification or documentation updates

Current version: `1.0.0` (Phase B baseline)

### Backward Compatibility

Changes to this policy:

1. MUST NOT automatically disable active integrations
2. MUST NOT change existing configuration files without user approval
3. SHOULD provide migration tooling if breaking changes introduced
4. MUST document upgrade path in changelog

---

**Document Status**: Canonical (Phase B)
**Last Updated**: 2026-01-09
**Maintained By**: Repository Governance Team
