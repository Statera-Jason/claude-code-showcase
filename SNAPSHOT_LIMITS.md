# Repository Limits and Constraints

**Snapshot Date:** 2026-01-09

This document identifies what this repository cannot do without modification, assumptions baked into the current setup, and areas where governance or safety controls are not yet enforced.

## What This Repository Cannot Do Without Modification

### 1. Production Application Development
- **Limitation:** No actual application code exists
- **Current State:** Configuration and examples only
- **To Enable:** Add production code, build tooling, deployment configuration
- **Impact:** This is purely a template/showcase repository

### 2. Language-Specific Support Beyond JavaScript/TypeScript
- **Limitation:** All skills, hooks, and examples assume JavaScript/TypeScript
- **Current State:**
  - Hooks check for `.js`, `.jsx`, `.ts`, `.tsx` file extensions
  - Skills reference React, GraphQL, Formik (JS ecosystem)
  - No Python, Go, Rust, Java skills or hooks present
- **To Enable:**
  - Create language-specific skills
  - Add hooks for language-specific formatters and linters
  - Configure LSP for target languages
- **Impact:** Non-JS projects would need substantial reconfiguration

### 3. Framework Support Beyond React
- **Limitation:** All UI patterns assume React
- **Current State:**
  - react-ui-patterns skill is React-specific
  - core-components assumes React component patterns
  - formik-patterns is tied to React forms
- **To Enable:**
  - Replace/supplement with Vue, Angular, Svelte skills
  - Update hook patterns for framework-specific files
- **Impact:** Other frameworks would need custom skills

### 4. Non-GraphQL API Patterns
- **Limitation:** API examples assume GraphQL with Apollo Client
- **Current State:** graphql-schema skill is GraphQL-specific
- **To Enable:**
  - Create REST API, gRPC, tRPC skills
  - Add OpenAPI/Swagger integration
- **Impact:** REST-based projects need alternative skills

### 5. Non-Jest Testing Frameworks
- **Limitation:** Testing patterns assume Jest
- **Current State:**
  - testing-patterns skill references Jest syntax
  - PostToolUse hook runs `npm test` assuming Jest
- **To Enable:**
  - Create skills for Vitest, Mocha, Pytest, etc.
  - Update test runner hooks
- **Impact:** Other test frameworks need custom configuration

### 6. Monorepo Support
- **Limitation:** Configuration assumes single package structure
- **Current State:**
  - Hooks operate on single package.json
  - No workspace-aware commands
  - Skills don't account for monorepo patterns
- **To Enable:**
  - Add workspace detection logic
  - Update hooks for workspace-aware npm/yarn/pnpm
  - Create monorepo-specific skills
- **Impact:** Monorepos would have limited hook effectiveness

### 7. Multi-Language Projects
- **Limitation:** Assumes homogeneous tech stack
- **Current State:** All configuration points to single stack (JS/TS + React)
- **To Enable:** Add polyglot skills and conditional hook activation
- **Impact:** Full-stack projects (e.g., Python backend + React frontend) need expanded configuration

### 8. Custom CI/CD Beyond GitHub Actions
- **Limitation:** All automation examples use GitHub Actions
- **Current State:** No GitLab CI, CircleCI, Jenkins examples
- **To Enable:** Create equivalent workflow files for other CI/CD platforms
- **Impact:** Teams using other platforms need to adapt workflows

### 9. Enterprise Authentication/Authorization
- **Limitation:** No SSO, SAML, or enterprise auth integration
- **Current State:** MCP servers use simple API tokens
- **To Enable:** Configure OAuth flows, corporate SSO
- **Impact:** Enterprise environments may need security layer

### 10. Offline Operation
- **Limitation:** Many features require internet connectivity
- **Current State:**
  - MCP servers call external APIs
  - npm/npx downloads packages
  - GitHub Actions require cloud access
- **To Enable:** Configure local mirrors, air-gap compatible tools
- **Impact:** Air-gapped environments need significant adaptation

## Baked-In Assumptions

### Technology Stack Assumptions

#### JavaScript/TypeScript
- **Assumption:** Project uses npm as package manager
- **Evidence:** All hooks use `npm` commands
- **Alternative:** Could use yarn, pnpm with modifications
- **Risk:** Hooks will fail if npm is not available

#### React Framework
- **Assumption:** UI is built with React
- **Evidence:** Multiple React-specific skills
- **Reality Check:** No actual React code in this repository
- **Risk:** Non-React teams would have irrelevant skills

#### GraphQL with Apollo
- **Assumption:** API layer uses GraphQL with Apollo Client
- **Evidence:** graphql-schema skill
- **Reality Check:** No actual GraphQL code present
- **Risk:** REST API teams would have irrelevant guidance

