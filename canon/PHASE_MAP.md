# Phase Map

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This document defines the phase structure for claude-code-showcase foundation governance, authority transitions between phases, and the explicit rule that Claude prompts may not be generated unless explicitly requested.

---

## Phase Structure

The claude-code-showcase foundation follows a five-phase governance model:

```
Phase A → Phase B → Phase C → Phase D → Phase E
(Canon)   (Impl.)   (Integ.)  (Oper.)   (Evol.)
```

Each phase has distinct scope, authority, and constraints.

---

## Phase A: Canonisation (Current)

**Authority:** GC_CLAUDE_FOUNDATION-A_CANON

**Status:** ACTIVE

**Scope:** Documentation, specification, governance structure

### Objectives

1. ✓ Define scope boundaries (CLAUDE_FOUNDATION_SCOPE.md)
2. ✓ Enumerate capabilities (CLAUDE_FOUNDATION_CAPABILITIES_REGISTER.md)
3. ✓ Establish adoption contracts (PROJECT_ADOPTION_CONTRACT.md)
4. ✓ Define attachment patterns (ATTACHMENT_PACKAGING_STRATEGY.md)
5. ✓ Document automation policies (FAIL_CLOSED_AUTOMATION_POLICY.md)
6. ✓ Specify integration posture (INTEGRATIONS_POLICY.md)
7. ✓ Map phases and transitions (PHASE_MAP.md)

### Deliverables

All Phase A deliverables are located in `docs/canon/`:

| Document | Status | Purpose |
|----------|--------|---------|
| CLAUDE_FOUNDATION_SCOPE.md | ✓ Complete | Defines what is/isn't in scope |
| CLAUDE_FOUNDATION_CAPABILITIES_REGISTER.md | ✓ Complete | Enumerates all capabilities, dependencies, risks |
| PROJECT_ADOPTION_CONTRACT.md | ✓ Complete | Must/should/must-not for adopters |
| ATTACHMENT_PACKAGING_STRATEGY.md | ✓ Complete | How to attach foundation to projects |
| FAIL_CLOSED_AUTOMATION_POLICY.md | ✓ Complete | Fail-closed doctrine and safety |
| INTEGRATIONS_POLICY.md | ✓ Complete | MCP servers, GitHub Actions, defaults |
| PHASE_MAP.md | ✓ Complete | Phase structure and transitions |

### Constraints

Phase A **MUST NOT**:

- ❌ Generate Claude Code prompts
- ❌ Generate Claude Web prompts
- ❌ Define runtime behavior
- ❌ Implement execution logic
- ❌ Create operational hooks (only document patterns)
- ❌ Enable integrations (only document configuration)

Phase A **MUST**:

- ✓ Document structure and patterns
- ✓ Define governance rules
- ✓ Enumerate capabilities
- ✓ Specify constraints
- ✓ Establish acceptance criteria

### Completion Criteria

Phase A is complete when:

- [x] All seven canon documents are written
- [x] All documents have clear sections
- [x] All decisions are explicit
- [x] All acceptance criteria are met
- [x] All documents are committed to version control

### Exit Condition

Phase A concludes when:

1. All deliverables are complete
2. Phase A authority approves completion
3. Repository is in clean state
4. No runtime behavior has been implemented

**Phase A STOPS here. Do not proceed to Phase B.**

---

## Phase B: Implementation (Future)

**Authority:** GC_CLAUDE_FOUNDATION-B_IMPL (not yet assigned)

**Status:** NOT STARTED

**Prerequisite:** Phase A completion

**Scope:** Runtime behavior, execution logic, Claude prompt generation

### Objectives (Proposed)

1. Define Claude Code behavior for foundation patterns
2. Implement skill activation logic
3. Define hook execution semantics
4. Specify command runtime behavior
5. Create agent execution patterns
6. Generate Claude prompts (when explicitly requested)

### Constraints

Phase B **MAY**:

- ✓ Generate Claude prompts (only when explicitly requested)
- ✓ Define runtime behavior
- ✓ Implement execution logic
- ✓ Create operational hooks
- ✓ Define Claude decision-making patterns

Phase B **MUST NOT**:

- ❌ Generate prompts without explicit request
- ❌ Modify Phase A canon without governance approval
- ❌ Enable integrations without user consent
- ❌ Violate fail-closed doctrine

