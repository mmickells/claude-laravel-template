# New Feature Workflow

Run this command at the start of every new feature.
Enforces plan-first development, worktree isolation, correct build order,
and a thorough quality review before merging.

---

## Step 1 — Check the Project Brief

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

---

## Step 4 — Create a Worktree

Once I approve the plan:

```
claude --worktree feature-[descriptive-name]
```

Confirm directory and branch before proceeding.

---

## Step 5 — Build in This Order

Work through implementation in this sequence. Do not skip ahead:

1. **Migration** — generate it, show it to me, wait for approval before
   running `php artisan migrate`. Verify all foreign keys have indexes.
2. **Model** — relationships, casts, scopes, fillable. Create factory.
3. **Seeder** — add realistic fake data for this model to DatabaseSeeder
4. **Form Request(s)** — all validation logic
5. **Service class** — all business logic. Add class-level and method-level
   PHPDoc comments to all public methods.
6. **Filament Resource / Page / Widget** — use Filament patterns appropriate
   for the installed version. Check CLAUDE.md for the recorded version.
   Never build manual controllers and views for something Filament can handle.
7. **Tests** — Feature tests for happy path and key edge cases.
   Minimum: successful creation, validation failure, authorization check.

After each file is created, briefly confirm before moving to the next.

---

## Step 6 — Pre-PR Quality Checks

Before running tests, review for:

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

---

## Step 12 — Ready to Merge

Once I approve the diff and tests are passing:
"Ready to push this branch and open a pull request?"

If yes, push and create a PR targeting main:
`feat: [short description of what was added]`
