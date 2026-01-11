# SKILL_DOC_UPDATE

## Purpose
To ensure that user-facing and system-facing documentation reflects the true state, behavior, and usage of the system as implemented.

## Authorized Roles
- **Primary Executor**: Claude Implementer (docs/governance/roles/claude_implementer.md)
- **May Contribute**: GPT_PAC (scope/requirements clarification), Claude Reviewer (doc review)
- **Must NOT Execute**: Governance Auditor (must remain independent)

## When This Skill MUST Be Used
- After implementation changes behavior, APIs, workflows, or dependencies.
- Before release to users or handoff to downstream teams.

## When This Skill MUST NOT Be Used
- For internal experiments or feature flags not exposed externally.
- When change is rolled back prior to being merged or released.

## Required Inputs (Fail-Closed)
- List of user-facing impacts and behavioral changes.
- Record of implemented changes and links to associated work items.
- Existing documentation source of truth.

## Pre-Execution Checks
- Confirm that documentation exists and is version-controlled.
- Ensure technical changes are finalized and not in flux.
- Identify all impacted consumers, environments, and entry points.

## Step-by-Step Procedure
1. Identify documentation artifacts that need update (e.g., README, API refs, onboarding guides).
2. Draft changes in same version control system as codebase or documentation repo.
3. Ensure documentation matches implemented behavior, not intended behavior.
4. Include examples, edge cases, and migration steps if relevant.
5. STOP: Reject doc update if any behavior described cannot be verified in code or tests.
6. Request review from owners/maintainers of the impacted docs.
7. Merge documentation changes alongside or immediately after code changes.

## Output Artifacts
- Updated documentation with links to the work item.
- Change summary (what changed, why, who is impacted).
- Any required migration or upgrade instructions.

## Quality Gates (Manual)
- Maintainer review confirms accuracy and clarity.
- Doc changes are traceable to the specific implementation/release.

## Evidence Checklist
- Links to updated docs.
- Review approval(s).
- Validation that examples are runnable or otherwise verified.

## Failure Modes
- Docs reflect intent, not reality.
- Missing migration steps for breaking changes.
- Undocumented new dependencies or configuration.

## Definition of Done
- Documentation accurately describes current behavior.
- Users/operators can follow docs without tribal knowledge.

## Escalation Conditions
- Unclear doc ownership.
- Behavior is ambiguous or inconsistent across environments.
- Breaking changes without documented mitigation.

## Common Anti-Patterns This Skill Exists To Prevent
- "We'll update docs later."
- Copy-pasted examples that don't run.
- Release notes substituted for real documentation.
