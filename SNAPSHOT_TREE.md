# Repository Structure

**Snapshot Date:** 2026-01-09

## Annotated Directory Tree

```
claude-code-showcase/
├── README.md                           [Documentation: Main repository guide]
│                                       Feature: N/A
│                                       Status: Active documentation
│
├── CLAUDE.md                           [Configuration: Project memory/instructions]
│                                       Feature: Core configuration (auto-loaded by Claude Code)
│                                       Status: Active by default
│
├── .gitignore                          [Configuration: Git ignore patterns]
│                                       Feature: Version control
│                                       Status: Standard git file
│
├── .mcp.json                           [Configuration: MCP server definitions]
│                                       Feature: External integrations (JIRA, GitHub, Linear, Slack, etc.)
│                                       Status: Configured but requires API keys to activate
│                                       External Dependencies: Node.js, API tokens
│
├── .claude/                            [Directory: Claude Code configuration root]
│   │
│   ├── settings.json                   [Configuration: Hooks, environment, permissions]
│   │                                   Feature: Hook system, environment variables
│   │                                   Status: Active by default
│   │                                   External Dependencies: npm, prettier, typescript (for hooks)
│   │
│   ├── settings.md                     [Documentation: Human-readable hook documentation]
│   │                                   Feature: N/A (documentation only)
│   │                                   Status: Reference documentation
│   │
│   ├── .gitignore                      [Configuration: Claude-specific git ignore]
│   │                                   Feature: Ignores settings.local.json
│   │                                   Status: Active
│   │
│   ├── agents/                         [Directory: Specialized AI agents]
│   │   ├── code-reviewer.md            [Agent: Comprehensive code review]
│   │   │                               Trigger: Manual invocation or proactive after code changes
│   │   │                               External Dependencies: None
│   │   │                               Safe to Ignore: Yes (opt-in feature)
│   │   │                               Extension: Modify checklist, add team-specific rules
│   │   │
│   │   └── github-workflow.md          [Agent: Git commits, branches, PRs]
│   │                                   Trigger: Manual invocation
│   │                                   External Dependencies: git, gh CLI
│   │                                   Safe to Ignore: Yes
│   │                                   Extension: Add team-specific git conventions
│   │
│   ├── commands/                       [Directory: Slash commands for workflows]
│   │   ├── onboard.md                  [Command: /onboard - Deep task exploration]
│   │   │                               Trigger: Manual (/onboard)
│   │   │                               Safe to Ignore: Yes
│   │   │
│   │   ├── pr-review.md                [Command: /pr-review - PR review workflow]
│   │   │                               Trigger: Manual (/pr-review <pr-number>)
│   │   │                               External Dependencies: gh CLI
│   │   │                               Safe to Ignore: Yes
│   │   │
│   │   ├── pr-summary.md               [Command: /pr-summary - Generate PR description]
│   │   │                               Trigger: Manual (/pr-summary)
│   │   │                               External Dependencies: git
│   │   │                               Safe to Ignore: Yes
│   │   │
│   │   ├── code-quality.md             [Command: /code-quality - Quality checks on directory]
│   │   │                               Trigger: Manual (/code-quality <directory>)
│   │   │                               Safe to Ignore: Yes
│   │   │
│   │   ├── docs-sync.md                [Command: /docs-sync - Documentation alignment check]
│   │   │                               Trigger: Manual (/docs-sync)
│   │   │                               Safe to Ignore: Yes
│   │   │
│   │   └── ticket.md                   [Command: /ticket - JIRA/Linear workflow integration]
│   │                                   Trigger: Manual (/ticket <ticket-id>)
│   │                                   External Dependencies: MCP server (JIRA or Linear)
│   │                                   Safe to Ignore: Yes
│   │                                   Extension: Customize for team's ticket system
│   │
│   ├── hooks/                          [Directory: Hook scripts and configuration]
│   │   ├── skill-eval.sh               [Hook: Skill evaluation wrapper script]
│   │   │                               Trigger: UserPromptSubmit (automatic on every prompt)
│   │   │                               External Dependencies: node (for skill-eval.js)
│   │   │                               Safe to Ignore: Yes (can disable in settings.json)
│   │   │                               Extension: Modify skill-rules.json to customize
│   │   │
│   │   ├── skill-eval.js               [Hook: Node.js skill matching engine]
│   │   │                               Trigger: Called by skill-eval.sh
│   │   │                               Status: Active (via UserPromptSubmit hook)
│   │   │
│   │   ├── skill-rules.json            [Configuration: Pattern matching rules for skills]
│   │   │                               Feature: Skill suggestion system
│   │   │                               Status: Active (used by skill-eval.js)
│   │   │                               Extension: Add/modify skill triggers
│   │   │
│   │   └── skill-rules.schema.json     [Configuration: JSON schema for skill-rules.json]
│   │                                   Feature: Validation schema
│   │                                   Status: Reference document
│   │
│   └── skills/                         [Directory: Domain knowledge modules]
│       ├── README.md                   [Documentation: Skills overview]
│       │                               Feature: N/A
│       │                               Status: Reference documentation
│       │
│       ├── testing-patterns/           [Skill: Jest testing, TDD, mocking]
│       │   └── SKILL.md                Trigger: Automatic (keyword/file path matching)
│       │                               External Dependencies: Jest (for actual testing)
│       │                               Safe to Ignore: Yes
│       │                               Extension: Add project-specific test patterns
│       │
│       ├── systematic-debugging/       [Skill: Four-phase debugging methodology]
│       │   └── SKILL.md                Trigger: Automatic (bug/debug keywords)
│       │                               Safe to Ignore: Yes
│       │                               Extension: Add project-specific debug tools
│       │
│       ├── react-ui-patterns/          [Skill: React patterns, loading/error states]
│       │   └── SKILL.md                Trigger: Automatic (React file paths, UI keywords)
│       │                               External Dependencies: React
│       │                               Safe to Ignore: Yes
│       │                               Extension: Add team's React conventions
│       │
│       ├── graphql-schema/             [Skill: GraphQL queries, mutations, codegen]
│       │   └── SKILL.md                Trigger: Automatic (GraphQL keywords/files)
│       │                               External Dependencies: GraphQL, Apollo Client
│       │                               Safe to Ignore: Yes
│       │                               Extension: Add project's schema patterns
│       │
│       ├── core-components/            [Skill: Design system, component library]
│       │   └── SKILL.md                Trigger: Automatic (component keywords/files)
│       │                               External Dependencies: Project's design system
│       │                               Safe to Ignore: Yes
│       │                               Extension: Replace with actual design system docs
│       │
│       └── formik-patterns/            [Skill: Form handling, validation]
│           └── SKILL.md                Trigger: Automatic (form keywords/files)
│                                       External Dependencies: Formik
│                                       Safe to Ignore: Yes
│                                       Extension: Add team's form patterns
│
└── .github/                            [Directory: GitHub-specific configuration]
    └── workflows/                      [Directory: GitHub Actions workflows]
        ├── pr-claude-code-review.yml   [Workflow: Automated PR review]
        │                               Trigger: PR events or @claude mentions
        │                               External Dependencies: ANTHROPIC_API_KEY secret
        │                               Safe to Ignore: Yes (opt-in CI/CD)
        │                               Extension: Customize review criteria
        │
        ├── scheduled-claude-code-docs-sync.yml
        │                               [Workflow: Monthly documentation sync]
        │                               Trigger: Scheduled (1st of month) or manual
        │                               External Dependencies: ANTHROPIC_API_KEY secret
        │                               Safe to Ignore: Yes
        │                               Extension: Adjust schedule, scope
        │
        ├── scheduled-claude-code-quality.yml
        │                               [Workflow: Weekly code quality review]
        │                               Trigger: Scheduled (Sunday) or manual
        │                               External Dependencies: ANTHROPIC_API_KEY secret
        │                               Safe to Ignore: Yes
        │                               Extension: Customize quality checks
        │
        └── scheduled-claude-code-dependency-audit.yml
                                        [Workflow: Biweekly dependency updates]
                                        Trigger: Scheduled (1st & 15th) or manual
                                        External Dependencies: ANTHROPIC_API_KEY secret
                                        Safe to Ignore: Yes
                                        Extension: Adjust update strategy
```

## Status Legend

- **Active by default**: Automatically loaded/executed by Claude Code without configuration
- **Opt-in**: Requires manual invocation or explicit enablement
- **Configured but inactive**: Defined but requires external setup (API keys, etc.)
- **Dormant/Example**: Present as template/example, not functional without customization
- **Reference documentation**: Informational only, not executable

## Feature Type Summary

| Type | Count | Default Status |
|------|-------|----------------|
| Core Configuration | 3 files | Active by default |
| Skills | 6 modules | Auto-suggested (active) |
| Agents | 2 modules | Manual invocation |
| Commands | 6 workflows | Manual invocation |
| Hooks | 3 types (multiple instances) | Active by default |
| GitHub Workflows | 4 workflows | Opt-in (requires secrets) |
| MCP Servers | 8 configured | Requires API keys |
| LSP Support | Documentation only | Not configured |

## Notes

1. Most features are opt-in or require manual triggers
2. Hook system is active by default but can be disabled per-hook
3. Skills are automatically suggested but require user confirmation
4. GitHub workflows require ANTHROPIC_API_KEY secret to function
5. MCP servers are configured but require environment variables with API credentials
