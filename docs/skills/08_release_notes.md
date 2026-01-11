# SKILL_RELEASE_NOTES

## Purpose
To communicate externally visible changes, risks, and required actions associated with a release in a clear, accurate, and auditable way.

## Authorized Roles
- **Primary Executor**: GPT_PAC (docs/governance/roles/gpt_pac.md)
- **May Contribute**: Claude Implementer (technical input on changes)
- **Must NOT Execute**: Governance Auditor (must remain independent)

## When This Skill MUST Be Used
- Before any release to production or external consumers.
- When behavior, APIs, dependencies, or operational procedures change.

## When This Skill MUST NOT Be Used
- For internal-only experiments that are not released.

## Required Inputs (Fail-Closed)
- List of shipped changes linked to work items.
- Risk assessment and known limitations.
- Rollback/mitigation steps.

## Pre-Execution Checks
- Confirm scope is final and matches what will be deployed.
- Ensure docs and testing evidence exist for user-facing changes.

## Step-by-Step Procedure
1. Summarize user-visible changes (what's new/changed/fixed).
2. Call out breaking changes and required consumer actions.
3. Include migration steps if applicable.
4. Document known issues and mitigations.
5. Include links to work items, PRs, and testing evidence.
6. STOP: Reject release notes if they claim changes not present in the release.
7. Obtain review from release owner / maintainer.

## Output Artifacts
- Release notes entry (changelog, GitHub release, internal release doc).
- Links to evidence and work items.

## Quality Gates (Manual)
- Maintainer confirms accuracy.
- Release manager confirms readiness.

## Evidence Checklist
- Links to PRs/issues.
- Test evidence link.
- Release tag/version.

## Failure Modes
- Missing breaking-change callouts.
- Release notes are marketing copy without operational detail.
- Unverified claims.

## Definition of Done
- Release notes accurately describe the shipped release and required actions.

## Escalation Conditions
- Disagreement on scope or risk.
- Missing verification artifacts.

## Common Anti-Patterns This Skill Exists To Prevent
- Silent breaking changes.
- "Just ship it" without communicating risk.
