# Projects Governance Specification

## Purpose

This document defines the canonical governance model for GitHub Projects. It establishes authority, decision-making processes, configuration standards, and maintenance responsibilities for project boards used in work management.

## Scope

This specification governs:

- Authority and ownership of GitHub Projects
- Project creation and archival processes
- Configuration standards (fields, views, workflows)
- Access control and permissions model
- Change management for project structure
- Audit and compliance requirements

## Non-goals

This specification does NOT:

- Define GitHub Projects API implementation details
- Prescribe specific automation code or GitHub Actions
- Replace GitHub's native Projects documentation
- Define git workflow or code review processes
- Specify individual team workflow preferences

## Explicit Constraints

1. **Single Source of Truth**: One canonical project per repository (may have multiple views)
2. **Explicit Ownership**: Each project MUST have designated owner(s)
3. **Schema Stability**: Project field changes require governance approval
4. **No Shadow Projects**: All work MUST be tracked in official projects, not personal boards
5. **Fail-Closed Permissions**: When access unclear, deny rather than grant
6. **Audit Trail Required**: All structural changes MUST be documented

## Definitions

- **Project**: GitHub Projects (beta/new Projects, not classic project boards)
- **Project Owner**: Individual or team with authority to modify project structure
- **Project Field**: Custom metadata field added to project (beyond native issue properties)
- **Project View**: Saved configuration of filters, grouping, and sorting
- **Project Workflow**: Automation rules for state transitions
- **Structural Change**: Modification to fields, required metadata, or validation rules
- **Operational Change**: Day-to-day issue updates, status changes, or assignments

## Canonical Model

### Project Authority Structure

#### Repository-Level Projects

1. Each repository SHOULD have exactly one canonical project:
   ```
   Repository: company/backend
   Project: "Backend Development"
   ```

2. Project owner is repository maintainer team by default

3. Project name SHOULD match repository purpose

4. Project MUST be linked from repository README

#### Organization-Level Projects

1. Organization MAY have cross-repo projects for coordination:
   ```
   Organization: company
   Project: "Q1 2026 Roadmap"
   Scope: Issues from multiple repos
   ```

2. Organization project owner is designated program/product manager

3. Organization projects are INFORMATIONAL only, not canonical source

4. Repository projects remain source of truth for individual work items

### Ownership and Permissions

#### Owner Responsibilities

Project owners MUST:

1. Define and maintain project field schema
2. Approve structural changes (new fields, field removal)
3. Train team on project usage
4. Monitor for misuse or stale issues
5. Conduct quarterly audits
6. Document project configuration in repository

Project owners MAY:

1. Delegate view creation to team members
2. Configure automation workflows
3. Archive completed work
4. Generate reports and metrics

#### Access Levels

| Role | Permissions | Granted To |
|------|-------------|------------|
| Admin | Full project control, field management, settings | Repository maintainers, Project owner |
| Write | Add/remove issues, update fields, create views | Repository contributors |
| Read | View project, issues, and fields | Repository read access |
| None | No access | External users (unless public repo) |

#### Permission Model

1. Access MUST follow principle of least privilege
2. Field schema changes REQUIRE admin access
3. View creation REQUIRES write access
4. Issue updates REQUIRE write access
5. No anonymous editing, even for public repos

### Project Creation Process

#### Prerequisites

Before creating new project:

1. **Justification**: Document why existing project insufficient
2. **Owner Identified**: Designated individual accepts ownership
3. **Schema Design**: Draft field structure and validation rules
4. **Team Buy-In**: Affected team members agree to use project

#### Creation Steps

1. **Propose Project**:
   - Create RFC issue in repository
   - Include: purpose, scope, owner, field schema
   - Tag repository maintainers for review

2. **Review and Approval**:
   - Maintainers review proposal
   - At least one approval required
   - No unresolved concerns

3. **Create Project**:
   ```
   - Go to repository Projects tab
   - Click "New project"
   - Select "Table" or "Board" template
   - Name: "[Repo] [Purpose]" (e.g., "Backend Development")
   - Visibility: Private (default) or Public
   ```

4. **Configure Fields**:
   - Add required custom fields (see Field Schema section)
   - Set default values
   - Configure field descriptions

5. **Create Initial Views**:
   - Default: All issues
   - Backlog: Status = Backlog
   - Active: Status = Ready, In Progress, In Review
   - Done: Status = Done (archived after 30 days)

6. **Document Configuration**:
   - Add `.github/PROJECT_CONFIG.md` to repository
   - Include: field definitions, view descriptions, workflows
   - Link from main README

7. **Announce to Team**:
   - Team meeting or Slack announcement
   - Share project link and documentation
   - Schedule training if needed

### Field Schema Standards

#### Required Fields

All projects MUST include:

1. **Status** (Single Select)
   - Type: Single Select
   - Values: `Backlog`, `Ready`, `In Progress`, `In Review`, `Done`, `Cancelled`
   - Default: `Backlog`
   - Description: "Current workflow state"

