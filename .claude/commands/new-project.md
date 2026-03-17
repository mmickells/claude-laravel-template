# New Project Discovery

Run this command before anything else when starting a new project.
Do NOT run /laravel-setup until this command is fully complete and the
project brief and tech stack have been approved.

Work through each phase in order. Ask questions one at a time — never
present a list of questions all at once. Wait for my answer before asking
the next. My answers may be vague — that is fine. Work with what I give
you and ask natural follow-up questions to draw out more detail.

---

## Phase 0 — Initialize Activity Log

Before asking any questions, do the following:

Create the `docs/` folder if it does not exist:
```bash
mkdir -p docs
```

Create `docs/claude-log.md` if it does not exist:
```markdown
# Claude Activity Log

This file is maintained automatically by Claude Code.
It records every meaningful action taken during development
so you can always review what happened even if the IDE
session is no longer available.

Entries are appended chronologically and never deleted.

---
```

Then append the first entry:
```markdown
## [DATE TIME]

**Action:** /new-project started
**Notes:** Beginning discovery conversation. Log created.

---
```

Also check for `tasks/lessons.md`. If it exists, read it in full
before proceeding. These are lessons learned from previous sessions
on this or other projects. Apply them throughout this session.

If `tasks/lessons.md` does not exist, create it now:
```markdown
# Lessons Learned

This file is maintained automatically by Claude Code.
After any correction from the user, Claude writes a lesson here
so the same mistake is never repeated.

Read this file at the start of every session before doing anything.
Lessons are permanent and never deleted.

---
```

Now proceed to Phase 1.

---

## Phase 1 — The Problem

Goal: Understand why this app needs to exist.

Ask me:
1. What problem are you trying to solve with this app? Describe it like
   you would to a friend — no technical terms needed.
2. What does your current workaround or process look like today?
3. What is the most painful or frustrating part of how things work now?
4. How often does this problem come up — daily, weekly, occasionally?

After receiving answers to all Phase 1 questions, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project Phase 1 complete — The Problem
**Problem identified:** [one sentence summary of the core problem]
**Current workaround:** [how they handle it today]
**Pain level:** [how often and how painful]

---
```

---

## Phase 2 — The Users

Goal: Understand who will actually use this.

Ask me:
1. Who is the primary person using this app day to day — you, your team,
   your clients, or a mix?
2. How technical are the people who will use it?
3. What devices will they use — desktop, laptop, phone, or all of them?
4. Will multiple people need to log in separately, or is this a
   single-user tool?

After receiving answers to all Phase 2 questions, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project Phase 2 complete — The Users
**Primary users:** [who uses this]
**Technical level:** [technical / non-technical / mixed]
**Devices:** [desktop / mobile / both]
**Multi-user:** [yes / no]

---
```

---

## Phase 3 — The Vision

Goal: Understand what success looks like.

Ask me:
1. If this app worked perfectly, what would a great day using it look like?
   Walk me through it.
2. What is the one thing this app absolutely must do well — the thing that
   makes it worth building?
3. Have you seen any other app — even one that does something completely
   different — that has the right feel or layout? What did you like about it?
4. Is there a version 1 in your mind — a smaller starting point — and a
   bigger vision for later?

After receiving answers to all Phase 3 questions, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project Phase 3 complete — The Vision
**Core purpose:** [the one thing this app must do well]
**Version 1 scope:** [what V1 looks like]
**Future vision:** [where this could go]
**Design reference:** [any apps mentioned as inspiration]

---
```

---

## Phase 4 — The Data

Goal: Understand what information lives in this app.

Ask me:
1. What are the main things this app needs to keep track of?
   (Examples: customers, tickets, invoices, devices, jobs, employees)
2. Where does that data come from — do you type it in, does it come from
   another system, or both?
3. Does this app need to connect to any external services or APIs?
   If yes, which ones?
4. Is there data that already exists somewhere that needs to be brought
   into this app?

After receiving answers to all Phase 4 questions, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project Phase 4 complete — The Data
**Main entities:** [what the app tracks]
**Data sources:** [manual entry / external API / both]
**Integrations:** [external services mentioned]
**Existing data:** [any data that needs importing]

---
```

---

## Phase 5 — The Boundaries

Goal: Understand what is out of scope.

Ask me:
1. Is there anything this app should NOT do — things that are out of scope
   or belong in a different tool?
