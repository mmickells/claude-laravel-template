# Fix Bug Workflow

Run this command when something is broken and needs to be fixed.
Lighter than /new-feature but still keeps fixes isolated, diagnosed properly, and verified before merging.

---

## Step 1 — Understand the Problem First

Do NOT touch any code yet.

Ask me the following if I haven't already provided this information:
1. What is the error or unexpected behavior? (exact error message or description)
2. Where does it happen? (which page, route, action, or command)
3. When did it start? (after a specific change, always been there, random?)
4. Can you reproduce it consistently or is it intermittent?

Once I answer, use Laravel Boost and the codebase to:
- Find the relevant files (controller, model, service, job, etc.)
- Read the recent error log: `php artisan log:tail` or check `storage/logs/laravel.log`
- Trace the code path from where the error originates
- Form a clear hypothesis about what is causing the problem

Tell me your diagnosis in plain English before touching anything:
- What is broken
- Why it is broken
- What the fix will be
- Whether the fix could affect anything else

Wait for my confirmation before proceeding.

---

## Step 2 — Create an Isolated Worktree

Create a worktree for this fix:

```
claude --worktree bugfix-[short-description]
```

Confirm the worktree and branch name before proceeding.

---

## Step 3 — Apply the Fix

Make only the changes needed to fix this specific problem. Do not refactor unrelated code, rename things, or improve other areas while you are in here — that belongs in a separate branch.

After making the fix, explain in plain English exactly what you changed and why.

---

## Step 4 — Verify the Fix

1. Reproduce the original error scenario and confirm it no longer occurs
2. Run the test suite to make sure nothing else broke:

```bash
php artisan test
```

If existing tests fail, investigate before proceeding — the fix may have introduced a regression.

---

## Step 5 — Add or Update a Test

If a test exists that should have caught this bug, show me why it didn't and update it.

If no test covers this scenario, write one now that would catch this bug if it ever regressed. A bug that gets fixed without a test can silently come back later.

---

## Step 6 — Review and Merge

Show me the diff:

```bash
git diff main
```

Summarize:
- What file(s) changed
- What the fix was in one sentence
- What test covers it now

Once I approve, push the branch and open a pull request. Use a clear PR title like:
`fix: [short description of what was broken]`
