# Projects Board Configuration Specification

## Purpose

This document defines the normative configuration for the GitHub Projects v2 board used by this repository. It specifies exact field schemas, view definitions, board layout, and operational constraints. All project configuration MUST conform to this specification.

## Document Status

- **Status**: Canonical (Phase D.1)
- **Authority**: Binding
- **Enforcement**: Manual verification required
- **Last Updated**: 2026-01-10

## Scope

This specification governs:

- Project board name and visibility settings
- Field definitions (name, type, values, defaults, descriptions)
- View configurations (filters, grouping, sorting, visibility)
- Board layout and organization
- Permitted operations and constraints
- Configuration change control

## Non-Scope

This specification does NOT govern:

- Automation rules or workflows (Phase D.1 explicitly prohibits automation)
- GitHub Actions integration
- API-based synchronization or tooling
- External system integration (Jira, Linear, etc.)
- Issue content or formatting requirements (see ISSUE_SCHEMA.md)
- Issue-to-PR traceability mechanics (see ISSUE_TO_PR_TRACEABILITY.md)

## Explicit Constraints

### Phase D.1 Boundaries

1. **Configuration Only**: This document defines structure only. No automation MUST be configured.
2. **Manual Operations**: All state transitions, field updates, and assignments MUST be manual.
3. **GitHub-Native Only**: No third-party tools, bots, or integrations MUST be used.
4. **Fail-Closed**: When configuration is unclear, operations MUST be blocked pending clarification.
5. **Visibility Surface**: Projects board is read-only view of work state; source of truth is GitHub Issues.

### Operational Constraints

1. **No Automation**: Project MUST NOT use GitHub Projects automation features (auto-add, auto-archive, status sync).
2. **No External Sync**: Project MUST NOT sync with external systems.
3. **Manual Updates Only**: All field updates MUST be performed manually by authorized users.
4. **No Enforcement Logic**: No code MUST validate or enforce field values; human review only.

## Canonical Project Definition

### Project Metadata

| Property | Value | Rationale |
|----------|-------|-----------|
| **Name** | `Claude Foundation — Work Board` | Identifies project purpose and scope |
| **Visibility** | Private | Restricts access to repository collaborators |
| **Owner** | Repository maintainers | Aligns with repository permissions |
| **Template** | Table | Provides structured data view |
| **README** | MUST contain link to this specification | Ensures discoverability of config |

### Project Description

The Projects board MUST include this description:

```
Work tracking board for Claude Foundation project. All configuration is defined in docs/canon/PROJECTS_BOARD_CONFIGURATION.md. No automation enabled. Manual updates only.
```

## Field Schema

### Overview

The project MUST include exactly the fields defined in this section. Fields MUST be configured with exact names, types, values, and defaults as specified. No additional fields MAY be added without amending this specification.

### Field Definitions

#### 1. Status (Required)

| Property | Value |
|----------|-------|
| **Field Name** | Status |
| **Type** | Single select |
| **Values** | `Backlog`, `Ready`, `In Progress`, `Blocked`, `In Review`, `Done`, `Cancelled` |
| **Default** | `Backlog` |
| **Description** | Current workflow state. Updated manually only. |
| **Notes** | No auto-transitions. Manual updates required for all state changes. |

**Value Definitions:**

- `Backlog`: Issue identified but not yet prioritized or ready for work
- `Ready`: Issue ready to be started; all prerequisites met
- `In Progress`: Active work underway; assignee currently working
- `Blocked`: Work cannot proceed due to external dependency or blocker
- `In Review`: Pull request open and undergoing code review
- `Done`: Work completed and merged; issue closed
- `Cancelled`: Work will not be completed; issue closed without implementation

**Constraints:**

- Transitions MUST be manual
- `Blocked` status REQUIRES blocker documentation in issue comments
- `Done` and `Cancelled` are terminal states; issues SHOULD be closed

#### 2. Priority (Required)

| Property | Value |
|----------|-------|
| **Field Name** | Priority |
| **Type** | Single select |
| **Values** | `P0`, `P1`, `P2`, `P3` |
| **Default** | `P3` |
| **Description** | Priority level for issue. P0=critical/urgent, P3=low priority. |

**Value Definitions:**

