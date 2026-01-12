# Project Adoption Contract

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This contract defines the expectations, requirements, and constraints for projects that adopt the claude-code-showcase foundation. It establishes what adopting projects must, should, and must not do, as well as detachment expectations.

---

## Adoption Model

The claude-code-showcase repository operates as an **attachable foundation**, not a hard dependency. Projects may:

- Adopt all or part of this foundation
- Customize and extend capabilities
- Diverge from patterns as needed
- Detach without destabilization

This is **not** a framework. It is a **reference implementation and governance layer**.

---

## Must Do (Required)

Projects that adopt this foundation **MUST**:

### 1. Acknowledge Source

Adopting projects must retain attribution in an appropriate location:

```markdown
<!-- In README.md or CLAUDE.md -->
Claude Code configuration adapted from:
https://github.com/Statera-Jason/claude-code-showcase
```

**Why:** Maintains provenance and allows others to find the source.

### 2. Review Security Implications

Before adopting CAP-007 (MCP Server Integration), projects must:

- Review credential exposure risks
- Ensure environment variables are not committed
- Understand API access granted to Claude Code
- Document which MCP servers are enabled

**Why:** MCP servers grant Claude Code access to external systems. This must be intentional.

### 3. Test Hooks Before Production Use

Before relying on CAP-002 (Hook Configuration), projects must:

- Test hooks in development environment
- Verify shell compatibility (bash vs sh)
- Confirm timeout values are appropriate
- Test failure scenarios (blocked actions, timeouts)

**Why:** Hook failures can block workflows or cause unexpected behavior.

### 4. Customize for Project Context

Projects must customize:

- `CLAUDE.md` - Replace with project-specific information
- `.claude/skills/` - Adapt or remove skills that don't apply
- `.claude/commands/` - Modify commands for project workflows
- `.claude/agents/` - Update checklists for project standards

**Why:** Template content is illustrative only. Direct usage will cause confusion.

### 5. Version Control Configuration

Projects must commit to version control:

- `.claude/settings.json` (hook configuration)
- `.claude/skills/` (team-shared skills)
- `.claude/agents/` (team-shared agents)
- `.claude/commands/` (team-shared commands)
- `CLAUDE.md` (project memory)
- `.mcp.json` (MCP server configuration - WITHOUT credentials)

**Why:** Configuration is a team asset that should be versioned and reviewed.

### 6. Exclude Secrets from Version Control

Projects must ensure the following are **never** committed:

- `.claude/settings.local.json` (personal overrides)
- `CLAUDE.local.md` (personal notes)
- Environment files with credentials (`.env`, etc.)
- API keys or tokens in any configuration

**Why:** Prevents credential exposure.

---

## Should Do (Recommended)

Projects that adopt this foundation **SHOULD**:

### 1. Start Incrementally

Adopt capabilities in phases:

1. Phase 1: `CLAUDE.md` only
2. Phase 2: Add 1-2 skills for common patterns
3. Phase 3: Add hooks for automation
4. Phase 4: Add commands for workflows
5. Phase 5: Consider MCP servers and GitHub Actions

**Why:** Reduces complexity and allows learning curve.

### 2. Document Local Customizations

When diverging from foundation patterns, document why:

```markdown
<!-- In .claude/README.md or similar -->
## Deviations from claude-code-showcase

- Hooks: Modified to use POSIX sh instead of bash (DEF-001 workaround)
- Skills: Removed graphql-schema (not applicable)
- MCP: Added custom server for internal API
```

**Why:** Helps future maintainers understand decisions.

### 3. Contribute Improvements Back

If you fix defects or improve patterns, consider:

- Opening issues in claude-code-showcase
- Submitting PRs with improvements
- Sharing knowledge with community

**Why:** Benefits all adopters and improves foundation.

### 4. Monitor Costs

If adopting CAP-008 (GitHub Actions):

- Track Anthropic API usage
- Monitor GitHub Actions minutes
- Review cost reports monthly
- Adjust workflow frequency if needed

**Why:** Prevents unexpected costs.

### 5. Review and Update Periodically

Every quarter, review:

- Are skills still accurate?
- Do hooks still work correctly?
- Are commands still relevant?
- Can any automation be improved?

**Why:** Prevents configuration drift and staleness.

---

## Must Not Do (Prohibited)

Projects that adopt this foundation **MUST NOT**:

### 1. Assume Patterns Are Universal

Do not assume:

- All skills apply to your project
- Hook timeouts are optimal for your environment
- Command workflows match your processes
- MCP server configurations are complete

**Why:** This is a reference implementation, not a universal solution.

