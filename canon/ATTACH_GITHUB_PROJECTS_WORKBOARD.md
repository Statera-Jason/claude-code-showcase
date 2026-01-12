# GitHub Projects Workboard Attachment

## Document Status

- **Authority**: Governance attachment (adopt-by-reference)
- **Phase**: D.1 Attachment (StateraGroup MCP)
- **Status**: Canonical
- **Last Updated**: 2026-01-10

## Purpose

This document formally attaches the GitHub Projects v2 workboard configuration canon to Project420 by reference. It establishes that if Project420 chooses to instantiate a GitHub Projects board in the future, it MUST comply exactly with the externally-ratified canonical configuration.

## Scope

This attachment governs:

- Adoption-by-reference of external Projects configuration canon
- Compliance requirements if Projects board is created
- Minimal operational linkage conventions (documentation only)
- Prohibited automation and tooling
- Change control for this attachment

## Non-Scope

This attachment does NOT:

- Create a GitHub Projects board (board instantiation is deferred)
- Define automation or workflows (explicitly prohibited)
- Implement GitHub Actions
- Configure bots or sync scripts
- Replace GitHub Issues as source of truth
- Modify the external canon (which is authoritative and immutable)

---

## Source of Truth: GitHub Issues + PRs

### Canonical Data Sources

The authoritative sources for work tracking are:

1. **GitHub Issues**: Primary work items, requirements, bugs, enhancements
2. **GitHub Pull Requests**: Code changes, reviews, implementation evidence
3. **Git Commits**: Atomic change history with traceability

**GitHub Projects boards are visibility surfaces only.** They provide views and aggregations but are NOT the source of truth.

### Implications

- Projects boards MAY be created, modified, or deleted without data loss
- Issues remain discoverable and traceable with or without Projects board
- Issue metadata (labels, assignees, milestones) is defined in Issues, not Projects
- Projects custom fields (Status, Priority, Size) supplement but do not replace Issue fields

---

## Adopt-By-Reference: External Canon

### Authoritative Configuration

Project420 **adopts by reference** the canonical Projects board configuration defined in:

**`docs/canon/PROJECTS_BOARD_CONFIGURATION.md`**

This document originated from the Claude Foundation / StateraGroup MCP program (Phase D.1) and is the **mandatory configuration contract** for any GitHub Projects v2 board used by Project420.

### Binding Requirements

If Project420 creates a GitHub Projects v2 board in the future:

1. **MUST** name the board: `Claude Foundation — Work Board`
2. **MUST** configure fields exactly as specified in the canon:
   - Status (Single select): `Backlog`, `Ready`, `In Progress`, `Blocked`, `In Review`, `Done`, `Cancelled`
   - Priority (Single select): `P0`, `P1`, `P2`, `P3`
   - Size (Single select): `XS`, `S`, `M`, `L`, `XL`
   - Plus GitHub native fields: Assignees, Labels, Milestone
3. **MUST** create exactly six views as defined in canon:
   - All Issues (default)
   - Backlog
   - Active Work
   - My Work
   - Blocked Items
   - Completed
4. **MUST** configure views with exact filters, grouping, and sorting per canon
5. **MUST NOT** enable automation, workflows, or GitHub Actions integration
6. **MUST** perform all status transitions and field updates manually
7. **MUST** follow audit and compliance procedures defined in canon (quarterly audits)

### Compliance Rule

**If any canon requirement cannot be satisfied due to GitHub UI/API limitations:**

- **DO NOT** approximate or substitute
- **DO NOT** modify the canon to match tooling constraints
- **MUST** record the limitation as **BLOCKED**
- **MUST** document the exact discrepancy
- **MUST** propose manual remediation or defer board instantiation

**The canon does not bend to tooling.** Tooling bends to canon, or the requirement is deferred.

---

## Board Instantiation: Deferred and Optional

### Current Status

**No GitHub Projects board exists for Project420 at this time.**

Board creation is **deferred** to avoid premature coupling between governance documentation and operational tooling.

### Future Instantiation

A Projects board MAY be created when:

1. **Explicit Request**: User or maintainer explicitly requests board creation
2. **Capacity Exists**: Time and resources available for manual configuration
3. **Canon Verified**: External canon is reviewed and confirmed current
4. **Compliance Possible**: All canon requirements can be satisfied exactly
5. **Approval Obtained**: Repository maintainer approves board instantiation

### Instantiation Process

If board creation is requested:

1. **Review Canon**: Read `docs/canon/PROJECTS_BOARD_CONFIGURATION.md` in full
2. **Verify Compliance**: Confirm GitHub Projects v2 supports all requirements
3. **Manual Configuration**: Use GitHub web UI (NO CLI/API automation)
4. **Field-by-Field Setup**: Create fields with exact names, types, values, defaults
5. **View-by-View Setup**: Create six views with exact filters and grouping
6. **Disable Automation**: Verify no workflows or automation rules are enabled
7. **Document Blockers**: Record any requirements that cannot be met
8. **Validation**: Use checklist in canon (lines 568-604) to verify compliance
9. **Announce**: Communicate board availability to team

---

## Operational Linkage Conventions (Documentation Only)

These conventions are **documented** but **not enforced**. They represent best practices for work tracking when a Projects board exists.

### Issue Origination

- **Project items originate from GitHub Issues**, not Pull Requests
- PRs are linked to Issues but are not primary project items
- Project board shows Issues; PRs are visible via Issue links

**Rationale**: Issues represent work to be done. PRs represent implementation. The project board tracks work, not implementation details.

### Pull Request to Issue Linking

- **Every PR MUST reference at least one Issue** (convention, not enforcement)
- PR title or description SHOULD include: `Closes #123` or `Fixes #456`
- GitHub automatically links PRs to Issues when keywords are used

**Rationale**: Ensures traceability from code changes back to requirements/bugs.

### Branch Naming Convention

- **Branch names SHOULD reference phase + topic** when applicable
- Examples:
  - `claude/stateragroup-phase-d1-HXLW4` (StateraGroup MCP work)
  - `feature/user-auth-implementation`
  - `bugfix/api-timeout-issue`
- **No enforcement**: This is guidance, not a blocking requirement

**Rationale**: Contextual branch names improve git history readability.

### Issue-to-Branch Traceability

- Branch names MAY include issue number: `feature/123-add-user-auth`
- Commit messages SHOULD reference issues: `fix: resolve API timeout (#456)`
- First commit on feature branch SHOULD reference issue

**Rationale**: Bidirectional traceability between issues, branches, commits, and PRs.

---

## Prohibitions (Explicit and Binding)

The following are **explicitly prohibited** for Project420 GitHub Projects usage:

### 1. No Workflows

- **MUST NOT** enable GitHub Projects built-in workflows
- **MUST NOT** configure "auto-add to project" automation
- **MUST NOT** enable "auto-archive" features
- **MUST NOT** use "item closed" or "PR merged" triggers

**Enforcement**: Manual verification during quarterly audits

### 2. No Automation Rules

- **MUST NOT** create custom automation rules via Projects UI
- **MUST NOT** use GitHub Actions to sync Projects state
- **MUST NOT** run background scripts that update Projects fields
- **MUST NOT** use bots (Dependabot, etc.) to modify Projects board

**Enforcement**: Repository maintainer review; disable if discovered

### 3. No Bots

- **MUST NOT** grant bot accounts write access to Projects board
- **MUST NOT** allow bots to update Status, Priority, or Size fields
- **MUST NOT** use bots to auto-assign Issues or transition states

**Exception**: Bots MAY add labels to Issues (native GitHub feature), but MUST NOT modify Projects custom fields.

### 4. No Sync Scripts

- **MUST NOT** create scripts that sync Projects with Jira, Linear, or other systems
- **MUST NOT** run scheduled jobs that update Projects fields
- **MUST NOT** use APIs to bulk-update Projects state

**Rationale**: Violates manual-first posture; introduces runtime behavior prohibited in Phase D.1.

### 5. No Background Processes

- **MUST NOT** run daemons or services that monitor Projects board
- **MUST NOT** use webhooks to trigger external actions based on Projects state
- **MUST NOT** configure real-time sync with external systems

**Rationale**: Projects board is a visibility surface, not an event bus.

---

## Change Control

