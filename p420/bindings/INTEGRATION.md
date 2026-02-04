# P420 Integration Guide

**How to bind P420 agents to your repository**

---

## Quick Integration

### Option 1: Copy Bindings Directory

```bash
# From your project root
cp -r path/to/p420/bindings/.claude ./.claude
cp path/to/p420/bindings/CLAUDE.md ./CLAUDE.md
```

### Option 2: Git Submodule

```bash
git submodule add https://github.com/[org]/StateraGroup-AI-Control.git vendor/ai-control
ln -s vendor/ai-control/p420/bindings/.claude .claude
ln -s vendor/ai-control/p420/bindings/CLAUDE.md CLAUDE.md
```

### Option 3: npm Package (if published)

```bash
npm install @statera/p420-agents --save-dev
npx p420-init
```

---

## What Gets Installed

```
your-project/
├── CLAUDE.md                    # P420 execution contract
└── .claude/
    ├── settings.json            # P420 configuration
    └── agents/
        ├── p420-executor.md     # Code implementation
        ├── p420-reviewer.md     # Code review
        ├── p420-architect.md    # Design/architecture
        ├── p420-security.md     # Security analysis
        ├── p420-tester.md       # Test generation
        └── p420-coordinator.md  # Multi-agent orchestration
```

---

## Verification

After integration, verify agents are available:

```
@p420/executor: echo test
```

Expected response:
```
@p420/executor
STATUS: COMPLETE

EXECUTED:
- Echo test command received

OUTPUT:
P420 Executor agent is active and ready.
```

---

## Configuration Options

### Customize settings.json

```json
{
  "env": {
    "P420_MODE": "active",
    "P420_STRICT": "true"      // Enable strict mode
  }
}
```

### Add Project-Specific Agents

Create additional agents in `.claude/agents/`:

```markdown
# Custom Agent

model: opus
trigger: "@p420/custom"

## Role
[Define custom role]

## Capabilities
[List capabilities]

## Boundaries
[Define limits]
```

---

## Coexistence with Governance Mode

P420 can coexist with full governance (GitHub Issue-based) mode:

### Dual CLAUDE.md Setup

```
your-project/
├── CLAUDE.md           # Currently active contract
├── CLAUDE.governance.md  # Full governance mode (backup)
└── CLAUDE.p420.md      # P420 mode (backup)
```

### Switch Modes

```bash
# Switch to P420 mode
cp CLAUDE.p420.md CLAUDE.md

# Switch to Governance mode
cp CLAUDE.governance.md CLAUDE.md
```

### Mode Indicators

Add to CLAUDE.md header:
```markdown
**Mode:** P420 (Direct Command)
# or
**Mode:** GOVERNANCE (Issue-Bound)
```

---

## Agent Invocation

### Basic Commands

```
@p420/executor: implement [feature]
@p420/reviewer: review [target]
@p420/architect: design [system]
@p420/security: scan [target]
@p420/tester: test [component]
@p420/coordinator: coordinate [task]
```

### With Context

```
@p420/executor: implement user authentication
CONTEXT: Express.js backend, PostgreSQL
CONSTRAINTS: No external auth libraries
OUTPUT: Middleware and routes
```

### Multi-Agent Workflow

```
@p420/coordinator: coordinate payment integration
REQUIREMENTS:
- Stripe integration
- Webhook handling
- Idempotency keys
- Error recovery
```

---

## Protected Paths

By default, P420 agents cannot modify:

| Path | Reason |
|------|--------|
| `/.git/*` | Git internals |
| `/.env*` | Environment/secrets |
| `/node_modules/*` | Dependencies |

### Customize Protected Paths

Edit `.claude/settings.json`:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "event": "Edit",
        "command": "case \"$CLAUDE_TOOL_INPUT_FILE_PATH\" in\n  */.git/*|*/.env*|*/node_modules/*|*/secrets/*)\n    echo '{\"decision\": \"block\"}'\n    ;;\nesac"
      }
    ]
  }
}
```

---

## Extending P420

### Add Custom Agent

1. Create `.claude/agents/p420-[name].md`
2. Follow the agent template structure
3. Add to settings.json agents section
4. Document in team wiki

### Agent Template

```markdown
# P420 [Name] Agent

model: [opus|sonnet]
invoke: manual
trigger: "@p420/[name]"

## Role
[Single sentence purpose]

## Capabilities
- [What it CAN do]

## Boundaries
- [What it CANNOT do]

## Tools
- [Tool]: [Access level]

## Response Format
```
@p420/[name]
STATUS: [codes]
[output structure]
```

## Command Patterns
- `[command]` - [description]
```

---

## Troubleshooting

### Agent Not Responding

1. Check CLAUDE.md is present at repo root
2. Verify .claude/agents/ contains agent definitions
3. Ensure settings.json references agents correctly

### Protected Path Blocking Valid Edit

Edit `.claude/settings.json` to adjust protected paths.

### Wrong Agent Behavior

1. Verify correct agent is being invoked
2. Check agent definition for capabilities
3. Review command format matches patterns

---

## Uninstalling

```bash
rm CLAUDE.md
rm -rf .claude/
# If using submodule
git submodule deinit vendor/ai-control
```

---

## Version Compatibility

| P420 Version | Claude Code Version |
|--------------|---------------------|
| 1.0.0 | 1.x+ |

---

**P420 Framework - Direct Command Execution**
