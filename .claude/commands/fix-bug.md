# Fix Bug Workflow

Run this command when something is broken and needs to be fixed.
Diagnose before touching code. Keep fixes isolated. Verify thoroughly.

---

## Step 1 — Understand the Problem First

Before diagnosing anything, read `tasks/lessons.md` if it exists:

```bash
cat tasks/lessons.md 2>/dev/null
```

If it exists, read it and apply all lessons. Pay special attention
to any lessons about the type of bug being reported. Append a log entry:

```markdown
## [DATE TIME]

**Action:** Lessons learned reviewed before /fix-bug
**Lessons found:** [count or "none yet"]
**Relevant lessons for this bug:** [list or "none"]

---
```

Do NOT touch any code yet.

Ask me the following if not already provided:
1. What is the error or unexpected behavior?
2. Where does it happen? (page, route, action, or command)
3. When did it start? (after a specific change, always there, random?)
4. Can you reproduce it consistently or is it intermittent?

Once I answer, use Laravel Boost and the codebase to:
- Find the relevant files
- Check `storage/logs/laravel.log` for the most recent related error
- Trace the code path from where the error originates
- Form a clear hypothesis about the root cause

Tell me your diagnosis in plain English:
- What is broken
- Why it is broken
- What the fix will be
- Whether the fix could affect anything else

Wait for my confirmation before proceeding.

After I confirm the diagnosis, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /fix-bug diagnosis confirmed
**Problem:** [what is broken in one sentence]
**Root cause:** [why it is broken]
**Proposed fix:** [what will be changed]
**Risk of side effects:** [yes — description / low]
**Files to touch:** [list]

---
```

---

## Step 2 — Create an Isolated Worktree

```
claude --worktree bugfix-[short-description]
```

Confirm worktree and branch name before proceeding.

---

## Step 3 — Apply the Fix

Make only the changes needed to fix this specific problem.
Do not refactor unrelated code or improve other areas in this branch.
After making the fix, explain exactly what you changed and why.

After applying the fix, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /fix-bug fix applied
**Problem:** [what was broken in one sentence]
**Root cause:** [why it was broken]
**Fix:** [what was changed to fix it]
**Files changed:** [list files modified]
**Notes:** [anything non-obvious about the fix]

---
```

---

## Step 4 — Verify the Fix

1. Reproduce the original scenario and confirm the error no longer occurs
2. Run the full test suite:

```bash
php artisan test
```

If existing tests fail, investigate before proceeding — the fix may have
introduced a regression.

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /fix-bug tests run after fix
**Original error reproduced and confirmed fixed:** [yes / no]
**Full test suite result:** [passed / failed]
**Regressions introduced:** [yes — description / none]
**Notes:** [anything worth noting]

---
```

---

## Step 5 — Post-Fix Checks

**Code Formatting (run first):**
```bash
./vendor/bin/pint
```
Commit any formatting changes before proceeding.

**Security check:**
Did this fix touch any input handling, authentication, authorization,
or data exposure? If yes, review for security implications.

**Performance check:**
Did this fix touch any database queries? If yes, check for N+1 issues
and missing indexes on any affected columns.

**Environment check:**
Did this fix require any new .env variables? If yes, add them to
.env.example immediately with a comment explaining their purpose.

**Documentation check:**
Does docs/architecture.md need to be updated to reflect what was wrong
and how it was fixed? Update if the bug revealed a non-obvious aspect
of how the system works.

---

## Step 6 — Add or Update a Test

If a test should have caught this bug, show me why it did not and update it.
If no test covers this scenario, write one now.
A bug fixed without a test can silently return.

---

## Step 7 — Review and Merge

```bash
git diff main
```

Summarize:
- What file(s) changed
- What the fix was in one sentence
- What test covers it now
- Results of all post-fix checks

---

## Step 8 — Update Changelog and Docs

1. Add an entry to CHANGELOG.md under `[Unreleased]` → `Fixed`
2. Update docs/architecture.md if the bug revealed something worth
   documenting about how the system works

---

## Step 9 — Push and PR

Once I approve, push and open a PR:
`fix: [short description of what was broken]`

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /fix-bug complete — PR opened
**Fix:** [one sentence description]
**Regression test:** [test name or description]
**Commit:** [commit hash and message]
**PR:** [PR URL]
**Tests:** [passed / failed]

---
```
