# Claude Foundation Capabilities Register

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This register enumerates all capabilities present in the claude-code-showcase repository, their locations, default states, dependencies, risks, and approved extension paths.

---

## Capability Classification

| Status | Definition |
|--------|------------|
| **Enabled** | Active by default, requires no configuration |
| **Disabled** | Present but inactive by default, requires explicit enablement |
| **Dormant** | Complete implementation present, intentionally inactive, requires governance approval |
| **Template** | Reference implementation only, requires customization before use |

---

## Core Capabilities

### CAP-001: Project Memory (CLAUDE.md)

**Location:** `/CLAUDE.md`

**Status:** Enabled

**Description:** Persistent project memory loaded at session start containing stack information, conventions, and critical rules.

**Dependencies:**
- None

**Risks:**
- Low: Read-only documentation

**Extension Path:**
- Adopting projects should customize content to reflect their stack, conventions, and rules
- File location can be `.claude/CLAUDE.md`, `./CLAUDE.md`, or `~/.claude/CLAUDE.md`

**Notes:**
- Automatically loaded by Claude Code at session initialization

---

### CAP-002: Hook Configuration

**Location:** `.claude/settings.json`

**Status:** Template

**Description:** Hook-based automation for code formatting, testing, type-checking, and branch protection.

**Sub-capabilities:**
- **CAP-002a:** UserPromptSubmit hooks (skill evaluation)
- **CAP-002b:** PreToolUse hooks (branch protection)
- **CAP-002c:** PostToolUse hooks (formatting, testing, type-checking)

**Dependencies:**
- Bash shell (known issue: requires bash, not sh)
- npm ecosystem
- prettier or biome (formatting)
- jest (testing)
- TypeScript (type checking)

**Risks:**
- **HIGH - Known Defect:** Hooks use bash-specific syntax (`[[`, `=~`) but system may execute with `/bin/sh`
  - Status: Documented issue
  - Impact: Hook failures in sh-only environments
  - Mitigation: Deferred to later phase
  - Workaround: Use bash-compatible shell or rewrite hooks for POSIX compliance

- Medium: Hook failures can block workflow
- Medium: Timeout configuration may need tuning
- Low: npm commands may fail without dependencies installed

**Extension Path:**
- Copy `.claude/settings.json` to target project
- Customize matchers, commands, and timeouts for project needs
- Add or remove hooks as needed
- Configure environment variables in `env` section
- Test hooks in target environment before committing

**Notes:**
- Hook exit code 2 blocks actions (PreToolUse only)
- Hook exit code 0 or 1 is non-blocking
- Hooks receive environment variables: `$CLAUDE_TOOL_INPUT_FILE_PATH`, `$CLAUDE_TOOL_NAME`, `$CLAUDE_PROJECT_DIR`

---

### CAP-003: Skill Evaluation System

**Location:** `.claude/hooks/`

**Files:**
- `skill-eval.sh` - Wrapper script
- `skill-eval.js` - Node.js matching engine
- `skill-rules.json` - Pattern matching rules
- `skill-rules.schema.json` - JSON schema for rules

**Status:** Template

**Description:** Automated skill suggestion system that analyzes prompts for keywords, file paths, and patterns to recommend relevant skills.

**Dependencies:**
- Node.js runtime
- Bash shell
- UserPromptSubmit hook configuration

**Risks:**
- Low: False positive skill suggestions
- Low: Performance impact on prompt submission (<5s timeout)
- Medium: Requires maintenance as skills evolve

**Extension Path:**
1. Copy `.claude/hooks/` directory to target project
2. Customize `skill-rules.json` with project-specific skills
3. Adjust trigger patterns (keywords, regexes, paths)
4. Configure confidence thresholds and priorities
5. Add UserPromptSubmit hook to `settings.json`

**Notes:**
- Confidence scoring: keyword=2, keywordPattern=3, pathPattern=4, directoryMatch=5, intentPattern=4
- Threshold: 6+ points = HIGH, 4-5 = MEDIUM, <4 = not suggested

---

### CAP-004: Domain Knowledge Skills

**Location:** `.claude/skills/`

**Status:** Template

**Available Skills:**
- `testing-patterns` - Jest, TDD, factory functions, mocking
- `systematic-debugging` - Four-phase debugging methodology
- `react-ui-patterns` - Loading, error, empty states
- `graphql-schema` - Queries, mutations, code generation
- `core-components` - Design system and component library
- `formik-patterns` - Form handling and validation

