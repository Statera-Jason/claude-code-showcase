# Fail Closed Automation Policy

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This document establishes the fail-closed doctrine for automation in the claude-code-showcase foundation, the "no inference from absence" rule, action gating principles, and prohibited fail-open behaviors.

---

## Fail-Closed Doctrine

### Core Principle

**Automation MUST fail closed, not open.**

When automation encounters:
- Ambiguity
- Missing configuration
- Unexpected state
- Errors or timeouts

The system MUST:
- ❌ **STOP** - Halt the action
- 🚫 **BLOCK** - Prevent potentially harmful operations
- 📣 **ALERT** - Notify user of the failure
- ⏸️ **WAIT** - Require explicit human decision

The system MUST NOT:
- ✗ Guess or infer intent
- ✗ Proceed with "best effort"
- ✗ Apply defaults that may be inappropriate
- ✗ Silently skip or ignore errors

---

## No Inference from Absence

### Rule: Absence ≠ Permission

**If a configuration, setting, or capability is not explicitly present, its behavior is UNDEFINED.**

Automation MUST NOT infer:

| Absent Element | ❌ MUST NOT Assume | ✓ MUST DO Instead |
|----------------|-------------------|-------------------|
| Missing skill | "Not needed" | Ask user or document requirement |
| Missing hook | "Disabled" | Document default behavior |
| Missing MCP server | "Not authorized" | Block actions requiring that server |
| Missing environment variable | "Use empty value" | Fail with clear error message |
| Missing command | "Not supported" | Fail gracefully, suggest alternatives |

### Examples

#### Example 1: Missing Environment Variable

```json
// .mcp.json
{
  "mcpServers": {
    "jira": {
      "env": {
        "JIRA_HOST": "${JIRA_HOST}"
      }
    }
  }
}
```

**Fail-Closed Behavior:**
```
❌ ERROR: JIRA_HOST environment variable not set
   Required for MCP server: jira
   Set with: export JIRA_HOST="https://yourcompany.atlassian.net"

   Blocking JIRA operations until configured.
```

**Prohibited Fail-Open:**
```
⚠️ WARNING: JIRA_HOST not set, using default: https://localhost:8080
   [INCORRECT - This is dangerous guessing]
```

#### Example 2: Missing Hook Configuration

**Scenario:** PostToolUse hook for `Edit` tool is not configured.

**Fail-Closed Behavior:**
```
No PostToolUse hook configured for Edit tool.
Edit will proceed without post-processing.
(This is documented default behavior - acceptable)
```

**Prohibited Fail-Open:**
```
No PostToolUse hook configured, so I'll run Prettier anyway since most projects want that.
[INCORRECT - This is undocumented inference]
```

#### Example 3: Unknown Command Argument

**Scenario:** `/ticket PROJ-123 --merge`

Where `--merge` flag is not documented.

**Fail-Closed Behavior:**
```
❌ ERROR: Unknown flag: --merge
   Usage: /ticket <ticket-id>

   Halting execution.
```

**Prohibited Fail-Open:**
```
⚠️ Unknown flag --merge ignored, continuing...
[INCORRECT - Silently ignoring input is dangerous]
```

---

## Action Gating Principles

### Principle 1: Explicit Enablement

**High-risk actions MUST require explicit enablement.**

#### High-Risk Actions

- Writing to external systems (JIRA, GitHub issues, Slack)
- Executing destructive operations (delete, drop, rm -rf)
- Committing or pushing code
- Modifying configuration files
- Installing dependencies
- Running builds or deployments

#### Gating Mechanism

High-risk actions require:

1. **Explicit configuration:**
   ```json
   {
     "enabledMcpjsonServers": ["jira"],  // Explicit list
     "allowDestructiveOperations": false  // Default deny
   }
   ```

2. **Confirmation prompts:**
   ```
   About to commit changes to 15 files.
   Continue? [y/N]
   ```

3. **Capability checks:**
   ```javascript
   if (!config.mcpServers.jira.enabled) {
     throw new Error("JIRA MCP server not enabled. Cannot update ticket.");
   }
   ```

### Principle 2: Least Privilege

**Grant minimum permissions necessary.**

| Operation | Default Permission | Rationale |
|-----------|-------------------|-----------|
| Read files | ✓ Allowed | Low risk, high utility |
| Write files | ✓ Allowed (with hooks) | Necessary for primary function, protected by hooks |
| Execute bash | ✓ Allowed (sandboxed) | Necessary, sandboxed by default |
| Network access | ✓ Allowed | Required for MCP, LSP |
| MCP server calls | ❌ Denied by default | High risk, require explicit enablement |
| GitHub Actions | ❌ Disabled by default | Cost and governance concerns |

### Principle 3: Audit Trail

**All high-risk actions MUST be auditable.**

Automation should:

- Log what action was taken
- Log who/what initiated it
- Log when it occurred
- Log the result (success/failure)

Example:
```
[2026-01-09T12:34:56Z] MCP:JIRA:UPDATE ticket=PROJ-123 status="In Review" user=claude-code result=success
```

### Principle 4: Timeout Defaults

**Operations MUST have reasonable timeout defaults.**

| Operation Type | Default Timeout | Rationale |
|----------------|-----------------|-----------|
| File operations | No timeout | Local, fast |
| Bash commands | 7 minutes | Allows builds/tests |
| Hook execution | 5-90 seconds | Depends on hook type |
| MCP server calls | 30 seconds | Network operation |
| Network requests | 30 seconds | Standard HTTP timeout |

**On timeout, MUST fail closed:**
```
❌ TIMEOUT: Operation exceeded 30 second limit
   Operation: MCP:JIRA:GET ticket=PROJ-123

   Halting execution. Check network and retry.
```

