# SKILL_CODE_REVIEW

## Purpose
To verify that the implementation faithfully matches the approved design, adheres to standards, and includes sufficient tests and documentation before merge or deployment.

## Authorized Roles
- **Primary Executor**: Claude Reviewer (docs/governance/roles/claude_reviewer.md)
- **May Contribute**: GPT_PAC (scope validation), Governance Auditor (scope audit post-review)
- **Must NOT Execute**: Claude Implementer on own code (conflict of interest)

## When This Skill MUST Be Used
- Before any code is merged to main, trunk, or production branches.
- When reviewing contributions to shared infrastructure or services.

## When This Skill MUST NOT Be Used
- For personal projects or sandbox environments not used in production.
- When code is explicitly flagged as non-reviewable spike work (with written approval).

## Required Inputs (Fail-Closed)
- Implementation linked to approved design and work item.
- Automated test results available.
- Self-review completed by author.

## Pre-Execution Checks
- Confirm that a design document exists and is linked.
- Verify that scope of change matches the implementation plan.
- Ensure that test coverage data is available and understandable.

## Step-by-Step Procedure
1. Confirm traceability to intake and design documents.
2. Review code for clarity, correctness, and scope alignment.
3. Verify that tests exist and cover expected behaviors and edge cases.
4. STOP: Reject if tests are missing, failing, or irrelevant.
5. Check for security, performance, and maintainability concerns.
6. Validate naming, structure, and architectural consistency.
7. Provide feedback with clear reasoning and required actions.
8. STOP: Do not approve if justification is missing for known deviations.
9. Confirm all comments resolved before approving merge.

## Output Artifacts
- Review comments and approvals in version control.
- Links to resolved design discrepancies, if any.
- Summary of issues found and addressed.

## Quality Gates (Manual)
- At least one qualified peer or lead must approve.
- Review must confirm test coverage and design alignment.

## Evidence Checklist
- Link to pull/merge request.
- Review history showing comment resolution.
- Explicit approval with rationale noted (e.g., test coverage confirmed).

## Failure Modes
- "Reviewed but not verified": Code skimmed but not understood.
- "Evidence referenced but not present": Approval given without checking test results.
- "Docs updated after the fact": Reviewer assumes alignment without cross-check.

## Definition of Done
- Code is reviewed, approved, and linked to traceable design and scope.
- No unresolved comments or open issues.

## Escalation Conditions
- Repeated low-quality submissions.
- Significant divergence from approved design without explanation.
- Blocked due to lack of available reviewers.

## Common Anti-Patterns This Skill Exists To Prevent
- Rubber-stamp approvals.
- Merging code that fails tests or introduces regressions.
- Unscoped additions disguised as "refactors."
