# Issue to PR Traceability Specification

## Purpose

This document defines the canonical requirements for linking GitHub Issues to Pull Requests. It establishes bidirectional traceability rules to ensure all code changes are associated with documented work items.

## Scope

This specification governs:

- Required references from PRs to issues
- Validation of issue-PR linkage before merge
- Automatic state transitions triggered by PR events
- Audit requirements for change traceability
- Branch naming conventions that encode issue references

## Non-goals

This specification does NOT:

- Define git commit message format (see git conventions in CLAUDE.md)
- Prescribe GitHub Actions implementation for validation
- Specify bot behavior for auto-linking
- Define PR review requirements or approval workflows
- Replace existing code review standards

## Explicit Constraints

1. **No Unlinked PRs**: Pull requests MUST reference at least one issue before merge
2. **Explicit References Only**: Automatic linking via branch names is insufficient; PR body MUST contain issue reference
3. **No Retroactive Linking**: Issue-PR links MUST be established before PR merge, not after
4. **Fail-Closed Merge**: If traceability validation fails, PR merge MUST be blocked
5. **Bidirectional Visibility**: Both issue and PR timelines MUST show the linkage
6. **No Ghost Work**: Issues marked "Done" MUST have at least one merged PR

## Definitions

- **Issue Reference**: GitHub syntax linking to an issue (`#123`, `owner/repo#123`, full URL)
- **Closing Keyword**: GitHub keyword that auto-closes issue on PR merge (`fixes`, `closes`, `resolves`)
- **Traceability**: Ability to navigate from code change to originating work item and vice versa
- **Merge Block**: Automated prevention of PR merge due to policy violation
- **Orphan PR**: Pull request with no issue reference
- **Orphan Issue**: Issue marked "Done" with no merged PRs

## Canonical Model

### PR to Issue References

#### Required References

1. PR body MUST include at least one issue reference using one of these formats:
   ```markdown
   Fixes #123
   Closes #123
   Resolves #123

   Addresses #123 (for partial work)
   Related to #123 (for supporting changes)
   ```

2. Reference MUST appear in PR body, not just PR title or commit messages

3. Multiple issues MAY be referenced if PR addresses multiple work items:
   ```markdown
   Fixes #123
   Related to #124, #125
   ```

#### Reference Types

1. **Closing References** (automatically close issue on merge):
   - `Fixes #123`
   - `Closes #123`
   - `Resolves #123`
   - Use when PR fully implements issue