**Dependencies:**
- Project-specific: Each skill assumes certain technologies/patterns

**Risks:**
- Low: Skills may not match adopting project's tech stack
- Low: Skills may become outdated as patterns evolve

**Extension Path:**
1. Copy relevant skill directories to target project
2. Customize SKILL.md content for project-specific patterns
3. Update frontmatter (name, description)
4. Add project-specific examples
5. Remove or modify patterns that don't apply
6. Create new skills for project-specific domains

**Notes:**
- SKILL.md must have YAML frontmatter with `name` and `description`
- Description is critical for Claude's semantic matching
- Maximum frontmatter field lengths: name=64, description=1024

---

### CAP-005: Specialized Agents

**Location:** `.claude/agents/`

**Status:** Template

**Available Agents:**
- `code-reviewer.md` - Comprehensive code review with checklist
- `github-workflow.md` - Git operations, branches, commits, PRs

**Dependencies:**
- Project-specific: Code reviewer assumes project standards exist
- git, gh CLI (github-workflow agent)

**Risks:**
- Low: Agent prompts may need customization for project standards

**Extension Path:**
1. Copy agent markdown files to target project
2. Customize agent system prompts
3. Update checklists and processes for project standards
4. Modify allowed tools if needed
5. Adjust model selection (sonnet/opus/haiku)

**Notes:**
- Agents are autonomous and have dedicated prompts
- Agent frontmatter: name, description, model (optional), tools (optional)

---

### CAP-006: Slash Commands

**Location:** `.claude/commands/`

**Status:** Template

**Available Commands:**
- `/onboard` - Deep task exploration and planning
- `/ticket` - JIRA/Linear ticket workflow (read → implement → update)
- `/pr-review` - Pull request review workflow
- `/pr-summary` - Generate PR description from git diff
- `/code-quality` - Code quality checks on directory
- `/docs-sync` - Documentation alignment verification

**Dependencies:**
- Command-specific: `/ticket` requires MCP servers (JIRA/Linear)
- git, gh CLI (PR commands)

**Risks:**
- Medium: `/ticket` command requires MCP server configuration
- Low: Commands may need project-specific customization

**Extension Path:**
1. Copy command markdown files to target project
2. Customize command instructions for project workflows
3. Update allowed-tools as needed
4. Add inline bash commands with `!`
5. Use `$ARGUMENTS`, `$1`, `$2`, etc. for parameters

**Notes:**
- Commands are invoked with `/command-name arguments`
- Command frontmatter: description, allowed-tools (optional)

---

### CAP-007: MCP Server Integration

**Location:** `.mcp.json`

**Status:** Dormant

**Description:** External integrations via Model Context Protocol servers (JIRA, GitHub, Linear, Slack, Sentry, Postgres, Notion, Memory).

**Configured Servers:**
- `jira` - JIRA integration
- `github` - GitHub API integration
- `linear` - Linear issue tracking
- `sentry` - Error monitoring
- `postgres` - Database access
- `slack` - Slack messaging
- `notion` - Notion workspace
- `memory` - Persistent memory storage

**Dependencies:**
- Server-specific: Each MCP server requires API credentials
- npx (for stdio servers)
- Environment variables with credentials

**Risks:**
- **HIGH - Security:** Requires API credentials in environment
- Medium: MCP servers may not be available or compatible
- Medium: Cost implications for API usage
- Low: Server startup latency

**Extension Path:**
1. Copy `.mcp.json` to target project
2. Remove unused server configurations
3. Set required environment variables (never commit credentials)
4. Configure server enablement in `.claude/settings.json`:
   - `enableAllProjectMcpServers: true` OR
   - `enabledMcpjsonServers: ["jira", "github"]`
5. Test server connectivity before relying on capabilities

**Notes:**
- Environment variable expansion: `${VAR}` or `${VAR:-default}`
- Server types: `stdio` (local process) or `http` (remote)
- All configured servers are dormant by default (require explicit enablement)

---

### CAP-008: GitHub Actions Workflows

**Location:** `.github/workflows/`

**Status:** Disabled

**Description:** Automated CI/CD workflows for PR review, scheduled maintenance, documentation sync, and dependency management.

**Available Workflows:**
- `pr-claude-code-review.yml` - Automated PR reviews
- `scheduled-claude-code-docs-sync.yml` - Monthly documentation alignment
- `scheduled-claude-code-quality.yml` - Weekly code quality reviews
- `scheduled-claude-code-dependency-audit.yml` - Biweekly dependency updates

