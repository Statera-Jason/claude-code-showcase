# P420 Settings Documentation

**Configuration reference for P420 agent framework**

---

## Settings Structure

The `settings.json` file controls P420 behavior:

```json
{
  "$schema": "https://claude.ai/claude-code/settings.schema.json",
  "name": "P420 Agent Framework",
  "version": "1.0.0",
  "permissions": { ... },
  "env": { ... },
  "hooks": { ... },
  "agents": { ... }
}
```

---

## Permissions

Controls which tools P420 agents can use:

```json
{
  "permissions": {
    "allow": [
      "Read",      // Read files
      "Write",     // Create files
      "Edit",      // Modify files
      "Glob",      // Find files
      "Grep",      // Search content
      "Bash",      // Execute commands
      "Task",      // Spawn sub-agents
      "WebFetch",  // Fetch URLs
      "WebSearch", // Web search
      "TodoWrite"  // Task tracking
    ]
  }
}
```

### Restricting Permissions

To create a read-only configuration:

```json
{
  "permissions": {
    "allow": ["Read", "Glob", "Grep"]
  }
}
```

---

## Environment Variables

Set environment variables for P420 execution:

```json
{
  "env": {
    "P420_MODE": "active",
    "P420_VERSION": "1.0.0",
    "P420_STRICT": "false"
  }
}
```

### Available Variables

| Variable | Values | Description |
|----------|--------|-------------|
| `P420_MODE` | active, disabled | Enable/disable P420 |
| `P420_VERSION` | semver | Framework version |
| `P420_STRICT` | true, false | Strict boundary enforcement |

---

## Hooks

Hooks control pre/post tool execution:

### UserPromptSubmit

Runs when user submits a prompt:

```json
{
  "hooks": {
    "UserPromptSubmit": [
      {
        "matcher": "@p420/",
        "command": "echo '{\"feedback\": \"P420 Agent Activated\"}'"
      }
    ]
  }
}
```

### PreToolUse

Runs before a tool executes (can block):

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "event": "Edit",
        "command": "# Block protected paths\ncase \"$CLAUDE_TOOL_INPUT_FILE_PATH\" in\n  */.git/*) echo '{\"decision\": \"block\"}' ;;\n  *) echo '{\"decision\": \"allow\"}' ;;\nesac"
      }
    ]
  }
}
```

### PostToolUse

Runs after a tool executes:

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "event": "Write",
        "command": "echo '{\"feedback\": \"File created\"}'"
      }
    ]
  }
}
```

---

## Agents

Register P420 agents:

```json
{
  "agents": {
    "p420-executor": {
      "definition": ".claude/agents/p420-executor.md",
      "model": "opus"
    },
    "p420-reviewer": {
      "definition": ".claude/agents/p420-reviewer.md",
      "model": "opus"
    }
  }
}
```

### Agent Configuration

| Property | Type | Description |
|----------|------|-------------|
| `definition` | path | Path to agent .md file |
| `model` | string | Model to use (opus, sonnet) |

---

## Protected Paths Configuration

Default protected paths:

```json
{
  "hooks": {
    "PreToolUse": [
      {
        "event": "Edit",
        "command": "case \"$CLAUDE_TOOL_INPUT_FILE_PATH\" in\n  */.git/*|*/.env*|*/node_modules/*)\n    echo '{\"decision\": \"block\", \"reason\": \"Protected path\"}'\n    ;;\n  *)\n    echo '{\"decision\": \"allow\"}'\n    ;;\nesac"
      }
    ]
  }
}
```

### Adding Custom Protected Paths

```bash
*/.git/*           # Git directory
*/.env*            # Environment files
*/node_modules/*   # Dependencies
*/secrets/*        # Custom: secrets directory
*/.aws/*           # Custom: AWS credentials
```

---

## Model Selection

Recommended models per agent type:

| Agent Type | Recommended | Acceptable |
|------------|-------------|------------|
| Executor | opus | sonnet |
| Reviewer | opus | - |
| Architect | opus | - |
| Security | opus | - |
| Tester | sonnet | opus |
| Coordinator | opus | - |

---

## Example Configurations

### Development Mode

```json
{
  "env": {
    "P420_MODE": "active",
    "P420_STRICT": "false"
  },
  "permissions": {
    "allow": ["Read", "Write", "Edit", "Glob", "Grep", "Bash", "Task"]
  }
}
```

### Production Review Mode

```json
{
  "env": {
    "P420_MODE": "active",
    "P420_STRICT": "true"
  },
  "permissions": {
    "allow": ["Read", "Glob", "Grep"]
  }
}
```

### CI/CD Mode

```json
{
  "env": {
    "P420_MODE": "active"
  },
  "permissions": {
    "allow": ["Read", "Glob", "Grep", "Bash"]
  }
}
```

---

**End of Settings Documentation**
