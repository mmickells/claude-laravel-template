# New Feature Workflow

Run this command at the start of every new feature.
Enforces plan-first development, worktree isolation, correct build order,
and a thorough quality review before merging.

---

## Step 1 — Check the Project Brief

Before checking the project brief, read `tasks/lessons.md` if it exists:

```bash
cat tasks/lessons.md 2>/dev/null
```

If it exists, read it and apply all lessons to this session. Append a log entry:

```markdown
## [DATE TIME]

**Action:** Lessons learned reviewed before /new-feature
**Lessons found:** [count or "none yet"]
**Relevant lessons for this feature:** [list or "none"]

---
```

Check if `docs/project-brief.md` exists and read it:

```bash
cat docs/project-brief.md
```

The feature being built must align with the brief. If the requested feature
seems outside the brief, flag it and ask me to confirm before proceeding.

Also check the installed Filament version if Filament is the frontend:

```bash
composer show filament/filament | grep versions
```

---

## Step 2 — Enter Plan Mode (No Code Yet)

Do NOT write any code or generate any files yet.

Analyze the existing codebase to understand:
- What models, resources, services, and widgets already exist
  that are relevant to this feature
- What database tables will be involved or need to be created
- What routes or Filament resources already exist that might overlap
- What tests already exist in this area

Ask me these clarifying questions one at a time:
1. What is the expected input and output for this feature?
2. Who can access this — is there an authorization rule to consider?
3. Should any part of this run asynchronously (queued job)?
4. Are there any edge cases or business rules I should know upfront?
5. Does this touch any existing functionality that could break?
6. Does this feature require any new .env variables or external API calls?

Wait for all answers before moving to Step 3.

---

## Step 3 — Propose an Implementation Plan

Based on codebase analysis and my answers, propose a plan covering:

- Files to create with their purpose
- Files to modify and what changes
- Migrations needed — plain English description only, no files yet
- Filament components needed (Resource / Page / Widget / Action)
- Service classes needed
- Tests to write
- Any N+1 risks or security considerations to address

Present as a numbered list. Be specific.
Wait for my explicit approval before writing any code.

After I explicitly approve the plan, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-feature plan approved
**Feature:** [feature name]
**Files to create:** [list]
**Files to modify:** [list]
**Migrations needed:** [yes — description / no]
**Tests to write:** [list]
**N+1 risks identified:** [yes — description / none]
**Security considerations:** [yes — description / none]

---
```

---

## Step 4 — Create a Worktree

Once I approve the plan:

```
claude --worktree feature-[descriptive-name]
```

Confirm directory and branch before proceeding.

After creating the worktree, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-feature worktree created
**Feature:** [feature name from the plan]
**Worktree:** [worktree directory]
**Branch:** [branch name]
**Plan approved:** yes
**Notes:** [summary of the approved plan in 2-3 sentences]

---
```

---

## Step 5 — Detect Stack and Build in the Correct Order

Before building anything, read `CLAUDE.md` and identify the frontend stack
for this project. The build order depends on what is installed.

```bash
grep -i "frontend" CLAUDE.md
```

Then follow the appropriate build order below. Do not mix patterns —
if the project uses Filament, use the Filament order. If it uses Blade
or Livewire, use that order. If you are unsure, ask before proceeding.

---

### If Frontend is Filament

Work through this sequence. Do not skip ahead:

1. **Migration** — generate it, show it to me, wait for approval before
   running `php artisan migrate`. Verify all foreign keys have indexes.
2. **Model** — relationships, casts, scopes, fillable. Create factory.
3. **Seeder** — add realistic fake data for this model to DatabaseSeeder
4. **Form Request(s)** — all validation logic
5. **Service class** — all business logic. Add class-level and method-level
   PHPDoc to all public methods.
6. **Filament Resource** — for CRUD interfaces. Use the correct patterns
   for the installed Filament version (check CLAUDE.md for recorded version).
   Generate with: `php artisan make:filament-resource [Name] --generate`
7. **Filament Page** — for custom non-CRUD screens if needed
8. **Filament Widget** — for stats panels, charts, or summary data if needed
9. **Tests** — Feature tests covering happy path, validation failure,
   and authorization. Minimum one test per Resource.

Never build a manual controller and Blade view for something Filament
can handle natively. Always check if Filament has a built-in component
before building custom.

---

### If Frontend is Livewire + Tailwind

Work through this sequence. Do not skip ahead:

1. **Migration** — generate it, show it to me, wait for approval before
   running `php artisan migrate`. Verify all foreign keys have indexes.
2. **Model** — relationships, casts, scopes, fillable. Create factory.
3. **Seeder** — add realistic fake data for this model to DatabaseSeeder
4. **Form Request(s)** — all validation logic
5. **Service class** — all business logic. Add class-level and method-level
   PHPDoc to all public methods.
6. **Controller** — thin, delegates to service, returns view or response.
   Use route model binding wherever possible.
7. **Livewire Component** — for any interactive UI elements (forms, tables,
   filters, real-time updates). Generate with:
   `php artisan make:livewire [ComponentName]`
8. **Blade View** — layout and static structure. Use Tailwind for all styling.
   Never write custom CSS unless Tailwind cannot achieve the result.
9. **Routes** — add to appropriate routes file (web.php or api.php)
10. **Tests** — Feature tests covering happy path, validation failure,
    and authorization.

Use Livewire for interactivity. Use plain Blade for static content.
Never use JavaScript frameworks — Livewire handles reactivity.

---

### If Frontend is Blade + Tailwind (no Livewire)

