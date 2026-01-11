# SKILL_TECHNICAL_DESIGN

## Purpose
To translate scoped requirements into an auditable, testable, and peer-reviewed technical approach before implementation begins.

## Authorized Roles
- **Primary Executor**: GPT_PAC (docs/governance/roles/gpt_pac.md)
- **May Contribute**: Claude Implementer (technical input), Claude Reviewer (design review)
- **Must NOT Execute**: Governance Auditor (must remain independent)

## When This Skill MUST Be Used
- After work item intake has been approved.
- Before writing production code, infrastructure, or schema changes.
- When implementing significant logic, systems integration, or risk-bearing change.

## When This Skill MUST NOT Be Used
- For typos, UI copy fixes, or one-line config toggles with no downstream impact.
- During time-sensitive incident remediation where design can be deferred post-fix.

## Required Inputs (Fail-Closed)
- Approved work item with documented scope and acceptance criteria.
- Known dependencies and constraints.
- Confirmed priority and ownership.

## Pre-Execution Checks
- Validate that the work item has "INTAKE_APPROVED" status.
- Confirm that similar designs or related systems have been reviewed to avoid redundancy.
- Check that implementation urgency does not bypass design necessity.

## Step-by-Step Procedure
1. Review intake artifacts and confirm alignment with goals.
2. Identify design options and assess tradeoffs (e.g., complexity, reliability, extensibility).
3. Draft a technical design document (TDD) covering architecture, data flow, interfaces, failure modes, and test strategy.
4. STOP: Reject design if tradeoffs or test strategy are not articulated.
5. Circulate the design for peer review and stakeholder feedback.
6. Revise based on comments and record all key decision points.
7. STOP: Do not proceed until design is explicitly approved.
8. Log design version and approval in GitHub (committed to docs/ directory, attached to Issue, or recorded in PR description).

## Output Artifacts
- Versioned technical design document in GitHub repository or attached to GitHub Issue.
- Record of reviewers, feedback, and approvals.
- Link from intake record to final approved design.

## Quality Gates (Manual)
- Peer reviewers sign off on testability, clarity, and risk coverage.
- Tech lead or architect confirms adherence to standards and constraints.

## Evidence Checklist
- Link to approved design doc.
- List of reviewed alternatives and rationale.
- Review sign-off timestamps and approvers.

## Failure Modes
- "Reviewed but not verified": Sign-off without actually reading the design.
- "Implementation before agreement": Code written while design is in flux.
- "Evidence referenced but not present": Claims of approval without links.

## Definition of Done
- Approved technical design with test and failure mode coverage.
- Traceable linkage between work item and design artifact.

## Escalation Conditions
- Disagreement on architecture, test strategy, or implementation scope.
- No qualified reviewers available.
- Design changes made after approval without notification.

## Common Anti-Patterns This Skill Exists To Prevent
- Overbuilding due to lack of scoping discipline.
- Code diverging from design due to verbal agreements.
- Retroactive design documents written to justify already-built code.
