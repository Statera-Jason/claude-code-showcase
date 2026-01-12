# Attachment Packaging Strategy

**Phase:** A (Canonisation)
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
**Repository:** claude-code-showcase (Statera-Jason)
**Status:** Authoritative
**Version:** 1.0.0

---

## Purpose

This document defines the permitted patterns for attaching the claude-code-showcase foundation to other projects, the bounded footprint rules, and how upgrade and divergence are handled.

---

## Core Principle: Bounded Footprint

All attachment methods MUST maintain a **bounded footprint** within adopting projects:

- Configuration must be isolated to known directories (`.claude/`, `docs/`, etc.)
- No scattered configuration files throughout project structure
- No modification of existing project files without explicit action
- Clear separation between foundation patterns and project code

**Why:** Enables clean detachment and prevents deep coupling.

---

## Permitted Attachment Patterns

### Pattern 1: Direct Copy (Recommended)

**Method:** Copy files directly into target project repository

```bash
# Example attachment via direct copy
cd target-project/

# Copy core configuration
cp -r /path/to/claude-code-showcase/.claude .
cp /path/to/claude-code-showcase/CLAUDE.md .
cp /path/to/claude-code-showcase/.mcp.json .

# Optionally copy GitHub workflows
cp -r /path/to/claude-code-showcase/.github/workflows .github/

# Customize for project
# Edit CLAUDE.md, skills, commands, etc.
```

**Characteristics:**
- ✓ Full control over files
- ✓ Easy to customize
- ✓ No external dependencies
- ✓ Simple version control
- ✗ No automatic updates
- ✗ Manual synchronization required

**Best For:**
- Projects that will heavily customize
- Teams that want full ownership
- Long-term stable configurations

**Footprint:**
```
target-project/
├── .claude/              # Bounded directory
├── CLAUDE.md             # Single file
├── .mcp.json             # Single file
└── .github/workflows/    # Opt-in directory
```

---

### Pattern 2: Git Subtree (Advanced)

**Method:** Merge foundation as a subtree in project repository

```bash
# Initial setup
cd target-project/
git subtree add --prefix=.claude-foundation \
  https://github.com/Statera-Jason/claude-code-showcase.git main --squash

# Create symlinks to active configuration
ln -s .claude-foundation/.claude .claude
ln -s .claude-foundation/CLAUDE.md CLAUDE.md.template

# Later: Pull updates
git subtree pull --prefix=.claude-foundation \
  https://github.com/Statera-Jason/claude-code-showcase.git main --squash
```

**Characteristics:**
- ✓ Preserves update path
- ✓ Full history tracking
- ✓ Can push improvements back
- ✗ More complex to manage
- ✗ Requires git subtree knowledge
- ✗ Can cause merge conflicts

**Best For:**
- Projects tracking foundation updates
- Teams comfortable with git subtree
- Projects planning to contribute back

**Footprint:**
```
target-project/
├── .claude-foundation/   # Bounded subtree directory
├── .claude -> .claude-foundation/.claude  # Symlink
└── CLAUDE.md.template -> .claude-foundation/CLAUDE.md
```

---

### Pattern 3: Vendor Copy (Isolated Updates)

**Method:** Copy to vendor directory, reference from configuration

```bash
# Copy to vendor
cd target-project/
mkdir -p vendor/claude-foundation
cp -r /path/to/claude-code-showcase/.claude vendor/claude-foundation/
cp /path/to/claude-code-showcase/CLAUDE.md vendor/claude-foundation/

# Create project-specific configuration that extends vendored patterns
mkdir -p .claude/skills
cat > .claude/settings.json <<EOF
{
  "hooks": {
    "_import": "../vendor/claude-foundation/.claude/settings.json"
  }
}
EOF
```

**Characteristics:**
- ✓ Clear separation: vendor vs project
- ✓ Preserves original patterns
- ✓ Easy to compare changes
- ✗ Requires configuration composition
- ✗ May not support imports (manual merge needed)

**Best For:**
- Projects with strict vendoring policies
- Organizations tracking third-party code
- Compliance-heavy environments

**Footprint:**
```
target-project/
├── vendor/
│   └── claude-foundation/  # Bounded vendor directory
├── .claude/                # Project-specific overrides
└── CLAUDE.md               # Project-specific content
```

---

## Prohibited Attachment Patterns

The following patterns are **PROHIBITED** as they violate bounded footprint principles:

### ❌ Scattered Configuration

```
# PROHIBITED - Configuration scattered throughout project
target-project/
├── src/components/.claude-component-rules
├── src/api/.claude-api-rules
├── tests/.claude-test-rules
└── docs/.claude-docs-rules
```

**Why Prohibited:** Makes detachment difficult, creates maintenance burden, unclear scope.

---

### ❌ Package Manager Dependency

```json
// PROHIBITED - Foundation as npm/pip/gem dependency
{
  "dependencies": {
    "claude-code-showcase": "github:Statera-Jason/claude-code-showcase"
  }
}
```

**Why Prohibited:** Foundation is configuration, not executable code. Not suitable for package management.

---

### ❌ Remote Reference Without Local Copy

```json
// PROHIBITED - Loading remote configuration at runtime
{
  "claude": {
    "skills": [
      "https://raw.githubusercontent.com/.../SKILL.md"
    ]
  }
}
```

**Why Prohibited:** Creates external runtime dependency, network failures break functionality, no version control.

---

### ❌ Automatic Code Injection

```bash
# PROHIBITED - Modifying project files automatically
./attach-foundation.sh --inject-hooks --modify-gitignore --update-readme
```

**Why Prohibited:** Violates explicit consent, makes changes hard to track, difficult to reverse.

