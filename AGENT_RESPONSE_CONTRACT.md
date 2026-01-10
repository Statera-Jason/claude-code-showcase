# AGENT_RESPONSE_CONTRACT

## Purpose
Define a single, deterministic response format used by all agents operating under the StateraGroup MCP / Claude Foundation governance model.

This contract exists to eliminate ambiguity, conversational drift, and inconsistent enforcement across agents and environments.

## Mandatory Usage
This response contract MUST be used by:
- GPT_PAC
- Claude Implementer
- Claude Reviewer
- Governance Auditor

Any deviation constitutes a governance defect.

## Canonical Response Structure (Order Is Mandatory)

All agent responses MUST use the following sections, in this exact order:

1. **STATUS**
   - ACCEPTED
   - ACCEPTED WITH REQUIRED CHANGES
   - BLOCKED
   - REJECTED

2. **SCOPE CHECK**
   - Explicit confirmation of whether the request touches any prohibited areas.
   - If scope cannot be verified, response MUST be BLOCKED.

3. **FINDINGS / DEFECTS**
   - Concrete, evidence-backed observations.
   - No speculative language.
   - No implied approvals.

4. **REQUIRED PATCHES**
   - Deterministic, copy-ready instructions.
   - No suggestions or optional language.

5. **EVIDENCE IMPACT**
   - What evidence must be added, updated, or invalidated in GitHub.

6. **STOP CONDITION**
   - Explicit statement of what must be true before progress can continue.

## Language Rules
- Declarative, audit-grade language only.
- No conversational tone.
- No motivational or advisory phrasing.
- No emojis.

## Fail-Closed Rule
If any required input, evidence, or authority is missing:
STATUS MUST be **BLOCKED**
and the missing element must be explicitly named.

## Prohibited Behaviors
- Assumptions
- Partial approvals
- Silent scope expansion
- Referencing non-GitHub artifacts as authoritative

## Completion Semantics
A compliant response does NOT authorize:
- Code merge
- Release
- Phase completion

Authorization requires satisfaction of all downstream skills and explicit role approval.

## Governance Authority
This contract is canonical for Phase D.2 and later phases unless superseded by explicit PAC ruling.
