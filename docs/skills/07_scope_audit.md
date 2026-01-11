# SKILL_SCOPE_AUDIT

## Purpose
To verify that delivered work strictly matches approved scope and that no unauthorized expansion or omission occurred.

## Authorized Roles
- **Primary Executor**: Governance Auditor (docs/governance/roles/governance_auditor.md)
- **May Contribute**: None (must remain independent)
- **Must NOT Execute**: GPT_PAC, Claude Implementer, Claude Reviewer (conflict of interest)

## When This Skill MUST Be Used
- Before closing a work item.
- Before declaring a milestone complete.

## When This Skill MUST NOT Be Used
- During incident response.

## Required Inputs (Fail-Closed)
- Approved intake record / work item.
- Approved technical design.
- Implementation and testing evidence.

## Pre-Execution Checks
- Confirm the scope boundaries are explicit.
- Confirm acceptance criteria are listed and testable.

## Step-by-Step Procedure
1. Compare implementation outputs to acceptance criteria.
2. Identify any scope creep (new features) or scope gaps (missing requirements).
3. Validate docs and release notes reflect delivered behavior.
4. STOP: Fail audit if any scope delta lacks explicit approval.
5. Record audit findings and link evidence.

## Output Artifacts
- Audit summary (pass/fail) with links.
- List of scope deltas and approvals (if any).

## Quality Gates (Manual)
- Reviewer confirms audit is evidence-based.

## Evidence Checklist
- Links to work item, design, PR, tests, docs, release notes.

## Failure Modes
- Closing work without verifying against acceptance criteria.
- Treating "looks good" as evidence.

## Definition of Done
- Audit passes with traceable evidence.

## Escalation Conditions
- Unapproved scope changes.
- Incomplete acceptance criteria.

## Common Anti-Patterns This Skill Exists To Prevent
- Shipping extra/unreviewed features.
- Forgetting edge-case requirements.
