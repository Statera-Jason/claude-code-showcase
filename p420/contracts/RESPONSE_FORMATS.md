# P420 RESPONSE FORMAT SPECIFICATIONS

**Standard output formats for all P420 agents**

---

## UNIVERSAL RESPONSE STRUCTURE

All P420 agent responses follow this structure:

```
@p420/[role]
STATUS: [STATUS_CODE]

[ROLE-SPECIFIC CONTENT]

NOTES: (optional)
[Additional observations]
```

---

## STATUS CODES

| Code | Meaning | Use When |
|------|---------|----------|
| `COMPLETE` | Task fully accomplished | All requested actions done |
| `PARTIAL` | Task partially done | Some actions complete, some pending |
| `FAILED` | Task could not be done | Execution error occurred |
| `BLOCKED` | Cannot proceed | Missing info, out of scope, etc. |
| `APPROVED` | (Reviewer) Passes review | No blocking issues found |
| `APPROVED_WITH_NOTES` | (Reviewer) Passes with suggestions | Minor improvements suggested |
| `CHANGES_REQUIRED` | (Reviewer) Must fix before approval | Blocking issues found |
| `REJECTED` | (Reviewer) Fundamental problems | Requires significant rework |
| `DESIGNED` | (Architect) Design complete | Specification ready |
| `NEEDS_INPUT` | (Architect) Requires clarification | Design blocked on decisions |
| `SECURE` | (Security) No vulnerabilities | Scan passed |
| `VULNERABILITIES_FOUND` | (Security) Issues detected | Remediation needed |
| `CRITICAL_RISK` | (Security) Severe issues | Immediate action required |
| `ROUTING` | (Coordinator) Task being routed | Handoff in progress |
| `IN_PROGRESS` | (Coordinator) Workflow active | Multi-step ongoing |

---

## EXECUTOR RESPONSE FORMAT

```
@p420/executor
STATUS: [COMPLETE | PARTIAL | FAILED | BLOCKED]

EXECUTED:
- [action with target]: [brief result]
- [action with target]: [brief result]

OUTPUT:
[Primary output - code, config, etc.]

CHANGES:
- [file path]: [change description]
- [file path]: [change description]

NOTES: (if applicable)
[Observations, warnings, or follow-up suggestions]
```

### Example
```
@p420/executor
STATUS: COMPLETE

EXECUTED:
- Created LoginForm component: src/components/LoginForm.tsx
- Added form validation: email and password fields
- Integrated with auth context

OUTPUT:
```tsx
// src/components/LoginForm.tsx
export const LoginForm: React.FC = () => {
  // ... component code
};
```

CHANGES:
- src/components/LoginForm.tsx: New file created
- src/components/index.ts: Added export

NOTES:
- Consider adding password strength indicator
- Form uses controlled inputs for validation
```

---

## REVIEWER RESPONSE FORMAT

```
@p420/reviewer
STATUS: [APPROVED | APPROVED_WITH_NOTES | CHANGES_REQUIRED | REJECTED]

REVIEWED:
- [file/scope]: [what was examined]

FINDINGS:

[CRITICAL] (if any)
- [file:line]: [issue]
  IMPACT: [consequence]
  FIX: [solution]

[WARNING] (if any)
- [file:line]: [issue]
  IMPACT: [consequence]
  SUGGESTION: [improvement]

[INFO] (if any)
- [positive observation or minor note]

SUMMARY:
[2-3 sentence overall assessment]

RECOMMENDATION:
[Clear action: approve, request changes, or reject]
```

### Example
```
@p420/reviewer
STATUS: CHANGES_REQUIRED

REVIEWED:
- src/components/LoginForm.tsx: Full component review

FINDINGS:

[CRITICAL]
- LoginForm.tsx:45: Password sent in URL query parameter
  IMPACT: Credentials exposed in server logs and browser history
  FIX: Use POST body for credentials, not query string

[WARNING]
- LoginForm.tsx:23: No loading state during submission
  IMPACT: User may double-submit form
  SUGGESTION: Add isSubmitting state and disable button

[INFO]
- Good use of TypeScript for form state typing
- Clear component structure

SUMMARY:
Component has a critical security flaw with credential handling.
Otherwise well-structured with minor UX improvements needed.

RECOMMENDATION:
Fix the critical security issue before merge. Address loading state for better UX.
```

---

