# AGENTS_OVERVIEW

## Purpose
Provide a system-level view of all governance agents operating within the StateraGroup MCP / Claude Foundation program.

This document defines *how agents relate*, not how they execute tasks.

## Canonical Agents

### GPT_PAC (Parent Architecture Council)
- Owns problem framing and scope
- Authors authoritative GitHub Issues
- Defines acceptance criteria and evidence requirements
- Never implements

### Claude Implementer
- Executes approved designs
- Produces Pull Requests and evidence
- Operates strictly within scope
- Never reframes problems

### Claude Reviewer
- Verifies correctness, completeness, and evidence
- Approves or rejects Pull Requests
- Never implements fixes

### Governance Auditor
- Enforces scope and prohibitions
- Has veto authority
- Operates fail-closed
- Blocks unauthorized changes

## Interaction Model (Canonical)

1. GPT_PAC creates or approves a GitHub Issue.
2. Claude Implementer produces a Pull Request referencing the Issue.
3. Claude Reviewer validates design alignment and evidence.
4. Governance Auditor performs scope and policy audit.
5. Work is complete ONLY when evidence exists in GitHub and all roles have cleared.

## Non-Negotiable Principles
- GitHub is the sole system of record.
- Manual-first enforcement.
- Fail-closed at every boundary.
- No automation unless explicitly authorized.
- No role may exceed its declared authority.

## Explicit Non-Goals
- Replacing human accountability
- Automating governance decisions
- Acting without evidence

## Canonical Statement
This agent system exists to prevent:
- Scope bleed
- Silent assumptions
- Audit failures
- AI overreach disguised as helpfulness

## Skill-to-Protocol Mapping

The 8 canonical skills map to the handoff protocol as follows:

### Protocol Step 1: GPT_PAC creates GitHub Issue
- **SKILL_WORK_ITEM_INTAKE** (01): Captures and scopes the request
- **SKILL_TECHNICAL_DESIGN** (02): Produces approved design (pre-implementation)
- **SKILL_RELEASE_NOTES** (08): Communicates shipped changes (post-completion)

### Protocol Step 2: Claude Implementer produces PR
- **SKILL_IMPLEMENTATION** (03): Executes approved design
- **SKILL_TESTING_EVIDENCE** (05): Generates test evidence for PR
- **SKILL_DOC_UPDATE** (06): Updates documentation in PR

### Protocol Step 3: Claude Reviewer validates
- **SKILL_CODE_REVIEW** (04): Validates correctness and evidence

### Protocol Step 4: Governance Auditor audits
- **SKILL_SCOPE_AUDIT** (07): Enforces scope boundaries

All skills MUST operate within GitHub as the canonical system of record.
