<div align="center">

# Claude Laravel Template

**A batteries-included Laravel project template pre-wired for Claude Code**

[![Laravel](https://img.shields.io/badge/Laravel-10%2B-FF2D20?style=flat-square&logo=laravel&logoColor=white)](https://laravel.com)
[![PHP](https://img.shields.io/badge/PHP-8.1%2B-777BB4?style=flat-square&logo=php&logoColor=white)](https://php.net)
[![Claude Code](https://img.shields.io/badge/Claude_Code-Ready-blueviolet?style=flat-square&logo=anthropic&logoColor=white)](https://claude.ai/code)
[![Tailwind CSS](https://img.shields.io/badge/Tailwind_CSS-3.x-38B2AC?style=flat-square&logo=tailwind-css&logoColor=white)](https://tailwindcss.com)
[![Livewire](https://img.shields.io/badge/Livewire-3.x-FB70A9?style=flat-square&logo=livewire&logoColor=white)](https://livewire.laravel.com)
[![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)](LICENSE)

Drop this template into any Laravel project to get instant, context-aware Claude Code assistance — with architecture conventions, MCP servers, and a proven AI-assisted development workflow baked in from day one.

</div>

---

## What's Included

| File | Purpose |
|------|---------|
| `CLAUDE.md` | Persistent project context — Claude reads this on every session |
| `.mcp.json` | Pre-configured MCP servers: laravel-boost, github, filesystem |
| `.claude/commands/laravel-setup.md` | `/laravel-setup` — guided walkthrough for onboarding Claude to a new project |
| `.claude/commands/new-feature.md` | `/new-feature` — structured workflow for building any new feature |

---

## Why This Template

Working with Claude Code on a Laravel project without context means re-explaining your architecture on every session. This template solves that by giving Claude a stable, version-controlled understanding of:

- Your **stack** (framework, frontend, database, tooling)
- Your **architecture conventions** (Services, thin Controllers, Form Requests, Policies)
- Your **coding standards** (strict types, PHP 8.1+ features, early returns)
- Your **testing philosophy** (Pest, integration-first, RefreshDatabase)
- The **MCP servers** active in the project
- A **preferred workflow** for shipping new features with AI assistance

---

## Getting Started

### 1. Copy the template files into your Laravel project

```bash
cp CLAUDE.md /path/to/your-project/
cp .mcp.json /path/to/your-project/   # once you've created it
```

### 2. Customize CLAUDE.md

Open `CLAUDE.md` and update the sections marked with `(update ...)` to match your actual project:

```md
- **Frontend**: Livewire / Blade / Tailwind CSS   ← swap for Inertia/Vue/React if needed
- **Database**: MySQL / PostgreSQL / SQLite        ← match your .env
- **Queue**: Redis / Database                      ← match your config
```

### 3. Configure MCP servers

Copy `.mcp.json` to your project root and add your tokens:

```jsonc
{
  "mcpServers": {
    "laravel-boost": {
      "command": "php",
      "args": ["artisan", "boost:mcp"]
      // Requires: composer require laravel/boost --dev && php artisan boost:install
    },
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "YOUR_GITHUB_TOKEN_HERE" }
    },
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "."]
    }
  }
}
```

Optional servers (Figma, Context7) are commented out in `.mcp.json` — uncomment as needed.

> **Tip:** Keep active MCP servers under 10 and total tools under 30. Every server consumes context window tokens before you type your first message.

### 4. Launch Claude Code

```bash
cd your-project
claude
```

Claude will automatically pick up `CLAUDE.md` and have full context from the first message.

---

## Slash Commands

These live in `.claude/commands/` and are available as slash commands inside any Claude Code session.

### `/laravel-setup`

A guided, step-by-step onboarding sequence for a new project. Run this once per project to:

1. Install and verify Laravel Boost
2. Confirm `CLAUDE.md` is in place
3. Audit `.mcp.json` for relevant servers
4. Install official Laravel plugins
5. Onboard Claude to your schema, routes, and packages
6. Create a worktree for the first feature

### `/new-feature`

A structured workflow for every new feature. Enforces:

1. **Plan first** — analysis and clarifying questions before any code
2. **Proposed approach** — files to create/modify, migrations to review, tests to write
3. **Worktree isolation** — all work on a fresh branch, main stays clean
4. **Build order** — migration → model → form request → service → controller → routes → tests
5. **Review gate** — diff + test run before merging
6. **laravel-simplifier review** — code clarity pass after approval

---

## Architecture at a Glance

```
app/
├── Http/
│   ├── Controllers/     # Thin — validate, call service, return response
│   └── Requests/        # All validation lives here (Form Requests)
├── Models/              # Relationships, scopes, casts — no business logic
├── Services/            # All business logic lives here
├── Jobs/                # Async work
├── Events/
└── Listeners/           # Decoupled side effects

tests/
├── Feature/             # Integration tests (preferred)
└── Unit/                # Pure logic tests
```

---

## Recommended Workflow for New Features

```
1. Plan     →  Ask Claude to analyze the codebase and propose an approach
2. Review   →  Read the plan, adjust scope or approach
3. Build    →  claude --worktree feature-name  (isolated git worktree)
4. Diff     →  Review changes before merging
5. Verify   →  php artisan test && php artisan boost:mcp
```

Starting in Plan Mode (`Shift+Tab` to toggle) prevents wasted implementation effort and keeps Claude aligned with your actual architecture.

---

## Coding Standards Quick Reference

- `declare(strict_types=1)` in every file
- Type hints on all method signatures — parameters and return types
- PHP 8.1+ features: enums, readonly properties, named arguments
- Eloquent over raw queries (raw only for performance-critical paths)
- Early returns over nested conditionals
- PHPDoc only when types can't express intent

---

## Local Dev Tools

| Tool | Purpose |
|------|---------|
| [Laravel Valet](https://laravel.com/docs/valet) | Zero-config local HTTPS |
| [PHPStorm](https://www.jetbrains.com/phpstorm/) | IDE |
| [TablePlus](https://tableplus.com) | Database GUI |
| [Tinkerwell](https://tinkerwell.app) | In-app PHP REPL |

---

## Contributing

This is a personal template — fork it, make it yours. If you have an improvement that would be universally useful, PRs are welcome.

---

<div align="center">

Built for use with [Claude Code](https://claude.ai/code) · Laravel · PHP 8.1+

</div>