- `P0`: Critical priority. Blocking production or core functionality. Immediate attention required.
- `P1`: High priority. Important feature or significant bug. Next in queue.
- `P2`: Medium priority. Useful enhancement or minor bug. Schedule when capacity allows.
- `P3`: Low priority. Nice-to-have or deferred work. No immediate schedule.

**Constraints:**

- Default MUST be `P3`; explicit prioritization required to escalate
- Priority changes SHOULD be documented in issue comments
- `P0` SHOULD be reserved for genuine blockers; requires maintainer approval

#### 3. Size (Optional)

| Property | Value |
|----------|-------|
| **Field Name** | Size |
| **Type** | Single select |
| **Values** | `XS`, `S`, `M`, `L`, `XL` |
| **Default** | None (field may be empty) |
| **Description** | Estimated complexity/scope. Not a time estimate. |

**Value Definitions:**

- `XS`: Trivial change. Single file, < 20 lines, no tests needed. Example: typo fix.
- `S`: Small change. 1-2 files, < 100 lines, minimal testing. Example: add validation.
- `M`: Medium change. 2-5 files, < 300 lines, standard tests. Example: new API endpoint.
- `L`: Large change. 5-10 files, < 1000 lines, extensive tests. Example: new feature module.
- `XL`: Extra large. 10+ files, > 1000 lines, architectural impact. Example: refactor core system.

**Constraints:**

- Size is advisory only; no enforcement
- Empty value is permitted
- Size SHOULD be estimated during issue triage, not at creation

#### 4. Assignee (GitHub Native)

| Property | Value |
|----------|-------|
| **Field Name** | Assignees |
| **Type** | GitHub native field |
| **Values** | Repository collaborators |
| **Default** | Unassigned |
| **Description** | Person(s) responsible for completing the issue. |

**Constraints:**

- MUST use GitHub's native assignee field (not custom field)
- Multiple assignees permitted
- Assignment SHOULD occur when work transitions to `In Progress`
- Unassigned issues in `In Progress` status are NOT permitted; if found, revert to `Ready`

#### 5. Labels (GitHub Native)

| Property | Value |
|----------|-------|
| **Field Name** | Labels |
| **Type** | GitHub native field |
| **Values** | Repository-defined labels |
| **Default** | None |
| **Description** | Categorization tags. See repository label schema. |

**Constraints:**

- MUST use GitHub's native labels (not custom field)
- Label schema defined in repository settings, not this document
- Labels MAY be used for filtering views

#### 6. Milestone (GitHub Native)

| Property | Value |
|----------|-------|
| **Field Name** | Milestone |
| **Type** | GitHub native field |
| **Values** | Repository-defined milestones |
| **Default** | None |
| **Description** | Release or delivery milestone. Optional. |

**Constraints:**

- MUST use GitHub's native milestone field (not custom field)
- Milestones defined in repository settings, not this document
- Empty milestone is permitted

### Prohibited Fields

The project MUST NOT include:

- Time tracking fields (hours, estimates, actuals)
- Sprint or iteration fields (Phase D.1 scope)
- Team or ownership fields beyond GitHub Assignees
- Custom text fields for blockers (use issue comments)
- Duplicate fields for GitHub native properties
- Fields containing PII, secrets, or sensitive data

## View Configuration

### Overview

The project MUST include exactly the views defined in this section. Views MUST be configured with exact names, filters, grouping, and sorting as specified.

### View Definitions

#### 1. All Issues (Default View)

| Property | Value |
|----------|-------|
| **View Name** | All Issues |
| **Type** | Table |
| **Default View** | Yes |
| **Filter** | None (show all issues) |
| **Group By** | Status |
| **Sort By** | 1. Priority (descending), 2. Created date (ascending) |
| **Visible Fields** | Status, Priority, Size, Assignees, Labels, Title |

**Purpose**: Provides complete view of all work items regardless of state.

**Constraints**:

- MUST be default view when project is opened
- MUST show all issues without exception
- Grouping by Status MUST preserve value order: Backlog, Ready, In Progress, Blocked, In Review, Done, Cancelled

#### 2. Backlog

| Property | Value |
|----------|-------|
| **View Name** | Backlog |
| **Type** | Table |
| **Default View** | No |
| **Filter** | Status = `Backlog` |
| **Group By** | Priority |
| **Sort By** | 1. Priority (descending), 2. Created date (ascending) |
| **Visible Fields** | Priority, Size, Title, Labels |