2. **Priority** (Single Select)
   - Type: Single Select
   - Values: `P0`, `P1`, `P2`, `P3`
   - Default: `P3`
   - Description: "Priority level (P0=critical, P3=low)"

#### Recommended Fields

Projects SHOULD include:

3. **Size** (Single Select)
   - Type: Single Select
   - Values: `XS`, `S`, `M`, `L`, `XL`
   - No default
   - Description: "Estimated complexity (not time)"

4. **Sprint** (Iteration)
   - Type: Iteration
   - Duration: 2 weeks (configurable)
   - Description: "Sprint assignment for teams using sprints"

5. **Team** (Single Select)
   - Type: Single Select
   - Values: Team names from organization
   - Description: "Owning team"

#### Optional Fields

Projects MAY include:

6. **Blocked By** (Text)
   - Type: Text
   - Description: "Issue numbers blocking this work (#123)"

7. **Customer** (Text)
   - Type: Text
   - Description: "Customer or stakeholder requesting feature"

8. **Target Release** (Single Select)
   - Type: Single Select
   - Values: Version numbers or release names
   - Description: "Intended release version"

#### Prohibited Fields

Projects MUST NOT include:

- Time estimates or time tracking (use external tools)
- Personally identifiable information (PII)
- Secrets or credentials
- Duplicate data already in issue body
- Fields with ambiguous or overlapping purpose

### View Configuration Standards

#### Standard View Set

Projects SHOULD provide these views:

1. **All Items** (Default)
   - Filter: None
   - Group by: Status
   - Sort: Priority descending, then creation date

2. **Backlog**
   - Filter: Status = Backlog
   - Group by: Priority
   - Sort: Priority descending

3. **Current Sprint**
   - Filter: Sprint = Current
   - Group by: Status
   - Sort: Priority descending

4. **My Work**
   - Filter: Assignee = @me
   - Group by: Status
   - Sort: Priority descending

5. **Blocked**
   - Filter: Status = Blocked OR "Blocked By" is not empty
   - Group by: None
   - Sort: Priority descending

#### View Naming Convention

Views MUST follow naming convention:
- Use title case: "Current Sprint" not "current sprint"
- Be concise: ≤ 25 characters
- Describe filter: "Blocked Items" not "View 3"
- Avoid team-specific jargon in shared projects

#### View Permissions

1. Anyone with write access MAY create personal views
2. Shared views (visible to all) REQUIRE owner approval
3. Default view MAY only be changed by owner
4. View configuration SHOULD be documented in `.github/PROJECT_CONFIG.md`

### Workflow Automation

#### Allowed Automations

Projects MAY configure these automations:

1. **Status: Issue Opened**
   - Trigger: Issue opened
   - Action: Add to project with Status = Backlog

2. **Status: PR Opened**
   - Trigger: PR opened that closes issue
   - Action: Set issue Status = In Review

3. **Status: PR Merged**
   - Trigger: PR merged that closes issue
   - Action: Set issue Status = Done, close issue

4. **Auto-Archive**
   - Trigger: Issue in Done state for 30 days
   - Action: Archive issue from project

#### Prohibited Automations

Projects MUST NOT configure automations that:

1. Modify issue body or title
2. Add comments automatically (except for explicit commands)
3. Change labels based on time or external events
4. Sync with external systems (Jira, Linear)
5. Make assumptions about priority or size
6. Close issues without human approval

### Change Management

#### Structural Changes

**Definition**: Changes to required fields, field values, or validation rules

**Process**:

1. **Propose Change**:
   - Open issue titled "Project Change: [description]"
   - Include: rationale, impact analysis, migration plan
   - Tag: `project-governance`

2. **Impact Analysis**:
   - Identify affected issues
   - Document breaking changes
   - Estimate effort to update existing items

3. **Approval**:
   - Required: Project owner approval
   - Required: At least one team member sign-off
   - Optional: Stakeholder review for major changes

4. **Implementation**:
   - Make configuration changes
   - Update `.github/PROJECT_CONFIG.md`
   - Bulk update existing issues if needed
   - Announce change to team

5. **Validation**:
   - Verify no broken views or filters
   - Check automation still functions
   - Confirm team can access project

#### Operational Changes

**Definition**: Day-to-day updates to issue status, assignments, or field values

**Process**:
- No approval required
- Anyone with write access may make changes
- Follow documented workflow and conventions
- No audit trail beyond GitHub timeline

#### Emergency Changes

For urgent fixes (e.g., broken automation blocking work):

1. Owner MAY make immediate change without approval
2. MUST document change in issue within 24 hours
3. MUST retroactively propose formal change if structural
4. Team MUST be notified of emergency change

### Audit and Compliance

#### Quarterly Audit Requirements

Project owners MUST conduct quarterly audit:

1. **Field Usage Review**:
   - Identify unused fields (consider removal)
   - Verify field values still relevant
   - Check for data quality issues