#### Jest Testing
- **Assumption:** Tests use Jest framework
- **Evidence:** testing-patterns skill, test runner hook
- **Risk:** Other test frameworks would fail hook execution

### Development Environment Assumptions

#### Unix-Like Operating System
- **Assumption:** Development on Linux or macOS
- **Evidence:**
  - Hooks use bash syntax (`[[`, `PIPESTATUS`)
  - Shell scripts assume Unix paths
- **Known Issue:** Hooks currently failing due to sh vs bash mismatch
- **Risk:** Windows users without WSL may have compatibility issues

#### Git Repository
- **Assumption:** Project is a git repository
- **Evidence:**
  - Branch protection hooks check `git branch --show-current`
  - Multiple commands rely on git history
- **Risk:** Non-git VCS would need alternative implementation

#### GitHub Hosting
- **Assumption:** Repository hosted on GitHub
- **Evidence:** GitHub Actions workflows, gh CLI usage
- **Risk:** GitLab, Bitbucket users need alternative automation

#### Node.js Availability
- **Assumption:** Node.js installed on system
- **Evidence:**
  - MCP servers use `npx`
  - Skill evaluation uses Node.js script
- **Risk:** Systems without Node.js cannot use most features

### Organizational Assumptions

#### Single-Team Development
- **Assumption:** Repository used by single team with shared conventions
- **Evidence:** No multi-tenant configuration, single CLAUDE.md
- **Risk:** Multi-team organizations need namespace separation

#### English Language
- **Assumption:** All documentation and prompts in English
- **Evidence:** All files are English-only
- **Risk:** Non-English teams would need translations

#### Continuous Access to External Services
- **Assumption:** Always-on internet, no network restrictions
- **Evidence:** MCP servers, npm registry access, API calls
- **Risk:** Restricted networks may block functionality

## Missing Governance and Safety Controls

### 1. Code Review Requirements
- **Status:** Not enforced
- **Current State:** Code review agent exists but is opt-in
- **Missing:**
  - Required approvals before merge
  - Automated blocking on critical issues
  - Escalation paths for security concerns
- **Risk:** Code could be merged without review

### 2. Security Scanning
- **Status:** Not implemented
- **Current State:** No security linting, SAST, or dependency scanning
- **Missing:**
  - Vulnerability scanning for dependencies
  - Secret detection in commits
  - SAST for security anti-patterns
- **Risk:** Security vulnerabilities may go undetected

### 3. Access Control
- **Status:** Not configured
- **Current State:** Hooks protect main branch, but no role-based controls
- **Missing:**
  - User role definitions
  - Permission boundaries for different team members
  - Audit logging of Claude Code actions
- **Risk:** Any user can perform any action

### 4. Cost Controls
- **Status:** Not enforced
- **Current State:** GitHub Actions workflows can run unbounded
- **Missing:**
  - Budget limits for API usage
  - Rate limiting on workflow triggers
  - Cost allocation by team/project
- **Risk:** Unexpected API costs

### 5. Data Privacy
- **Status:** Not addressed
- **Current State:** No PII detection or data classification
- **Missing:**
  - PII detection in prompts and code
  - Data residency controls
  - GDPR/CCPA compliance measures
- **Risk:** Sensitive data could be sent to external APIs

### 6. Compliance and Audit
- **Status:** Not implemented
- **Current State:** No audit trail of Claude Code operations
- **Missing:**
  - Audit logging of all actions
  - Compliance reporting
  - Change tracking and attribution
- **Risk:** Cannot prove compliance with regulations

### 7. Secrets Management
- **Status:** Partially addressed
- **Current State:**
  - MCP configs use environment variables (good)
  - No validation that secrets aren't committed
- **Missing:**
  - Pre-commit hooks to detect secrets
  - Integration with corporate secret managers
  - Rotation policies
- **Risk:** Secrets could be accidentally committed

### 8. Quality Gates
- **Status:** Not enforced
- **Current State:** PostToolUse hooks are non-blocking for most checks
- **Missing:**
  - Mandatory quality thresholds
  - Blocking on test failures
  - Coverage requirements
- **Risk:** Low-quality code can be committed

### 9. Dependency Management
- **Status:** Minimal
- **Current State:** Dependency audit workflow exists but is scheduled, not blocking
- **Missing:**
  - License compliance checking
  - Vulnerability blocking on high-severity issues
  - Approved dependency lists
- **Risk:** Unapproved or vulnerable dependencies could be added

