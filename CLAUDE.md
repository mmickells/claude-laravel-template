# CLAUDE.md — Laravel Project Context

---

## ⚠️ FIRST RUN INSTRUCTIONS (Read This First)

If the Stack section below still contains placeholder text like "update this", run the following initialization routine before doing anything else:

1. Detect what you can automatically by reading `composer.json`, `.env`, and scanning the `app/` directory
2. For anything you can't determine automatically, ask me directly — one question at a time, not all at once
3. Once I answer, update this CLAUDE.md file with the correct values
4. Remove this "First Run Instructions" section entirely after the stack is filled in
5. Confirm to me that CLAUDE.md has been updated and is ready

Questions to ask me if you can't detect them automatically:
- What frontend stack are you using? (Blade only / Livewire / Tailwind / Inertia + Vue / Inertia + React)
- What is your preferred testing framework? (PHPUnit / Pest)
- Are you using queues? If so, what driver? (Redis / Database / Sync / none)
- What is the main purpose of this application in plain English?
- Are there any architectural decisions I should know about that aren't obvious from the code?

Do not ask about things you can already detect (PHP version, Laravel version, database driver, installed packages).

---

## My Stack

- **Framework**: Laravel — (auto-detect from composer.json)
- **PHP**: (auto-detect from composer.json)
- **Frontend**: (update: Blade / Livewire / Tailwind / Inertia + Vue / Inertia + React)
- **Database**: (auto-detect from .env)
- **Local Dev**: Laravel Valet + PHPStorm + TablePlus + Tinkerwell
- **Testing**: (update: PHPUnit / Pest)
- **Queue**: (update: Redis / Database / Sync / not used)
- **Cache**: (auto-detect from .env)

---

## Architecture Conventions

- **Business logic** lives in `app/Services/` — not in controllers or models
- **Controllers** are thin: validate input, call a service, return a response
- **Models** handle relationships, scopes, and casts — no business logic
- **Form Requests** handle all validation
- **Policies** handle all authorization
- **Jobs** handle anything async
- **Events/Listeners** for decoupled side effects

> Update this section to reflect the actual architecture decisions for this specific project.

---

## Coding Standards

- PHP 8.1+ features preferred: enums, readonly properties, intersection types
- Strict types declared in all files: `declare(strict_types=1);`
- Type hints on all method signatures (parameters + return types)
- PHPDoc only when types alone can't express intent
- Eloquent over raw queries; raw queries only for performance-critical paths
- Prefer named arguments for clarity on methods with multiple parameters
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
- Don't use raw user input in queries (always use bindings)
- Don't generate migrations without reviewing them with me first
- Don't assume a package is available — check `composer.json` first

---

## MCP Servers Active in This Project

See `.mcp.json` for full configuration. Key servers:

- **laravel-boost** — DB schema, routes, artisan commands, Laravel docs for this version
- **github** — PR and branch management
- **filesystem** — scoped read/write access to this project

---

## Preferred Workflow for New Features

1. Run `/new-feature` to start — Claude analyzes the codebase and asks questions before writing anything
2. Review and approve the plan
3. Claude builds in an isolated worktree (`claude --worktree feature-name`)
4. Review the diff, run tests
5. Run `laravel-simplifier` agent for a code review pass
6. Merge

---

## Notes for Claude

- Always ask clarifying questions before generating migrations — schema changes are hard to undo
- When adding packages, check if Laravel has a first-party solution first
- Prefer Eloquent relationships over manual joins
- When writing tests, use Pest syntax if Pest is installed
- Always check `composer.json` before assuming a package is available
- Keep controllers thin — if logic is growing in a controller, move it to a Service