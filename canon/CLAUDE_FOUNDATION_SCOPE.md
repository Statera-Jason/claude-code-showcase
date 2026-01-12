# Claude Foundation Scope

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This document defines the explicit scope boundaries for the claude-code-showcase repository as a foundational governance layer for Statera / Project420. It establishes what is and is not included in Phase A canonisation.

---

## In Scope

### 1. Configuration Patterns

The following configuration patterns are in scope as reference implementations:

- `.claude/settings.json` - Hook configuration, environment variables
- `.claude/settings.md` - Human-readable hook documentation
- `CLAUDE.md` - Project memory patterns
- `.mcp.json` - MCP server configuration patterns

### 2. Domain Knowledge Artifacts

All skill definitions are in scope as templates:

- `.claude/skills/testing-patterns/` - TDD and testing conventions
- `.claude/skills/systematic-debugging/` - Debugging methodology
- `.claude/skills/react-ui-patterns/` - UI state handling patterns
- `.claude/skills/graphql-schema/` - GraphQL conventions
- `.claude/skills/core-components/` - Component library patterns
- `.claude/skills/formik-patterns/` - Form handling patterns

### 3. Agent Definitions

Agent patterns are in scope as reference templates:

- `.claude/agents/code-reviewer.md` - Code review checklist and process
- `.claude/agents/github-workflow.md` - Git workflow automation

### 4. Command Templates

Slash command patterns are in scope:

- `.claude/commands/onboard.md` - Task exploration pattern
- `.claude/commands/ticket.md` - Ticket workflow pattern
- `.claude/commands/pr-review.md` - PR review pattern
- `.claude/commands/pr-summary.md` - PR summary generation
- `.claude/commands/code-quality.md` - Quality check pattern
- `.claude/commands/docs-sync.md` - Documentation alignment pattern

### 5. Hook Implementations

Hook patterns for automation are in scope:

- `.claude/hooks/skill-eval.sh` - Skill evaluation wrapper
- `.claude/hooks/skill-eval.js` - Skill matching engine
- `.claude/hooks/skill-rules.json` - Pattern matching rules
- `.claude/hooks/skill-rules.schema.json` - Rule schema definition

### 6. GitHub Actions Templates

Workflow patterns are in scope as templates (off-by-default):

- `.github/workflows/pr-claude-code-review.yml` - PR review automation
- `.github/workflows/scheduled-claude-code-docs-sync.yml` - Documentation sync
- `.github/workflows/scheduled-claude-code-quality.yml` - Quality reviews
- `.github/workflows/scheduled-claude-code-dependency-audit.yml` - Dependency management

### 7. Documentation

Repository documentation is in scope:

- `README.md` - Repository overview and patterns
- `.claude/skills/README.md` - Skills overview

### 8. Governance Definitions

This canon layer itself is in scope:

- `docs/canon/*.md` - All Phase A governance documents

---

## Out of Scope

### 1. Runtime Behavior

Phase A DOES NOT define or implement:

- How Claude Code executes prompts
- Runtime decision-making logic
- Execution orchestration
- Live agent behavior
- Dynamic skill activation logic

**Rationale:** Phase A is documentation and structure only. Runtime behavior is deferred to later phases.

### 2. Project-Specific Implementation

Phase A DOES NOT include:

- Actual application code
- Project-specific business logic
- Real test suites
- Real component libraries
- Real GraphQL schemas
- Real form implementations

**Rationale:** This repository provides patterns and templates, not actual application code.

### 3. Environment-Specific Configuration

Phase A DOES NOT include:

- Real API credentials
- Production environment variables
- User-specific settings
- Local development overrides (`.local.*` files)

**Rationale:** These are environment-specific and must be provided by adopting projects.

### 4. Claude Prompt Generation

Phase A explicitly FORBIDS:

- Generation of Claude Code prompts
- Generation of Claude Web prompts
- Any executable AI instructions targeting Claude models

**Rationale:** Prompt generation is deferred to later phases when explicitly requested.

### 5. Build/Compilation Artifacts

Phase A DOES NOT include:

- Compiled code
- Built assets
- Generated types
- Transpiled JavaScript
- Bundled dependencies

**Rationale:** These are build-time artifacts, not source governance.

### 6. External System State

Phase A DOES NOT define:

- Actual JIRA projects or tickets
- GitHub repository state
- External database schemas
- Slack workspace configuration
- Sentry project configuration

**Rationale:** These are external systems beyond repository control.

---

## Explicit Exclusions

The following are EXPLICITLY EXCLUDED from Phase A scope:

### 1. Inference from Absence

If a pattern or capability is not explicitly documented in Phase A canon, it MUST NOT be inferred as prohibited or permitted. Absence does not imply policy.

### 2. Implementation Guidance for Claude

Phase A does not provide guidance on how Claude should interpret or execute these patterns. That is a Phase B+ concern.

### 3. Cost/Performance Optimization

Phase A does not define cost models, performance targets, or optimization strategies.

### 4. Security Policies

While examples may show security patterns, Phase A does not establish comprehensive security policies or threat models.

### 5. Access Control

Phase A does not define who may modify or adopt these patterns beyond the governance structure itself.

---

## Non-Goals

Phase A explicitly does NOT attempt to:

1. **Standardize All Conventions** - Only patterns present in this repository are in scope
2. **Provide Complete Coverage** - Gaps are acceptable and documented
3. **Enforce Adoption** - Adoption is voluntary per PROJECT_ADOPTION_CONTRACT.md
4. **Guarantee Compatibility** - Patterns may require adaptation
5. **Prevent Divergence** - Forks and modifications are permitted per attachment policy

---

## Scope Boundaries

| Area | In Scope | Out of Scope |
|------|----------|--------------|
| Configuration files | ✓ Yes | Runtime values |
| Pattern templates | ✓ Yes | Actual implementations |
| Documentation | ✓ Yes | User guides |
| Governance rules | ✓ Yes | Enforcement mechanisms |
| GitHub Actions | ✓ Yes (templates) | Actual execution |
| MCP server configs | ✓ Yes (patterns) | Server implementations |
| Hook scripts | ✓ Yes | Hook runtime behavior |

---

## Acceptance Criteria

This scope definition is complete when:

1. ✓ All in-scope items are explicitly listed
2. ✓ All out-of-scope items are explicitly stated
3. ✓ Explicit exclusions are documented
4. ✓ Non-goals are stated
5. ✓ Scope boundaries are clear and unambiguous

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
