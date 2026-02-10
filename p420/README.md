# P420 Agent Framework

**Manual-Command AI Agent System for Direct Execution**

---

## Overview

P420 is a role-based agent framework that operates **without GitHub issue requirements**. Agents respond consistently to direct manual commands based on their defined roles.

### Key Features
- **No Issue Required** - Direct command execution
- **Role-Based Behavior** - Consistent, predictable responses
- **Manual Control** - You command, agents execute
- **Clear Boundaries** - Each agent knows its limits
- **Repo-Level Binding** - Drop-in `.claude/` and `CLAUDE.md` integration

---

## Integration (Bind to Your Repo)

### Quick Setup
```bash
# From your project root
cp -r /path/to/p420/bindings/.claude ./.claude
cp /path/to/p420/bindings/CLAUDE.md ./CLAUDE.md
```

### What Gets Installed
```
your-project/
├── CLAUDE.md                    # P420 execution contract (replaces governance)
└── .claude/
    ├── settings.json            # P420 configuration
    ├── settings.md              # Settings documentation
    └── agents/
        ├── p420-executor.md     # Code implementation agent
        ├── p420-reviewer.md     # Code review agent
        ├── p420-architect.md    # Design/architecture agent
        ├── p420-security.md     # Security analysis agent
        ├── p420-tester.md       # Test generation agent
        └── p420-coordinator.md  # Multi-agent orchestration
```

See `bindings/INTEGRATION.md` for detailed integration instructions.

---

## Quick Start

### Command an Agent
```
@p420/executor: implement user login form
CONTEXT: React, TypeScript
```

### Route Complex Tasks
```
@p420/coordinator: coordinate authentication feature
REQUIREMENTS:
- Login/logout
- Session management
- Remember me option
```

---

## Available Agents

| Agent | Role | Use For |
|-------|------|---------|
| `@p420/executor` | Implementation | Writing code, making changes |
| `@p420/reviewer` | Validation | Code review, quality checks |
| `@p420/architect` | Design | Architecture, specifications |
| `@p420/security` | Security | Vulnerability analysis |
| `@p420/tester` | Testing | Test generation, coverage |
| `@p420/coordinator` | Orchestration | Multi-agent workflows |

---

## Directory Structure

```
p420/
├── README.md                    # This file
├── P420_EXECUTION.md            # Main execution contract (reference)
├── agents/                      # Agent definitions (reference)
│   ├── executor.md
│   ├── reviewer.md
│   ├── architect.md
│   ├── security.md
│   ├── tester.md
│   └── coordinator.md
├── roles/                       # Role registry
│   └── ROLE_REGISTRY.md
├── tools/                       # Tool configurations
│   ├── TOOL_REGISTRY.md
│   └── commands.md
├── contracts/                   # Behavioral contracts
│   ├── BEHAVIOR_CONTRACT.md
│   └── RESPONSE_FORMATS.md
└── bindings/                    # REPO-LEVEL INTEGRATION FILES
    ├── CLAUDE.md                # Drop-in execution contract
    ├── INTEGRATION.md           # Integration guide
    └── .claude/                 # Drop-in .claude config
        ├── settings.json
        ├── settings.md
        └── agents/
            ├── p420-executor.md
            ├── p420-reviewer.md
            ├── p420-architect.md
            ├── p420-security.md
            ├── p420-tester.md
            └── p420-coordinator.md
```

---

## Command Syntax

### Basic
```
@p420/[agent]: [action] [target]
```

### With Context
```
@p420/[agent]: [action] [target]
CONTEXT: [background information]
CONSTRAINTS: [limitations]
OUTPUT: [expected format]
```

---

## Common Commands

### Executor
```
@p420/executor: implement [feature]
@p420/executor: fix [bug]
@p420/executor: refactor [target]
@p420/executor: create [component]
```

### Reviewer
```
@p420/reviewer: review [file/directory]
@p420/reviewer: analyze [concern]
@p420/reviewer: check [criteria]
```

### Architect
```
@p420/architect: design [system]
@p420/architect: plan [implementation]
@p420/architect: evaluate [options]
```

### Security
```
@p420/security: scan [target]
@p420/security: audit [scope]
@p420/security: threat-model [system]
```

### Tester
```
@p420/tester: test [component]
@p420/tester: cover [feature]
@p420/tester: mock [dependency]
```

### Coordinator
```
@p420/coordinator: coordinate [task]
@p420/coordinator: route [request]
@p420/coordinator: workflow [name]
```

---

## Response Format

All agents respond with:

```
@p420/[role]
STATUS: [COMPLETE | PARTIAL | FAILED | BLOCKED]

[ROLE-SPECIFIC OUTPUT]

NOTES: (optional)
[Additional observations]
```

---

## Key Principles

### 1. Role Consistency
Agents always behave according to their role definition. Same command → same behavior pattern.

### 2. Explicit Boundaries
Each agent has clear capabilities and prohibitions. They refuse out-of-scope requests.

### 3. Minimal Action
Agents do exactly what's asked, no more. No scope creep.

### 4. Transparent Handoffs
When a task requires another agent, explicit handoff with context.

---

## Differences from Main CLAUDE.md

| Aspect | CLAUDE.md | P420 |
|--------|-----------|------|
| Issue Required | Yes | No |
| Execution Mandate | Required | Not required |
| Command Source | GitHub Issue only | Direct manual |
| Governance | Full PAC control | Role-based self-control |
| Scope Control | External (Issue) | Internal (Role definition) |

---

## Best Practices

### 1. Be Specific
```
Good: @p420/executor: add email validation to LoginForm
Bad:  @p420/executor: improve the form
```

### 2. Provide Context
```
Good:
@p420/executor: implement search
CONTEXT: React, uses existing SearchService
CONSTRAINTS: Must support pagination

Bad:
@p420/executor: implement search
```

### 3. Use the Right Agent
```
Need code? → @p420/executor
Need review? → @p420/reviewer
Need design? → @p420/architect
Complex task? → @p420/coordinator
```

### 4. Check Handoffs
When an agent hands off, follow the suggested command to the target agent.

---

## Extending P420

### Add New Agent
1. Create definition in `agents/[name].md`
2. Add to `roles/ROLE_REGISTRY.md`
3. Define tool permissions in `tools/TOOL_REGISTRY.md`
4. Document commands in `tools/commands.md`

### Agent Template
See `agents/executor.md` for the standard template structure.

---

## Version

P420 Framework v1.0.0

---

**Built for direct, consistent AI agent execution.**