2. What would make this too complicated or too big for a first version?
3. Are there any hard constraints — timeline, things that must integrate
   with existing systems, anything non-negotiable?

After receiving answers to all Phase 5 questions, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project Phase 5 complete — The Boundaries
**Out of scope:** [what this app will not do]
**Too complex for V1:** [features deferred]
**Hard constraints:** [non-negotiables]

---
```

---

## Phase 6 — Deployment and Environment

Goal: Understand where this app will live.

Note: This project owner uses Laravel Forge for deployment on a VPS.
Default to Forge-compatible recommendations. Do not suggest Laravel Cloud
or Vapor unless explicitly requested.

Ask me:
1. Is this just for local use on your machine, or does it need to be
   accessible online?
2. Will anyone other than you need access to the production version?
3. Do you have a domain name in mind or will that come later?
4. Will this app need to run tasks in the background — for example,
   syncing data from an external API on a schedule, sending emails
   asynchronously, or processing uploads without making the user wait?
   (This determines whether we need Redis queues and scheduled tasks.)

After receiving answers to all Phase 6 questions, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project Phase 6 complete — Deployment
**Hosting:** [local / Forge / other]
**Public access:** [yes / no]
**Domain:** [known / TBD]
**Queue needed:** [yes / no]

---
```

---

## Phase 7 — Generate Project Brief

Once all six phases are complete, generate a Project Brief:

---

### PROJECT BRIEF

**App Name:** (suggest one based on the conversation if not provided)

**Problem Being Solved:**
2-3 sentences describing the core problem in plain English.

**Who Uses It:**
Description of primary user(s), their technical level, and devices.

**Core Purpose:**
The one thing this app must do well.

**Key Features (Version 1):**
- Bullet list of essential features for the first version only

**Future Vision:**
Brief description of where this could go beyond version 1.

**Data and Integrations:**
What the app tracks and what external systems it connects to.

**Deployment Target:**
Where this will live and who will access it.

**Out of Scope:**
What this app explicitly will not do.

---

Present the brief and ask:
"Does this capture what you are looking for? Is there anything missing,
wrong, or that you would like to change?"

Revise until approved. Do not move to Phase 8 until I explicitly say
the brief is approved.

After brief is approved, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** Project brief approved
**App name:** [name]
**Core purpose:** [one sentence]
**Key V1 features:** [bullet list]
**Saved to:** docs/project-brief.md

---
```

---

## Phase 8 — Tech Stack Recommendation

Goal: Recommend the right tech stack based on the approved brief.
Do NOT present a multiple choice list. Make a recommendation and explain
why in plain English. Explain what the alternatives would mean and why
they are a worse fit for this specific use case.

Recommend a frontend approach based on these guidelines:

- **Filament** — for internal tools, admin dashboards, and data-heavy
  interfaces. Best when the primary users are internal (you or your team)
  and the UI is mostly tables, forms, charts, and navigation panels.

- **Livewire + Tailwind** — for custom UI where Filament's opinionated
  design system would be limiting. Best for client-facing apps, marketing
  sites, or any interface where you want full design control.

- **Inertia + Vue** — for apps that need to feel like a desktop application
  with complex interactivity and client-side routing. Higher learning curve.

- **Inertia + React** — same as Vue but for teams already familiar with React.

Also recommend:
- **Database**: MySQL unless a specific feature requires PostgreSQL
- **Queue driver**: Redis if background jobs are needed, sync if not
- **Testing**: Always recommend Pest — explain why it is preferred over
  PHPUnit for new projects
- **Caching**: Based on the app's specific needs

After presenting say:
"Does this make sense? Do you have any questions about why I recommended
any of these, or would you like to discuss any of the alternatives?"

Answer all questions in plain English. Do not assume technical knowledge.
Revise until approved.

After tech stack is approved, append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** Tech stack approved
**Frontend:** [choice and reason]
**Database:** [choice]
**Queue:** [choice]
**Testing:** Pest
**Deployment:** Laravel Forge
**Notes:** [any non-standard decisions]

---
```

---

## Phase 9 — Detect Filament Version (if Filament was recommended)

If Filament was recommended and approved, check the installed version:

```bash
composer show filament/filament | grep versions
```

