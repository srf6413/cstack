# No Claw Needed

Autonomous agents. Just Claude.

---

OpenClaw proved everyone wants autonomous agents. But it's unsafe, it's complex, and it requires real technical knowledge to set up.

You don't need any of that. You already have everything.

## The Pattern

Each domain of your life gets its own Claude Cowork scheduled session. Each one has:

1. **A state file** — a Notion page (or markdown file) where the session writes what it's working on, what's blocked, and what happened. It reads this every time it wakes up. That's how it remembers.
2. **A skill file** — a text file with the rules. "Read your state first. Do your work. Write what you did. Reach out if urgent."
3. **A schedule** — the session wakes up on its own. Every hour, every morning, once a week. It picks up where it left off.
4. **A way to reach you** — if something is urgent, it texts you, pings your Discord, whatever you use. You answer when you want. It keeps working.

That's one domain. Now multiply it.

Run 5, 10, 20 of these. One for outreach, one for research, one for scheduling, one for client follow-ups. Each with its own memory, its own rules, its own schedule. They don't interfere because they don't share space. But you control what context flows between them.

You just spun up an entire digital department — a researcher, an outreach person, an inbox manager, a scheduler — with zero server overhead. The barrier went from "$50,000 for a developer" to "an afternoon of writing prompts."

## The Heartbeat

How do you know what needs you?

One session checks all the others. It reads every state file, finds anything marked "needs human," and sends you one summary:

> "Gig outreach is stuck on a venue that hasn't replied. Content agent finished 3 drafts. Client follow-up needs you to approve a message."

If everything's green, you hear nothing.

## New Domain in 2 Minutes

A meta-skill walks you through it: "What's this about? What should it track? What rules?" It creates the state file and skill file. Tell Cowork when to run it. Done.

## Why This Matters

Until now, building an AI agent was an engineering problem — API keys, servers, Python scripts, debugging. This flips it into a design problem. If you can describe how your workflow works in plain English, you can build an agent. You're not writing code. You're writing job descriptions.

One person. Ten agents. Each one running on its own schedule, maintaining its own memory, contacting you only when it's stuck.

No framework. No infrastructure. No code. Just Claude.

That's **cstack**.

---

|                   | OpenClaw                         | cstack                              |
| ----------------- | -------------------------------- | ----------------------------------- |
| Computer control  | Full desktop (unsafe)            | Sandboxed with Cowork's safeguards  |
| Infrastructure    | Custom framework                 | None                                |
| Setup             | Hours                            | Minutes                             |
| Technical skill   | Developer                        | Anyone                              |
| Safety            | You're on your own               | Built in                            |
| Memory            | A bunch of markdown files        | A bunch of markdown files           |

---

## Try It Right Now

Paste this into a Claude Cowork session or claude.ai. Answer six questions. You'll have a running agent in 2 minutes.

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

**1. State file** — a Notion page (or markdown file) with this structure:
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

That's it. You now have a state file, a skill file, and a schedule. Your agent wakes up, reads its state, does work, writes back what happened, and goes to sleep. Repeat forever.

Want a second agent? Run the prompt again for a different domain. They don't interfere — each one has its own state, its own rules, its own schedule.

## Example: What the Agent Actually Produces

When you tell the prompt "DJ gig booking agent, tracks venues I've contacted and new spots to reach out to, can research venues and draft messages but never send them, ping me on Discord when something needs me, run every morning" — here's what you get:

### State file (your agent's memory)

```markdown
# DJ Gig Booking — State

## Session Lock
**Status:** IDLE
**Last Agent:** —
**Timestamp:** —

## Agent Status
**Last Run:** 2026-03-28 9:00am
**Summary:** Researched 4 new venues in SF. Drafted intro for The Independent. Waiting on
human approval to send. No reply yet from Milk Bar (5 days).
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
- [2026-03-28] Found 4 SF venues matching criteria. Added The Independent, Brick & Mortar,
  The Knockout, and Amnesia. Prioritized by fit.
- [2026-03-27] Researched 3 venues. 1 was permanently closed, 2 added to pipeline.
```

### Skill file (your agent's rules)

```markdown
# DJ Gig Booking Agent

You are the DJ gig booking agent. Keep the pipeline moving — find venues, draft outreach,
follow up on leads, and flag anything that needs a human decision.

## Session Start
1. Read the state file. Check the Session Lock.
   - If another session is IN_PROGRESS and the timestamp is less than 20 minutes old, EXIT.
   - Otherwise, set lock to IN_PROGRESS with your session name and current time.
2. Read the Agent Status section to remember what happened last time.
3. Load all venue sections.

## Each Tick
1. Active outreach first — check for replies, flag any that need human action.
2. Follow-ups — if a venue hasn't replied in 7 days, draft a follow-up.
3. Research — if outreach queue is low, find new venues that match the criteria.
4. Update the state file with everything you did.
5. Update Agent Status with a plain-English summary and whether you need the human.

## Session End
1. Write all changes to state file.
2. Release lock (set back to IDLE, clear timestamp).

## Never
- Send a message without human approval
- Book or confirm a gig without human sign-off
- Delete a venue from the pipeline
- Make up venue details or contact info
```

### Scheduled task prompt

```
You are the DJ gig booking agent.
Read the skill file and follow its protocol.
Research venues, draft outreach, and track follow-ups.
Never send anything without my approval — flag it and I'll review.
Update the state file when done.
Schedule: every morning, 9am.
```

That's a running agent. It wakes up every morning, checks which venues need follow-ups, researches new ones, drafts messages, and writes everything down. You just review what it found and approve what goes out.

Change "gig booking" to anything — hiring, client delivery, content planning, finances. Same pattern, same structure.

## Setting Up the Heartbeat

The heartbeat is just another agent — but instead of doing domain work, it reads everyone else's state files and tells you what needs you.

Paste this into a Claude Cowork session:

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

That's it. Add new state files to the list as you create new agents. The heartbeat doesn't need to know what each agent does — it just scans for anything stuck.

## The Real Shift

When a new AI tool drops, it's easy to get hypnotized by terminals and frameworks. But 99% of knowledge workers — marketers, writers, managers, freelancers — will never open a terminal.

If a solution requires typing `npm install`, you've already lost 95% of the people who actually need it.

cstack operates entirely inside tools you already know: a browser and Notion. If you can write a job description in plain English, you can build an autonomous agent. That's not a hack. That's the actual future — and it belongs to everyone, not just developers.

---

*[@sauloferreira64](https://x.com/sauloferreira64)*
