# SKILL_WORK_ITEM_INTAKE

## Purpose
To ensure every work item is captured with clarity, properly scoped, and explicitly authorized before technical activity begins.

## Authorized Roles
- **Primary Executor**: GPT_PAC (docs/governance/roles/gpt_pac.md)
- **May Contribute**: Claude Implementer, Claude Reviewer (consultation only)
- **Must NOT Execute**: Governance Auditor (must remain independent)

## When This Skill MUST Be Used
- Before any engineering, design, or investigation begins.
- When a request is made by a stakeholder or external team.
- When a new priority is introduced during a sprint or milestone.

## When This Skill MUST NOT Be Used
- When performing triage of alerts or incident response.
- For ad hoc exploratory efforts not intended to produce deliverables.

## Required Inputs (Fail-Closed)
- Written request or documented ask from stakeholder.
- Defined business objective.
- Initial scope boundaries (what is in/out).
- Named decision-maker.

## Pre-Execution Checks
- Confirm the requester has authority to initiate the work.
- Confirm the scope does not conflict with in-progress efforts.
- Validate that acceptance criteria are specified or request is returned.

## Step-by-Step Procedure
1. Identify the request origin and verify its legitimacy.
2. Confirm that the request includes business context and goals.
3. Require scoping information (inclusions, exclusions, constraints).
4. Identify stakeholders and confirm decision authority.
5. Record the request as a GitHub Issue (canonical system of record per governance policy).
6. Validate acceptance criteria are testable and documented.
7. STOP: Reject intake if acceptance criteria or decision authority is missing.
8. Assign priority based on intake rubric (e.g., impact, urgency).
9. Confirm agreement on definition of ready.
10. STOP: Mark work item "INTAKE_APPROVED" only after all criteria are satisfied.

## Output Artifacts
- GitHub Issue with documented scope boundaries and decision-maker.
- Documented scope boundaries and decision-maker.
- Acceptance criteria and rationale for prioritization.

## Quality Gates (Manual)
- Manager or designated triager reviews intake form completeness.
- Stakeholder signs off on scope, goals, and decision-making authority.

## Evidence Checklist
- Link to intake record.
- Names of requestor and approver.
- Scope and acceptance criteria present in intake documentation.

## Failure Modes
- "Implementation before agreement": Work begins without intake approval.
- "Docs updated after the fact": Documentation fabricated post hoc.
- "Evidence referenced but not present": Intake record links to non-existent scope.
- "Implicit scope expansion": No boundaries defined, team builds more than required.

## Definition of Done
- Work item is approved, scoped, and recorded with clear acceptance criteria.
- No execution begins until approval is logged and traceable.

## Escalation Conditions
- Scope ambiguity or conflict with existing work.
- Missing decision authority or unclear ownership.
- Business impact not articulated.

## Common Anti-Patterns This Skill Exists To Prevent
- Starting implementation based on Slack threads.
- Ambiguity around "who asked for this" or "what does success look like."
- Doing helpful but unauthorized work based on assumptions.