**Dependencies:**
- GitHub repository
- `ANTHROPIC_API_KEY` secret in repository settings
- Claude Code Action (`anthropics/claude-code-action@beta`)
- git, gh CLI

**Risks:**
- **HIGH - Cost:** API usage costs for Anthropic API ($10-50/month estimated)
- **HIGH - Governance:** Requires approval for automated commits/PRs
- Medium: Workflow failures may not be monitored
- Medium: Rate limiting on GitHub Actions or Anthropic API

**Extension Path:**
1. Review workflows and customize for project needs
2. Add `ANTHROPIC_API_KEY` to repository secrets
3. Test workflows on non-production branches first
4. Enable workflows by committing to `.github/workflows/`
5. Monitor costs and adjust schedules as needed
6. Configure branch protection if automated commits are enabled

**Notes:**
- **Disabled by default due to cost and governance concerns**
- Workflows are templates only - require explicit enablement
- Cost estimates: PR review ($0.05-0.50), Docs sync ($0.50-2.00), Quality ($1-5), Audit ($0.20-1.00)

---

### CAP-009: LSP Plugin Support

**Location:** `.claude/settings.json` (configuration only)

**Status:** Template (configuration present, requires plugin installation)

**Description:** Language Server Protocol integration for real-time code intelligence (type checking, completions, navigation).

**Supported Languages:**
- TypeScript/JavaScript (`typescript-lsp@claude-plugins-official`)
- Python (`pyright-lsp@claude-plugins-official`)
- Rust (`rust-lsp@claude-plugins-official`)

**Dependencies:**
- Language-specific: TypeScript requires `typescript-language-server` and `typescript` npm packages
- Project must have language tooling installed
- Claude Code LSP plugin system

**Risks:**
- Low: LSP servers may have performance impact
- Low: Configuration errors may prevent activation

**Extension Path:**
1. Install language server binaries (e.g., `npm install -g typescript-language-server typescript`)
2. Enable plugins in `.claude/settings.json`: `"enabledPlugins": {"typescript-lsp@claude-plugins-official": true}`
3. Optionally create `.lsp.json` for custom configuration
4. Verify with `claude /plugin` command

**Notes:**
- LSP provides diagnostics, hover info, completions, and navigation
- Not all languages have LSP support
- Some LSP servers require project-specific configuration

---

## Capability Dependencies Matrix

| Capability | Requires | Optional Enhancement |
|------------|----------|---------------------|
| CAP-001 (CLAUDE.md) | None | - |
| CAP-002 (Hooks) | bash, npm | prettier, biome, jest, tsc |
| CAP-003 (Skill Eval) | Node.js, bash, CAP-002 | CAP-004 (skills) |
| CAP-004 (Skills) | None | Project-specific tools |
| CAP-005 (Agents) | None | git, gh CLI |
| CAP-006 (Commands) | None | git, gh CLI, CAP-007 (MCP) |
| CAP-007 (MCP) | npx, credentials | Network access |
| CAP-008 (GitHub Actions) | GitHub repo, API key | Branch protection |
| CAP-009 (LSP) | Language servers | `.lsp.json` config |

---

## Risk Summary

| Risk Level | Count | Capabilities |
|------------|-------|--------------|
| **HIGH** | 3 | CAP-002 (bash/sh mismatch), CAP-007 (credentials), CAP-008 (cost/governance) |
| **Medium** | 6 | Various operational and maintenance risks |
| **Low** | 8 | Minor customization and compatibility risks |

---

## Known Defects

### DEF-001: Bash vs Sh Shell Mismatch

**Affected Capability:** CAP-002 (Hook Configuration)

**Description:** Hooks in `.claude/settings.json` use bash-specific syntax (`[[`, `=~`, `${PIPESTATUS[0]}`) but may execute under `/bin/sh` which does not support these features.

**Impact:** Hook failures with syntax errors in sh-only environments

**Status:** Documented, Deferred

**Action:** Not Phase A

**Workaround Options:**
1. Configure environment to use bash instead of sh
2. Rewrite hooks for POSIX sh compatibility
3. Disable problematic hooks in sh environments

**Phase for Resolution:** Phase B or later (requires governance approval)

---

## Acceptance Criteria

This register is complete when:

1. ✓ All capabilities are enumerated with unique identifiers
2. ✓ Each capability has location, status, description
3. ✓ Dependencies are documented
4. ✓ Risks are assessed
5. ✓ Extension paths are defined
6. ✓ Known defects are documented
7. ✓ Dependency matrix is complete

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