Work through this sequence. Do not skip ahead:

1. **Migration** — generate it, show it to me, wait for approval before
   running `php artisan migrate`. Verify all foreign keys have indexes.
2. **Model** — relationships, casts, scopes, fillable. Create factory.
3. **Seeder** — add realistic fake data for this model to DatabaseSeeder
4. **Form Request(s)** — all validation logic
5. **Service class** — all business logic. Add class-level and method-level
   PHPDoc to all public methods.
6. **Controller** — thin, delegates to service, returns view or response.
   Use resource controllers where appropriate:
   `php artisan make:controller [Name]Controller --resource`
7. **Blade View** — use Tailwind for all styling. Build layouts using
   Blade components and slots. Never write custom CSS unless Tailwind
   cannot achieve the result.
8. **Routes** — add to appropriate routes file
9. **Tests** — Feature tests covering happy path, validation failure,
   and authorization.

Keep controllers thin. If logic is growing in a controller, move it to
a Service class immediately.

---

### If Frontend is Inertia + Vue

Work through this sequence. Do not skip ahead:

1. **Migration** — generate it, show it to me, wait for approval before
   running `php artisan migrate`. Verify all foreign keys have indexes.
2. **Model** — relationships, casts, scopes, fillable. Create factory.
3. **Seeder** — add realistic fake data for this model to DatabaseSeeder
4. **Form Request(s)** — all validation logic
5. **Service class** — all business logic. Add PHPDoc to all public methods.
6. **Controller** — thin, uses `Inertia::render()` to return Vue components
   with props. Never return raw JSON from a controller unless building
   a separate API endpoint.
7. **Vue Component** — in `resources/js/Pages/`. Use Composition API.
   Use Tailwind for all styling.
8. **Shared Vue Components** — reusable UI pieces go in
   `resources/js/Components/`
9. **Routes** — add to web.php. Inertia handles client-side routing.
10. **Tests** — Feature tests covering happy path, validation failure,
    and authorization.

---

### If Frontend is Inertia + React

Same order as Inertia + Vue above, but:
- Components go in `resources/js/Pages/` as `.tsx` files
- Use functional components with hooks
- Shared components go in `resources/js/Components/`
- Use TypeScript if it is already configured in the project

---

### If Stack is Mixed or Unclear

If CLAUDE.md shows a mixed stack (for example, Filament for admin plus
Livewire for the public-facing side), ask me which pattern to use for
this specific feature before proceeding. A single project can use multiple
patterns in different areas — but each feature should follow one pattern
consistently.

If the stack is not recorded in CLAUDE.md, stop and ask:
"I cannot find the frontend stack in CLAUDE.md. What frontend stack
is this project using? I will update CLAUDE.md before proceeding."

---

After each file is created, briefly confirm before moving to the next.

---

## Step 6 — Pre-PR Quality Checks

Before running tests, review for:

**Code Formatting (run first):**
Run Laravel Pint to enforce consistent code style:
```bash
./vendor/bin/pint
```
If Pint makes any changes, commit them before proceeding to the other
checks. Never open a PR with unformatted code.

**N+1 Queries:**
- Are all relationships eager loaded with `with()`?
- Are there any loops that trigger database queries inside them?
- Are new columns used in WHERE or ORDER BY indexed?

**Security:**
- Is all input validated through a Form Request?
- Are authorization checks in place via a Policy?
- Are any new API endpoints rate limited?
- Are any new .env variables added to .env.example with a comment?

**Documentation:**
- Does every new Service class have a class-level PHPDoc comment?
- Does every public Service method have a PHPDoc comment?
- Are there inline comments on any complex business logic?
- Does docs/architecture.md need to be updated for this feature?
- Does docs/integrations.md need to be updated if a new API was added?

Report all findings before proceeding to tests.

---

## Step 7 — Run Tests

```bash
php artisan test
```

If tests fail, fix them before proceeding. Show output either way.

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-feature tests run
**Result:** [passed / failed]
**Tests written:** [count]
**Test files:** [list]
**Failures:** [list any failures or "none"]
**Notes:** [anything worth noting about test coverage]

---
```

---

## Step 8 — Brief vs Implementation Check

Compare what was built against docs/project-brief.md:
- Does this feature align with the stated purpose?
- Does it serve the right users in the right way?
- Does it match the data model described in the brief?

Flag any drift from the brief and ask me how to proceed.

---

## Step 9 — Review the Diff

```bash
git diff main
```

List all changed files with a one-line description of each.

---

## Step 10 — Laravel Simplifier

```
Review recent changes using the laravel-simplifier agent
```

Apply suggested improvements and show what changed.

---

## Step 11 — Update Documentation

1. Add an entry to CHANGELOG.md under `[Unreleased]` → `Added`
2. Update docs/architecture.md with a plain English description of
   how this feature works if it is a significant addition
3. Update docs/integrations.md if a new external API was added
4. If this feature adds or changes any API endpoints, update `docs/api.md`
   immediately. Include the route, method, authentication, parameters,
   and an example response. An undocumented API endpoint must never be
   merged.

---

## Step 12 — Ready to Merge

Once I approve the diff and tests are passing:
"Ready to push this branch and open a pull request?"

If yes, push and create a PR targeting main:
`feat: [short description of what was added]`

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-feature complete — PR opened
**Feature:** [feature name]
**Files changed:** [list all files created or modified]
**Commit:** [final commit hash and message]
**PR:** [PR URL]
**Tests:** [passed / failed]
**Notes:** [anything worth noting about the implementation]

---
```
