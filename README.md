# Autonomous agents. Just Claude.

---

## What You're Getting

A team of autonomous agents — one per area of your life or work — each running on its own schedule, keeping its own memory, doing real work while you're not watching.

Not a chatbot. Not a one-shot task runner. Agents that wake up every morning (or every hour, or once a week), pick up where they left off, and keep moving.

Each agent handles an open-ended domain — not just a fixed checklist, but ongoing work that evolves: a pipeline that keeps growing, research that keeps accumulating, relationships that need nurturing, content that keeps shipping. The agent figures out what matters next. You only get pulled in when it actually matters.

**The perfect amount of human in the loop — by design.**

Urgent blocker? The agent pings you immediately — Discord, email, wherever you want. Can't proceed without you, stops everything and waits. Already told you about something? It won't keep pinging you about it. All green? You hear nothing. You unblock what needs you, walk away, and everything else keeps moving.

**Dispatch is the control plane.** Claude's Dispatch feature is the one place to manage all your agents from. Start a session immediately without waiting for the schedule. Check what any agent is doing right now. Change a schedule, update a rule, tell an agent to pivot. You never have to open a config file or restart anything. Just leave Cowork running on your computer — you can leave the house, and your agents keep working. Dispatch is how you reach them.

---

## Try It Right Now

Paste this into a Claude Cowork session or claude.ai. Answer six questions. You'll have a running agent in under 5 minutes.

**You'll need a free Notion account** — that's where your agent stores its memory. If you don't have one, [create it here](https://www.notion.so) before you start. Everything else runs inside Claude.

```
You are helping me create a new autonomous agent for a domain in my life or work.
Ask me these questions one at a time:

1. What domain is this for? (e.g., DJ gig booking, content calendar, project tracking, hiring, client follow-ups)
2. What should this agent track? Give me the categories. (e.g., venues I've contacted, warm leads, cold prospects, content ideas)
3. What can this agent do without asking me? (e.g., research new prospects, draft messages, update statuses)
4. What should this agent NEVER do? (e.g., send messages, spend money, delete anything)
5. How should it reach me when it's stuck? (Discord, text, email, or just write it down and wait)
6. How often should it run? (every hour, every morning, twice a day)

After I answer, create these three files:

**1. State file** — a Notion page with this structure:
   - A Session Lock at the top (Status: IDLE/IN_PROGRESS, Last Agent, Timestamp).
     This prevents two sessions from writing at the same time.
   - An Agent Status section (what the agent did last time, what it needs from you)
   - One section per category I gave you in question 2. Each section gets:
     - Status (Active / Waiting / Done)
     - Current Blocker (what's stopping progress)
     - Next Action (what the agent will do next time it wakes up)
     - Running Log (dated entries — what happened and when)

**2. Skill file** — a markdown file with these rules:
   - Session start: read the state file, check the lock, load current state
   - Each tick: find the highest-priority item, do autonomous work, update the state file, notify me if stuck
   - Session end: write all changes to the state file, release the lock
   - Hard constraints: whatever I said the agent should never do

**3. Scheduled task prompt** — a short prompt I can paste into Cowork to run this agent on a schedule.

Present all three for me to review. Keep the language plain — no jargon.
```

That's it. You get a state file, a skill file, and a schedule. Your agent wakes up, reads its memory, does work, writes back what happened, and goes to sleep. Repeat forever.

---

## What This Actually Is

Imagine you hired someone for every area of your life. Each person has a notebook — that's their memory. A job description — those are their rules. A shift they show up for. And one instruction: "Text me only if something is actually stuck. Everything else, handle it."

That's cstack. The notebook is a Notion page. The job description is a text file. The shift is a Cowork schedule. The text message is a Discord ping or email.

One person handles outreach. One handles research. One manages your inbox. One tracks client deliverables. They don't interfere with each other — each has their own notebook, their own rules, their own hours. You just built a digital department.

No servers. No code. No API keys. No terminal.

---

## The Heartbeat (Your Daily Digest)

