# Autonomous agents. Just Claude.

Imagine you hired someone for every area of your life. Each person has a notebook — that's their memory. A job description — those are their rules. A shift they show up for. And one instruction: *"Reach out only if something is actually stuck. Everything else, handle it."*

That's cstack. The notebook is a Notion page. The job description is a text file. The shift is a scheduled Claude session. No servers. No code. No API keys. No terminal.

If you can write a job description in plain English, you can have an autonomous agent.

---

## Try It Right Now

**You'll need a free [Notion account](https://www.notion.so)** — that's where your agent stores its memory. Everything else runs inside Claude.

Paste this into [Claude Cowork](https://claude.ai) or claude.ai, answer six questions, and you'll have a running agent in under 5 minutes:

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
   - A Session Lock at the top (Status: IDLE/IN_PROGRESS, Last Agent, Timestamp)
   - An Agent Status section (what the agent did last time, what it needs from you)
   - One section per category from question 2, each with: Status, Current Blocker,
     Next Action, Running Log

**2. Skill file** — a markdown file with these rules:
   - Session start: read the state file, check the lock, load current state
   - Each tick: find the highest-priority item, do autonomous work, update state, notify if stuck
   - Session end: write all changes, release the lock
   - Hard constraints: whatever I said the agent should never do

**3. Scheduled task prompt** — a short prompt I can paste into Cowork to run this on a schedule.

Present all three for me to review. Keep the language plain — no jargon.
```

---

## What You Get

**One agent per domain.** Outreach. Research. Client follow-ups. Content. Each runs on its own schedule, keeps its own memory, and does real ongoing work — not a one-shot task, but a process that picks up exactly where it left off every time it wakes up.

**The perfect amount of human in the loop — by design.** New blocker? The agent pings you immediately. Already told you? It won't keep notifying you about the same thing. All green? You hear nothing. You unblock what needs you and walk away.

**The Heartbeat.** One extra agent reads everyone else's notebooks and sends you a single summary of anything waiting on a decision. Five minutes to unblock everything. If nothing needs you, silence.

```
You are the heartbeat agent. Your only job is to check on all my other agents.

Here are the state files you monitor:
[list your state file locations here]

Every time you run:
1. Read each state file
2. Find anything marked "Needs Human"
3. Send me one summary. If nothing needs me, say nothing.
4. Never modify another agent's state file. Read only.

Schedule: every 30 minutes.
```

**Dispatch.** Claude's built-in control plane. Fire a session immediately without waiting for the schedule. Check what any agent is doing right now. Change a rule, adjust a schedule, pivot an agent's focus — without touching a config file. Leave Cowork running on your computer, walk out the door, and your agents keep working. Dispatch is how you reach them from anywhere.

---

## Example: What the Agent Produces

Tell the setup prompt: *"DJ gig booking agent, tracks venues I've contacted and new spots to reach out to, can research venues and draft messages but never send them, ping me on Discord when stuck, run every morning."*

**State file (the agent's memory)**

```markdown
# DJ Gig Booking — State

## Session Lock
**Status:** IDLE

## Agent Status
**Last Run:** 2026-03-28 9:00am
**Summary:** Researched 4 new venues in SF. Drafted intro for The Independent.
**Needs Human:** Yes — approve outreach draft for The Independent

## Venues — Active Outreach

### The Independent
**Status:** Draft Ready
**Current Blocker:** Waiting for human to approve outreach message
**Next Action:** Send once approved
**Running Log:**
- [2026-03-28] Found booker contact. Drafted intro referencing their hip-hop Thursdays.

### Milk Bar
**Status:** Waiting
**Current Blocker:** No reply after first message (sent 2026-03-23)
**Next Action:** Draft follow-up if no reply by day 7
```

**Skill file (the agent's rules)**

```markdown
# DJ Gig Booking Agent

## Session Start
1. Read state file. Check Session Lock — if IN_PROGRESS under 20 min, EXIT.
2. Set lock to IN_PROGRESS. Load Agent Status.

## Each Tick
1. Active outreach — check for replies, flag anything needing approval.
2. Follow-ups — draft follow-up for any venue silent 7+ days.
3. Research — find new venues if queue is low.
4. Update state file. Update Agent Status summary.

## Session End
Write changes. Release lock (set back to IDLE).

## Never
- Send a message without human approval
- Book a gig without human sign-off
- Delete a venue from the pipeline
```

Change "gig booking" to anything — hiring, client delivery, content planning, finances. Same pattern, same structure.

---

## Why Not OpenClaw?

|  | OpenClaw | cstack |
|---|---|---|
| Computer control | Full desktop (unsafe) | Sandboxed with Cowork's safeguards |
| Browser use | Build flows manually with Playwright | Claude Chrome connector — plain English, safety built in |
| Infrastructure | Custom framework | None |
| Setup | Hours | Minutes |
| Technical skill | Developer | Anyone |
| Safety | You're on your own | Built in |
| Memory | A bunch of markdown files | A bunch of markdown files |

OpenClaw proved everyone wants autonomous agents. It also proved that handing an AI unrestricted access to your computer is a security disaster — multiple critical CVEs, 12% of its plugin registry contained malicious code, Kaspersky and Cisco both published "don't use this" advisories.

cstack is the same idea with the unsafe parts removed.

---

## How It Works

Three pieces per agent:

**State file** — a Notion page the agent reads at the start of every session and writes back to at the end. That's the memory. No database, no vector store. Just a structured text file.

**Skill file** — a markdown file with rules. Read your state first. Work on the highest-priority item. Write what you did. Reach out only if something is actually stuck. The agent reads this every session. That's the identity.

**Schedule** — a Cowork scheduled session. The session fires, the agent reads its state, does work, writes back, sleeps. No server running between sessions.

**The session lock** prevents two sessions from writing simultaneously. The agent checks a lock field at the top of the state file: if another session is IN_PROGRESS within the last 20 minutes, it exits. Otherwise it sets the lock, does its work, and releases it. Distributed coordination in five lines of markdown.

**Domain isolation by design.** Agents don't share state files — they can't interfere. Cross-domain context is opt-in: define it explicitly in the skill file if you want one agent to feed another.

---

## The Real Shift

Until now, building an AI agent was an engineering problem — servers, API keys, Python scripts, debugging infrastructure. cstack flips it into a design problem.

99% of knowledge workers will never open a terminal. If the solution requires `npm install`, you've already lost most of the people who actually need this. cstack runs entirely inside tools you already have: a browser and Notion.

If you can describe how your workflow works in plain English, you can build an autonomous agent. That's not a hack. That's the actual future — and it belongs to everyone, not just developers.

---

*[@sauloferreira64](https://x.com/sauloferreira64)*
