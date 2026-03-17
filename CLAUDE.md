# CLAUDE.md — Laravel Project Context

---

## ⚠️ FIRST RUN INSTRUCTIONS (Read This First)

If the Stack section below still contains placeholder text, run `/new-project`
first. Do NOT run /laravel-setup until /new-project is complete and the project
brief and tech stack have been approved.

/new-project will:
1. Run a full discovery conversation to understand what the app needs to do
2. Ask questions one at a time about users, data, integrations, and deployment
3. Generate a project brief for your approval
4. Recommend the right tech stack with plain English explanations
5. Save everything to docs/project-brief.md
6. Automatically trigger /laravel-setup with full context loaded

---

## My Stack

- **Framework**: Laravel — (detect actual installed version from composer.json
  after installation, never assume or hardcode)
- **PHP**: (auto-detect from composer.json)
- **Frontend**: (populated by /new-project based on app requirements)
- **Database**: (auto-detect from .env — default MySQL)
- **Local Dev**: Laravel Valet + PHPStorm + TablePlus + Tinkerwell + Warp
- **Testing**: Pest
- **Queue**: (populated by /new-project based on app requirements)
- **Cache**: (auto-detect from .env)
- **Deployment**: Laravel Forge on VPS

---

## Laravel Installation Rule

When creating a new Laravel project, always install the latest stable version:

```bash
composer create-project laravel/laravel app-name
```

Never hardcode a version constraint like `laravel/laravel:^11` or
`laravel/laravel:^12`. Always let Composer resolve the latest stable release.
After installation, read the actual installed version from `composer.json`
and record it in the Stack section above.

---

## Filament Version Detection

When this project uses Filament, always detect the installed version before
generating any Filament-specific code:

```bash
composer show filament/filament | grep versions
```

Filament v3 and v4 have different panel configuration patterns. Never assume
the version — always check composer.json first.

---

## Third-Party Integrations

List any external APIs or services this application connects to.
Claude will use this to ensure all integrations follow consistent patterns,
are routed through dedicated service classes, and credentials are never
hardcoded outside of .env.

For each integration include:
- What it is used for in this application
- Where the API documentation lives
- How it authenticates (token, OAuth, key, etc.)
- The .env variable name(s) for credentials
- The dedicated service class that wraps it

When adding any new integration:
1. Add the credential key(s) to .env
2. Add the same key(s) with empty values and a comment to .env.example immediately
3. Add the integration details to this section
4. Create a dedicated service class in app/Services/
5. Never call external APIs directly from controllers or models

---

## Architecture Conventions

- **Business logic** lives in `app/Services/` — not in controllers or models
- **Controllers** are thin: validate input, call a service, return a response
- **Models** handle relationships, scopes, and casts — no business logic
- **Form Requests** handle all validation
- **Policies** handle all authorization
- **Jobs** handle anything async
- **Events/Listeners** for decoupled side effects
- **Never call external APIs directly from controllers** — always use a
  dedicated service class in app/Services/

---

## Filament Conventions (when Filament is the frontend)

- **Resources** for all CRUD interfaces — never build manual controllers
  for data that Filament can manage
- **Pages** for custom dashboard views and non-CRUD screens
- **Widgets** for stats panels, charts, and data summaries
- **Actions** for buttons and user-triggered operations
- Keep Resources in `app/Filament/Resources/`
- Keep Pages in `app/Filament/Pages/`
- Keep Widgets in `app/Filament/Widgets/`
- Always check if Filament has a built-in component before building custom
- Always check the installed Filament version before generating code

---

## Coding Standards

- PHP 8.1+ features preferred: enums, readonly properties, intersection types
- Strict types declared in all files: `declare(strict_types=1);`
- Type hints on all method signatures (parameters + return types)
- PHPDoc only when types alone cannot express intent
- Eloquent over raw queries; raw queries only for performance-critical paths
- Prefer named arguments for clarity on methods with multiple parameters
- Early returns over nested conditionals

---

## Database and Performance Standards

- Every foreign key column must have an index
- Every column used in WHERE or ORDER BY should be evaluated for indexing
- Always use eager loading — never lazy load in loops (N+1 queries)
- Use `with()` when fetching related models that will be displayed
- Use `chunk()` or `cursor()` for large dataset operations
- Never load more columns than needed — use `select()` to limit columns
  on large tables
- Review query count during development — N+1 queries must be resolved
  before any PR is opened

---

## Security Standards

- Always validate input through Form Requests — never trust raw request data
- Always authorize actions through Policies
- Rate limit all API endpoints and any public-facing forms
- Never store sensitive data in session or cookies unencrypted
- Never expose internal IDs in URLs — use UUIDs or route model binding
- Always use HTTPS in production
- Sanitize any user input that gets displayed back in the UI

---

## Environment Variable Standards

