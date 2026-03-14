# Laravel Claude Code Setup Walkthrough

You are helping me set up and configure Claude Code for Laravel development. Walk me through the following steps one at a time, pausing after each to confirm before proceeding. Do not rush ahead.

---

## Step 1 — Install Laravel Boost (MCP Server)

Check if Laravel Boost is already installed:

```bash
composer show laravel/boost
```

If not installed, install it:

```bash
composer require laravel/boost --dev
php artisan boost:install
```

After install, verify a `.mcp.json` file was created or updated in the project root. Show me its contents.

---

## Step 2 — Verify CLAUDE.md Exists

Check if `CLAUDE.md` exists in the project root:

```bash
ls -la CLAUDE.md
```

If it doesn't exist, tell me — I have a template to add. If it does exist, show me its contents so I can review it.

---

## Step 3 — Check .mcp.json Configuration

Show me the current `.mcp.json` contents. Verify:
- `laravel-boost` is configured
- GitHub MCP token placeholder exists (if using GitHub)
- No more than 10 servers are listed

Recommend disabling any servers not relevant to our current session.

---

## Step 4 — Install Official Laravel Plugins

Check if the official Laravel agents/skills are installed:

```bash
/plugin marketplace list | grep laravel
```

If not installed, install them:

```
/plugin marketplace add laravel/agent-skills
/plugin install laravel-simplifier@laravel
/plugin install laravel-cloud@laravel
```

---

## Step 5 — Onboard Claude to the Project

Now run a project onboarding sequence. Do the following in order:

1. Use Laravel Boost to inspect the database schema — understand all tables, relationships, and data model
2. List all registered routes (`php artisan route:list`)
3. Check `composer.json` for installed packages
4. Review the directory structure focusing on `app/`, `database/`, and `routes/`
5. Check the `.env` for database driver, queue driver, and cache driver

After completing these, give me a 5-bullet summary of what you learned about this project.

---

## Step 6 — Set Up a Worktree for the First Feature

Ask me: "What's the first feature you'd like to build?"

Once I answer, create a worktree for it:

```bash
claude --worktree feature-[name]
```

Confirm the worktree was created and which branch we're on.

---

## Step 7 — Confirm Everything is Ready

Run a final checklist:

- [ ] Laravel Boost MCP is running and responding
- [ ] CLAUDE.md is in place with project context
- [ ] .mcp.json is configured with relevant servers only
- [ ] Laravel plugins are installed
- [ ] Project has been onboarded (schema, routes, packages understood)
- [ ] Worktree is ready for first feature

Report any items that are missing or need attention.

---

**You're ready to build. What would you like to work on first?**