2. **Stale Issue Review**:
   - Issues in "In Progress" > 30 days → revert to Backlog or close
   - Issues in "In Review" with no active PR → revert to Ready
   - Issues in "Blocked" > 60 days → escalate or close

3. **Access Review**:
   - Verify admin access limited to owners
   - Remove access for departed team members
   - Confirm permissions align with current roles

4. **Automation Validation**:
   - Test each automation still functions
   - Review automation logs for errors
   - Disable non-functioning automations

5. **Documentation Sync**:
   - Verify `.github/PROJECT_CONFIG.md` is current
   - Update examples in documentation
   - Refresh team training materials

#### Audit Deliverable

Audit MUST produce:
- Issue titled "Q[X] YYYY Project Audit Results"
- Summary of findings and actions taken
- List of proposed improvements
- Sign-off from project owner

### Project Archival

#### When to Archive

Archive project when:
- Repository archived or deleted
- Project replaced by new structure
- Migration to different work management system
- Project unused for > 6 months

#### Archival Process

1. **Announce Intent**:
   - Notify team at least 2 weeks in advance
   - Provide migration plan for open work items
   - Document reason for archival

2. **Migrate Open Work**:
   - Move open issues to new project OR
   - Close issues no longer relevant
   - Document migration in issue comments

3. **Archive Project**:
   - GitHub Projects Settings → Archive project
   - Project remains read-only, searchable
   - Cannot be reopened (must create new)

4. **Update Documentation**:
   - Update README to remove project link
   - Add historical note: "Project archived YYYY-MM-DD"
   - Archive `.github/PROJECT_CONFIG.md` with timestamp

### Multi-Repository Projects

#### Use Cases

Organization-level projects appropriate for:
- Cross-team initiatives (e.g., "API Standardization")
- Quarterly planning and roadmaps
- Executive visibility into multiple products

#### Constraints

1. Organization project is NOT canonical source for individual issues
2. Issues MUST belong to repository project first
3. Organization project acts as aggregator/view only
4. Status updates MUST occur in repository project
5. No exclusive issues in org project (must also be in repo project)

#### Governance

1. Organization project owner is program/product manager
2. Repository owners maintain their issues independently
3. Org project owner MAY NOT modify repository issue metadata
4. Conflicts default to repository project authority

## Failure Modes

### Unauthorized Field Addition

**Scenario**: Team member adds custom field without owner approval.

**Failure Mode**: Field may be used inconsistently or conflict with plans. Owner MUST review field, either approve retroactively or remove with migration plan.

### Schema Migration Failure

**Scenario**: Field renamed but existing issues not bulk-updated.

**Failure Mode**: Issues have mixed old/new field values. Filters and views break. Owner MUST complete migration before new work added.

### Access Creep

**Scenario**: Too many users granted admin access over time.

**Failure Mode**: Unauthorized structural changes occur. Audit MUST identify excess admins and downgrade to write access.

### Stale Automation

**Scenario**: Automation configured but no longer functions due to GitHub API changes.

**Failure Mode**: Issues don't auto-transition, causing manual work. Quarterly audit MUST identify and fix or disable.

### Duplicate Projects

**Scenario**: Team creates personal project instead of using canonical project.

**Failure Mode**: Work scattered across projects, no single source of truth. Owner MUST consolidate and archive duplicate.

### Missing Documentation

**Scenario**: Project configured but `.github/PROJECT_CONFIG.md` never created.

**Failure Mode**: New team members confused about fields and workflow. Owner MUST document within 1 week of creation.

## Out-of-Scope Items

The following are explicitly OUT OF SCOPE:

1. **Classic Projects**: This spec covers GitHub Projects (beta/new), not classic project boards
2. **Third-Party Tools**: No governance for Notion, Asana, Trello integrations
3. **Metrics and Reporting**: Analytics tooling is separate concern
4. **Team Process**: Scrum vs Kanban vs other methodologies are team decisions
5. **Issue Content**: Governance covers project structure, not what goes in issues
6. **GitHub Actions**: Workflow files are separate from project configuration

## Change Control

### Amendment Process

1. Changes to governance policy MUST be proposed via PR to this document
2. PRs modifying governance MUST include:
   - Rationale for change
   - Impact on existing projects
   - Migration plan if breaking change
   - Stakeholder consultation summary

3. Approval requirements:
   - Review from at least two repository maintainers
   - Sign-off from at least one project owner
   - No unresolved concerns from affected teams

### Versioning

This document follows semantic versioning:
- **Major**: Breaking change to governance process or required fields
- **Minor**: New optional field or process enhancement
- **Patch**: Clarification or documentation improvement

Current version: `1.0.0` (Phase B baseline)

### Grandfather Clause

Existing projects created before this policy:

1. SHOULD migrate to standard schema within 6 months
2. MAY continue with existing field structure if documented
3. MUST comply with ownership and audit requirements immediately
4. MUST NOT be used as precedent for new projects

---

**Document Status**: Canonical (Phase B)
**Last Updated**: 2026-01-09
**Maintained By**: Repository Governance Team
