# cstack: AI assistants for recurring work

Set up a personal AI assistant in minutes. It handles recurring tasks, keeps notes up to date, and only pings you when it is actually stuck.

If you can describe a workflow in plain English, you can run cstack.

---

## Who this is for

This is for non-technical operators:

- freelancers juggling client follow-ups
- founders handling ops and outreach
- small teams managing recurring admin work

No servers. No coding. No local runtime to maintain.

---

## What problem this solves

Most people do not need a fully autonomous coding agent with system-level access. They need reliable help with repeating work:

- follow-ups that fall through the cracks
- too much context switching and mental overhead
- repetitive status updates and coordination tasks

cstack turns these into repeatable loops with clear boundaries.

---

## First-day payoff

In your first run, you should get:

- one working assistant for one domain (for example, client follow-up)
- a clean state page that remembers progress between runs
- a summary of what changed and what needs your decision

You should feel, "This saved me time today."

---

## How it works (simple)

Think of each assistant as a person with:

- a **notebook** (state file) for memory
- a **job description** (skill file) for behavior
- a **shift schedule** (scheduled session) for when to work

That is it.

---

## Try it in 5 minutes

**You'll need a free [Notion account](https://www.notion.so)** for state. Everything else runs inside Claude.

Paste this into [Claude Cowork](https://claude.ai), answer six questions, and it will generate your setup:

```
You are helping me create a new AI assistant for one recurring part of my work or life.
Ask me these questions one at a time:

1. What domain is this for? (examples: client follow-ups, content planning, hiring pipeline, appointment management)
2. What should this assistant track? List categories.
3. What can it do without asking me?
4. What should it never do?
5. How should it reach me when it needs a decision?
6. How often should it run?

After I answer, create these 3 assets:

1) State file (Notion page):
   - Session Lock:
     - Status: IDLE/IN_PROGRESS
     - Lock Owner: unique run ID
     - Lock Expires At: timestamp
     - Last Heartbeat: timestamp
   - Agent Status section:
     - Last Run
     - Summary
     - Needs Human
   - One section per category with:
     - Status
     - Current Blocker
     - Next Action
     - Running Log

2) Skill file (markdown rules):
   - Session start: read state + check lock lease
   - Lock rule: if lock is active and not expired, exit; if expired, safely take over
   - Each tick: do highest-priority work, update state, refresh heartbeat
   - Session end: release lock only if lock owner is this run
   - Hard constraints: enforce the "never do" list

3) Scheduled task prompt:
   - short prompt to run on a schedule in Cowork

Use plain language and avoid jargon.
```

---

## Instant wins: 3 starter templates

Start with one template before building anything custom:

1. **Client Follow-up Assistant**
   - tracks open conversations
   - drafts follow-ups
   - flags only contacts that need your approval

2. **Content Pipeline Assistant**
   - tracks ideas, drafts, and publish steps
   - creates weekly draft queues
   - flags blockers for your decision

3. **Weekly Ops Assistant**
   - reviews recurring tasks every week
   - updates status and next actions
   - sends one decision summary instead of many pings

---

## Safety and boundaries

cstack is designed to reduce risk for normal business workflows:

- **No persistent runtime:** each run starts clean, does work, writes state, exits.
- **Lease lock:** lock owner + expiry + heartbeat reduce overlapping writes.
- **Decision gates:** define actions that always require approval (send/spend/delete).
- **Kill switch:** disable the schedule and the loop stops.

This is safer by design for most operator workflows, but you should still treat state, transcripts, and credentials as sensitive data.

---

## Known limits (current design)

- **Locking is lease-based, not atomic.** A read-check-write race is still possible under concurrent starts.
- **Heartbeat currently scans all state files each run.** This is simple, but it scales poorly as the number of assistants grows.
- **No separate server runtime does not mean no runtime dependency.** cstack still depends on your local Claude/Cowork environment being available.

---

## Heartbeat and Dispatch

**Heartbeat** checks all assistants and sends one summary of items that need human decisions.

```text
You are the heartbeat assistant. Your only job is to check all state files.

Every run:
1. Read each state file
2. Find items marked "Needs Human"
3. Send one summary
4. If nothing needs human input, send nothing
5. Never modify other assistants' state files
```

**Dispatch** is your remote control gateway: check status, redirect work, and trigger sessions from your phone without changing system internals.

---

## For technical readers (appendix)

### Stateless architecture

cstack intentionally avoids an always-on agent process:

- scheduled sessions wake up
- read/write explicit state
- exit after each run

This limits hidden runtime drift and keeps behavior inspectable.

### Control plane model

- **state file** is durable truth for each assistant
- **skill file** is behavior contract
- **schedule** is autonomy trigger
- **heartbeat/dispatch** are coordination and escalation surfaces

### Why this differs from always-on frameworks

- no long-lived daemon required for basic workflows
- no local process with constant tool access
- simpler failure recovery (next run resumes from state)
- easier auditability (human-readable state and rules)

### Example state and skill

```markdown
# DJ Gig Booking — State

## Session Lock
**Status:** IDLE
**Lock Owner:** none
**Lock Expires At:** none
**Last Heartbeat:** none

## Agent Status
**Last Run:** 2026-03-28 9:00am
**Summary:** Researched 4 new venues in SF. Drafted intro for The Independent.
**Needs Human:** Yes — approve outreach draft for The Independent
```

```markdown
# DJ Gig Booking Agent

## Session Start
1. Generate run ID.
2. Read lock.
3. If lock is active and unexpired, EXIT.
4. Else claim lock with owner + expiry + heartbeat.

## Session End
1. Re-read lock.
2. Release only if lock owner matches this run.
```

---

*[@sauloferreira64](https://x.com/sauloferreira64)*
