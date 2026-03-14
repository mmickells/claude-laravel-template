# New Feature Workflow

Use this command when starting any new feature. It ensures proper isolation, planning, and context before writing any code.

---

## Step 1 — Plan First (Read-Only Analysis)

Enter Plan Mode. Do NOT write any code yet.

Analyze the codebase to understand:
- What existing models, controllers, and services are relevant to this feature
- What database tables will be affected
- What routes already exist that might overlap
- What tests already exist in this area

Ask me any clarifying questions before proposing an approach. Specifically ask:
- What is the expected input/output?
- Are there any edge cases I should know about?
- Does this need to be queued or synchronous?
- Are there authorization rules to consider?

---

## Step 2 — Propose an Approach

Present a brief implementation plan:
- Files to create
- Files to modify
- Migrations needed (describe schema changes — do NOT generate yet)
- Tests to write

Wait for my approval before proceeding.

---

## Step 3 — Create a Worktree

Create an isolated worktree for this feature:

```
claude --worktree feature-[descriptive-name]
```

All work happens in this worktree. The main branch stays clean.

---

## Step 4 — Build in This Order

1. Migration (review with me before running `php artisan migrate`)
2. Model + relationships + casts
3. Form Request(s) for validation
4. Service class for business logic
5. Controller (thin — delegates to service)
6. Routes
7. Tests

---

## Step 5 — Review Before Merging

When implementation is complete:

```bash
git diff main
```

Show me a summary of all changes. Run tests:

```bash
php artisan test
```

If tests pass and I approve, offer to push and create a PR.

---

## Step 6 — Run Laravel Simplifier

After I approve the implementation, invoke the `laravel-simplifier` agent to review the code for clarity, consistency, and maintainability:

```
> Review recent changes using the laravel-simplifier agent
```

Apply any suggested improvements and show me the diff.
