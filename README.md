# StateraGroup AI Governance Framework

> A fail-closed governance system for AI-assisted software development.

This repository contains a **documentation-first governance framework** for managing AI agent interactions in software development. Unlike typical automation-first approaches, this system enforces manual accountability, scope boundaries, and evidence requirements through a structured agent role system.

## What This Is

This is **not** a typical Claude Code showcase focused on automation and productivity hacks.

This **is** a governance framework that:
- Defines clear roles and authorities for AI agents
- Enforces fail-closed boundaries (no implicit permissions)
- Requires GitHub as the sole canonical system of record
- Mandates evidence-based decision-making
- Prevents scope creep and silent assumptions

**Core Principle:** AI agents are powerful tools that require explicit constraints, not implicit trust.

---

## Repository Status

**Current Phase:** Phase D.2 Complete (Cross-Consistency Audit Applied)

### Recent Updates
- ✅ **Phase D.2 Audit**: 7 consistency defects identified and patched
- ✅ **Skill Ownership**: All 8 skills now have explicit executor roles
- ✅ **GitHub-Only Evidence**: External system references removed
- ✅ **Response Contract Alignment**: All roles standardized to canonical format

See [PR #12](https://github.com/Statera-Jason/claude-code-showcase/pull/12) for full details.

---

## Architecture

### The Four Agents

This governance system defines **4 agent roles** with explicit authorities and prohibitions:

| Agent | Purpose | Authority | Prohibitions |
|-------|---------|-----------|--------------|
| **GPT_PAC** | Problem framing & scope definition | Define scope, acceptance criteria, evidence requirements | Must not implement code, approve without evidence |
| **Claude Implementer** | Execute approved designs | Write code, tests, documentation within scope | Must not reframe problems, change scope |
| **Claude Reviewer** | Validate correctness & evidence | Approve/reject PRs, verify alignment | Must not implement fixes, approve without evidence |
| **Governance Auditor** | Enforce scope & prohibitions | Veto work, block merges | Must not relax controls, assume intent |

**Key Constraint:** No agent may exceed its declared authority. All boundaries are fail-closed.

📄 **Documentation:** [docs/governance/roles/](docs/governance/roles/)

### The Eight Skills

The framework defines **8 canonical skills** mapped to the agent handoff protocol:

| Skill | Primary Executor | Protocol Step |
|-------|------------------|---------------|
| **01. Work Item Intake** | GPT_PAC | 1. Create GitHub Issue |
| **02. Technical Design** | GPT_PAC | 1. Create GitHub Issue |
| **03. Implementation** | Claude Implementer | 2. Produce PR |
| **04. Code Review** | Claude Reviewer | 3. Validate |
| **05. Testing Evidence** | Claude Implementer | 2. Produce PR |
| **06. Doc Update** | Claude Implementer | 2. Produce PR |
| **07. Scope Audit** | Governance Auditor | 4. Audit |
| **08. Release Notes** | GPT_PAC | Post-completion |

**Key Constraint:** Each skill explicitly defines who may execute it and who must not (separation of concerns).

📄 **Documentation:** [docs/skills/](docs/skills/)

### Handoff Protocol

```
┌─────────────┐
│  GPT_PAC    │  1. Authors GitHub Issue with scope, acceptance criteria
└──────┬──────┘
       │
       ▼
┌─────────────────────┐
│ Claude Implementer  │  2. Produces PR with tests, docs, evidence
└──────┬──────────────┘
       │
       ▼
┌─────────────────┐
│ Claude Reviewer │  3. Validates correctness, evidence completeness
└──────┬──────────┘
       │
       ▼
┌──────────────────────┐
│ Governance Auditor   │  4. Audits scope boundaries, blocks violations
└──────┬───────────────┘
       │
       ▼
┌─────────────────────┐
│ GitHub (canonical)  │  5. Evidence exists, work complete
└─────────────────────┘
```

**Key Constraint:** GitHub is the sole system of record. No external ticket systems, Confluence, or CI/CD assumptions.

📄 **Documentation:** [docs/governance/agents/overview.md](docs/governance/agents/overview.md)

---

## Core Principles

### 1. Fail-Closed by Default

**What this means:**
- If a required input is missing → **STOP** (do not infer or assume)
- If authority is unclear → **STOP** (do not proceed without explicit permission)
- If evidence is absent → **STOP** (do not approve or continue)

**Why this matters:**
AI agents are prone to "helpfulness creep"—doing more than asked because it seems useful. Fail-closed boundaries prevent scope bleed, unauthorized changes, and silent assumptions.

### 2. GitHub as Canonical System

**What this means:**
- All work items are GitHub Issues
- All implementations are Pull Requests
- All evidence is committed, attached, or linked in GitHub
- No ticketing systems (JIRA, Linear), no Confluence, no external tools unless explicitly authorized

**Why this matters:**
Multiple systems of record create ambiguity about what is authoritative. Single source of truth enables deterministic audits.

📄 **Policy:** [docs/canon/FAIL_CLOSED_AUTOMATION_POLICY.md](docs/canon/FAIL_CLOSED_AUTOMATION_POLICY.md)

### 3. Manual-First Enforcement

**What this means:**
- Human approval required for scope changes
- No CI/CD assumed or required (must be explicitly authorized)
- No automation unless documented in GitHub Issue

**Why this matters:**
Automation often masks governance violations. Manual-first ensures accountability remains with humans.

### 4. Evidence-Based Decision Making

**What this means:**
- Every approval requires traceable evidence
- "Looks good" is not evidence
- Evidence must exist in GitHub before approval

**Why this matters:**
Post-hoc reconstruction of "why we approved this" is unreliable. Evidence captured at decision time is auditable.

📄 **Contract:** [docs/governance/response_contract.md](docs/governance/response_contract.md)

---

## Directory Structure

```
claude-code-showcase/
├── README.md                          # This file
├── CLAUDE.md                          # Project instructions (example)
│
├── docs/
│   ├── canon/                         # Canonical policies
│   │   ├── FAIL_CLOSED_AUTOMATION_POLICY.md
│   │   ├── ISSUE_SCHEMA.md
│   │   ├── GITHUB_WORK_MANAGEMENT_SPEC.md
│   │   └── ...
│   │
│   ├── governance/                    # Agent system governance
│   │   ├── response_contract.md       # Mandatory response format
│   │   ├── agents/
│   │   │   └── overview.md            # Agent system architecture
│   │   └── roles/
│   │       ├── gpt_pac.md             # GPT_PAC role definition
│   │       ├── claude_implementer.md  # Claude Implementer role
│   │       ├── claude_reviewer.md     # Claude Reviewer role
│   │       └── governance_auditor.md  # Governance Auditor role
│   │
│   ├── skills/                        # 8 canonical skills
│   │   ├── 01_work_item_intake.md
│   │   ├── 02_technical_design.md
│   │   ├── 03_implementation.md
│   │   ├── 04_code_review.md
│   │   ├── 05_testing_evidence.md
│   │   ├── 06_doc_update.md
│   │   ├── 07_scope_audit.md
│   │   └── 08_release_notes.md
│   │
│   └── pac/                           # Phase closeout documents
│       └── PHASE_D1_CLOSEOUT.md
│
└── .claude/                           # Claude Code configuration (examples)
    ├── settings.json                  # Hooks & environment
    ├── agents/                        # Custom agents (examples)
    ├── commands/                      # Slash commands (examples)
    └── skills/                        # Skills (examples, not governance)
```

---

## Phase D.2 Audit Results

The Phase D.2 cross-consistency audit identified and resolved **7 governance defects**:

### Defects Resolved

1. ✅ **Skills lacked explicit executor roles** → Added "Authorized Roles" section to all 8 skills
2. ✅ **External system references violated GitHub-only policy** → Removed ticketing/Confluence/CI assumptions
3. ✅ **Response contract section list mismatch** → Standardized all 4 roles to canonical 6-section format
4. ✅ **SKILL 05 referenced CI without authorization guard** → Added explicit authorization requirement
5. ✅ **Skills not mapped to handoff protocol** → Added Skill-to-Protocol mapping to AGENTS_OVERVIEW
6. ✅ **PAC ownership ambiguities for Skills 02, 05, 08** → Applied binding PAC rulings
7. ✅ **Missing file path references in role docs** → Added explicit docs/governance/response_contract.md references

### Audit Findings

- **Files Audited:** 14 (6 roles/agents + 8 skills)
- **Defects Found:** 7
- **Pass Findings:** 4
- **Status:** ACCEPTED WITH REQUIRED CHANGES (all patches applied)

📄 **Full Report:** See commit `3aed39d` and PR description for detailed findings and patches.

---

## How to Use This Framework

### For AI Agent Developers

If you're building AI-assisted development systems, this framework provides:
- **Agent role definitions** with clear boundaries
- **Skill-based task decomposition** with explicit ownership
- **Fail-closed governance patterns** to prevent overreach
- **Evidence requirements** for auditable decision-making

**Start here:** [docs/governance/agents/overview.md](docs/governance/agents/overview.md)

### For Project Adopters

If you want to adopt this governance approach in your project:

1. **Review the canonical policies** in `docs/canon/`
2. **Adapt the role definitions** in `docs/governance/roles/` to your team structure
3. **Customize the skills** in `docs/skills/` to your development workflow
4. **Enforce GitHub-only evidence** by removing external system dependencies
5. **Implement the handoff protocol** in your PR/review process

**Note:** This is a documentation framework, not a runtime system. Enforcement is manual by design.

### For Governance Auditors

If you're auditing AI-assisted development processes:

- **Check for fail-closed boundaries**: Are missing inputs handled by stopping, or by inferring?
- **Verify evidence exists**: Are approvals backed by traceable artifacts in GitHub?
- **Audit scope boundaries**: Did delivered work match approved scope exactly?
- **Check role separation**: Did any agent exceed its declared authority?

**Audit checklist:** [docs/skills/07_scope_audit.md](docs/skills/07_scope_audit.md)

---

## What Makes This Different

### Typical AI Automation Approach
❌ "Claude, implement this feature" → Claude guesses requirements, builds something, hopes it's right
❌ Automation assumed (CI/CD runs tests automatically)
❌ Approval based on "looks good"
❌ Scope creep accepted as "helpful additions"
❌ Evidence collected post-hoc when someone asks

### StateraGroup Governance Approach
✅ GPT_PAC defines scope + acceptance criteria → Claude Implementer builds exactly that → Claude Reviewer validates against criteria → Governance Auditor checks scope compliance
✅ Manual-first (CI/CD must be explicitly authorized)
✅ Approval requires traceable evidence in GitHub
✅ Scope changes blocked unless explicitly approved by GPT_PAC
✅ Evidence captured at decision time, linked to work items

**The difference:** Accountability remains with humans. AI agents are powerful tools with explicit constraints, not autonomous decision-makers.

---

## Non-Goals

This framework is **not** designed to:
- ❌ Replace human accountability
- ❌ Automate governance decisions
- ❌ Maximize AI agent autonomy
- ❌ Optimize for speed over correctness
- ❌ Allow AI agents to act without evidence

**If you want maximum automation and minimal constraints, this framework is not for you.**

**If you want auditable, fail-closed governance with explicit boundaries, this framework is for you.**

---

## Contributing

This repository is maintained by **StateraGroup** as part of the Claude Foundation governance program.

### Governance Changes

Changes to the governance framework (docs/governance/**, docs/skills/**, docs/canon/**) require:
1. GitHub Issue authored by GPT_PAC with scope + acceptance criteria
2. PR implementing exactly the approved scope
3. Review by Claude Reviewer with evidence validation
4. Scope audit by Governance Auditor

**No exceptions.** This framework is self-enforcing.

### Example Configurations

Changes to .claude/** (example agents, commands, skills) can be contributed via standard PR without governance workflow, as these are reference examples only.

---

## License

MIT License - See [LICENSE](LICENSE) for details.

This framework is provided as-is for teams building AI-assisted development systems with strong governance requirements.

---

## Questions?

- **Framework documentation:** [docs/governance/agents/overview.md](docs/governance/agents/overview.md)
- **Canonical policies:** [docs/canon/](docs/canon/)
- **Skill definitions:** [docs/skills/](docs/skills/)
- **Phase history:** [docs/pac/](docs/pac/)

For governance questions, file a GitHub Issue following the intake process defined in [docs/skills/01_work_item_intake.md](docs/skills/01_work_item_intake.md).