---

## Prohibited Fail-Open Behaviors

The following behaviors are **EXPLICITLY PROHIBITED**:

### 1. Silent Failure

❌ **PROHIBITED:**
```javascript
try {
  updateJiraTicket(id, status);
} catch (error) {
  // Silently catch and continue
}
```

✓ **REQUIRED:**
```javascript
try {
  updateJiraTicket(id, status);
} catch (error) {
  console.error(`Failed to update JIRA ticket ${id}:`, error);
  throw error; // Propagate failure
}
```

### 2. Best-Effort Guessing

❌ **PROHIBITED:**
```javascript
if (!config.testCommand) {
  // Guess based on package.json
  config.testCommand = hasJest ? "jest" : hasVitest ? "vitest" : "npm test";
}
```

✓ **REQUIRED:**
```javascript
if (!config.testCommand) {
  throw new Error("testCommand not configured. Set in CLAUDE.md");
}
```

### 3. Undocumented Defaults

❌ **PROHIBITED:**
```javascript
// Applying default that's not documented anywhere
const formatOnSave = config.formatOnSave ?? true; // Hidden default
```

✓ **REQUIRED:**
```javascript
// Default is documented in settings schema
const formatOnSave = config.formatOnSave ?? false; // Explicit, documented default
// OR
if (config.formatOnSave === undefined) {
  throw new Error("formatOnSave must be explicitly set to true or false");
}
```

### 4. Ignoring User Input

❌ **PROHIBITED:**
```javascript
// User provided --dry-run flag
if (args.dryRun) {
  console.log("Dry run mode not supported, ignoring flag");
  // Proceed with real operation anyway
}
```

✓ **REQUIRED:**
```javascript
if (args.dryRun) {
  console.log("Dry run mode:");
  console.log("Would execute:", operation);
  return; // Actually honor the flag
}
```

### 5. Automatic Fallbacks Without Consent

❌ **PROHIBITED:**
```javascript
// Primary operation failed, automatically try alternative
try {
  runPrettier();
} catch {
  console.log("Prettier failed, trying Biome instead");
  runBiome(); // Automatic fallback
}
```

✓ **REQUIRED:**
```javascript
try {
  runPrettier();
} catch (error) {
  console.error("Prettier failed:", error);
  console.log("Alternative: Install Biome and configure formatTool: 'biome'");
  throw error; // Don't automatically fallback
}
```

### 6. Proceeding on Dependency Failure

❌ **PROHIBITED:**
```javascript
// Hook to run tests after edit
try {
  execSync("npm test");
} catch {
  console.log("Tests failed, but continuing anyway");
  // Return success despite test failure
  return { success: true };
}
```

✓ **REQUIRED:**
```javascript
try {
  execSync("npm test");
  return { success: true, message: "Tests passed" };
} catch (error) {
  return { success: false, message: "Tests failed", error };
  // Caller decides whether to block or warn
}
```

### 7. Credential Guessing

❌ **PROHIBITED:**
```javascript
// Try common credential locations
const apiKey = process.env.ANTHROPIC_API_KEY
  || readFileSync("~/.anthropic/api_key")
  || readFileSync("/etc/anthropic/api_key")
  || ""; // Empty fallback
```

✓ **REQUIRED:**
```javascript
const apiKey = process.env.ANTHROPIC_API_KEY;
if (!apiKey) {
  throw new Error("ANTHROPIC_API_KEY environment variable required");
}
```

---

## Automation Safety Checklist

Before implementing any automation, verify:

- [ ] **Explicit Configuration** - All settings are documented and required
- [ ] **No Inference** - System does not guess or assume
- [ ] **Fail Closed** - Errors halt execution
- [ ] **User Feedback** - Clear error messages guide users
- [ ] **Audit Trail** - High-risk actions are logged
- [ ] **Timeout Protection** - Operations have reasonable timeouts
- [ ] **Least Privilege** - Minimum permissions granted
- [ ] **Confirmation** - High-risk actions require confirmation
- [ ] **Reversibility** - Actions can be undone or rolled back
- [ ] **Documentation** - Behavior is fully documented

---

## Exception: Read-Only Operations

The following operations MAY use documented defaults and need not fail closed:

### Permitted Default Behaviors

1. **File Reading:**
   - Default: Read entire file if limit not specified
   - Rationale: Low risk, high utility

2. **Search Operations:**
   - Default: Search current directory if path not specified
   - Rationale: Reasonable default, non-destructive

3. **Display Formatting:**
   - Default: Use terminal width for formatting
   - Rationale: Cosmetic, no side effects

4. **Log Levels:**
   - Default: INFO level if not specified
   - Rationale: Standard practice, non-destructive

### Requirements for Defaults

Even for low-risk operations, defaults MUST be:

1. **Documented** - Stated in configuration reference
2. **Sensible** - Match user expectations
3. **Non-destructive** - Cannot cause data loss
4. **Overridable** - User can explicitly configure

---

## Enforcement

### Phase A Compliance

In Phase A (Canonisation), this policy:

- ✓ Documents principles
- ✓ Provides examples
- ✓ States requirements
- ✗ Does NOT enforce technically (no code execution in Phase A)

### Future Phase Enforcement

In later phases, enforcement may include:

- Automated checks for fail-open patterns
- Code review requirements
- Testing for error handling
- Audit log verification

---

## Acceptance Criteria

This policy is complete when:

1. ✓ Fail-closed doctrine is stated
2. ✓ "No inference from absence" rule is defined
3. ✓ Action gating principles are documented
4. ✓ Prohibited fail-open behaviors are explicit
5. ✓ Examples of correct and incorrect behavior are provided
6. ✓ Safety checklist is available
7. ✓ Exceptions are documented

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
