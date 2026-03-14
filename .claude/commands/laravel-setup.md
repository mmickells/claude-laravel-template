# Laravel Claude Code Setup Walkthrough

Run this command once when starting Claude Code on a new Laravel project.
If you ran /new-project first, the project brief and tech stack are already
decided — skip any questions already answered there.

Walk through each step one at a time. Pause after each step and wait for
confirmation before moving to the next.

---

## Step 0 — Check for Project Brief

Before doing anything else, check if `docs/project-brief.md` exists:

```bash
cat docs/project-brief.md
```

If it exists, read it fully. All decisions in that brief are already
approved — do not ask about them again. Reference the brief throughout
setup to ensure everything aligns with what was decided.

If it does not exist, ask me these questions one at a time before proceeding:
- What frontend stack are you using?
  (Filament / Livewire + Tailwind / Inertia + Vue / Inertia + React)
- What is the main purpose of this application in plain English?
- Are there any third-party integrations or APIs this app connects to?
- Will this app need background jobs, or is everything synchronous?

---

## Step 1 — Install Latest Laravel (New Projects Only)

If this is a brand new project with no existing code, install Laravel:

```bash
composer create-project laravel/laravel app-name
```

Never use a version constraint. Always let Composer resolve the latest
stable release. After installation, read the version from composer.json
and record it in CLAUDE.md.

---

## Step 2 — Install Laravel Boost

```bash
composer show laravel/boost
```

If not installed:

```bash
composer require laravel/boost --dev
php artisan boost:install
```

---

## Step 3 — Verify Core Files Exist

Check that the following files exist in the project root:
- `CLAUDE.md`
- `.mcp.json`
- `.env.example`
- `.gitignore`
- `CHANGELOG.md`

If any are missing, create them using the templates defined in CLAUDE.md.
Report what was found and what was created.

---

## Step 4 — Verify .gitignore is Correct

Confirm `.gitignore` includes at minimum:
- `.env`
- `.mcp.json`
- `/vendor`
- `/node_modules`

If any are missing, add them immediately. A token committed to git
cannot be safely un-committed.

---

## Step 5 — Review Active MCP Servers

Show the current `.mcp.json`. Check:
- `laravel-boost` is present and configured
- GitHub token has been replaced (not REPLACE_WITH_YOUR_GITHUB_TOKEN)
- No more than 10 servers are active

If the GitHub token is still a placeholder, remind me to generate one at:
github.com/settings/tokens (needs repo + workflow scopes)

---

## Step 6 — Verify Required API Keys and Credentials

This step ensures no required credentials are forgotten before building.
A missing API key discovered mid-development causes unnecessary interruption.

### Step 6a — Read .env.example for required credentials

Read `.env.example` and identify every variable that:
- Has an empty value (e.g. `SYNCRO_API_KEY=`)
- Has a comment indicating it is required (e.g. `# Required — get from...`)
- Is under the `# Third-Party Integrations` section

List every required credential you find. For each one, show:
- The variable name
- What it is for (from the comment in .env.example)
- Where to get it (if documented in .env.example or docs/integrations.md)

### Step 6b — Check which credentials are already in .env

Compare against the current `.env` file. Identify which required variables:
- Are already set with a real value (not empty, not a placeholder)
- Are missing or empty

Do not display the values of any credentials — only confirm present or missing.

### Step 6c — Prompt for each missing credential one at a time

For each missing credential, ask me directly:

"I need the [VARIABLE_NAME] to proceed. This is your [plain English
description of what this is]. You can find it at [location if known].
Please paste it now or type 'skip' to come back to this later."

If I provide the value, write it to `.env` immediately before asking
for the next one.

If I type 'skip', note it and continue. At the end of this step, list
any skipped credentials and warn:
"These credentials are still missing. The app may not function correctly
until they are added to .env. Add them before running the app."

### Step 6d — Verify .env.example is complete

After collecting credentials, verify that every variable in `.env` also
exists in `.env.example` (with an empty value and a comment).

If any `.env` variables are missing from `.env.example`, add them now
with an empty value and a descriptive comment. The rule is:
.env.example must always be a complete map of every variable .env needs.

---

## Step 7 — Install Official Laravel Plugins

```
/plugin marketplace add laravel/agent-skills
/plugin install laravel-simplifier@laravel
/plugin install laravel-cloud@laravel
```

---

## Step 8 — Install Frontend Stack

Based on the project brief or Step 0 answers:

**If Filament:**
First check the version to install:
- Filament v4 is current as of 2025 — use unless there is a reason for v3
```bash
composer require filament/filament
php artisan filament:install --panels
```
Record the installed version in CLAUDE.md.