### Amendments to This Attachment

Changes to this attachment document MUST follow this process:

1. **Propose Change**:
   - Open GitHub Issue titled: `[Governance] Propose: [description]`
   - Include: rationale, impact on existing Projects board (if any), migration plan
   - Tag: `governance`, `projects`

2. **Review**:
   - **MUST** have repository maintainer review
   - **SHOULD** have at least one contributor sign-off
   - **MUST** resolve all blocking concerns

3. **Approval**:
   - Repository maintainer MUST approve
   - No unresolved objections from active contributors

4. **Implementation**:
   - Update this document via Pull Request
   - Update Projects board configuration (if board exists)
   - Communicate changes to all team members

5. **Verification**:
   - Confirm changes do not violate external canon
   - Verify no automation introduced
   - Update validation checklist if needed

### Amendments to External Canon

**This attachment document CANNOT modify the external canon.**

The canon defined in `docs/canon/PROJECTS_BOARD_CONFIGURATION.md` is **authoritative and immutable** from Project420's perspective.

If Project420 requires deviations from canon:

1. **DO NOT** modify canon
2. **DO NOT** create alternate configuration
3. **MUST** document deviation as **NON-COMPLIANT**
4. **MUST** propose upstream change to canon stewards (StateraGroup MCP)
5. **MUST** wait for canon amendment before implementing deviation

**Rationale**: Canon is shared across multiple projects. Local deviations fragment governance.

### Emergency Changes

For critical fixes (e.g., Projects board blocking work):

1. Maintainer MAY make immediate change to Projects board configuration
2. **MUST** document change in GitHub Issue within 24 hours
3. **MUST** submit retroactive amendment PR to this document
4. **MUST** verify change does not violate canon prohibitions (no automation)

---

## Validation and Compliance

### Pre-Instantiation Checklist

Before creating Projects board, verify:

- [ ] External canon reviewed and understood
- [ ] All canon field requirements can be satisfied in GitHub UI
- [ ] All canon view requirements can be configured exactly
- [ ] No GitHub Projects automation will be enabled
- [ ] Team trained on manual-only operations
- [ ] Quarterly audit process defined

### Post-Instantiation Validation

After creating Projects board, validate:

- [ ] Board name is exactly: `Claude Foundation — Work Board`
- [ ] All fields match canon exactly (names, types, values, defaults)
- [ ] All six views exist with correct filters, grouping, sorting
- [ ] No automation workflows enabled (verified in Projects Settings)
- [ ] No GitHub Actions syncing with Projects (verified in `.github/workflows/`)
- [ ] Permissions correctly set (admins only for maintainers)
- [ ] Board linked from repository README

### Quarterly Audit

If Projects board exists, conduct quarterly audit:

- [ ] Verify no automation rules added since last audit
- [ ] Confirm field schema matches canon (no unauthorized fields)
- [ ] Review stale blocked issues (>60 days)
- [ ] Verify desynchronized Status/Issue state fixed
- [ ] Confirm access permissions still correct
- [ ] Document audit results in GitHub Issue

---

## Relationship to Other Canon Documents

This attachment integrates with:

### 1. PROJECTS_BOARD_CONFIGURATION.md (External Canon)

- **Relationship**: This document adopts that canon by reference
- **Authority**: External canon is authoritative; this document is binding adoption
- **Conflict Resolution**: External canon wins; this document cannot override

### 2. PROJECTS_GOVERNANCE.md

- **Relationship**: Governance policy for Projects ownership, permissions, change control
- **Authority**: Governance defines process; configuration canon defines structure
- **Conflict Resolution**: Governance for "who/how", configuration for "what"

### 3. ISSUE_SCHEMA.md

- **Relationship**: Defines Issue structure; Projects board visualizes Issues
- **Authority**: Issues are source of truth; Projects are views
- **Conflict Resolution**: Issue schema wins; Projects fields supplement but don't replace

### 4. ISSUE_TO_PR_TRACEABILITY.md

- **Relationship**: Defines PR-to-Issue linking requirements
- **Authority**: Both apply; Projects board shows Issues, PRs linked via Issues
- **Conflict Resolution**: Traceability requirements apply regardless of Projects board existence

