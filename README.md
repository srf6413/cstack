# cstack: AI assistants for recurring work

Set up a personal AI assistant in minutes. It handles recurring tasks, keeps notes up to date, and only pings you when it is actually stuck. You manage it from one place through Dispatch.

If you can describe a workflow in plain English, you can run cstack.

## TL;DR for operators (30 seconds)

cstack is like hiring a team of reliable virtual assistants who work from one shared notebook in Notion.

- you describe the job once in plain English
- they run on schedule and handle one task at a time
- they keep state updated and only escalate when a human decision is needed
- you steer everything from Dispatch (the control center you can use from your phone to check status, redirect work, and launch follow-ups)

If you want architecture and tradeoffs, jump to **For technical readers (appendix)** below.

### Reading paths

- **New here?** Read through **Heartbeat and Dispatch**.
- **Evaluating safety?** Read **Safety and boundaries** and **Known limits (current design)**.
- **Technical deep dive?** Jump to **For technical readers (appendix)**.

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
- **Decision gates:** policy prompts that require approval for send/spend/delete actions; treat these as governance controls, not hard enforcement.
- **Kill switch:** disable the schedule and the loop stops.
- **Human review is by design:** for high-stakes state in Notion SSOTs and rule files, a human should review and approve critical updates rather than hiding compaction and consolidation behind an opaque runtime.

This is safer by design for most operator workflows, but you should still treat state, transcripts, and credentials as sensitive data.

---

## Known limits (current design)

- **Locking is lease-based, not atomic.** A read-check-write race is still possible under concurrent starts.
- **Heartbeat currently scans all state files each run.** This is simple, but it scales poorly as the number of assistants grows.
- **No separate server runtime does not mean no runtime dependency.** cstack still depends on your local Claude/Cowork environment being available.

---

## Where this model is not a fit

cstack is great for recurring operational work. It is not the best choice for work that needs constant, real-time thinking.

Examples people usually understand:

- trading systems
- live incident response
- active security defense
- robotic control systems

Simple rule of thumb:

- if the work looks like **read state -> decide -> write state -> wait**, cstack is a good fit
- if the work needs **continuous monitoring and instant reactions**, use a continuous agent/runtime design

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

**Dispatch** is the game-changing control gateway: one place to check status, redirect work, and trigger sessions from your phone.

In practice, this feels like having one "Jarvis" over many workers:

- heartbeat and scheduled runs do proactive work in the background
- urgency notifications route your attention
- Dispatch is where you follow up, ask for context, and issue the next action

This gives strong capability without granting unmanaged autonomy by default.

---

## For technical readers (appendix)

### Stateless architecture

cstack intentionally avoids an always-on agent process:

- scheduled sessions wake up
- read/write explicit state
- exit after each run

This limits hidden runtime drift and keeps behavior inspectable.

### Externalized cognition thesis

cstack assumes that useful cognition can be reconstructed on demand from explicit state.

Working model:

- cognition for a run = state + retrieval + reasoning
- continuity comes from SSOT quality, not a persistent internal process
- proactive behavior comes from scheduling + clear goals, not an always-on daemon

Why this works well:

- most operational work is reconstructible from goals, history, constraints, and current tasks
- hard resets reduce hidden drift and keep alignment visible
- explicit state makes audit and handoff easier across runs and assistants

Where this model breaks:

- retrieval is not the same as open-ended exploration
- writebacks are lossy compressions of reasoning (dead ends and alternatives are often dropped)
- reconstruction is framing-sensitive, so different runs may rebuild slightly different context
- discrete runs are less responsive than truly continuous processing for real-time flows

Bottom line: cstack maximizes explicit cognition for reliability and inspectability; it deliberately trades some depth and continuity for operational clarity.

### Why a small Dispatch context is a feature

Dispatch does not need to carry the whole system in one giant context window.

- it starts from a small boot context (rules + pointers)
- it retrieves current state from SSOTs when needed
- it can launch or coordinate focused sub-sessions to do deeper work

This keeps the control layer lean, reduces stale-context risk, and still gives broad operational reach from one interface.

### Human-in-the-loop is a feature, not a fallback

The SSOT in this pattern is intentionally treated as an operational approximation that must stay legible to humans.

- for high-stakes state (active blockers, commitments, approvals, financial decisions), explicit overwrite + human review is the preferred control
- for lower-stakes pattern work (clustering, style trends, weak signals), fuzzier model-driven consolidation can be acceptable

This is a deliberate design tradeoff: less automation purity, more correctness and accountability where mistakes are expensive.

### Memory is visible, not hidden

Many AI tools keep memory in opaque internals or buried settings. cstack puts memory artifacts front and center:

- Notion SSOT pages are primary operational state
- rule files are explicit and reviewable
- updates are expected to be checked regularly for correctness

The goal is not to hide memory mechanics. The goal is to make memory inspectable enough that human review can continuously catch drift and bad writebacks.

### Control plane model

- **state file** is durable operational state for each assistant (not perfect truth)
- **skill file** is behavior contract
- **schedule** is autonomy trigger
- **heartbeat/dispatch** are coordination and escalation surfaces
- **Dispatch** is the single command surface for human steering across sessions

### Parallel assistants without coordination hell

For most workloads, cstack keeps parallel execution simple:

- each assistant owns its own state file
- assistants do not share in-memory context
- each run is short-lived and resumes from file state

This reduces coupling and limits blast radius when one assistant fails.

Important caveat: this does not remove coordination entirely. Cross-assistant handoffs still need explicit contracts, and lease locks remain best-effort (non-atomic).

### Notion + MCP: where the pattern is strong, and where it bends

Using Notion through MCP is a major reason this pattern is accessible:

- `notion-search` and `notion-fetch` make context retrieval simple
- `notion-create-pages` and `notion-update-page` support readable state writes
- `notion-query-database-view` helps with pre-filtered operational views
- `notion-update-data-source` enables schema evolution without writing backend code

At larger scale, treat Notion as an operational store, not a high-concurrency transactional database:

- schema discipline is soft unless you enforce strict templates and property conventions
- write quality can degrade over time if many assistants update the same structures
- concurrency guarantees are limited for race-sensitive coordination

Practical guidance:

- up to ~10-20 assistants, this model is usually manageable with good conventions
- beyond ~100 assistants, expect quality and coordination degradation unless you add stronger orchestration and storage controls

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

## Glossary

- **Dispatch:** a feature in the Claude app that lets you check status, steer work, and launch follow-ups from one chat window.
- **SSOT:** single source of truth. In this pattern, your Notion state pages.
- **Heartbeat:** a scheduled checker that summarizes what needs human decisions.

---

*[@sauloferreira64](https://x.com/sauloferreira64)*
