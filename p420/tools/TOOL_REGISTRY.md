# P420 TOOL REGISTRY

**Available tools and their permissions per agent role**

---

## TOOL CATEGORIES

### File Operations
| Tool | Description |
|------|-------------|
| `Read` | Read file contents |
| `Write` | Create new files |
| `Edit` | Modify existing files |
| `Glob` | Find files by pattern |
| `Grep` | Search file contents |

### Code Execution
| Tool | Description |
|------|-------------|
| `Bash` | Execute shell commands |
| `Task` | Spawn sub-agents |

### External
| Tool | Description |
|------|-------------|
| `WebFetch` | Fetch web content |
| `WebSearch` | Search the web |

### Management
| Tool | Description |
|------|-------------|
| `TodoWrite` | Track task progress |
| `NotebookEdit` | Edit Jupyter notebooks |

---

## TOOL PERMISSIONS BY ROLE

| Tool | Coordinator | Architect | Executor | Reviewer | Security | Tester |
|------|:-----------:|:---------:|:--------:|:--------:|:--------:|:------:|
| Read | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Write | - | ✓* | ✓ | - | - | ✓ |
| Edit | - | - | ✓ | - | - | ✓* |
| Glob | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Grep | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| Bash | ✓* | ✓* | ✓ | ✓* | ✓* | ✓ |
| Task | ✓ | - | - | - | - | - |
| WebFetch | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| WebSearch | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |
| TodoWrite | ✓ | ✓ | ✓ | ✓ | ✓ | ✓ |

**Legend:**
- ✓ = Full access
- ✓* = Limited access (see restrictions below)
- `-` = No access

---

## TOOL RESTRICTIONS

### Write Restrictions
| Role | Restriction |
|------|-------------|
| Architect | Specs and documentation only (no production code) |
| Tester | Test files only (`*.test.ts`, `*.spec.ts`) |

### Edit Restrictions
| Role | Restriction |
|------|-------------|
| Tester | Test files only |

### Bash Restrictions
| Role | Restriction |
|------|-------------|
| Coordinator | Read-only commands (ls, cat, git status) |
| Architect | Build/compile commands, no file modification |
| Reviewer | Linting, type checking, test execution |
| Security | Security scanning tools only |

---

## TOOL USAGE PATTERNS

### Read Tool
```
Purpose: Examine file contents
Use when: Need to understand existing code
Roles: All agents
```

### Write Tool
```
Purpose: Create new files
Use when: New component, new test, new spec
Roles: Executor, Tester, Architect (limited)
Caution: Check if file exists first
```

### Edit Tool
```
Purpose: Modify existing files
Use when: Bug fix, feature addition, refactor
Roles: Executor, Tester (tests only)
Caution: Read file first to understand context
```

### Glob Tool
```
Purpose: Find files by pattern
Use when: Locating files by name/extension
Roles: All agents
Example: "src/**/*.ts" to find all TypeScript files
```

### Grep Tool
```
Purpose: Search file contents
Use when: Finding code patterns, usages, definitions
Roles: All agents
Example: Search for function calls, imports
```

### Bash Tool
```
Purpose: Execute shell commands
Use when: Running builds, tests, git operations
Roles: Various (with restrictions)
Caution: Avoid destructive commands
```

### Task Tool
```
Purpose: Spawn sub-agents for complex work
Use when: Multi-step operations, parallel work
Roles: Coordinator only
Example: Route to specialist agents
```

---

## PROTECTED PATHS

Tools must NOT modify files in these paths:

| Path | Reason |
|------|--------|
| `/canon/*` | Governance documents |
| `/policies/*` | Policy definitions |
| `/CLAUDE.md` | Main execution contract |
| `/.git/*` | Git internals |
| `/.env*` | Environment/secrets |
| `/node_modules/*` | Dependencies |

---

## TOOL SAFETY RULES

### Before Write/Edit:
1. Read the file first (if exists)
2. Verify path is not protected
3. Confirm action aligns with role

### Before Bash:
1. Review command for destructive operations
2. Avoid: `rm -rf`, `git push -f`, `DROP TABLE`
3. Prefer dry-run where available

### Before Task:
1. Verify correct agent for the task
2. Provide complete context
3. Define expected output

---

## COMMAND EXAMPLES BY ROLE

### Executor (Full Access)
```
Read: Understand existing code
Edit: Apply changes
Bash: Run build, tests
Write: Create new files
```

### Reviewer (Read-Heavy)
```
Read: Examine code under review
Grep: Find patterns and usages
Bash: Run linting, type-check
```

### Security (Scan-Focused)
```
Read: Examine code for vulnerabilities
Grep: Search for security patterns
Bash: Run security scanning tools
```

### Tester (Test-Focused)
```
Read: Understand code to test
Write: Create test files
Edit: Update test files
Bash: Run test suite
```

### Architect (Design-Focused)
```
Read: Understand existing architecture
Glob: Survey codebase structure
Write: Create specifications
```

### Coordinator (Orchestration)
```
Read: Gather context
Task: Route to specialists
TodoWrite: Track workflow
```

---

**End of Tool Registry**