**Purpose**: Shows unprioritized and queued work items.

**Constraints**:

- MUST show only Backlog status issues
- Grouping by Priority MUST preserve value order: P0, P1, P2, P3

#### 3. Active Work

| Property | Value |
|----------|-------|
| **View Name** | Active Work |
| **Type** | Board |
| **Default View** | No |
| **Filter** | Status IN (`Ready`, `In Progress`, `Blocked`, `In Review`) |
| **Group By** | Status |
| **Sort By** | Priority (descending) |
| **Visible Fields** | Priority, Assignees, Title |

**Purpose**: Shows work currently in flight or ready to start.

**Constraints**:

- MUST exclude Backlog, Done, and Cancelled statuses
- Board columns MUST be: Ready, In Progress, Blocked, In Review (in that order)
- Empty columns MUST be visible

#### 4. My Work

| Property | Value |
|----------|-------|
| **View Name** | My Work |
| **Type** | Table |
| **Default View** | No |
| **Filter** | Assignee = `@me` AND Status NOT IN (`Done`, `Cancelled`) |
| **Group By** | Status |
| **Sort By** | Priority (descending) |
| **Visible Fields** | Status, Priority, Title |

**Purpose**: Personal view of assigned work.

**Constraints**:

- MUST use GitHub's `@me` filter for current user
- MUST exclude completed work (Done, Cancelled)
- Each user sees only their own assignments

#### 5. Blocked Items

| Property | Value |
|----------|-------|
| **View Name** | Blocked Items |
| **Type** | Table |
| **Default View** | No |
| **Filter** | Status = `Blocked` |
| **Group By** | None |
| **Sort By** | 1. Priority (descending), 2. Updated date (ascending, oldest first) |
| **Visible Fields** | Priority, Assignees, Title, Updated Date |

**Purpose**: Highlights work that is blocked and requires attention.

**Constraints**:

- MUST show only Blocked status issues
- Oldest updated items MUST appear first (indicates stale blockers)
- Blocked issues SHOULD include blocker explanation in issue comments

#### 6. Completed

| Property | Value |
|----------|-------|
| **View Name** | Completed |
| **Type** | Table |
| **Default View** | No |
| **Filter** | Status IN (`Done`, `Cancelled`) |
| **Group By** | Status |
| **Sort By** | Closed date (descending, most recent first) |
| **Visible Fields** | Status, Priority, Assignees, Title, Closed Date |

**Purpose**: Historical view of completed work.

**Constraints**:

- MUST show only terminal statuses (Done, Cancelled)
- MUST sort by most recently closed first
- Issues SHOULD remain visible for audit; archival is manual operation

### View Naming Conventions

All views MUST follow these conventions:

- **Title Case**: "Active Work", not "active work"
- **Concise**: ≤ 25 characters
- **Descriptive**: Name describes filter or purpose
- **No Jargon**: Use plain language; avoid abbreviations

### View Permissions

- **Shared Views** (defined above): Visible to all project users; MAY NOT be modified without amending this specification
- **Personal Views**: Users MAY create private views for personal workflows; MUST NOT be made shared without governance approval

## Board Layout

### Column Order (Board Views)

Board views (e.g., "Active Work") MUST display columns in this order:

1. Ready
2. In Progress
3. Blocked
4. In Review

### Row Sorting

Within each column, items MUST be sorted by:

1. Priority (P0 at top, P3 at bottom)
2. Updated date (oldest first, for priority ties)

### Empty States

- Empty columns MUST be visible (not hidden)
- Empty views MUST display message: "No issues match this view."

## Access and Permissions

### Permission Model

| Role | Project Access | Permitted Operations |
|------|---------------|----------------------|
| **Admin** | Full control | Configure fields, edit views, manage settings, modify all issues |
| **Write** | Edit access | Add/remove issues, update fields, create personal views |
| **Read** | View only | View project, issues, and fields |

### Access Control

- **Admin access** MUST be limited to repository maintainers
- **Write access** MUST be granted to all repository collaborators
- **Read access** MUST follow repository visibility (private repo = no anonymous read)

### Fail-Closed Enforcement

