# CLAUDE.md — Laravel Project Context

This file gives Claude Code persistent context about this Laravel project and the development workflow we use.

---

## My Stack

- **Framework**: Laravel (10, 11, or 12)
- **PHP**: 8.1+
- **Frontend**: Livewire / Blade / Tailwind CSS (update if using Inertia/Vue/React)
- **Database**: MySQL / PostgreSQL / SQLite (update to match your .env)
- **Local Dev**: Laravel Valet + PHPStorm + TablePlus + Tinkerwell
- **Testing**: PHPUnit / Pest
- **Queue**: Redis / Database (update as needed)

---

## Architecture Conventions

- **Business logic** lives in `app/Services/` — not in controllers or models
- **Controllers** are thin: validate input, call a service, return a response
- **Models** handle relationships, scopes, and casts — no business logic
- **Form Requests** handle all validation
- **Policies** handle all authorization
- **Jobs** handle anything async
- **Events/Listeners** for decoupled side effects

> Update this section to reflect YOUR actual architecture decisions.

---

## Coding Standards

- PHP 8.1+ features preferred: enums, readonly properties, intersection types, fibers
- Strict types declared in all files: `declare(strict_types=1);`
- Type hints on all method signatures (parameters + return types)
- PHPDoc only when types can't express the intent
- Eloquent over raw queries; raw queries only for performance-critical paths
- Prefer named arguments for clarity on methods with multiple params
- Early returns over nested conditionals

---

## Testing Philosophy

- Integration tests preferred over unit tests
- Tests live in `tests/Feature/` and `tests/Unit/`
- Every new feature gets a corresponding test
- Use `RefreshDatabase` for tests that touch the DB
- Factories for all test data — no hardcoded fixtures

---

## What NOT to Do

- Don't put business logic in controllers or models
- Don't skip Form Requests for validation
- Don't use `->get()` when you mean `->first()`
- Don't commit `.env` changes
- Don't use raw user input in queries (use bindings)
- Don't generate migrations without reviewing them first

---

## MCP Servers Active in This Project

See `.mcp.json` for the full configuration. Key servers:

- **laravel-boost** — DB schema, routes, artisan, docs search
- **github** — PR and branch management
- **database** — Direct DB queries (read-only)

---

## Preferred Workflow for New Features

1. Start in Plan Mode — ask Claude to analyze the codebase and propose an approach before writing code
2. Review the plan and adjust
3. Let Claude implement in a worktree (`claude --worktree feature-name`)
4. Review the diff before merging
5. Run `php artisan boost:mcp` to verify nothing is broken

---

## Notes for Claude

- Ask clarifying questions before generating migrations — schema changes are hard to undo
- When adding packages, check if Laravel has a first-party solution first
- Prefer Eloquent relationships over manual joins
- When writing tests, use `pest()` syntax if Pest is installed
- Always check `composer.json` before assuming a package is available
