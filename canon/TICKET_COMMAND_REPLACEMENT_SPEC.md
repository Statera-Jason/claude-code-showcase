# Ticket Command Replacement Specification

## Purpose

This document defines the canonical workflow for addressing GitHub Issues now that the `/ticket` command references Jira/Linear (which are dormant by default). It establishes GitHub-native patterns for issue intake, development workflow, and completion tracking.

## Scope

This specification governs:

- Workflow patterns for working with GitHub Issues without `/ticket` command
- Manual and assisted workflows for issue-to-PR lifecycle
- Integration points with existing CLI tooling
- Migration guidance from Jira/Linear-centric workflows
- Future extension points for GitHub-native automation

## Non-goals

This specification does NOT:

- Implement new slash commands or CLI tooling (Phase B is documentation-only)
- Define GitHub Actions workflows or automation
- Prescribe bot behavior or webhook handlers
- Replace the existing `/ticket` command implementation
- Modify MCP server configurations

## Explicit Constraints

1. **No New Commands**: Phase B is documentation-only; no new `/ticket-gh` or similar commands
2. **Backward Compatibility**: Existing `/ticket` command remains functional for Jira/Linear users
3. **Manual-First Posture**: Workflows MUST be accomplishable manually before automation considered
4. **No Implicit Migration**: Users with active Jira/Linear integrations SHALL NOT be auto-switched
5. **Fail-Closed**: Tooling MUST NOT guess which system (GitHub/Jira/Linear) is active
6. **No Runtime Behavior**: This spec documents processes, not executable code

## Definitions

- **Ticket Command**: Legacy `/ticket` command that works with Jira/Linear integrations
- **GitHub-Native Workflow**: Work management using only GitHub Issues, Projects, and PRs
- **Issue Intake**: Process of creating and refining GitHub Issue from user request
- **Development Workflow**: Standard cycle from issue assignment through PR merge
- **Completion Tracking**: Verification that issue acceptance criteria met before closure
- **Manual Workflow**: Process accomplishable via GitHub UI and git CLI without custom tooling

## Canonical Model

### GitHub-Native Workflow Overview

The canonical workflow for GitHub Issues replaces `/ticket` with the following phases:

1. **Issue Intake**: Create or identify existing GitHub Issue
2. **Issue Refinement**: Ensure issue has required metadata and clear acceptance criteria
3. **Branch Creation**: Create feature branch following naming convention
4. **Development**: Implement changes with incremental commits
5. **PR Creation**: Open PR with proper issue reference
6. **Review**: Code review and feedback cycles
7. **Merge**: Merge PR, auto-closing linked issue
8. **Verification**: Confirm acceptance criteria met

### Phase 1: Issue Intake

#### Manual Process

1. Navigate to repository's Issues tab
2. Click "New Issue" and select appropriate template
3. Fill in required fields:
   - Title (concise, imperative)
   - Problem statement
   - Acceptance criteria (checkboxes)
   - Additional context
4. Add labels: `type:*`, `priority:*`, `area:*`
5. Assign to GitHub Project
6. Submit issue

#### Assisted Process (Future)

When tooling is implemented (Phase C+), CLI MAY support:

```bash
# Hypothetical future command (NOT implemented in Phase B)
gh issue create --template feature \
  --title "Add dark mode toggle" \
  --label "type:feature,priority:medium" \
  --project "Main Board"
```

#### Comparison to `/ticket`

**What `/ticket` did (Jira/Linear)**:
- Fetched ticket details from external system
- Created local branch with ticket ID
- Updated ticket status to "In Progress"

**GitHub-native equivalent**:
- Issue already exists in GitHub (no fetch needed)
- Manually create branch with issue number
- Manually update Project status to "In Progress"

**Key difference**: GitHub-native is manual-first; no external API calls.

### Phase 2: Issue Refinement

#### Required Metadata Check

Before starting work, verify issue has:

1. ✅ One `type:*` label
2. ✅ One `priority:*` label
3. ✅ Clear acceptance criteria with checkboxes
4. ✅ Project assignment with status field
5. ✅ No `status:blocked` or `status:needs-info` labels

If missing, update issue before proceeding.

#### Manual Process

1. Open issue in browser
2. Click "Edit" on labels, add missing classifications
3. Edit issue body to add acceptance criteria if absent
4. Add to Project via right sidebar
5. Set Project status to "Ready"

#### Validation

**Manual check**:
- Read issue body and acceptance criteria
- Confirm you understand requirements
- Ask clarifying questions in comments if needed

