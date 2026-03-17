# Laravel Claude Code Setup Walkthrough

Run this command once when starting Claude Code on a new Laravel project.
If you ran /new-project first, the project brief and tech stack are already
decided — skip any questions already answered there.

Walk through each step one at a time. Pause after each step and wait for
confirmation before moving to the next.

---

## Step 0a — Initialize Log File (Safety Check)

Before doing anything else, check if `docs/claude-log.md` exists:

```bash
ls docs/claude-log.md 2>/dev/null
```

If it does NOT exist:
- Create the `docs/` folder if needed: `mkdir -p docs`
- Create `docs/claude-log.md` with the standard header:

```markdown
# Claude Activity Log

This file is maintained automatically by Claude Code.
It records every meaningful action taken during development
so you can always review what happened even if the IDE
session is no longer available.

Entries are appended chronologically and never deleted.

---
```

Then append:
```markdown
## [DATE TIME]

**Action:** /laravel-setup started — log created by setup
**Notes:** Log was not found — /laravel-setup may have been run
  without /new-project, or this is an existing project being
  onboarded. Log initialized now.

---
```

If it DOES exist, append:
```markdown
## [DATE TIME]

**Action:** /laravel-setup started
**Notes:** Continuing from /new-project. Log exists.

---
```

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

## Step 5 — Verify MCP Servers Are Connected and Working

This step is mandatory. Do not skip it. Do not proceed to Step 6
until all checks pass. Log every result.

### Step 5a — Verify Laravel Boost is responding

```bash
php artisan boost:mcp
```

If this fails, stop and tell the user:
"Laravel Boost is not responding. Run the following before proceeding:
composer require laravel/boost --dev
php artisan boost:install"

Do not continue until Boost is confirmed working.

### Step 5b — Verify GitHub MCP is connected

Test the GitHub MCP by fetching basic repository information.
If the connection fails or returns an error, stop immediately and
tell the user:

"The GitHub MCP is not connected. Automatic commits and pushes will
not work until this is resolved. Check the following:

1. Open .mcp.json and confirm GITHUB_PERSONAL_ACCESS_TOKEN contains
   a real token — not the placeholder text
2. Confirm the token has not expired — tokens expire silently
3. Confirm the token has repo and workflow scopes
4. Generate a new token at github.com/settings/tokens if needed

Do not proceed with the build until the GitHub MCP is confirmed
working. Automatic commits and pushes are required and cannot
be skipped."

### Step 5c — Create GitHub repository if it does not exist

Check if a remote GitHub repository exists:

```bash
git remote -v
```

If no remote exists, create one now using the GitHub MCP:
- Name matches the project folder name
- Private by default unless the user specifies otherwise
- Push the initial commit immediately after creating
- Report the repository URL to the user

If a remote already exists, confirm it is reachable and report the URL.

### Step 5d — Log MCP verification results

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** MCP server verification
**Laravel Boost:** [connected / failed]
**GitHub MCP:** [connected / failed]
**GitHub repository:** [URL or "created new"]

---
```

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

`docs/claude-log.md`:
```markdown
# Claude Activity Log

This file is maintained automatically by Claude Code.
It records every meaningful action taken during development
so you can always review what happened even if the IDE
session is no longer available.

Entries are appended chronologically and never deleted.

---

## [DATE TIME]

**Action:** Project initialized via /laravel-setup
**Setup source:** /new-project discovery brief at docs/project-brief.md
**Tech stack:** [confirmed stack from project brief]
**GitHub repository:** [URL]
**Notes:** Initial project setup complete. All checklist items confirmed.

---
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

## Step 10 — Generate Project README

This step is mandatory. A generic Laravel README is never acceptable.
Do not mark this step complete until a real project README with badges
exists and has been committed and pushed.

### Step 10a — Check for project brief

```bash
cat docs/project-brief.md
```

If docs/project-brief.md does not exist or is empty, stop and tell
the user:
"I cannot generate the project README because docs/project-brief.md
is missing or empty. This file should have been created by /new-project.
Please provide the project details and I will create the brief now."

Do not proceed until docs/project-brief.md has real content.

### Step 10b — Detect installed versions for accurate badges

Run the following to get exact installed versions:

```bash
php artisan --version
composer show laravel/framework | grep '^versions'
php --version
```

Also check for these if installed:
```bash
composer show statamic/cms | grep '^versions'
composer show filament/filament | grep '^versions'
composer show livewire/livewire | grep '^versions'
```

