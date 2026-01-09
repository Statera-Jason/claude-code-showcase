# Project Snapshot Overview

**Snapshot Date:** 2026-01-09
**Branch:** claude/analyze-project-snapshot-hs9Mz
**Repository:** claude-code-showcase

## Repository Purpose

This is a demonstration and template repository that showcases Claude Code configuration capabilities. It serves as both:

1. A reference implementation of Claude Code features
2. A template for teams to copy and adapt for their own projects

The repository does not contain production application code. Instead, it contains configuration, documentation, and examples of Claude Code integration patterns.

## Intended Usage Pattern

**Attachment Model**: This repository is designed to be attached to development projects as a reference or template. Teams would copy relevant configuration files into their own repositories rather than importing this as a dependency.

**Integration Model**: Not applicable. This is not a library or framework to be imported.

## Claude Code Feature Surface

This repository demonstrates the following Claude Code capabilities:

### Core Configuration
- CLAUDE.md project memory file
- settings.json with hook configurations
- MCP server integrations (.mcp.json)
- LSP server support (documentation only, no actual LSP configured)

### Extensibility Features
- Skills system (6 domain-specific skills)
- Agents (2 specialized agents)
- Slash commands (6 workflow commands)
- Hook system (UserPromptSubmit, PreToolUse, PostToolUse)
- Skill evaluation engine with pattern matching

### Automation & CI/CD
- GitHub Actions workflows (4 automated workflows)
- PR review automation
- Scheduled maintenance workflows

## Tailoring Status

**No tailoring has been performed.** This repository exists in its original state as a showcase and template. It is designed to be copied and tailored by other teams for their specific needs.

The examples provided (React, TypeScript, GraphQL, etc.) are illustrative patterns rather than project-specific implementations.

## Key Characteristics

1. **Read-Only Reference**: Designed to be studied and copied, not modified directly
2. **Example-Driven**: Contains example patterns for common development scenarios
3. **Framework-Agnostic Base**: While examples use React/TypeScript, the core patterns apply to any stack
4. **Documentation-Heavy**: Extensive inline documentation explaining each feature
5. **Zero Production Code**: No actual application code, only configuration and examples

## Repository State

- Git repository: Yes
- Current branch: claude/analyze-project-snapshot-hs9Mz
- Remote repository: Yes (GitHub)
- Active development: No (stable showcase state)

## Dependencies

This repository has minimal dependencies:
- Node.js/npm (for hook scripts and MCP server execution)
- Git (for version control and GitHub workflows)
- GitHub Actions (for automated workflows)
- Anthropic API key (for Claude Code and GitHub Actions)

All dependencies are external tools rather than code dependencies.