### Deliverables (Proposed)

- Runtime behavior specifications
- Skill activation logic
- Hook execution semantics
- Command runtime definitions
- Agent execution patterns
- Claude prompt templates (when requested)

### Entry Condition

Phase B may begin when:

1. Phase A is complete
2. User explicitly requests Phase B work
3. Authority is assigned for Phase B
4. Clear implementation scope is defined

**Phase B has NOT been requested. Do not proceed.**

---

## Phase C: Integration (Future)

**Authority:** GC_CLAUDE_FOUNDATION-C_INTEG (not yet assigned)

**Status:** NOT STARTED

**Prerequisite:** Phase B completion

**Scope:** External system integration, MCP server operationalization, GitHub Actions deployment

### Objectives (Proposed)

1. Operationalize MCP servers
2. Deploy GitHub Actions workflows
3. Integrate with JIRA/Linear/GitHub
4. Test integration workflows
5. Document integration patterns

### Constraints

Phase C **MAY**:

- ✓ Enable MCP servers (with consent)
- ✓ Deploy GitHub Actions (with approval)
- ✓ Create external system connections
- ✓ Test integrations in live environments

Phase C **MUST NOT**:

- ❌ Enable integrations without explicit consent
- ❌ Store credentials in version control
- ❌ Violate integration security requirements
- ❌ Proceed without cost approval

### Entry Condition

Phase C may begin when:

1. Phase B is complete
2. User explicitly requests integration work
3. Cost and governance gates are approved
4. Credentials are securely provided

**Phase C has NOT been requested. Do not proceed.**

---

## Phase D: Operations (Future)

**Authority:** GC_CLAUDE_FOUNDATION-D_OPER (not yet assigned)

**Status:** NOT STARTED

**Prerequisite:** Phase C completion

**Scope:** Operational monitoring, cost tracking, performance optimization, incident response

### Objectives (Proposed)

1. Monitor integration usage and costs
2. Track performance metrics
3. Respond to operational issues
4. Optimize for cost and latency
5. Maintain audit trails

### Entry Condition

Phase D may begin when:

1. Phase C is complete
2. System is in production operation
3. Monitoring and alerting are required

**Phase D has NOT been requested. Do not proceed.**

---

## Phase E: Evolution (Future)

**Authority:** GC_CLAUDE_FOUNDATION-E_EVOL (not yet assigned)

**Status:** NOT STARTED

**Prerequisite:** Phase D completion

**Scope:** Long-term evolution, pattern refinement, community feedback integration, major version updates

### Objectives (Proposed)

1. Gather community feedback
2. Refine patterns based on real-world usage
3. Plan major version updates
4. Deprecate obsolete patterns
5. Integrate improvements from adopters

### Entry Condition

Phase E is ongoing after Phase D and continues throughout foundation lifecycle.

**Phase E has NOT been requested. Do not proceed.**

---

## Authority Transitions

### Phase A → Phase B

**Trigger:** User explicitly requests implementation work

**Requirements:**
- Phase A completion verified
- New authority assigned (GC_CLAUDE_FOUNDATION-B_IMPL)
- Implementation scope defined
- User approval obtained

**Process:**
1. Verify Phase A deliverables complete
2. User requests: "Begin Phase B implementation"
3. Assign Phase B authority
4. Define implementation scope
5. Proceed with Phase B objectives

### Phase B → Phase C

**Trigger:** User explicitly requests integration work

**Requirements:**
- Phase B completion verified
- Integration targets identified
- Cost and governance approval obtained
- Credentials securely provided

### Phase C → Phase D

**Trigger:** System enters production operation

**Requirements:**
- Phase C completion verified
- Production deployment approved
- Monitoring and alerting configured

### Phase D → Phase E

**Trigger:** Ongoing evolution needs identified

**Requirements:**
- Phase D operational maturity
- Feedback collection mechanisms
- Version planning process

---

## Explicit Rules

### Rule 1: No Claude Prompts Unless Explicitly Requested

**Phase A PROHIBITION:**

Claude Code prompts, Claude Web prompts, or any executable AI instructions targeting Claude models **MUST NOT** be generated in Phase A.

**Rationale:**
- Phase A is documentation and governance only
- Prompt generation is runtime behavior (Phase B)
- Premature prompt generation violates phase boundaries