**Automated check (Future)**:
- CLI tool could validate issue schema compliance
- Could block branch creation if validation fails

### Phase 3: Branch Creation

#### Manual Process

```bash
# Create branch with issue number prefix
git checkout -b 123-add-dark-mode

# Or with type prefix
git checkout -b feature/123-add-dark-mode
```

#### Naming Convention

Format: `[type/]<issue-number>-<description>`

Examples:
- `123-fix-login-redirect`
- `feature/456-user-dashboard`
- `bugfix/789-memory-leak`

**CRITICAL**: Branch name alone does NOT establish traceability. PR body MUST still reference issue.

### Phase 4: Development

#### Incremental Commits

```bash
# Make changes
git add src/components/DarkModeToggle.tsx
git commit -m "feat: add DarkModeToggle component"

# More changes
git add src/hooks/useDarkMode.ts
git commit -m "feat: add useDarkMode hook"
```

#### Commit Messages

Follow conventional commits format:
- `feat:` for new features
- `fix:` for bug fixes
- `docs:` for documentation
- `refactor:` for code improvements
- `test:` for tests

#### Status Updates

Periodically update issue status in Project:
- Set to "In Progress" when starting
- Add comments on progress or blockers
- Update acceptance criteria checkboxes as completed

### Phase 5: PR Creation

#### Manual Process

```bash
# Push branch
git push -u origin 123-add-dark-mode

# Create PR via GitHub CLI
gh pr create --title "feat: add dark mode toggle" --body "$(cat <<'EOF'
Fixes #123

## Summary
- Added DarkModeToggle component
- Implemented useDarkMode hook
- Applied dark theme styles

## Test Plan
- [ ] Manual testing in dev environment
- [ ] Verified theme persists across sessions
- [ ] Tested in Chrome, Firefox, Safari
EOF
)"
```

#### Required PR Body Format

```markdown
Fixes #123

## Summary
[Bullet points describing changes]

## Test Plan
[How changes were tested]

## Additional Notes
[Optional: deployment notes, breaking changes, etc.]
```

**CRITICAL**: PR body MUST include `Fixes #123` or similar closing keyword.

#### What `/ticket` Did Differently

**Jira/Linear workflow**:
- `/ticket` command auto-created PR with ticket link
- Updated ticket status in external system
- Formatted PR body with ticket details

**GitHub-native workflow**:
- Manually create PR with `gh pr create` or GitHub UI
- Issue status auto-updates when PR created (GitHub native)
- Manually write PR body with issue reference

### Phase 6: Review

#### Manual Process

1. Request reviews from team members
2. Address review comments
3. Push additional commits to PR branch
4. Re-request review when ready

#### CI/CD Checks (If Configured)

**Note**: CI/CD is outside Phase B scope. If repository has existing CI:
- Tests
- Linting
- Type checking
- Build validation

These run per existing repository configuration, not Phase B requirements.

#### Issue Status

**Phase B**: Manually update issue status in Project board to "In Review"
**If configured**: User-created Project automation MAY auto-update (not Phase B requirement)

### Phase 7: Merge

#### Manual Process

```bash
# Via GitHub CLI
gh pr merge --squash --auto

# Or via GitHub UI
# Click "Squash and merge" button
```

#### Issue Closure (GitHub Native)

When PR with `Fixes #123` is merged:
- **GitHub Native**: Issue automatically closes (built-in GitHub behavior)
- **Phase B**: Manually update Project status to "Done"
- **If configured**: User-created Project automation MAY auto-update status
- Issue timeline shows merge event (GitHub native)

#### Pre-Merge Checks (Manual in Phase B)

**Phase B**: Reviewer MUST manually verify during PR review:
- All acceptance criteria checked off
- No `status:blocked` label
- At least one approval
- CI passing (if repository has CI configured)

### Phase 8: Verification

#### Post-Merge Check

**Phase B**: Manual verification steps:
1. Confirm GitHub closed the issue (native `Fixes` keyword behavior)
2. Verify all acceptance criteria were met
3. Deploy to staging/production (per repository process)
4. Add final comment confirming deployment

#### If GitHub Auto-Close Failed

If issue didn't close despite `Fixes #123` syntax:
1. Verify PR body has proper `Fixes #123` syntax
2. Manually close issue with comment referencing PR
3. GitHub native auto-close may fail due to permissions or syntax errors

### Workflow Comparison Matrix