### 2. Enable All Capabilities Without Review

Do not blindly enable:

- All MCP servers
- All GitHub Actions workflows
- All hooks without testing
- All skills without customization

**Why:** Each capability has risks and requirements that must be evaluated.

### 3. Store Credentials in Configuration

Do not commit:

- API keys in `.mcp.json`
- Tokens in `.claude/settings.json`
- Passwords anywhere in `.claude/`

**Why:** Credentials must be environment-specific and secret.

### 4. Depend on Defect Behavior

Do not rely on:

- DEF-001 (bash/sh mismatch) behavior
- Undocumented features
- Implementation details not in canon

**Why:** Defects may be fixed, causing unexpected breakage.

### 5. Block Detachment

Do not create dependencies that prevent:

- Removing `.claude/` directory
- Disabling specific capabilities
- Reverting to vanilla Claude Code

**Why:** Maintains attachment-first posture. Projects must be able to detach.

---

## Detachment Expectations

Projects may detach from this foundation at any time by:

1. **Removing Configuration:**
   ```bash
   rm -rf .claude/
   rm CLAUDE.md
   rm .mcp.json
   ```

2. **Keeping Customizations:**
   Projects may retain customized skills, agents, or commands even after detaching from the foundation.

3. **Minimal Impact:**
   Detachment should not break core project functionality. If it does, the project has become over-dependent.

### Detachment Checklist

- [ ] Remove `.claude/` directory or specific capability files
- [ ] Remove `CLAUDE.md` if desired
- [ ] Remove `.mcp.json` if desired
- [ ] Remove GitHub Actions workflows if present
- [ ] Verify project still functions without Claude Code configuration
- [ ] Update documentation to reflect detachment

---

## Adoption Levels

Projects may adopt at different levels:

### Level 1: Documentation Only

Adopt only:
- `CLAUDE.md` pattern

**Effort:** Low
**Risk:** None
**Benefit:** Basic project memory

### Level 2: Skills and Documentation

Adopt:
- `CLAUDE.md`
- Selected `.claude/skills/`

**Effort:** Low-Medium
**Risk:** Low
**Benefit:** Consistent code patterns

### Level 3: Automation with Hooks

Adopt:
- `CLAUDE.md`
- `.claude/skills/`
- `.claude/settings.json` (hooks)

**Effort:** Medium
**Risk:** Medium (DEF-001, shell compatibility)
**Benefit:** Automated formatting, testing, type-checking

### Level 4: Workflows and Commands

Adopt:
- Level 3 +
- `.claude/commands/`
- `.claude/agents/`

**Effort:** Medium-High
**Risk:** Medium
**Benefit:** Streamlined workflows, code review

### Level 5: Full Integration

Adopt:
- Level 4 +
- `.mcp.json` (MCP servers)
- `.github/workflows/` (GitHub Actions)

**Effort:** High
**Risk:** High (costs, credentials, governance)
**Benefit:** Full automation, external integrations

---

## Support and Maintenance

### What This Foundation Provides

- ✓ Reference implementations
- ✓ Documentation of patterns
- ✓ Known defect tracking
- ✓ Governance structure

### What This Foundation Does NOT Provide

- ✗ Ongoing support for adopting projects
- ✗ Compatibility guarantees across updates
- ✗ Testing of configurations in all environments
- ✗ Resolution of project-specific issues

### Adopter Responsibilities

Adopting projects are responsible for:

- Testing in their environment
- Maintaining their customizations
- Monitoring their costs and performance
- Updating as their needs evolve
- Seeking help through appropriate channels (not foundation maintainers)

---

## Compatibility and Updates

### Versioning

The foundation uses semantic versioning for canon documents:

- **Major (X.0.0):** Breaking changes to contracts or expectations
- **Minor (0.X.0):** New capabilities or significant updates
- **Patch (0.0.X):** Bug fixes, clarifications, documentation

Current Version: **1.0.0**

### Update Policy

- Adopting projects are NOT required to update
- Updates are pull-based, not push-based
- Breaking changes will be clearly documented
- Legacy patterns may be deprecated but remain documented

### Compatibility Promise

The foundation commits to:

- Not removing capabilities without major version bump
- Documenting all breaking changes
- Maintaining historical documentation

The foundation does NOT promise:

- Backwards compatibility for all customizations
- Support for old versions
- Migration guides for all changes

---

## Acceptance Criteria

This contract is complete when:

1. ✓ Must/Should/Must Not requirements are explicit
2. ✓ Detachment expectations are clear
3. ✓ Adoption levels are defined
4. ✓ Support boundaries are stated
5. ✓ Versioning policy is established

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