### Step 10c — Check if README is still the generic Laravel one

Read the current README.md. If it contains any of the following
it is the generic Laravel README and must be replaced:
- "Laravel is a web application framework"
- "Thank you for considering contributing"
- "In order to ensure that the Laravel community"

### Step 10d — Generate the README with shields.io badges

Generate a complete README.md. Every section must contain real
project information — no placeholder text of any kind.

The README must start with shields.io badges before anything else.
Generate badges for every applicable item from this list using the
shields.io format (https://shields.io):

**Always include:**
- Laravel version: ![Laravel](https://img.shields.io/badge/Laravel-[version]-FF2D20?style=flat&logo=laravel&logoColor=white)
- PHP version: ![PHP](https://img.shields.io/badge/PHP-[version]-777BB4?style=flat&logo=php&logoColor=white)
- Tailwind CSS: ![Tailwind](https://img.shields.io/badge/Tailwind_CSS-v4-38B2AC?style=flat&logo=tailwind-css&logoColor=white)
- MySQL: ![MySQL](https://img.shields.io/badge/MySQL-4479A1?style=flat&logo=mysql&logoColor=white)
- Laravel Forge: ![Forge](https://img.shields.io/badge/Deployed_on-Laravel_Forge-F4645F?style=flat)
- Pest: ![Pest](https://img.shields.io/badge/Tests-Pest-16B7FB?style=flat)
- License: ![License](https://img.shields.io/badge/License-MIT-green?style=flat)

**Include if installed:**
- Statamic: ![Statamic](https://img.shields.io/badge/Statamic-[version]-FF269E?style=flat&logo=statamic&logoColor=white)
- Filament: ![Filament](https://img.shields.io/badge/Filament-[version]-FDAE4B?style=flat)
- Livewire: ![Livewire](https://img.shields.io/badge/Livewire-[version]-4E56A6?style=flat)

Replace [version] with the actual detected version number.
Place all badges on a single line at the very top of the README
before the project title.

After the badges, the README must include:
- Project name and one-sentence tagline
- What the app does in plain English
- Full tech stack table with actual versions
- Prerequisites with actual version numbers
- Complete local setup instructions referencing Valet
- How to run tests (php artisan test)
- How to run Pint (./vendor/bin/pint)
- External integrations table if any exist
- Environment variables table from .env.example
- Page routes if applicable
- Project structure overview
- Deployment notes (Forge as target)
- Link to CHANGELOG.md
- Link to docs/project-brief.md

### Step 10e — Show and confirm

Show the generated README and ask:
"Does this README accurately represent the project?
Should I adjust anything before committing?"

Wait for confirmation before committing.

### Step 10f — Commit and log

Once confirmed, commit immediately:

```bash
./vendor/bin/pint README.md 2>/dev/null || true
git add README.md
git commit -m "docs: generate project README with badges from project brief"
git push
```

Then append to docs/claude-log.md:

```markdown
## [DATE TIME]

**Action:** Project README generated
**Badges included:** [list the badges added]
**Commit:** [commit hash] — docs: generate project README with badges
**Push:** confirmed to [repository URL]

---
```

If the push fails, stop and tell the user:
"The README was committed locally but the push failed. This likely
means the GitHub MCP connection dropped. Please check .mcp.json
and run: git push"

---

## Step 11 — Onboard Claude to This Project

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

## Step 12 — Scaffold Factories and Seeders

For every model that exists or will be created:
1. Create a factory in `database/factories/`
2. Add a seeder in `database/seeders/`
3. Register each seeder in `DatabaseSeeder.php`

Running `php artisan db:seed` should produce enough realistic fake data
to develop against without needing production data.

---

## Step 13 — Create a Worktree for the First Feature

Ask me: "What is the first feature you would like to build?"

Once I answer:

```bash
claude --worktree feature-[name]
```

Confirm directory and branch name.

---

## Step 14 — Final Checklist

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

Once all checklist items are confirmed, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /laravel-setup complete — full checklist confirmed
**Laravel version:** [version]
**Frontend stack:** [stack]
**Filament version:** [version or N/A]
**GitHub repository:** [URL]
**README generated:** yes — with badges
**Factories scaffolded:** [yes / partial / no]
**First worktree:** [name and branch]
**All checklist items:** confirmed
**Notes:** Project is ready to build.

---
```

Then commit the log:
```bash
git add docs/claude-log.md
git commit -m "chore: initialize Claude activity log"
git push
```

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