- When permission is unclear, DENY access
- Users MUST NOT be granted admin access "just in case"
- Personal views MUST NOT be shared without explicit approval

## Change Control

### Amendment Process

Changes to this specification MUST follow this process:

1. **Propose Change**:
   - Open issue titled: `[Config] Propose: [description]`
   - Include: rationale, affected views/fields, migration plan
   - Tag: `project-configuration`

2. **Impact Analysis**:
   - Document fields/views affected
   - Identify existing issues requiring updates
   - Estimate manual effort for migration

3. **Approval**:
   - MUST have repository maintainer approval
   - MUST have at least one contributor sign-off
   - MUST resolve all blocking concerns

4. **Implementation**:
   - Update project configuration in GitHub UI
   - Update this specification document via PR
   - Communicate changes to all project users

5. **Verification**:
   - Manually verify all views render correctly
   - Confirm field changes applied
   - Test user access and permissions

### Emergency Changes

For critical fixes (e.g., broken view blocking work):

1. Maintainer MAY make immediate change
2. MUST document change in issue within 24 hours
3. MUST submit retroactive amendment PR
4. MUST notify all project users

### Version Control

This specification is version-controlled in the repository:

- **Location**: `docs/canon/PROJECTS_BOARD_CONFIGURATION.md`
- **Branch**: Changes MUST go through PR review
- **History**: Git history provides audit trail

## Manual Operations Guide

### Adding Issues to Project

**Process**:

1. Create issue in GitHub repository (follow ISSUE_SCHEMA.md)
2. Navigate to Projects tab
3. Click "Add item" in project board
4. Search for issue by number or title
5. Select issue to add
6. Verify issue appears with default field values (Status=Backlog, Priority=P3)

**Constraints**:

- No automation MUST auto-add issues
- Each issue MUST be manually added
- Issues MUST exist before adding to project

### Updating Issue Status

**Process**:

1. Navigate to project board
2. Locate issue in current status column/group
3. Click Status field dropdown
4. Select new status value
5. Optionally add comment to issue explaining status change

**Constraints**:

- No automation MUST change status
- Status changes MUST be intentional (no accidental bulk edits)
- Blocked status SHOULD include blocker explanation in issue comments

### Assigning Issues

**Process**:

1. Open issue in GitHub (either from project or repository)
2. Click "Assignees" in right sidebar
3. Select assignee(s) from collaborators list
4. Save assignment

**Constraints**:

- Assignment SHOULD occur when transitioning to "In Progress"
- Issues in "In Progress" MUST have at least one assignee
- Unassigned "In Progress" issues MUST be reverted to "Ready"

### Closing Issues

**Process**:

1. Update Status field to "Done" or "Cancelled"
2. Close issue in GitHub (via issue page, not project board)
3. Optionally add closing comment with summary

**Constraints**:

- Status field and issue state MUST be synchronized manually
- Closing issue does NOT auto-update Status field
- Status field update does NOT auto-close issue

### Archiving Completed Work

**Process**:

1. Navigate to "Completed" view
2. Review issues in Done/Cancelled status
3. Manually select issues to archive (typically 30+ days old)
4. Click "Archive" from item menu

**Constraints**:

- No auto-archival MUST be configured
- Archival is manual operation only
- Archived issues remain accessible via search

## Failure Modes and Remediation

### Desynchronized Status and Issue State

**Scenario**: Issue marked "Done" in project but not closed, or vice versa.

**Detection**: Manual review of "Completed" view; identify open issues with Status=Done.

**Remediation**:

1. Manually review issue to determine true state
2. Close issue if work truly complete, OR
3. Revert Status to correct value if work incomplete
4. Add comment documenting correction

### Unassigned In-Progress Issues

**Scenario**: Issue in "In Progress" status with no assignee.

**Detection**: Filter "Active Work" view for In Progress status + no assignee.

**Remediation**:

1. If work actively underway: assign to responsible person
2. If work stalled: revert Status to "Ready"
3. Add comment documenting correction

### Blocked Issues Without Explanation

**Scenario**: Issue in "Blocked" status with no blocker documentation.

**Detection**: Review "Blocked Items" view; check issue comments for blocker explanation.

**Remediation**:

1. Contact assignee or reporter to identify blocker
2. Add comment documenting blocker
3. If blocker resolved: update Status appropriately
4. If blocker unclear: revert Status to previous state