- Every credential and environment-specific value goes in .env
- Never hardcode API keys, URLs, or credentials anywhere in the codebase
- Every time a new .env variable is added, add the same key with an empty
  value and an explanatory comment to .env.example immediately
- .env is never committed to git
- .mcp.json is never committed to git (contains GitHub token)

---

## Testing Standards

- Integration tests preferred over unit tests
- Tests live in `tests/Feature/` and `tests/Unit/`
- Every new feature gets at least one Feature test
- Every bug fix gets a regression test that would have caught the bug
- Use `RefreshDatabase` for tests that touch the DB
- Factories required for all models — no hardcoded test data
- Seeders required for local development — `php artisan db:seed` should
  produce a realistic local dataset to develop against
- Use Pest syntax for all tests

---

## Documentation Standards

Two levels of documentation are required:

**Level 1 — PHPDoc (in code)**
- Every Service class must have a class-level PHPDoc explaining what it does
  and what external system it interacts with (if any)
- Every public method in a Service class must have a PHPDoc explaining
  parameters, return value, and what it does
- Complex business logic must have inline comments explaining the why

**Level 2 — Plain English (docs/ folder)**
- `docs/project-brief.md` — generated by /new-project, never delete
- `docs/architecture.md` — plain English explanation of how major pieces
  of the app work, updated as the app grows
- `docs/integrations.md` — plain English explanation of each external
  integration: what it does, how it authenticates, and any gotchas
- When a major feature is completed, update docs/architecture.md with
  a plain English description of how it works

**Level 3 — API Documentation (if app exposes API endpoints)**
- Every API endpoint must be documented in `docs/api.md`
- Documentation must include: route, method, authentication requirement,
  all parameters with types, and an example response
- Update `docs/api.md` as part of the same commit when a new endpoint
  is added or an existing one changes
- Never leave an undocumented API endpoint — if Claude adds an endpoint,
  it documents it immediately

---

## Claude Activity Log

Every project maintains a `docs/claude-log.md` file that records
everything Claude Code does. This log exists so you can always review
what happened even if the IDE session scrolls away or closes.

### What gets logged
Claude must append a log entry after every one of these events:
- /new-project completes and hands off to /laravel-setup
- /laravel-setup completes each major step
- /laravel-setup checklist is fully completed
- /new-feature plan is approved and worktree is created
- /new-feature build is complete and PR is opened
- /fix-bug diagnosis is confirmed and fix is applied
- /fix-bug PR is opened
- Any MCP verification (pass or fail)
- Any commit and push to GitHub
- Any error or unexpected situation that caused a stop

### Log entry format

```markdown
## [DATE TIME]

**Action:** [What happened in plain English]
**Files changed:** [List files created or modified, or "none"]
**Commit:** [hash and message, or "none"]
**Push:** [confirmed / failed / not applicable]
**Notes:** [Any relevant context, decisions made, or issues encountered]

---
```

### Log rules
- Never delete entries from the log — it is append-only
- Always include the date and time at the start of each entry
- Keep entries concise but complete enough to understand what happened
- If a step was skipped explain why in the notes
- If something failed explain what failed and what was done about it
- The log is committed to git so it travels with the project

---

## Changelog Standards