**Exception:**
- Phase B or later phases MAY generate prompts when **user explicitly requests** them
- Example explicit request: "Generate a Claude prompt for skill activation"

**Violation Examples:**
```
❌ "Let me create a prompt for the code-reviewer agent"
❌ "Here's a Claude prompt to use with this skill"
❌ "I'll generate instructions for Claude to follow"
```

**Compliant Examples:**
```
✓ "The code-reviewer agent is documented in .claude/agents/code-reviewer.md"
✓ "Skill content is defined in SKILL.md files"
✓ "Prompt generation is deferred to Phase B when explicitly requested"
```

### Rule 2: Phase Boundaries Are Strict

Each phase has distinct scope. **DO NOT** perform work from future phases without:

1. Completing current phase
2. Obtaining explicit user request
3. Assigning appropriate authority
4. Verifying prerequisites

### Rule 3: Backwards Compatibility

Later phases **MUST NOT** violate Phase A canon without:

1. Major version bump
2. Clear documentation of breaking changes
3. Migration guide
4. Deprecation period

### Rule 4: Fail-Closed at Phase Boundaries

When uncertain whether work belongs in current phase:

- **STOP**
- **ASK** user for clarification
- **DOCUMENT** ambiguity
- **WAIT** for explicit direction

**DO NOT** infer that work is appropriate for current phase.

---

## Branching Requirements (Recorded Only)

**Note:** This is a **recorded requirement**, not an executable instruction.

### Future Claude Execution Branches

When Claude Code operates in execution mode (Phase B+), all branches **MUST**:

1. **Branch from:** `Project420_Claude`
2. **Naming convention:** Include phase identifier in branch name
3. **Examples:**
   - `Project420_Claude/phase-b-impl-skills`
   - `Project420_Claude/phase-c-jira-integration`
   - `Project420_Claude/phase-d-monitoring`

### Current Branch

Current work (Phase A canonisation) is on:
- Branch: `claude/foundation-phase-a-canon-0xzBZ`

This branch naming follows the current session convention.

### Phase A Action

Phase A records this requirement but **does NOT**:
- Create the `Project420_Claude` branch
- Enforce branch naming
- Perform git operations beyond documentation commits

**Implementation:** Deferred to Phase B+ when execution begins.

---

## Phase Progression Summary

| Phase | Status | Authority | Work Type | Prompt Generation |
|-------|--------|-----------|-----------|-------------------|
| **A: Canonisation** | ✓ Active | GC_CLAUDE_FOUNDATION-A_CANON | Documentation | ❌ Prohibited |
| **B: Implementation** | Not Started | Unassigned | Runtime behavior | ✓ When requested |
| **C: Integration** | Not Started | Unassigned | External systems | ✓ When requested |
| **D: Operations** | Not Started | Unassigned | Monitoring | ✓ When requested |
| **E: Evolution** | Not Started | Unassigned | Refinement | ✓ When requested |

---

## Acceptance Criteria

This phase map is complete when:

1. ✓ All five phases are defined
2. ✓ Phase A objectives and deliverables are complete
3. ✓ Future phase objectives are proposed (not committed)
4. ✓ Authority transitions are documented
5. ✓ Explicit rules are stated (no prompts unless requested)
6. ✓ Branching requirements are recorded
7. ✓ Phase boundaries are clear

---

## Phase A Completion Statement

### Status: COMPLETE

All Phase A deliverables have been created:

1. ✓ CLAUDE_FOUNDATION_SCOPE.md
2. ✓ CLAUDE_FOUNDATION_CAPABILITIES_REGISTER.md
3. ✓ PROJECT_ADOPTION_CONTRACT.md
4. ✓ ATTACHMENT_PACKAGING_STRATEGY.md
5. ✓ FAIL_CLOSED_AUTOMATION_POLICY.md
6. ✓ INTEGRATIONS_POLICY.md
7. ✓ PHASE_MAP.md

### Next Steps

**STOP HERE.**

Phase A is complete. Do not proceed to Phase B or generate Claude prompts.

To continue:
1. User must explicitly request Phase B work
2. Authority must be assigned for Phase B
3. Implementation scope must be defined

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Phase A Status:** ✓ COMPLETE - STOP