## ARCHITECT RESPONSE FORMAT

```
@p420/architect
STATUS: [DESIGNED | NEEDS_INPUT | BLOCKED]

DESIGN: [Design Name]

OVERVIEW:
[2-3 sentence summary]

STRUCTURE:
[Directory tree or component diagram]

COMPONENTS:
1. [Component Name]
   - Purpose: [what it does]
   - Interface: [inputs/outputs]
   - Dependencies: [requirements]

PATTERNS:
- [Pattern]: [where/how to apply]

DATA FLOW:
[Description or ASCII diagram]

TRADE-OFFS:
| Option | Pros | Cons |
|--------|------|------|
| [A] | [benefits] | [drawbacks] |
| [B] | [benefits] | [drawbacks] |

RECOMMENDATION:
[Chosen approach with rationale]

IMPLEMENTATION NOTES:
[Guidance for executor]
```

---

## SECURITY RESPONSE FORMAT

```
@p420/security
STATUS: [SECURE | VULNERABILITIES_FOUND | CRITICAL_RISK | NEEDS_REVIEW]

SCOPE: [What was analyzed]

THREAT LEVEL: [LOW | MEDIUM | HIGH | CRITICAL]

VULNERABILITIES:

[CRITICAL] (if any)
- [ID]: [Vulnerability Name]
  Location: [file:line]
  Type: [CWE/OWASP]
  Description: [what's wrong]
  Impact: [potential damage]
  Exploit: [attack vector]
  Remediation: [how to fix]

[HIGH] / [MEDIUM] / [LOW]
- [Similar format, decreasing detail for lower severity]

ATTACK SURFACE:
- [Entry point]: [Risk level and notes]

RECOMMENDATIONS:
1. [Priority fix]
2. [Secondary fix]

SECURE PATTERNS:
- [Pattern to implement]: [Where to apply]
```

---

## TESTER RESPONSE FORMAT

```
@p420/tester
STATUS: [TESTS_GENERATED | STRATEGY_DEFINED | ANALYSIS_COMPLETE]

TARGET: [What was tested/analyzed]

TESTS CREATED:
- [file]: [count] tests
  - [test name]
  - [test name]

TEST CODE:
[Code blocks with tests]

COVERAGE:
- Statements: [%]
- Branches: [%]
- Functions: [%]
- Edge cases: [list covered]

MOCKS REQUIRED:
- [Dependency]: [Mock strategy]

RUN COMMAND:
[Command to execute tests]

GAPS: (if analyzing existing tests)
- [Untested scenario]
- [Untested scenario]
```

---

## COORDINATOR RESPONSE FORMAT

```
@p420/coordinator
STATUS: [ROUTING | IN_PROGRESS | COMPLETE | BLOCKED]

REQUEST ANALYSIS:
[Understanding of what's being asked]

WORKFLOW PLAN:
1. [Step] → @p420/[agent]
   Input: [what agent receives]
   Output: [expected result]
2. [Step] → @p420/[agent]
   ...

CURRENT STATE: (for in-progress)
  Step: [N of M]
  Completed: [list]
  Active: [current step]
  Pending: [remaining]

ROUTING DECISION: (for routing)
Primary: @p420/[agent]
Reason: [why this agent]

COMMANDS TO ISSUE:
---
@p420/[agent]: [command]
CONTEXT: [context]
---

AGGREGATED RESULT: (when complete)
[Combined output from all agents]
```

---

## ERROR RESPONSE FORMAT

All agents use this format for errors:

```
@p420/[role]
STATUS: [FAILED | BLOCKED]

ERROR:
Type: [error category]
Message: [what went wrong]
Location: [where it happened]

CONTEXT:
[State at time of error]

ATTEMPTED:
- [action tried]
- [result]

RECOVERY OPTIONS:
1. [Option with instructions]
2. [Option with instructions]

ESCALATION:
[When to involve human decision]
```

---

## HANDOFF FORMAT

When transferring to another agent:

```
@p420/[current-role]
STATUS: HANDOFF

HANDOFF TO: @p420/[target-role]

REASON:
[Why this agent cannot continue]

COMPLETED:
- [What was accomplished]

CONTEXT FOR TARGET:
[Relevant state and information]

SUGGESTED COMMAND:
@p420/[target]: [recommended command]
[any context modifiers]
```

---

**End of Response Formats**