Maintain CHANGELOG.md using Keep a Changelog format:
(https://keepachangelog.com/en/1.0.0/)

- `Added` for new features
- `Changed` for changes to existing functionality
- `Fixed` for bug fixes
- `Removed` for removed features
- `Security` for security fixes

Update CHANGELOG.md as part of every PR — not as an afterthought.
New entries go under `[Unreleased]` until a version is tagged.

---

## Deployment Standards (Laravel Forge)

This project deploys via Laravel Forge on a VPS. Follow these conventions:

- Environment variables are managed via the Forge dashboard — never in
  deployment scripts
- Queue workers are configured via Forge's Worker tab — document any
  required workers in docs/architecture.md
- Scheduled tasks are configured via Forge's Scheduler tab
- Zero-downtime deployments — always use `php artisan down` pattern or
  Forge's deployment hooks
- Never hard-code server paths — use Laravel's storage_path() and base_path()
- Document any Forge-specific configuration in docs/architecture.md

---

## Local Development Standards

This project uses **Laravel Valet** for local development.

- Local site URL follows Valet convention: `[project-folder-name].test`
- Do not use `php artisan serve` — use Valet
- Ensure Valet is running before starting development: `valet start`
- If the project uses queues, run the queue worker in a separate terminal:
  ```bash
  php artisan queue:work
  ```
- If the project uses the scheduler locally, run:
  ```bash
  php artisan schedule:work
  ```
- Document the local Valet domain in the project README and docs/architecture.md

When generating setup instructions or README content, always reference
Valet rather than `php artisan serve`.

---

## Laravel Upgrade Policy

Laravel releases a new major version approximately every year (Q1).
When a new major version is released:

### How to know when to upgrade
- Laravel News (laravel-news.com) announces new releases
- Context7 will start returning documentation for the new version
- composer.json will show a newer version available

### Upgrade process
1. Check the official upgrade guide at laravel.com/docs/[version]/upgrade
2. Use Laravel Shift (laravelshift.com) for automated upgrade assistance —
   it opens a PR with the required changes
3. After Shift runs, review the PR carefully before merging
4. Run the full test suite after upgrading: `php artisan test`
5. Run Pint after upgrading: `./vendor/bin/pint`
6. Update the Framework version in CLAUDE.md after a successful upgrade

### Claude's role in upgrades
- Never automatically upgrade Laravel without being asked
- When asked to upgrade, always read the official upgrade guide first
  via Context7 before making any changes
- Always create a dedicated worktree for the upgrade:
  `claude --worktree upgrade-laravel-[version]`
- Never upgrade in the middle of a feature branch
- Run the full test suite before and after to identify breakage

---

## Git Workflow

Claude must follow these rules automatically without being reminded.

### Commit Rules
- Commit after every meaningful change
- Never commit directly to main — all work happens in a worktree branch
- Always run tests before committing — never commit broken code
- Write commit messages that describe why the change was made
- Always run Laravel Pint before committing to enforce consistent code
  formatting: `./vendor/bin/pint`
  If Pint is not installed, install it: `composer require laravel/pint --dev`
  Commit any formatting changes Pint makes as part of the same commit —
  never as a separate commit

### Commit Message Format
```
type: short description of what changed and why

Types:
feat     — new feature
fix      — bug fix
test     — adding or updating tests
refactor — code improvement with no behavior change
chore    — config, dependencies, tooling
docs     — documentation changes
```

### Push Rules
- Push to remote branch after every commit
- After pushing, report: what was committed, branch name, GitHub link
- Never let commits pile up locally

### Pull Request Rules
- Only open a PR when feature or fix is fully complete and tests pass
- PR title must match commit message format
- Always target main
- Include a brief description of what changed and why

---

## Seeding Standards

- Every model must have a factory
- The DatabaseSeeder must produce a realistic local dataset
- Running `php artisan db:seed` should give enough fake data to develop
  and test against without needing real production data
- Seeders should be idempotent — safe to run multiple times

---

## MCP Servers Active in This Project

See `.mcp.json` for full configuration. Key servers:

- **laravel-boost** — DB schema, routes, artisan commands, Laravel docs
- **github** — PR and branch management
- **context7** — current documentation for Laravel, PHP, Filament, packages
- **filesystem** — scoped read/write access to this project

---

## Preferred Workflow

1. New project → `/new-project` (discovery + tech stack + brief)
2. New feature → `/new-feature` (plan + worktree + build + review)
3. Bug fix → `/fix-bug` (diagnose + isolate + fix + test + merge)

---

## Self-Improvement Loop

After any correction from the user — a wrong assumption, a repeated mistake,
or a misunderstood pattern — do the following immediately:

1. Write a new rule to `.claude/lessons.md` that would have prevented the mistake
2. Frame it as a specific, actionable rule: start with "Never..." or "Always..."
3. One rule per mistake — no vague reminders

At the start of each session, read `.claude/lessons.md` and apply all rules
before writing any code.

The goal is to make the same mistake exactly once.

---

## Notes for Claude

- The setup chain is mandatory and must complete in full before any
  building begins: /new-project → docs/project-brief.md saved →
  /laravel-setup → full checklist confirmed → docs/claude-log.md created
- Never begin building features if /laravel-setup has not completed
  its full checklist — if unsure whether setup ran, check
  docs/claude-log.md for a setup entry before proceeding
- Never assume the GitHub MCP is connected — verify it explicitly
  during /laravel-setup and stop loudly if it fails
- Never leave the generic Laravel README — replace it with a
  project-specific README with shields.io badges during /laravel-setup
- Never skip a log entry — append to docs/claude-log.md after every
  meaningful action without exception
- If /new-project completes but /laravel-setup was not triggered,
  tell the user immediately rather than proceeding with any building
- A project is not ready to build until all of the following exist
  and are confirmed:
  - docs/project-brief.md with real project content
  - docs/claude-log.md with at least one setup entry
  - A project-specific README.md with shields.io badges
  - Confirmed GitHub MCP connection
  - Completed /laravel-setup checklist
- Always install Laravel with no version constraint —
  `composer create-project laravel/laravel app-name`
- Always detect actual installed versions from composer.json — never assume
- Always check Filament version before generating Filament-specific code
- Always ask clarifying questions before generating migrations
- When adding packages, check if Laravel or Filament has a first-party solution
- Always update .env.example when adding new .env variables
- Always update CHANGELOG.md when completing a feature or fix
- Always update docs/architecture.md when completing a major feature
- Check for N+1 queries before every PR
- Deploy via Laravel Forge — document Forge-specific config in docs/
