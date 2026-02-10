# P420 COMMAND REFERENCE

**Quick reference for commanding P420 agents**

---

## COMMAND SYNTAX

### Basic Format
```
@p420/[agent]: [action] [target]
```

### With Context
```
@p420/[agent]: [action] [target]
CONTEXT: [relevant information]
CONSTRAINTS: [limitations]
OUTPUT: [expected format]
```

---

## EXECUTOR COMMANDS

### Implementation
```
@p420/executor: implement [feature-name]
SPEC: [specification or requirements]
```

```
@p420/executor: create [component-type] [name]
LOCATION: [target directory]
PATTERN: [design pattern to follow]
```

### Modification
```
@p420/executor: fix [issue-description]
FILE: [target file]
```

```
@p420/executor: refactor [target]
GOAL: [improvement objective]
PRESERVE: [what must not change]
```

```
@p420/executor: update [target]
CHANGES: [list of modifications]
```

### Deletion
```
@p420/executor: remove [target]
REASON: [why removing]
CONFIRM: yes
```

---

## REVIEWER COMMANDS

### Code Review
```
@p420/reviewer: review [file-or-directory]
FOCUS: [specific concerns]
```

```
@p420/reviewer: analyze [aspect]
TARGET: [file-or-component]
DEPTH: [surface|standard|deep]
```

### Validation
```
@p420/reviewer: validate [implementation]
AGAINST: [specification or requirements]
```

```
@p420/reviewer: check [criteria]
TARGET: [scope]
STANDARD: [reference standard]
```

### Audit
```
@p420/reviewer: audit [scope]
CRITERIA: [evaluation criteria]
```

---

## ARCHITECT COMMANDS

### Design
```
@p420/architect: design [system-or-feature]
REQUIREMENTS: [list]
CONSTRAINTS: [limitations]
```

```
@p420/architect: structure [codebase-area]
GOALS: [organizational objectives]
```

### Planning
```
@p420/architect: plan [implementation]
SCOPE: [what to implement]
APPROACH: [preferred strategy]
```

### Evaluation
```
@p420/architect: evaluate [options]
CRITERIA: [evaluation criteria]
```

### Specification
```
@p420/architect: specify [component-or-api]
INTERFACE: [expected interface]
```

---

## SECURITY COMMANDS

### Scanning
```
@p420/security: scan [target]
FOCUS: [vulnerability types]
```

```
@p420/security: audit [scope]
STANDARDS: [security standards to check]
```

### Analysis
```
@p420/security: analyze [flow-or-component]
THREATS: [threat types to consider]
```

```
@p420/security: threat-model [system]
ACTORS: [who might attack]
ASSETS: [what's valuable]
```

### Hardening
```
@p420/security: harden [target]
LEVEL: [basic|standard|strict]
```

---

## TESTER COMMANDS

### Test Generation
```
@p420/tester: test [component]
FRAMEWORK: [jest|vitest|playwright]
COVERAGE: [target percentage]
```

```
@p420/tester: cover [feature]
SCENARIOS: [list of scenarios]
```

### Mocking
```
@p420/tester: mock [dependency]
INTERFACE: [interface to mock]
SCENARIOS: [mock scenarios]
```

### Analysis
```
@p420/tester: analyze [test-directory]
IDENTIFY: [gaps|quality|patterns]
```

### Strategy
```
@p420/tester: strategy [scope]
TYPE: [unit|integration|e2e]
```

---

## COORDINATOR COMMANDS

### Orchestration
```
@p420/coordinator: coordinate [task]
REQUIREMENTS: [full requirements]
```

```
@p420/coordinator: workflow [workflow-name]
INPUTS: [starting inputs]
```

### Routing
```
@p420/coordinator: route [request]
```

### Planning
```
@p420/coordinator: plan [complex-task]
BREAKDOWN: [how to decompose]
```

### Aggregation
```
@p420/coordinator: aggregate [results]
FORMAT: [output format]
```

---

## COMMON MODIFIERS

### Context Modifiers
| Modifier | Purpose | Example |
|----------|---------|---------|
| `CONTEXT:` | Background info | `CONTEXT: React 18, TypeScript 5` |
| `CONSTRAINTS:` | Limitations | `CONSTRAINTS: No external deps` |
| `SCOPE:` | Boundaries | `SCOPE: src/components/` |
| `TARGET:` | Specific file/dir | `TARGET: src/auth/login.ts` |

### Output Modifiers
| Modifier | Purpose | Example |
|----------|---------|---------|
| `OUTPUT:` | Expected format | `OUTPUT: JSON config` |
| `FORMAT:` | Response style | `FORMAT: Brief summary` |
| `INCLUDE:` | Extra info | `INCLUDE: Examples` |
| `EXCLUDE:` | Skip info | `EXCLUDE: Boilerplate` |

### Quality Modifiers
| Modifier | Purpose | Example |
|----------|---------|---------|
| `DEPTH:` | Analysis depth | `DEPTH: Deep` |
| `COVERAGE:` | Target coverage | `COVERAGE: 90%` |
| `PRIORITY:` | Focus area | `PRIORITY: Security` |
| `LEVEL:` | Strictness | `LEVEL: Strict` |

---

## QUICK EXAMPLES

### Implement a Login Form
```
@p420/executor: implement login form
CONTEXT: React, TypeScript, Formik
CONSTRAINTS: Email and password fields, validation required
OUTPUT: LoginForm.tsx component
```

### Review Authentication Module
```
@p420/reviewer: review src/auth/
FOCUS: security, error handling
DEPTH: deep
```

### Design API Architecture
```
@p420/architect: design REST API for user management
REQUIREMENTS:
- CRUD operations
- Authentication required
- Rate limiting
CONSTRAINTS: Express.js, PostgreSQL
```

### Security Scan API Routes
```
@p420/security: scan src/routes/
FOCUS: injection, authentication
STANDARDS: OWASP Top 10
```

### Generate Tests for Utils
```
@p420/tester: test src/utils/
FRAMEWORK: Jest
COVERAGE: 95%
INCLUDE: Edge cases
```

### Coordinate Feature Development
```
@p420/coordinator: coordinate user profile feature
REQUIREMENTS:
- View/edit profile
- Avatar upload
- Privacy settings
```

---

**End of Command Reference**