### Stale Blocked Issues

**Scenario**: Issue in "Blocked" status for > 60 days.

**Detection**: Review "Blocked Items" view; sort by Updated date (oldest first).

**Remediation**:

1. Escalate blocker to maintainers
2. Re-evaluate if issue still relevant
3. If blocker unresolvable: move to Cancelled with explanation
4. If blocker resolved: update Status and proceed

### Unauthorized Field Addition

**Scenario**: User adds custom field not defined in this specification.

**Detection**: Periodic project configuration audit; compare fields to specification.

**Remediation**:

1. Identify who added field and why
2. If field useful: initiate amendment process to add to spec
3. If field unnecessary: remove field (may require migrating data)
4. Communicate policy to user

## Audit and Compliance

### Quarterly Audit Checklist

Project owner MUST conduct quarterly audit:

- [ ] **Field Schema Verification**: Confirm all fields match specification exactly
- [ ] **View Configuration Verification**: Confirm all views match specification exactly
- [ ] **Unauthorized Modifications**: Check for custom fields, views, or settings not in spec
- [ ] **Desynchronized Issues**: Identify issues with Status/state mismatch
- [ ] **Unassigned In-Progress**: Identify and remediate unassigned active work
- [ ] **Stale Blocked Issues**: Review blocked items older than 60 days
- [ ] **Access Review**: Verify admin access limited to maintainers
- [ ] **Permission Audit**: Confirm no unauthorized access grants
- [ ] **Specification Sync**: Confirm this document reflects actual configuration

### Audit Deliverable

Audit MUST produce issue:

- **Title**: `[Audit] Q[X] YYYY Project Configuration Audit`
- **Label**: `project-configuration`
- **Contents**:
  - Checklist completion status
  - List of discrepancies found
  - Remediation actions taken
  - Proposed specification amendments (if any)
  - Sign-off from project owner

### Continuous Compliance

- Manual spot-checks SHOULD occur monthly
- Configuration drift SHOULD be corrected immediately when discovered
- Specification amendments SHOULD be batched quarterly unless urgent

## Validation Checklist

Use this checklist to verify project configuration compliance:

### Project-Level

- [ ] Project name is exactly "Claude Foundation — Work Board"
- [ ] Project visibility is Private
- [ ] Project description matches specification
- [ ] Project README links to this specification

### Fields

- [ ] Field "Status" exists with exact values: Backlog, Ready, In Progress, Blocked, In Review, Done, Cancelled
- [ ] Field "Priority" exists with exact values: P0, P1, P2, P3
- [ ] Field "Size" exists with exact values: XS, S, M, L, XL
- [ ] No custom fields exist beyond those specified
- [ ] No prohibited fields exist (time tracking, sprint, etc.)

### Views

- [ ] View "All Issues" exists and is default view
- [ ] View "Backlog" exists with correct filter (Status=Backlog)
- [ ] View "Active Work" exists with correct filter (Status IN Ready/In Progress/Blocked/In Review)
- [ ] View "My Work" exists with correct filter (Assignee=@me, Status NOT IN Done/Cancelled)
- [ ] View "Blocked Items" exists with correct filter (Status=Blocked)
- [ ] View "Completed" exists with correct filter (Status IN Done/Cancelled)
- [ ] All views have correct grouping, sorting, and visible fields per specification

### Operations

- [ ] No automation rules configured
- [ ] No GitHub Actions syncing with project
- [ ] No external integrations (Jira, Linear, etc.)
- [ ] All status transitions are manual
- [ ] All field updates are manual

### Permissions

- [ ] Admin access limited to repository maintainers
- [ ] Write access granted to all collaborators
- [ ] No unauthorized admin grants
- [ ] No anonymous access (private project)

## Appendix: Configuration Change Log

Changes to this specification MUST be documented here:

| Date | Version | Change Summary | Author | Issue/PR |
|------|---------|----------------|--------|----------|
| 2026-01-10 | 1.0.0 | Initial specification (Phase D.1) | StateraGroup | TBD |

---

**Document Authority**: Normative (binding)
**Phase**: D.1 (Configuration + Documentation Only)
**Enforcement**: Manual verification required
**Automation**: Explicitly prohibited
**Maintained By**: Repository Maintainers
