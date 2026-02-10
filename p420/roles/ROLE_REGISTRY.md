# P420 ROLE REGISTRY

**Central registry of all P420 agent roles and their relationships**

---

## ROLE HIERARCHY

```
                    ┌─────────────────┐
                    │   COORDINATOR   │
                    │  (Orchestrator) │
                    └────────┬────────┘
                             │
         ┌───────────────────┼───────────────────┐
         │                   │                   │
         ▼                   ▼                   ▼
┌─────────────┐     ┌─────────────┐     ┌─────────────┐
│  ARCHITECT  │     │  EXECUTOR   │     │  REVIEWER   │
│  (Design)   │────▶│  (Implement)│────▶│  (Validate) │
└─────────────┘     └─────────────┘     └─────────────┘
                             │
              ┌──────────────┼──────────────┐
              │              │              │
              ▼              ▼              ▼
       ┌──────────┐   ┌──────────┐   ┌──────────┐
       │ SECURITY │   │  TESTER  │   │   ...    │
       │(Analyze) │   │ (Test)   │   │(Extend)  │
       └──────────┘   └──────────┘   └──────────┘
```

---

## ROLE DEFINITIONS

| Role ID | Type | Primary Function | Model |
|---------|------|------------------|-------|
| `@p420/coordinator` | Coordinator | Task routing & orchestration | opus |
| `@p420/architect` | Specialist | Design & technical decisions | opus |
| `@p420/executor` | Executor | Code implementation | opus/sonnet |
| `@p420/reviewer` | Reviewer | Code analysis & validation | opus |
| `@p420/security` | Specialist | Security analysis | opus |
| `@p420/tester` | Specialist | Test generation | sonnet/opus |

---

## ROLE INTERACTIONS

### Primary Flow
```
Request → Coordinator → [Architect] → Executor → Tester → Reviewer
```

### Security Flow
```
Request → Coordinator → Security → Executor → Security (verify) → Reviewer
```

### Direct Execution (Simple Tasks)
```
Request → Executor → [output]
```

### Review Only
```
Request → Reviewer → [findings]
```

---

## CAPABILITY MATRIX

| Capability | Coordinator | Architect | Executor | Reviewer | Security | Tester |
|------------|:-----------:|:---------:|:--------:|:--------:|:--------:|:------:|
| Route tasks | ✓ | - | - | - | - | - |
| Design systems | - | ✓ | - | - | - | - |
| Write code | - | - | ✓ | - | - | - |
| Review code | - | - | - | ✓ | ✓ | - |
| Generate tests | - | - | - | - | - | ✓ |
| Security scan | - | - | - | - | ✓ | - |
| Create specs | - | ✓ | - | - | - | - |
| Modify files | - | - | ✓ | - | - | - |
| Aggregate results | ✓ | - | - | - | - | - |

---

## ROLE BOUNDARIES (PROHIBITIONS)

### What NO Role May Do:
- Modify governance files (../canon/, ../policies/, ../CLAUDE.md)
- Access production systems without explicit permission
- Store or manage credentials/secrets
- Make irreversible changes without confirmation
- Assume another role's capabilities

### Role-Specific Prohibitions:

| Role | Cannot |
|------|--------|
| Coordinator | Execute, review, design directly |
| Architect | Write production code, review |
| Executor | Review, design, security audit |
| Reviewer | Modify code, design, test |
| Security | Modify code, deploy, certify compliance |
| Tester | Modify production code, review |

---

## HANDOFF PROTOCOLS

### Coordinator → Any Agent
```
ROUTING TO: @p420/[agent]
TASK: [description]
CONTEXT: [relevant state]
EXPECTED OUTPUT: [what to return]
```

### Architect → Executor
```
HANDOFF TO: @p420/executor
DESIGN: [specification reference]
IMPLEMENTATION NOTES: [guidance]
CONSTRAINTS: [limitations]
```

### Executor → Reviewer
```
HANDOFF TO: @p420/reviewer
CHANGES: [list of modifications]
FILES: [affected files]
REVIEW FOCUS: [specific concerns]
```

### Reviewer → Executor (Fixes Needed)
```
HANDOFF TO: @p420/executor
FINDINGS: [issues to address]
PRIORITY: [order of fixes]
CONSTRAINTS: [what not to change]
```

### Security → Executor (Remediation)
```
HANDOFF TO: @p420/executor
VULNERABILITIES: [list with severity]
REMEDIATION: [specific fixes]
VERIFICATION: [how to confirm fix]
```

---

## EXTENDING THE REGISTRY

To add a new P420 role:

1. Create role definition in `/p420/agents/[role].md`
2. Add to this registry table
3. Define capability matrix entry
4. Specify prohibitions
5. Document handoff protocols

### Role Template
```markdown
# P420 [ROLE_NAME] AGENT

**Role ID:** `@p420/[role]`
**Type:** [Executor|Reviewer|Coordinator|Specialist]
**Model:** [opus|sonnet]

## IDENTITY
[who this agent is]

## CAPABILITIES
[what it can do]

## BOUNDARIES
[what it cannot do]

## BEHAVIORAL TRIGGERS
[command patterns]

## RESPONSE FORMAT
[output structure]
```

---

**End of Role Registry**