### 10. Rate Limiting
- **Status:** Not implemented
- **Current State:** No limits on hook execution or API calls
- **Missing:**
  - Throttling for expensive operations
  - Circuit breakers for failing services
  - Backoff strategies
- **Risk:** Runaway costs or service disruption

## Technical Debt and Known Issues

### 1. Shell Compatibility Issue
- **Issue:** Hooks use bash syntax but execute with `/bin/sh`
- **Evidence:** Syntax errors in PostToolUse hooks (this snapshot)
- **Impact:** Formatting, testing, and type-checking hooks may fail
- **Resolution Needed:** Update hooks to POSIX-compliant syntax or ensure bash execution

### 2. No Actual Production Code
- **Issue:** Repository is examples-only
- **Evidence:** No src/ directory, no runnable application
- **Impact:** Cannot demonstrate end-to-end functionality
- **Resolution Needed:** Not applicable (this is by design for a showcase)

### 3. Hardcoded Assumptions in Skills
- **Issue:** Skills reference specific libraries and patterns
- **Evidence:** Formik, Apollo, Jest mentioned by name
- **Impact:** Skills become outdated if stack changes
- **Resolution Needed:** Parameterize or create skill variants

### 4. No Error Recovery in Hooks
- **Issue:** Hooks may fail silently or without clear guidance
- **Evidence:** PostToolUse hooks have minimal error messages
- **Impact:** Users may not understand why hooks failed
- **Resolution Needed:** Improve error messages and fallback behavior

### 5. Single Anthropic Model
- **Issue:** GitHub Actions hardcode specific Claude model
- **Evidence:** Workflows use `claude-opus-4-5-20251101`
- **Impact:** Model deprecation would break workflows
- **Resolution Needed:** Use model selection strategy or latest aliases

## Scale and Performance Limitations

### 1. Large Repositories
- **Limitation:** No optimization for large codebases
- **Evidence:** Skills and commands operate on full repository
- **Impact:** Slow performance on repositories with 100k+ files
- **Mitigation Needed:** Add scope limiting, caching strategies

### 2. High-Frequency Changes
- **Limitation:** Hooks run on every file change
- **Evidence:** PostToolUse hooks trigger for every Edit/Write
- **Impact:** Could slow down rapid development
- **Mitigation Needed:** Debouncing, batching, or conditional execution

### 3. Concurrent Development
- **Limitation:** No coordination between multiple Claude Code instances
- **Evidence:** No locking or state synchronization
- **Impact:** Race conditions possible with parallel sessions
- **Mitigation Needed:** State management for shared resources

### 4. API Rate Limits
- **Limitation:** No handling of Anthropic API rate limits
- **Evidence:** GitHub Actions make unbounded API calls
- **Impact:** Workflows could fail under high load
- **Mitigation Needed:** Retry logic, backoff, rate limiting

## Documentation Gaps

### 1. Migration Guide
- **Missing:** Step-by-step guide to adapt this repository for a new project
- **Impact:** Teams may struggle to customize effectively

### 2. Troubleshooting Guide
- **Missing:** Common problems and solutions
- **Impact:** Users may not know how to fix hook failures or MCP issues

### 3. Cost Estimation
- **Missing:** Detailed cost breakdown for different usage patterns
- **Impact:** Teams cannot budget accurately

### 4. Security Best Practices
- **Missing:** Security hardening guide
- **Impact:** Teams may deploy with insecure configurations

### 5. Performance Tuning
- **Missing:** Guide to optimize for large repositories
- **Impact:** Performance degradation in production

## Compatibility and Portability Constraints

### Cross-Platform
- **Limitation:** Unix-focused, limited Windows support
- **Workaround:** WSL or Git Bash for Windows users

### Air-Gapped Environments
- **Limitation:** Requires internet for MCP and npm
- **Workaround:** Local mirrors and offline MCP servers

### Enterprise Networks
- **Limitation:** Assumes open internet access
- **Workaround:** Proxy configuration may be needed

### Claude Code Version
- **Limitation:** Configuration format may change
- **Risk:** Future Claude Code versions may break configuration

## Summary of Critical Limitations

1. **No actual production code** - Template only
2. **JavaScript/TypeScript only** - Other languages need new configuration
3. **React-specific patterns** - Other frameworks not supported
4. **Unix-centric** - Windows compatibility issues
5. **No security controls** - Missing SAST, secret scanning, access control
6. **No cost controls** - Unbounded API usage possible
7. **Shell compatibility issue** - Hooks currently failing
8. **Single-team focus** - Multi-tenant setup not supported
9. **Internet required** - Most features need connectivity
10. **No governance framework** - Compliance and audit gaps

These limitations do not prevent using this repository as a template, but teams must address them during customization for production use.