2. **Non-Closing References** (link but don't auto-close):
   - `Addresses #123`
   - `Related to #123`
   - `Partial work for #123`
   - Use when PR is part of larger effort

3. **Blocking References** (indicate dependencies):
   - `Blocked by #123`
   - `Depends on #124`
   - Use when PR cannot merge until other issue resolved

#### Reference Validation

1. Issue number MUST exist in repository (or specified external repo)
2. Referenced issue MUST NOT be closed already (for closing keywords)
3. Referenced issue SHOULD be in "In Progress" or "Ready" state
4. Referenced issue MUST NOT be labeled `status:wontfix` or `status:duplicate`

### Branch Naming Conventions

1. Branches SHOULD encode issue number for developer convenience:
   ```
   123-add-dark-mode
   fix/123-login-oauth
   feature/123-user-dashboard
   ```

2. Branch name alone is INSUFFICIENT for traceability; PR body MUST still include reference

3. Branch name format is RECOMMENDED but not REQUIRED:
   ```
   <issue-number>-<description>
   <type>/<issue-number>-<description>
   ```

### State Transitions (GitHub Native Behavior)

**Phase B Note**: The behaviors below describe GitHub's native issue-closing functionality (e.g., `Fixes #123` auto-closes on merge). Phase B does NOT implement custom validation or blocking. Phase C+ MAY add enforcement; see failure modes for governance posture.

#### On PR Creation

1. When PR created with `Fixes #123` reference:
   - GitHub natively links PR to issue in timeline
   - Project board automation (if configured by user) MAY update status
   - Issue assignee updates are manual in Phase B

2. If issue not in "In Progress" state:
   - Phase B: Manual review during PR review
   - Phase C+: Validation MAY warn (not blocking)

#### On PR Merge

1. When PR merged with closing keyword (`Fixes`, `Closes`, `Resolves`):
   - **GitHub Native**: Issue automatically closes (built-in GitHub behavior)
   - Issue closed_at timestamp set by GitHub
   - Issue timeline shows merge event

2. If issue has unchecked acceptance criteria:
   - Phase B: Manual verification during PR review
   - Phase C+: Validation MAY block auto-close

3. If issue labeled `status:blocked`:
   - Phase B: Manual check; do not merge until label removed
   - Phase C+: Validation MAY block merge

#### On PR Close (without merge)

1. When PR closed without merge:
   - Issue state changes are manual in Phase B
   - Project automation (if user-configured) MAY revert status
   - No automatic label changes

### Validation Rules (Phase C+ Enforcement; Phase B Manual)

#### Pre-Merge Validation

**Phase B**: The checks below are governance policy. Teams MUST perform manually during PR review. No automated enforcement exists in Phase B.

**Phase C+**: MAY implement automated validation. If implemented, validation SHOULD check:

1. **Issue Reference Exists**: At least one valid issue reference in PR body
2. **Issue Not Closed**: Referenced issues are not already closed (for closing keywords)
3. **Issue Not Won't-Fix**: Referenced issues not labeled `status:wontfix`
4. **Acceptance Criteria Met**: If using closing keyword, issue acceptance criteria all checked
5. **No Conflicts**: Referenced issue not in conflict state

**Phase B Governance**: If any check fails, reviewer MUST reject PR. No automated merge blocking exists.

#### Post-Merge Validation

**Phase B**: Manual verification during retrospectives or issue review.

**Phase C+**: Automated post-merge validation MAY verify:

1. Issues with closing keywords successfully closed
2. Issue state correctly updated to "Done"
3. Issue timeline shows merge event
4. No orphan PRs remain unlinked

**Phase B**: If issues found, manual correction required. No automated alerts in Phase B.

#### Periodic Audit

**Phase B**: Manual weekly/monthly audit by project owner. Check for:

1. **Orphan PRs**: Merged PRs with no issue reference
   - Action: Manually add `missing-issue` label, notify PR author

2. **Orphan Issues**: Issues in "Done" state with no merged PR
   - Action: Manually add `needs-verification` label, request evidence

3. **Stale In-Review**: Issues in "In Review" state with closed (unmerged) PR
   - Action: Manually revert to "Ready" state

**Phase C+**: Audit MAY be automated.

### Exception Handling

#### Exempt PR Types

The following PR types MAY be exempt from issue reference requirement:

1. **Hotfixes**: Emergency production fixes (MUST be labeled `hotfix`)
2. **Dependency Updates**: Automated dependency PRs from Dependabot
3. **Reverts**: PRs that revert previous commits (MUST reference original PR)
4. **Documentation Typos**: Single-file doc fixes < 10 lines

Exempt PRs MUST:
- Have explicit exemption label (`no-issue-required`)
- Include rationale in PR body for why issue not needed
- Be approved by maintainer

#### Retroactive Linking

If PR merged without issue reference (due to validation bypass):

1. Create tracking issue documenting the change
2. Add comment to PR with issue link
3. Update issue with PR link in body
4. Flag for audit review

Retroactive linking does NOT satisfy traceability requirement but provides audit trail.

## Failure Modes

**Phase B Note**: Failure modes below describe governance posture and fail-closed doctrine. Phase B relies on manual review. Phase C+ MAY implement automated enforcement.

### Missing Issue Reference

**Scenario**: PR submitted without any issue reference in body.

**Failure Mode** (Governance):
- Phase B: Reviewer MUST reject PR during code review
- Phase C+: Validation MAY block merge; bot MAY comment with instructions

Developer MUST update PR body with issue reference.

### Invalid Issue Number

**Scenario**: PR references `#999` but issue doesn't exist.

**Failure Mode** (Governance):
- Phase B: Reviewer MUST identify during code review and reject PR
- Phase C+: Validation MAY block merge; bot MAY comment

Developer MUST fix reference or create missing issue.

### Issue Already Closed

**Scenario**: PR uses `Fixes #123` but issue already closed by previous PR.

**Failure Mode** (Governance):
- Phase B: Reviewer MUST identify conflict and reject PR
- Phase C+: Validation MAY block merge

Developer MUST update to `Related to #123` or explain in comments.

### GitHub Auto-Close Failure

**Scenario**: PR merged with `Fixes #123` but GitHub's native auto-close fails.

**Failure Mode** (Governance):
- Phase B: Manual detection during post-merge review; manual closure required
- Phase C+: Post-merge validation MAY detect and alert

Maintainer MUST manually close issue and investigate GitHub functionality.

### Acceptance Criteria Incomplete

**Scenario**: PR with closing keyword merged but issue acceptance criteria not all checked.

**Failure Mode** (Governance):
- Phase B: Reviewer MUST verify criteria during PR review; reject if incomplete
- Phase C+: Validation MAY block merge

Issue MUST NOT be marked complete until criteria verified.

### Multiple PRs, One Issue

**Scenario**: Issue requires multiple PRs to fully implement.

**Failure Mode**: First PR SHOULD use `Addresses #123` (non-closing). Final PR SHOULD use `Fixes #123` (closing). If multiple PRs use closing keyword, only first merge SHOULD close issue.

## Out-of-Scope Items

The following are explicitly PROHIBITED in this specification:

1. **Commit Message Parsing**: Issue references in commits do not satisfy traceability (MUST be in PR body)
2. **AI-Inferred Linking**: No automatic linking based on semantic analysis of PR and issue content
3. **External System Linking**: Traceability only covers GitHub issues, not Jira/Linear tickets
4. **Branch Protection Rules**: This spec defines policy, not GitHub branch protection config
5. **Merge Strategy Requirements**: No prescription of squash vs merge commit vs rebase
6. **Cross-Repo Traceability**: Links to issues in other repos are informational only

## Change Control

### Amendment Process

1. Changes to validation rules MUST be proposed via PR to this document
2. PRs modifying traceability requirements MUST include:
   - Rationale for change
   - Impact on existing workflows
   - Migration plan if stricter requirements introduced
   - Testing plan for validation logic

3. Approval requirements:
   - Review from repository maintainer
   - Sign-off from at least one team lead
   - Testing of validation changes in non-production environment

### Versioning

This document follows semantic versioning:
- **Major**: New blocking validation rule or removed exemption
- **Minor**: New non-blocking validation or exemption type
- **Patch**: Clarification or example updates

Current version: `1.0.0` (Phase B baseline)

### Backward Compatibility

Changes to validation rules:

1. MUST NOT retroactively apply to already-merged PRs
2. MAY apply to open PRs with grace period (minimum 1 week warning)
3. SHOULD include gradual rollout with warning-only phase before blocking

---

**Document Status**: Canonical (Phase B)
**Last Updated**: 2026-01-09
**Maintained By**: Repository Governance Team