### 5. FAIL_CLOSED_AUTOMATION_POLICY.md

- **Relationship**: Prohibits automation without explicit approval
- **Authority**: Both apply; Projects automation explicitly prohibited in Phase D.1
- **Conflict Resolution**: Fail-closed applies; when in doubt, disable automation

---

## Acceptance Criteria

This attachment is complete when:

1. ✓ External canon identified and referenced
2. ✓ Adoption-by-reference binding requirements stated
3. ✓ Board instantiation explicitly deferred
4. ✓ Operational linkage conventions documented (not enforced)
5. ✓ Prohibitions explicitly listed (workflows, automation, bots, sync, background processes)
6. ✓ Change control process defined
7. ✓ Validation and compliance procedures specified
8. ✓ Relationship to other canon documents clarified

---

## Failure Modes

### Unauthorized Board Creation

**Scenario**: Someone creates Projects board without following this attachment.

**Detection**: Repository maintainer notices board exists.

**Remediation**:
1. Evaluate if board complies with canon
2. If compliant: retroactively approve and document
3. If non-compliant: delete board and recreate following attachment process
4. Communicate attachment requirements to team

### Automation Silently Enabled

**Scenario**: GitHub enables default workflows without user action.

**Detection**: Quarterly audit discovers workflows in Projects Settings.

**Remediation**:
1. Immediately disable all workflows
2. Document what automation occurred
3. Verify no data corruption from automated transitions
4. Add explicit workflow check to audit checklist

### Canon Drift

**Scenario**: External canon updated but attachment not reviewed.

**Detection**: Periodic review reveals canon version mismatch.

**Remediation**:
1. Review canon changes
2. Assess impact on Project420
3. Update attachment if needed (via change control)
4. Communicate canon updates to team

### Unauthorized Deviations

**Scenario**: Projects board configured differently from canon without approval.

**Detection**: Validation checklist identifies mismatches.

**Remediation**:
1. Document exact deviations
2. If deviations valid: initiate canon amendment process
3. If deviations invalid: reconfigure board to match canon
4. Educate team on compliance requirements

---

## Phase D.1 Boundaries (Recorded)

This attachment is created during **Phase D.1: GitHub Visual Work Tracking** of the StateraGroup MCP / Claude Foundation program.

### Phase D.1 Constraints

Phase D.1 explicitly **prohibits**:

- ❌ Automation (workflows, rules, triggers)
- ❌ GitHub Actions integration
- ❌ Bots or background processes
- ❌ API/CLI-based sync scripts
- ❌ Runtime behavior or enforcement logic

Phase D.1 explicitly **permits**:

- ✓ Documentation (this attachment)
- ✓ Canonical configuration specification (external canon)
- ✓ Governance policy definition
- ✓ Manual operational guidance
- ✓ Validation and audit procedures

### Post-Phase D.1

Future phases (Phase D.2+, if defined) MAY:

- Revisit automation prohibitions (with explicit approval)
- Introduce enforcement mechanisms (with fail-closed safety)
- Enable selective workflows (with governance oversight)

**Until then, Phase D.1 constraints remain in force.**

---

## Summary

1. **Source of Truth**: GitHub Issues + PRs (Projects are views)
2. **Canon**: `docs/canon/PROJECTS_BOARD_CONFIGURATION.md` adopted by reference
3. **Board Status**: Deferred (not created yet)
4. **Compliance**: If board created, MUST match canon exactly
5. **Prohibitions**: No automation, workflows, bots, sync, or background processes
6. **Operations**: Manual only
7. **Change Control**: Amendments require maintainer approval
8. **Validation**: Quarterly audits if board exists

**This attachment establishes governance without creating operational burden.**

Projects board is optional. If created, it is tightly governed. If not created, Project420 continues using GitHub Issues + PRs as always.

---

**Document Authority**: Binding attachment (adopt-by-reference)
**External Canon**: `docs/canon/PROJECTS_BOARD_CONFIGURATION.md` (authoritative)
**Phase**: D.1 (GitHub Visual Work Tracking)
**Automation**: Explicitly prohibited
**Board Status**: Not instantiated (deferred)
**Maintained By**: Repository Maintainers
