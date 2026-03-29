# No Claw Needed

Autonomous agents. Just Claude.

---

OpenClaw proved everyone wants autonomous agents. But it's unsafe, it's complex, and it requires real technical knowledge to set up.

You don't need any of that. You already have everything.

## The Pattern

Each domain of your life gets its own Claude Cowork scheduled session. Each one has:

1. **A state file** — a markdown file where the session writes what it's working on, what's blocked, and what happened. It reads this every time it wakes up. That's how it remembers.

2. **A skill file** — a text file with the rules. "Read your state first. Do your work. Write what you did. Reach out if urgent."

3. **A schedule** — the session wakes up on its own. Every hour, every morning, once a week. It picks up where it left off.

4. **A way to reach you** — if something is urgent, it texts you, pings your Discord, whatever you use. You answer when you want. It keeps working.

That's one domain. Now multiply it.

Run 5, 10, 20 of these. One for outreach, one for research, one for scheduling, one for client follow-ups. Each with its own memory, its own rules, its own schedule. They don't interfere because they don't share space. But you control what context flows between them.

## The Heartbeat

How do you know what needs you?

One session checks all the others. It reads every state file, finds anything marked "needs human," and sends you one summary:

> "A is stuck on X. B finished Y. C needs you to approve Z."

If everything's green, you hear nothing.

## New Domain in 2 Minutes

A meta-skill walks you through it: "What's this about? What should it track? What rules?" It creates the state file and skill file. Tell Cowork when to run it. Done.

## Why This Matters

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

1. What domain is this for? (e.g., lead outreach, content calendar, project tracking, hiring)
2. What should this agent track? (e.g., leads, posts, tasks, deals)
3. What can this agent do without asking me? (e.g., draft messages, research, update statuses)
4. What should this agent NEVER do? (e.g., send messages, spend money, delete anything)
5. How should it reach me when it's stuck? (Discord, text, email, or just write it down and wait)
6. How often should it run? (every hour, every morning, twice a day)

After I answer:

1. Create a state file — a markdown file with a session lock, one section per thing I'm
   tracking, each with Status, Current Blocker, Next Action, and a Running Log. The session
   lock prevents two sessions from writing at the same time.

2. Create a skill file — a markdown file with these rules:
   - Session start: read the state file, check the lock, load current state
   - Each tick: find the highest-priority active item, do autonomous work, notify me if stuck
   - Session end: update the state file for everything that changed, release the lock
   - Hard constraints: whatever I said the agent should never do

3. Write a scheduled task prompt I can paste into Cowork.

Present all three for me to review. Keep the language plain — no jargon.
```

That's it. You now have a state file, a skill file, and a schedule. Your agent wakes up, reads its state, does work, writes back what happened, and goes to sleep. Repeat forever.

Want a second agent? Run the prompt again for a different domain. They don't interfere — each one has its own state, its own rules, its own schedule.

## Example: What the Agent Actually Produces

When you tell the prompt "lead outreach agent, tracks warm leads and cold prospects, can draft messages but never send them, notify me on Discord, run every 4 hours" — here's what you get:

### State file (your agent's memory)

```markdown
# Lead Outreach — State

## Session Lock
**Status:** IDLE
**Last Agent:** —

## Warm Leads
**Status:** Active
**Next Action:** Draft follow-ups for 3 leads who responded this week
**Running Log:**
- [2026-03-28] 5 new leads from campaign. Agent drafted intros. Sent to human for approval.

## Cold Outreach
**Status:** Active
**Next Action:** Research 10 new prospects
**Running Log:**
- [2026-03-28] Agent found 6 prospects matching criteria. Drafted outreach for top 3.
```

### Skill file (your agent's rules)

```markdown
# Lead Outreach Agent

You are the lead outreach agent. Keep the pipeline moving.

## Session Start
1. Read the state file. Check the lock. If another session is active, exit.
2. Set lock to IN_PROGRESS. Load all leads.

## Each Tick
1. Warm leads first — anyone who responded gets priority
2. Draft follow-ups (NEVER send without approval)
3. If warm leads are handled, research new prospects
4. Update state file with everything you did

## Session End
1. Write all changes to state file
2. Release lock

## Never
- Send a message without human approval
- Delete a lead
- Make up contact information
```

### Scheduled task prompt

```
You are the lead outreach agent.
Read the skill file and follow its protocol.
Draft messages but never send — flag for my approval.
Update the state file when done.
Schedule: every 4 hours, 9am–9pm.
```

That's a running agent. Change "leads" to anything — content, hiring, client delivery, finances. Same pattern.


## Setting Up the Heartbeat

The heartbeat is just another agent — but instead of doing domain work, it reads everyone else's state files, and notifies you at the end if you haven't been notified once already.

Paste this into a Claude Cowork session:
```
You are the heartbeat agent. Your only job is to check on all my other agents.

Here are the state files you monitor:
[list your state file locations here]

Every time you run:
1. Read each state file
2. Find anything marked "needs human," "blocked," or "waiting for approval"
3. Send me one summary of what needs me. If nothing needs me, say nothing.
4. Never modify another agent's state file. Read only.

Schedule: every 30 minutes.
```

That's it. Add new state files to the list as you create new agents. The heartbeat doesn't need to know what each agent does — it just scans for anything stuck.

---

*[@sauloferreira64](https://x.com/sauloferreira64)*
