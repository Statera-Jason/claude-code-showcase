# Issue Schema Specification

## Purpose

This document defines the canonical data model for GitHub Issues used as work items. It establishes required and optional metadata, label taxonomy, and field validation rules.

## Scope

This specification governs:

- Issue title and body formatting requirements
- Label taxonomy and classification system
- Custom field definitions for GitHub Projects
- Metadata validation rules
- Template structure for issue creation

## Non-goals

This specification does NOT:

- Define automation rules for label application
- Prescribe GitHub Actions for issue triage
- Specify bot behavior for issue validation
- Define PR templates or commit message formats
- Replace GitHub's native issue form schema

## Explicit Constraints

1. **Label Immutability**: Label names and colors SHALL NOT change once work items reference them
2. **Required Metadata**: Issues MUST have minimum viable metadata before entering "Ready" state
3. **No Computed Fields**: Issue fields MUST be explicitly set; no derivation from other data sources
4. **Schema Versioning**: Breaking changes to schema require major version bump and migration plan
5. **Validation at Edges**: Schema validation MUST occur at creation/update time, not during reads
6. **No Overlapping Semantics**: Each label or field MUST have one clear purpose; no ambiguous overlap

## Definitions

- **Label Taxonomy**: Hierarchical classification system using label prefixes (e.g., `type:`, `priority:`)
- **Required Metadata**: Fields that MUST be present for issue to be actionable
- **Custom Field**: GitHub Projects field added beyond native issue properties
- **Template Variable**: Placeholder in issue template that MUST be replaced during creation
- **Schema Validation**: Automated check that issue conforms to this specification

## Canonical Model

### Issue Title Requirements

1. Titles MUST be concise (≤ 100 characters recommended)
2. Titles MUST use imperative mood for features ("Add dark mode toggle")
3. Titles MUST use descriptive phrasing for bugs ("Login fails with OAuth redirect")
4. Titles SHALL NOT include issue number or external system IDs
5. Titles SHALL NOT use ALL CAPS except for acronyms

### Issue Body Structure

1. Issue body MUST include:
   ```markdown
   ## Problem Statement
   [Clear description of need or bug]

   ## Acceptance Criteria
   - [ ] Criterion 1
   - [ ] Criterion 2

   ## Additional Context
   [Optional: screenshots, logs, related links]
   ```

2. Issue body SHOULD include:
   - Links to related issues using `#123` syntax
   - Links to external references (Jira, Linear) if applicable
   - Technical constraints or dependencies
   - Open questions requiring clarification

3. Issue body MUST NOT:
   - Contain secrets, credentials, or API keys
   - Use ambiguous pronouns without clear antecedents
   - Include TODO comments intended for code

### Label Taxonomy

#### Type Labels (REQUIRED - exactly one)

- `type:feature` - New functionality or capability
- `type:bug` - Defect in existing functionality
- `type:docs` - Documentation changes only
- `type:refactor` - Code improvement without behavior change
- `type:test` - Test coverage additions or fixes
- `type:chore` - Maintenance tasks (deps, tooling, config)
- `type:spike` - Time-boxed research or exploration

#### Priority Labels (REQUIRED for Ready state)

- `priority:critical` - Blocks release or causes data loss
- `priority:high` - Significant impact on user experience
- `priority:medium` - Important but not blocking
- `priority:low` - Nice to have, no urgency

#### Area Labels (OPTIONAL - multiple allowed)

- `area:api` - Backend API changes
- `area:ui` - User interface components
- `area:auth` - Authentication and authorization
- `area:data` - Database schema or migrations
- `area:infra` - Infrastructure or deployment
- `area:docs` - Documentation and examples

#### State Labels (OPTIONAL - managed by automation)

- `status:blocked` - Cannot proceed due to external dependency
- `status:needs-info` - Requires additional information from author
- `status:duplicate` - Duplicate of another issue (link required)
- `status:wontfix` - Valid issue but won't be addressed

#### Special Labels (OPTIONAL)

- `good-first-issue` - Suitable for new contributors
- `help-wanted` - Maintainers seek external contributions
- `breaking-change` - Requires major version bump
- `security` - Security vulnerability or concern

### Custom Project Fields

Projects SHOULD define these custom fields:

1. **Status** (Single Select - REQUIRED)
   - Values: `Backlog`, `Ready`, `In Progress`, `In Review`, `Done`, `Cancelled`
   - Default: `Backlog`

2. **Priority** (Single Select)
   - Values: `P0`, `P1`, `P2`, `P3`
   - Mapping: `P0` = critical, `P1` = high, `P2` = medium, `P3` = low
   - Default: `P3`

3. **Size** (Single Select)
   - Values: `XS`, `S`, `M`, `L`, `XL`
   - Used for estimation, not time tracking
   - Default: None (estimated during refinement)

4. **Sprint** (Iteration Field)
   - Links to iteration dates if project uses sprints
   - Optional; not all projects use sprint cadence

5. **Team** (Single Select)
   - Values: Team names from repository org
   - Default: Repository default team

### Metadata Validation Rules