| Step | `/ticket` (Jira/Linear) | GitHub-Native |
|------|------------------------|---------------|
| Issue source | External system | GitHub Issues |
| Branch creation | Auto-generated | Manual git command |
| Status sync | Bidirectional API | GitHub automation only |
| PR creation | Command-assisted | Manual gh CLI or UI |
| PR body formatting | Auto-generated | Manual template |
| Issue closure | Manual in external system | Auto on PR merge |
| Traceability | External ticket ID | GitHub issue number |

### Migration from `/ticket` Command

#### For Current Jira/Linear Users

If you currently use `/ticket` command:

1. **Do NOT migrate yet**: Jira/Linear integrations remain functional
2. **Dormant by default**: New projects use GitHub-native workflow
3. **Opt-in migration**: See JIRA_LINEAR_DORMANCY_POLICY.md for activation policy

#### For New Projects

New repositories SHOULD:

1. Set up GitHub Projects with proper status fields
2. Configure issue templates (see ISSUE_SCHEMA.md)
3. Train team on GitHub-native workflow (this document)
4. Configure branch protection requiring issue references
5. Set up Project automation for status transitions

## Failure Modes

**Phase B Note**: Failure modes below describe governance posture. Phase B relies on manual review and correction.

### Forgot Issue Reference in PR

**Scenario**: Developer creates PR but forgets `Fixes #123` in body.

**Failure Mode**:
- Phase B: Reviewer MUST reject PR during code review
- Phase C+: Validation MAY block merge

Developer MUST edit PR body to add reference.

**Prevention**: Use PR template with reminder comment.

### Issue Not Refined

**Scenario**: Developer starts work on issue lacking acceptance criteria.

**Failure Mode**: PR review identifies unclear requirements. Work may need significant revision.

**Prevention**:
- Phase B: Manual validation before starting work
- Phase C+: CLI MAY validate issue schema before branch creation

### Branch Name Doesn't Match Issue

**Scenario**: Branch named `fix-bug` instead of `123-fix-bug`.

**Failure Mode**: No failure; this is optional convention. PR body reference is what matters.

**Prevention**: Document branch naming convention; mention in PR review if not followed.

### Multiple Issues, One PR

**Scenario**: PR addresses multiple issues but only references one.

**Failure Mode**: Other issues remain open despite being implemented.

**Prevention**: Use multiple issue references in PR body (`Fixes #123, Fixes #124`).

### GitHub Auto-Close Didn't Trigger

**Scenario**: PR merged with `Fixes #123` but GitHub's native auto-close failed.

**Failure Mode**:
- Phase B: Manual detection during issue review; manual closure required
- Phase C+: Post-merge validation MAY detect and alert

**Prevention**: Verify `Fixes` syntax during PR review; manually close if needed.

## Out-of-Scope Items

The following are explicitly PROHIBITED or out of scope:

1. **New Slash Commands**: No `/issue`, `/gh-ticket`, or similar commands in Phase B
2. **GitHub Actions**: No workflow files for automation (documentation-only phase)
3. **Bot Implementation**: No probot or GitHub App development
4. **MCP Extensions**: No new MCP servers for GitHub Projects API
5. **CLI Tool Development**: No new binaries or npm packages
6. **Jira/Linear Removal**: Existing integrations remain available (dormant)
7. **Forced Migration**: No automatic switch from Jira/Linear to GitHub

## Change Control

### Amendment Process

1. Changes to workflow steps MUST be proposed via PR to this document
2. PRs modifying workflow MUST include:
   - Rationale for change
   - Impact on developer experience
   - Updated examples reflecting new process
   - Training plan for team

3. Approval requirements:
   - Review from repository maintainer
   - Sign-off from developer experience team
   - Validation that workflow is manually accomplishable

### Versioning

This document follows semantic versioning:
- **Major**: Fundamental workflow change (e.g., new required step)
- **Minor**: Optional workflow enhancement or alternative approach
- **Patch**: Clarification or example updates

Current version: `1.0.0` (Phase B baseline)

### Future Evolution

Potential Phase C+ enhancements (NOT in scope for Phase B):

1. **GitHub CLI Extension**: `gh issue start <number>` to automate intake
2. **VS Code Extension**: Right-click issue to create branch
3. **Claude Code Skill**: `/start-issue` command for GitHub-native workflow
4. **Project Automation**: GitHub Actions for status transitions
5. **Template Validation**: Pre-commit hooks checking issue references

These are INTENTIONALLY deferred to keep Phase B documentation-focused.

---

**Document Status**: Canonical (Phase B)
**Last Updated**: 2026-01-09
**Maintained By**: Repository Governance Team