---

## Bounded Footprint Rules

### Rule 1: Isolated Directories

Foundation configuration MUST be contained in:

```
Permitted directories:
- .claude/           # Primary configuration
- CLAUDE.md          # Root-level file (single file only)
- .mcp.json          # Root-level file (single file only)
- .github/workflows/ # Opt-in workflows (clearly named)
- docs/canon/        # Governance documents (if adopted)
- vendor/*/          # Vendor directory (if using Pattern 3)
```

No configuration may exist outside these boundaries without explicit documentation.

### Rule 2: No Hidden Dependencies

Foundation configuration MUST NOT:

- Require specific project directory structure
- Assume specific file locations outside `.claude/`
- Depend on undocumented environment variables
- Require specific npm packages without declaring them

### Rule 3: Detachment in One Step

It MUST be possible to detach by:

```bash
# Complete detachment
rm -rf .claude/ CLAUDE.md .mcp.json docs/canon/

# Or selective removal
rm -rf .claude/hooks/        # Remove just hooks
rm -rf .claude/skills/       # Remove just skills
```

No manual cleanup or "uninstall" scripts should be required.

### Rule 4: No Project File Modification

Foundation MUST NOT require:

- Modifying `.gitignore` (except to add `.claude/settings.local.json`)
- Modifying `package.json` (except to add optional dependencies)
- Modifying build configuration
- Modifying CI/CD pipelines (except explicitly added workflows)

All changes MUST be opt-in and reversible.

---

## Upgrade Handling

### Upgrade Strategy: Pull-Based

Projects control when and how they upgrade:

1. **Manual Comparison:**
   ```bash
   # Compare local vs foundation
   diff -r .claude/ /path/to/claude-code-showcase/.claude/
   ```

2. **Selective Adoption:**
   ```bash
   # Copy only desired updates
   cp /path/to/claude-code-showcase/.claude/skills/new-skill/ .claude/skills/
   ```

3. **Full Refresh:**
   ```bash
   # Backup customizations
   mv .claude .claude.backup

   # Copy latest foundation
   cp -r /path/to/claude-code-showcase/.claude .

   # Restore customizations
   # Manually merge .claude.backup with new .claude
   ```

### Upgrade Compatibility

Foundation updates SHOULD maintain:

- ✓ Backwards compatibility within major version
- ✓ Deprecation notices for breaking changes
- ✓ Migration guides for major versions

Foundation updates MAY break:

- Custom modifications not in contract
- Undocumented feature usage
- Workarounds for defects after fixes

### Upgrade Communication

Foundation will communicate updates via:

- GitHub releases with changelogs
- Breaking changes in major version bumps
- Deprecation warnings in documentation

Foundation will NOT:

- Automatically update adopting projects
- Push breaking changes without major version
- Remove capabilities without deprecation period

---

## Divergence Handling

### Permitted Divergence

Projects MAY diverge by:

1. **Adding capabilities:**
   - New skills for project-specific patterns
   - New commands for project workflows
   - New agents for project needs
   - Additional MCP servers

2. **Modifying capabilities:**
   - Customizing skill content
   - Adjusting hook timeouts
   - Changing command logic
   - Updating agent checklists

3. **Removing capabilities:**
   - Deleting unused skills
   - Disabling hooks
   - Removing MCP servers
   - Excluding GitHub Actions

4. **Fixing defects:**
   - Workarounds for DEF-001 (bash/sh)
   - Environment-specific adjustments
   - Performance optimizations

### Documenting Divergence

Projects SHOULD document divergence:

```markdown
<!-- In .claude/README.md -->
## Divergence from claude-code-showcase

### Modifications
- skills/testing-patterns: Adapted for Playwright instead of Jest
- hooks: Converted to POSIX sh for compatibility (DEF-001 workaround)

### Additions
- skills/kubernetes-patterns: Added for K8s deployments
- commands/deploy: Added for deployment workflow

### Removals
- skills/graphql-schema: Not applicable (REST API only)
- agents/github-workflow: Using custom git workflow
```

### Reconciliation

Projects are NOT required to:

- Reconcile divergence with upstream
- Merge foundation updates
- Maintain compatibility after divergence

Projects that heavily diverge have **effectively detached** and should consider:

- Removing attribution if no longer recognizable
- Forking foundation for their organization
- Creating their own governance

---

## Footprint Validation

### Validation Checklist

Verify bounded footprint with:

```bash
# Check for scattered configuration
find . -name ".claude*" -not -path "./.claude/*" -not -path "./.git/*"

# Check for unexpected files
ls -la | grep -E "(claude|CLAUDE)" | grep -v "^d.*\.claude$" | grep -v "^-.*CLAUDE\.md$"

# Verify detachment is clean
rm -rf .claude/ CLAUDE.md .mcp.json && git status
```

Expected result: Only known files in known locations.

### Compliance Criteria

A compliant attachment:

- [ ] All configuration in permitted directories
- [ ] No scattered configuration files
- [ ] No modifications to existing project files (except opt-in)
- [ ] Detachment possible in one step
- [ ] No hidden dependencies
- [ ] Documentation of divergence (if applicable)

---

## Acceptance Criteria

This strategy is complete when:

1. ✓ Permitted patterns are defined (3 patterns)
2. ✓ Prohibited patterns are explicit
3. ✓ Bounded footprint rules are clear
4. ✓ Upgrade strategy is documented
5. ✓ Divergence handling is defined
6. ✓ Validation methods are provided

---

**Document Status:** COMPLETE
**Last Updated:** 2026-01-09
**Authority:** GC_CLAUDE_FOUNDATION-A_CANON