Note the installed version (v3 or v4) and confirm which patterns to use
throughout the project. Filament v4 uses a different panel configuration
approach than v3. Always check before generating any Filament-specific code.

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** Filament version detected
**Version:** [v3 / v4]
**Notes:** Version recorded in CLAUDE.md

---
```

---

## Phase 10 — Generate Domain Agent

Once the project brief is approved and the tech stack is confirmed, generate
a domain agent file that gives Claude permanent business context for this
project.

Create `.claude/agents/domain-expert.md` with the following structure,
populated entirely from the approved project brief and our conversation.
Do not use placeholder text — every section must contain real information
about this specific project and business domain:

```markdown
---
name: domain-expert
description: Business domain expert for [App Name]. Activate when generating
  code that touches business logic, data models, or domain-specific terminology
  to ensure Claude understands the business context behind the code.
---

# Domain Expert — [App Name]

## What This Business Does

[Plain English description of the business or organization this app serves.
2-3 sentences. Written so a developer who knows nothing about this industry
could understand it.]

## What This App Does

[Plain English description of what this specific app does within that business.
Not technical — focused on the business problem it solves.]

## Domain Vocabulary

[List of domain-specific terms and what they mean in this context.
Example for an MSP: "Managed client — a client on a monthly contract who
receives proactive support. Break-fix client — a client billed per incident
with no ongoing contract."]

| Term | What It Means in This App |
|------|--------------------------|
[Populate from the discovery conversation — every domain-specific term
that appeared should be defined here]

## Key Business Rules

[List of business rules that affect how the app should behave.
These are rules Claude must know to generate correct code.
Example: "A ticket cannot be closed if it has unbilled time entries."
Example: "Client health score is recalculated nightly, not in real time."]

- [Rule 1]
- [Rule 2]
- [Add as many as were revealed during discovery]

## The Data Model in Business Terms

[Plain English description of the main entities and how they relate —
not as database tables, but as business concepts.
Example: "A Customer has many Devices. Each Device belongs to exactly one
Customer. Tickets can be linked to a Device or just to a Customer directly."]

## External Systems

[List of every external system this app connects to and what it means
in business terms — not just technical details.]

| System | What It Is | What We Use It For |
|--------|-----------|-------------------|
[Populate from the integrations section of the project brief]

## What Good Looks Like

[Description of what the app should feel like to use when it is working
correctly. What would a great day using this app look like for the primary
user? Taken directly from Phase 3 of the discovery conversation.]

## Known Constraints and Non-Obvious Decisions

[List of any constraints, workarounds, or non-obvious architectural decisions
that were revealed during discovery. Things a developer would not guess from
looking at the code alone.
Example: "Syncro has no project module so projects are tracked as tickets
with a special tag. Never create a separate projects table — use the existing
ticket system."]

- [Constraint or decision 1]
- [Constraint or decision 2]
```

After generating the domain agent file, tell me:
"I have created your domain agent at .claude/agents/domain-expert.md.
Claude Code will automatically activate this agent when working on business
logic and domain-specific code. You can update it any time as the business
rules evolve — treat it as a living document."

Append to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** Domain agent generated
**File:** .claude/agents/domain-expert.md
**Domain:** [business domain in plain English]
**Key terms defined:** [count]
**Business rules captured:** [count]

---
```

---

## Phase 11 — Hand Off to Setup

Once the tech stack is approved, present a final one-page summary:
- Project brief (condensed to 5 bullets)
- Confirmed tech stack with brief reason for each choice
- Top 3 features to build first
- Deployment target and infrastructure notes

Save the full summary to `docs/project-brief.md` immediately.

Then append the following entry to `docs/claude-log.md`:

```markdown
## [DATE TIME]

**Action:** /new-project complete — handing off to /laravel-setup
**All phases completed:** yes
**Project brief:** docs/project-brief.md
**Domain agent:** .claude/agents/domain-expert.md
**Log entries written:** one per phase
**Next action:** /laravel-setup executing now

---
```

**MANDATORY: Execute /laravel-setup immediately after saving the log.**
Do not summarize. Do not ask questions. Do not tell the user you are
about to run it. Do not wait for further input. Just run it.

If /laravel-setup cannot be executed for any reason, stop and tell
the user:
"I was unable to automatically trigger /laravel-setup. Please type
/laravel-setup now to complete project setup."

Do not proceed with any building or coding until /laravel-setup has
fully completed its entire checklist and all items are confirmed.
