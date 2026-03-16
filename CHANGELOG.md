# Changelog

All notable changes to this project will be documented in this file.
Format follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/).

Types of changes:
- `Added` for new features
- `Changed` for changes to existing functionality
- `Fixed` for bug fixes
- `Removed` for removed features
- `Security` for security fixes

---

## [Unreleased]

### Added
- Initial Laravel Claude Code template
- CLAUDE.md with persistent project context and comprehensive standards
- .mcp.json with Laravel Boost, GitHub, Context7, and filesystem MCP servers
- /new-project slash command — full discovery conversation, project brief, tech stack, domain agent
- /laravel-setup slash command — full project onboarding including API key verification
- /new-feature slash command — plan-first feature workflow with adaptive build order, Pint, N+1, and security checks
- /fix-bug slash command — diagnose-first bug fix workflow with post-fix quality checks
- .claude/lessons.md — self-improvement log Claude updates after any correction
- .gitignore with .mcp.json excluded to protect GitHub tokens
- .env.example template maintained as integrations are added
- CHANGELOG.md following Keep a Changelog format
- docs/ folder scaffolding: architecture.md, integrations.md, api.md
- Domain agent generation — .claude/agents/domain-expert.md created per project
- Laravel Pint enforced before every commit and PR
- Laravel Valet local development standard
- Laravel upgrade policy and upgrade process documentation
