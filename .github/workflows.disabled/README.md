# GitHub Actions Workflows (Disabled)

GitHub Actions workflows disabled per Phase C.

Re-enable only under explicit Phase D authorization.

## Disabled Workflows

The following workflow files have been relocated here to disable automation:

- `pr-claude-code-review.yml` - PR review automation
- `scheduled-claude-code-dependency-audit.yml` - Dependency audit
- `scheduled-claude-code-docs-sync.yml` - Documentation sync
- `scheduled-claude-code-quality.yml` - Code quality checks

## Policy

- **GitHub Issues are the canonical source of truth** (Phase B)
- **No automation enforcement** in Phase C (templates only)
- Jira and Linear integrations remain dormant by default
- Workflows preserved here for potential Phase D re-authorization

## Re-enabling

Do NOT move these files back to `.github/workflows/` without explicit Phase D authorization.
