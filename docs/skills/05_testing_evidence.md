# SKILL_TESTING_EVIDENCE

## Purpose
To provide verifiable proof that implementation meets acceptance criteria and does not introduce regressions, using reproducible and auditable test results.

## Authorized Roles
- **Primary Executor**: Claude Implementer (docs/governance/roles/claude_implementer.md)
- **May Contribute**: Claude Reviewer (verification of evidence completeness)
- **Must NOT Execute**: Governance Auditor (must remain independent)

## When This Skill MUST Be Used
- After implementation is complete and before merge or release.
- When changes impact production systems, logic, workflows, or data paths.

## When This Skill MUST NOT Be Used
- For static content updates or cosmetic-only changes where test automation is not applicable.
- For one-time scripts with verified sandbox output and no persistence.

## Required Inputs (Fail-Closed)
- Completed implementation and associated test cases.
- Acceptance criteria from the approved work item.
- Functional, integration, and/or regression test plan.

## Pre-Execution Checks
- Confirm that automated tests exist and are runnable.
- Validate test environment is isolated and mirrors production configuration.
- Ensure test data and conditions are controlled and reproducible.

## Step-by-Step Procedure
1. Identify test cases mapped to acceptance criteria.
2. Run automated and/or manual tests in a clean environment.
3. Capture logs, screenshots, or outputs for each acceptance check.
4. STOP: Fail the skill if any acceptance test fails or cannot be validated.
5. Summarize test results and anomalies.
6. Record test run ID, timestamp, and test suite version.
7. Attach test output to work item or review thread before proceeding.

## Output Artifacts
- Test result logs or screenshots.
- Mapping of test results to acceptance criteria.
- Record of test environment and test tool versions.

## Quality Gates (Manual)
- Peer or QA validates test output matches success criteria.
- Any failed test must be addressed or explicitly deferred with approval.

## Evidence Checklist
- Link to manual test report, test logs, or explicitly authorized CI pipeline output (authorization must be documented in associated GitHub Issue per automation policy).
- Evidence MUST be captured in GitHub (PR attachments, committed artifacts, or PR comments). CI/CD is not assumed and is not required.
- Confirmation of environment, date/time, and data version.
- Test-to-requirements trace matrix.

## Failure Modes
- "Evidence referenced but not present": Claimed pass with no output.
- "Reviewed but not verified": Test logs ignored or unread.
- "Docs updated after the fact": Retroactively editing acceptance checks.

## Definition of Done
- All relevant tests pass, evidence is captured, and criteria are met.
- Reviewers can independently verify success without re-running.

## Escalation Conditions
- Inconsistent results between environments.
- Failing tests with unclear cause or outdated baselines.
- Acceptance criteria that cannot be tested.

## Common Anti-Patterns This Skill Exists To Prevent
- Assumed success without execution.
- Unverified test runs reused across branches or commits.
- Mismatched test scope and implementation scope.