#### Creation Validation

At issue creation, validation MUST check:

1. Title is non-empty and ≤ 150 characters
2. Body matches template structure or contains minimum sections
3. At least one `type:` label is present
4. Issue is added to at least one project

If validation fails, issue SHOULD be labeled `status:needs-info` and assigned back to creator.

#### Ready State Validation

Before moving to "Ready" state, validation MUST check:

1. Has exactly one `type:` label
2. Has exactly one `priority:` label
3. Has well-formed acceptance criteria (checkboxes)
4. Has project status field set
5. No `status:blocked` or `status:needs-info` labels present

If validation fails, transition to "Ready" MUST be rejected with specific error.

#### Completion Validation

Before moving to "Done" state, validation MUST check:

1. All acceptance criteria checkboxes are marked complete
2. At least one PR references this issue (see ISSUE_TO_PR_TRACEABILITY.md)
3. No unresolved blocking comments
4. Issue is not labeled `status:blocked`

If validation fails, transition to "Done" MUST be rejected.

### Template Structure

Repositories MUST provide issue templates for each type:

1. **Feature Request Template**
   ```yaml
   name: Feature Request
   description: Propose new functionality
   labels: ["type:feature"]
   body:
     - type: textarea
       id: problem
       label: Problem Statement
       required: true
     - type: textarea
       id: solution
       label: Proposed Solution
       required: true
     - type: textarea
       id: acceptance
       label: Acceptance Criteria
       required: true
   ```

2. **Bug Report Template**
   ```yaml
   name: Bug Report
   description: Report a defect
   labels: ["type:bug", "priority:high"]
   body:
     - type: textarea
       id: current
       label: Current Behavior
       required: true
     - type: textarea
       id: expected
       label: Expected Behavior
       required: true
     - type: textarea
       id: steps
       label: Steps to Reproduce
       required: true
     - type: textarea
       id: context
       label: Environment
       required: false
   ```

3. **Blank Template** (for maintainers)

Templates MUST use GitHub's issue form schema (YAML), not legacy markdown templates.

## Failure Modes

### Missing Required Labels

**Scenario**: Issue moved to "Ready" state without `priority:` label.

**Failure Mode**: Project automation MUST revert issue to "Backlog" state and add comment identifying missing label. No silent default SHALL be applied.

### Malformed Acceptance Criteria

**Scenario**: Acceptance criteria section exists but contains no checkboxes.

**Failure Mode**: Validation SHOULD warn but not block issue creation. Transition to "Ready" MUST be blocked until checkboxes exist.

### Label Conflicts

**Scenario**: Issue has multiple conflicting `type:` labels (e.g., both `type:feature` and `type:bug`).

**Failure Mode**: Validation MUST block state transitions and require manual resolution. No automatic label removal SHALL occur.

### Template Bypass

**Scenario**: Issue created without using template (e.g., via API or email).

**Failure Mode**: Issue SHOULD be labeled `status:needs-info` automatically. Bot SHOULD comment with link to proper template. Manual review REQUIRED before "Ready" state.

### Schema Migration

**Scenario**: Schema updated with new required field; existing issues lack this field.

**Failure Mode**: Existing issues MUST be grandfathered under old schema version. Only new issues MUST comply with new schema. Bulk backfill SHALL NOT be automatic.

## Out-of-Scope Items

The following are explicitly PROHIBITED in this specification:

1. **Auto-Generated Issues**: No bot creation of issues without human trigger
2. **AI-Generated Content**: No automatic rewriting of issue descriptions or acceptance criteria
3. **Cross-Repo Labels**: Labels belong to one repository; no shared label namespaces
4. **Computed Priority**: Priority MUST be explicitly set; no inference from labels or content
5. **Time Tracking**: Issue schema does not include time estimates or actuals (use external tools if needed)
6. **Hierarchical Issues**: No epic/story/task hierarchy; use relationships via links instead

## Change Control

### Amendment Process

1. Changes to label taxonomy MUST follow:
   - Propose new labels via PR to this document
   - Update `.github/labels.yml` if using label-sync
   - Migration plan for relabeling existing issues (if breaking)

2. Changes to required fields MUST include:
   - Rationale for why field is necessary
   - Impact analysis on existing issues
   - Grandfathering strategy for old issues

3. Approval requirements:
   - Review from repository maintainer
   - Sign-off from at least one team lead
   - Documentation of migration steps

### Schema Versioning

Schema follows semantic versioning in document header:

- **Major**: New required field or removed label
- **Minor**: New optional field or label
- **Patch**: Clarification of existing rules

Current version: `1.0.0` (Phase B baseline)

### Deprecation Policy

To deprecate a label:

1. Mark as deprecated in this document
2. Stop using in new issues (remove from templates)
3. Wait one release cycle (minimum 2 weeks)
4. Bulk update existing issues to new label
5. Delete deprecated label from repository

Deprecated labels MUST NOT be deleted while issues still reference them.

---

**Document Status**: Canonical (Phase B)
**Last Updated**: 2026-01-09
**Maintained By**: Repository Governance Team
