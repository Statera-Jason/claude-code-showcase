# SKILL_IMPLEMENTATION

## Purpose
To ensure that code, infrastructure, and configuration changes are executed in strict alignment with the approved design and documented requirements.

## Authorized Roles
- **Primary Executor**: Claude Implementer (docs/governance/roles/claude_implementer.md)
- **May Contribute**: GPT_PAC (clarifications on scope/design), Claude Reviewer (early feedback)
- **Must NOT Execute**: Governance Auditor (must remain independent)

## When This Skill MUST Be Used
- After a technical design has been approved.
- When building any logic, automation, or infrastructure that will be deployed to shared or production environments.

## When This Skill MUST NOT Be Used
- For non-executable documentation updates or comments.
- In environments explicitly designated for experimentation or learning.

## Required Inputs (Fail-Closed)
- Approved and versioned technical design.
- Test strategy documented in design.
- Linked work item with acceptance criteria.

## Pre-Execution Checks
- Confirm that the implementation branch or environment is isolated.
- Ensure all team members understand the design intent and constraints.
- Verify required tools, libraries, or services are available.

## Step-by-Step Procedure
1. Review approved technical design and validate scope alignment.
2. Set up isolated development environment or branch.
3. Implement functionality as described in the design.
4. STOP: If divergence from design is required, halt and update design before proceeding.
5. Write or update unit tests per test strategy.
6. Perform self-review for completeness and adherence to standards.
7. Document any known deviations or limitations.
8. STOP: Do not proceed to review if functionality is incomplete or untested.

## Output Artifacts
- Code in version control linked to work item.
- Associated tests covering designed functionality.
- Documentation of any deviations and rationale.

## Quality Gates (Manual)
- Developer self-certifies code readiness.
- Tech lead or peer confirms design alignment before code review.

## Evidence Checklist
- Commit references approved design and work item.
- Test coverage report or test execution logs.
- Record of known limitations and tradeoffs.

## Failure Modes
- "Implementation before agreement": Design ignored or partially implemented.
- "Implicit scope expansion": Developer builds features not in scope.
- "Docs updated after the fact": Code written without reflecting rationale.

## Definition of Done
- Code implements the approved design with testable outputs.
- All changes committed, traceable, and documented.

## Escalation Conditions
- Blockers due to unavailable dependencies or misalignment with design.
- Conflicts with other in-progress work.
- Inability to complete implementation within scoped effort.

## Common Anti-Patterns This Skill Exists To Prevent
- Unreviewed feature creep.
- "Helpful" implementation of future roadmap items.
- Rewrites or shortcuts that deviate from tested design.