One extra agent reads everyone else's notebooks every 30 minutes. Here's how it decides when to bother you:

**New blocker** — something is stuck that wasn't stuck last time. You get pinged.

**Already reported** — it won't keep notifying you about the same thing.

**Everything green** — nothing sent at all.

The result: you only hear about something once until it changes. When you unblock it, it goes quiet.

When something does need you, the message is plain:

> "Gig outreach is stuck on a venue that hasn't replied. Content agent finished 3 drafts. Client follow-up needs you to approve a message."

Five minutes to unblock everything. Then walk away.

Paste this to set it up:

```
You are the heartbeat agent. Your only job is to check on all my other agents.

Here are the state files you monitor:
[list your state file locations here]

Every time you run:
1. Read each state file
2. Look at the Agent Status section — find anything that says "Needs Human"
3. Send me one summary of what needs me. If nothing needs me, say nothing.
4. Never modify another agent's state file. Read only.

Schedule: every 30 minutes.
```

Add new state files as you create new agents. Done.

---

## Dispatch: The Control Plane

Scheduled agents are set-and-forget — but sometimes you want to reach in. That's what Dispatch is for.

Dispatch is Claude's built-in way to start, inspect, and adjust any running session without touching config files or restarting anything. Think of it the way OpenClaw users think of their gateway — the one place everything goes through.

With Dispatch you can:
- **Fire a session immediately** — don't wait for the scheduled run, start it now
- **Check what any agent is doing** — read its last run, see what it flagged
- **Change a rule on the fly** — tell an agent to pause outreach, switch priority, or handle a new situation
- **Adjust a schedule** — switch from daily to hourly while something's moving fast, then back

You don't have to be at your computer for any of this. Leave Cowork running, walk out the door, and your agents keep working their schedules. When one needs you, it pings you. When you want to check in or redirect one, open Dispatch from your phone. That's the whole system.

---

## What the Agent Actually Produces

Tell the setup prompt: *"DJ gig booking agent, tracks venues I've contacted and new spots to reach out to, can research venues and draft messages but never send them, ping me on Discord when something needs me, run every morning."*

Here's what comes out:

**State file (the agent's memory)**

```markdown
# DJ Gig Booking — State

## Session Lock
**Status:** IDLE
**Last Agent:** —
**Timestamp:** —

## Agent Status
**Last Run:** 2026-03-28 9:00am
**Summary:** Researched 4 new venues in SF. Drafted intro for The Independent.
Waiting on human approval to send. No reply yet from Milk Bar (5 days).
**Needs Human:** Yes — approve outreach draft for The Independent

## Venues — Active Outreach

### The Independent
**Status:** Draft Ready
**Current Blocker:** Waiting for human to approve outreach message
**Next Action:** Send once approved
**Running Log:**
- [2026-03-28] Agent found booker contact via venue website. Drafted intro message
  referencing their hip-hop Thursday nights. Flagged for approval.

### Milk Bar
**Status:** Waiting
**Current Blocker:** No reply after first message (sent 2026-03-23)
**Next Action:** Draft a follow-up if no reply by day 7
**Running Log:**
- [2026-03-23] First outreach sent (approved by human). Mentioned open format Fridays.
- [2026-03-28] No reply. Agent will draft follow-up tomorrow.

## Venues — Research Pipeline
**Status:** Active
**Next Action:** Research 5 more venues in Oakland/Berkeley with hip-hop nights
**Running Log:**
- [2026-03-28] Found 4 SF venues matching criteria.
- [2026-03-27] Researched 3 venues. 1 was permanently closed, 2 added to pipeline.
```

**Skill file (the agent's rules)**

```markdown
# DJ Gig Booking Agent

You are the DJ gig booking agent. Keep the pipeline moving.

## Session Start
1. Read the state file. Check the Session Lock.
   - If another session is IN_PROGRESS and the timestamp is less than 20 minutes old, EXIT.
   - Otherwise, set lock to IN_PROGRESS with your session name and current time.
2. Read the Agent Status section to remember what happened last time.

## Each Tick
1. Active outreach first — check for replies, flag anything that needs human action.
2. Follow-ups — if a venue hasn't replied in 7 days, draft a follow-up.
3. Research — if outreach queue is low, find new venues.
4. Update the state file with what you did.
5. Update Agent Status: plain-English summary + whether you need the human.

## Session End
1. Write all changes to state file.
2. Release lock (set back to IDLE, clear timestamp).

## Never
- Send a message without human approval
- Book or confirm a gig without human sign-off
- Delete a venue from the pipeline
- Make up venue details or contact info
```

**Scheduled task prompt**

```
You are the DJ gig booking agent.
Read the skill file and follow its protocol.
Research venues, draft outreach, track follow-ups.
Never send anything without my approval.
Update the state file when done.
Schedule: every morning, 9am.
```

It wakes up every morning. Checks follow-ups. Researches new venues. Drafts messages. Writes everything down. You review and approve. That's the whole loop.

---

## How It Works (The Technical Bit)

Each agent has three pieces:

**1. A state file** — a Notion page. It reads this every session to remember what it was doing. It writes back at the end. That's the memory. No database, no vector store, no embeddings. Notion is free, works in any browser, and Claude can read and write to it natively — no integration setup required.

**2. A skill file** — a text file with rules. "Read your state first. Work on the highest-priority item. Write what you did. Reach out only if something is actually stuck." The agent reads this at the start of every session. That's the identity.

**3. A schedule** — a Cowork scheduled session. Every hour, every morning, once a week. The session fires, the agent reads its state, does work, writes back, sleeps. No server running between sessions. No process to keep alive.

**The session lock** is the one piece of real engineering. If two sessions fire at the same time (it happens), they'd both try to write the state file simultaneously and corrupt it. So the agent checks a lock at the top of the file: if another session is IN_PROGRESS and the timestamp is under 20 minutes old, it exits immediately. Otherwise it sets the lock, does its work, and releases it. That's it — distributed coordination in five lines of markdown.

**Domain isolation is by design.** Agents don't share state files, so they can't interfere with each other. If you want context to flow between them (e.g., your research agent feeds leads to your outreach agent), you define that explicitly in the skill file. Isolation is the default. Cross-domain context is opt-in.

**Human-in-the-loop is the architecture, not a limitation.** The human is the quality layer, the approval gate, the final judgment on anything irreversible. The agent handles the volume. You handle the decisions. That's not a missing feature — that's the design.

---

## Why Not OpenClaw?

| | OpenClaw | cstack |
|---|---|---|
| Computer control | Full desktop (unsafe) | Sandboxed with Cowork's safeguards |
| Browser use | Build flows manually with Playwright | Claude Chrome connector — plain English, safety precautions built in |
| Infrastructure | Custom framework | None |
| Setup | Hours | Minutes |
| Technical skill | Developer | Anyone |
| Safety | You're on your own | Built in |
| Memory | A bunch of markdown files | A bunch of markdown files |

OpenClaw proved everyone wants autonomous agents. It also proved that handing an AI agent unrestricted access to your computer is a security disaster — 12% of its plugin registry contained malicious code, multiple CVEs in the 8+ CVSS range, Kaspersky and Cisco both published "don't use this" advisories.

cstack is the same idea with the unsafe parts removed. Cowork's sandboxed shell means the agent can't touch files it isn't supposed to. You approve before anything irreversible happens. And you don't need a developer to set it up.

---

## The Real Shift

Until now, building an AI agent was an engineering problem — API keys, servers, Python scripts, debugging. This flips it into a design problem.

99% of knowledge workers — marketers, writers, managers, freelancers — will never open a terminal. If the solution requires typing `npm install`, you've already lost 95% of the people who actually need this. cstack runs entirely inside tools you already have: a browser and Notion.

If you can write a job description in plain English, you can build an autonomous agent. That's not a hack. That's the actual future — and it belongs to everyone, not just developers.

---

*cstack is an open pattern, not a product*

*[@sauloferreira64](https://x.com/sauloferreira64)*
