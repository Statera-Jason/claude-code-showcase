# GitHub Work Management Specification

## Purpose

This document defines the canonical work management model for this repository. It establishes GitHub Issues and GitHub Projects as the sole source of truth for work item tracking, prioritization, and state management.

## Scope

This specification governs:

- Work item creation, lifecycle, and state transitions
- Issue classification and metadata requirements
- Project board configuration and usage
- Cross-reference requirements between issues and pull requests
- Integration points with external systems (dormant by default)

## Non-goals

This specification does NOT:

- Define implementation details of slash commands or CLI tooling
- Prescribe GitHub Actions workflows or automation behavior
- Specify bot configurations or webhook integrations
- Define MCP server implementations or protocol extensions
- Replace existing git commit or PR conventions

## Explicit Constraints

1. **Canonical Source**: GitHub Issues SHALL be the canonical source of truth for all work items
2. **No Dual Authority**: Work items SHALL NOT be simultaneously managed in multiple systems (GitHub + Jira/Linear)
3. **Attachment-First**: All work management tooling MUST attach to GitHub's native primitives (issues, projects, labels, milestones)
4. **Fail-Closed**: When integration state is ambiguous, tooling SHALL refuse operation rather than guess
5. **Dormancy by Default**: Jira and Linear integrations MUST remain present but inactive unless explicitly activated
6. **No Runtime Changes**: This specification defines data models only; runtime behavior is out of scope

## Definitions

- **Work Item**: A GitHub Issue representing a unit of work (feature, bug, task, spike)
- **Canonical Source**: The single authoritative system for a given data type
- **Dormant Integration**: An integration that exists in configuration but does not actively sync or modify data
- **Fail-Closed**: Operational posture where ambiguity or error results in no-op rather than potentially incorrect action
- **Attachment-First**: Design principle requiring tooling to use native platform primitives rather than abstraction layers

## Canonical Model

### Work Item Lifecycle

1. All work items MUST be represented as GitHub Issues
2. All work items MUST be linked to exactly one GitHub Project
3. Work items MUST use standardized labels for classification (see ISSUE_SCHEMA.md)
4. Work items MUST transition through defined states using Project status fields
5. Work items SHALL NOT exist solely in external systems (Jira, Linear, Notion, etc.)

### Issue Creation Requirements

1. New issues MUST include:
   - Descriptive title following conventional format
   - Body text explaining context and acceptance criteria
   - At least one classification label (type, priority, or area)
   - Assignment to a GitHub Project

2. Issues SHOULD include:
   - Links to related issues or PRs
   - Milestone assignment for release tracking
   - Assignee when work begins

3. Issues MUST NOT:
   - Contain secrets or credentials
   - Reference work items only by external system IDs (e.g., "JIRA-123") without GitHub issue link

### Project Board Requirements

1. Each repository MUST have at least one active GitHub Project
2. Projects MUST define a status field with minimum states:
   - Backlog
   - Ready
   - In Progress
   - In Review
   - Done

3. Projects MAY define additional custom fields for:
   - Priority (P0, P1, P2, P3)
   - Size estimation (XS, S, M, L, XL)
   - Sprint/iteration assignment
   - Team ownership

4. Projects SHALL NOT:
   - Mirror data from external systems automatically
   - Create issues without human approval
   - Modify issue content based on external triggers

### State Transitions

1. Issues MUST move through states explicitly via:
   - Manual Project board updates
   - PR-linked automation (when issue references exist)
   - Authorized bot commands (future, out of scope for Phase B)

2. State transitions SHALL NOT occur:
   - Based on time elapsed
   - Based on external system state changes
   - Without audit trail in GitHub timeline

### Cross-System References

1. Issues MAY reference external work items (Jira tickets, Linear issues) in body text
2. External references MUST be informational only, not bidirectionally synced
3. GitHub issue number SHALL be the canonical identifier for all internal tooling
4. External system IDs MUST NOT be used as primary keys in local tooling

## Failure Modes

### Ambiguous Authority

**Scenario**: Work item exists in both GitHub and external system with conflicting state.

**Failure Mode**: If sync mechanism cannot determine authority, it MUST NOT modify either system. Tooling SHALL log error and require manual resolution.

### Missing Required Metadata

**Scenario**: Issue lacks required labels or project assignment.

**Failure Mode**: Tooling that requires metadata MUST refuse operation and emit clear error message identifying missing fields. No default values SHALL be assumed.

### Orphaned External References

**Scenario**: Issue body references external ticket that no longer exists or is inaccessible.

**Failure Mode**: Reference SHOULD be treated as informational link. Tooling MUST NOT fail if external reference is broken, but SHOULD warn user during validation.

### Project Misconfiguration

**Scenario**: Project lacks required status field or uses non-standard state names.

**Failure Mode**: Tooling expecting standard schema MUST emit error identifying exact mismatch. Operation SHALL NOT proceed with guessed field mappings.

### Concurrent Modification

**Scenario**: Issue state updated in GitHub while external operation in progress.

**Failure Mode**: Operation MUST be rejected with optimistic locking error. User SHALL be prompted to retry after reviewing current state.

## Out-of-Scope Items

The following are explicitly PROHIBITED in this specification:

1. **Automatic Bidirectional Sync**: No automatic mirroring between GitHub and Jira/Linear
2. **Ghost Issues**: No creation of GitHub issues by bots without human-in-loop
3. **State Inference**: No guessing of issue state from commit messages or PR titles alone
4. **Schema Extension via Labels**: Labels are for classification, not arbitrary key-value storage
5. **Workflow Automation**: No GitHub Actions that modify issue content or state based on schedules or external events
6. **Cross-Repo Work Items**: Issues belong to one repository; no virtual work items spanning repos

## Change Control

### Amendment Process

1. Changes to this specification MUST be proposed via pull request
2. PRs modifying this document MUST include:
   - Rationale section explaining necessity of change
   - Impact analysis on dependent specifications
   - Migration plan if breaking changes introduced

3. Approval requirements:
   - At least one review from repository maintainer
   - No unresolved comments from stakeholders
   - Passing validation that dependent docs remain consistent

### Versioning

This document follows semantic versioning:
- **Major**: Breaking changes to canonical model (e.g., removing required fields)
- **Minor**: Additive changes (e.g., new optional fields)
- **Patch**: Clarifications and typo fixes

Current version: `1.0.0` (Phase B baseline)

### Conflict Resolution

If this specification conflicts with other canonical documents:
1. PROJECTS_GOVERNANCE.md takes precedence for process questions
2. ISSUE_SCHEMA.md takes precedence for data model questions
3. JIRA_LINEAR_DORMANCY_POLICY.md takes precedence for integration questions

---

**Document Status**: Canonical (Phase B)
**Last Updated**: 2026-01-09
**Maintained By**: Repository Governance Team