**If Livewire + Tailwind:**
```bash
composer require livewire/livewire
npm install -D tailwindcss
```

**If Inertia + Vue:**
```bash
composer require inertiajs/inertia-laravel
npm install @inertiajs/vue3 vue
```

**If Inertia + React:**
```bash
composer require inertiajs/inertia-laravel
npm install @inertiajs/react react react-dom
```

---

## Step 9 — Create docs/ Folder Structure

Create the following documentation files if they do not exist:

`docs/architecture.md`:
```markdown
# Architecture

Plain English documentation of how this application is structured.
Updated as major features are added.

## Overview
(populate after onboarding)

## Key Services
(document each Service class and what it does)

## External Integrations
(document each external API and how it is used)

## Deployment
- Platform: Laravel Forge
- (add server and domain details when known)
```

`docs/integrations.md`:
```markdown
# Integrations

Plain English documentation of each external service this app connects to.

## [Integration Name]
- **Purpose**: What this integration does in the app
- **Authentication**: How it authenticates
- **Service Class**: app/Services/[Name]Service.php
- **Documentation**: [URL]
- **Environment Variables**: [list of .env keys]
- **Gotchas**: Any known issues or non-obvious behavior
```

`docs/api.md` (only if this project exposes API endpoints):
```markdown
# API Documentation

## Authentication

[Describe how API consumers authenticate — token, OAuth, Sanctum, etc.]

## Base URL

Local: `http://localhost:8000/api`
Production: `https://[domain]/api`

## Endpoints

### [Resource Name]

#### GET /api/[resource]
**Description:** [What this returns]
**Authentication:** Required / Not required
**Query Parameters:**
| Parameter | Type | Required | Description |
|-----------|------|----------|-------------|
[Add parameters]

**Response:**
```json
{
  "data": [],
  "meta": {}
}
```

---

[Add one section per resource that has API endpoints]
```

---

## Step 10 — Onboard Claude to This Project

Run a full project onboarding in this order:

1. Use Laravel Boost to inspect the full database schema
2. Run `php artisan route:list`
3. Read `composer.json` — note all installed packages
4. Scan `app/` — note Models, Controllers, Services, Jobs that exist
5. Check `.env` for database driver, queue driver, cache driver,
   and any third-party API keys present (names only, not values)

After completing all five, give me a summary:
- What this app does based on what you have seen
- The data model in plain English
- How the current state compares to the project brief if one exists
- Anything that looks unusual or worth flagging

---

## Step 11 — Scaffold Factories and Seeders

For every model that exists or will be created:
1. Create a factory in `database/factories/`
2. Add a seeder in `database/seeders/`
3. Register each seeder in `DatabaseSeeder.php`

Running `php artisan db:seed` should produce enough realistic fake data
to develop against without needing production data.

---

## Step 12 — Create a Worktree for the First Feature

Ask me: "What is the first feature you would like to build?"

Once I answer:

```bash
claude --worktree feature-[name]
```

Confirm directory and branch name.

---

## Step 13 — Final Checklist

Report status of each item:

- [ ] Project brief exists at docs/project-brief.md
- [ ] Laravel installed at latest stable version
- [ ] Laravel Boost installed and MCP running
- [ ] CLAUDE.md in place and up to date
- [ ] .env.example exists and is current
- [ ] .gitignore includes .env and .mcp.json
- [ ] CHANGELOG.md exists
- [ ] docs/ folder created with architecture.md and integrations.md
- [ ] Laravel plugins installed (laravel-simplifier, laravel-cloud)
- [ ] Frontend stack installed and configured
- [ ] Filament version recorded in CLAUDE.md (if using Filament)
- [ ] Project onboarded (schema, routes, packages understood)
- [ ] Factories and seeders scaffolded
- [ ] First worktree created and ready
- [ ] Laravel Pint installed (`composer require laravel/pint --dev`)
- [ ] Valet local domain noted in README (format: [project-name].test)
- [ ] If app uses queues: queue worker documented in README local setup section
- [ ] If app uses scheduler: scheduler command documented in README

Flag anything incomplete before we start building.

**Queue and Scheduler Note:**
If this app uses queued jobs (QUEUE_CONNECTION is not sync), add this
to the README under Local Setup:

```markdown
### Running the Queue Worker

In a separate terminal window, keep this running during development:

```bash
php artisan queue:work
```
```

If this app uses scheduled commands, add:

```markdown
### Running the Scheduler Locally

In a separate terminal window:

```bash
php artisan schedule:work
```
```
