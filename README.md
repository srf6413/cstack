### No Claw Needed


Claw with no coding. No setup scripts. No technical knowledge needed.


OpenClaw and Dispatch showed us agents can reach you wherever. But one gateway for everything loses context between sessions. Your AI forgets what it was doing. And cramming every domain into one agent is chaos. Separate spaces per domain, each with its own memory is a good thing.



## The Pattern

Each part of your life gets its own Claude Cowork scheduled session. Each one gets:

1. **A state file** — a simple markdown file (or Notion page) where the session writes down what it's working on, what's blocked, and what happened. It reads this every time it wakes up. That's how it remembers.

2. **A skill file** — a text file that tells the session the rules. "Read your state file first. Do your work. Write down what you did before you stop in the state file, reach out if urgent."

3. **A schedule** — the session wakes up on its own. Every hour, every morning, once a week — whatever makes sense for that domain. It picks up where it left off.

4. **A way to reach you** — most of the time, sessions will stop quietly and unless you're already in cowork then you can be notified later. But if something is actually urgent, it reaches you right away — Discord, text, Slack, whatever you use, to go back and answer the question in Cowork.

That's one area of life. Now multiply it.

You can run 5, 10, 20 of these — one for email outreach, one for research, one for scheduling, one for client follow-ups — each with its own memory, its own rules, its own schedule. They don't interfere with each other because they each have their own space. But you can control which domains to share which context with.

## Keeping It Safe

What if two sessions try to update the same state file at the same time?

Every state file has a lock at the top. When a session starts working, it checks the lock. If another session is already in there, it backs off and waits. When the first session finishes, it releases the lock. No conflicts. No lost work.

If a session crashes mid-run, the lock has a timeout — after 20 minutes, the next session assumes it crashed and resets the lock automatically.


## Creating a New Domain

Want to add a new area of your life? You don't have to build anything from scratch.

A meta-skill (just another text file) walks you through it: "What's this domain about? What should the session track? What rules should it follow?" It creates the state file and domain specific skill file for you(to enforce the rules). New domain in 2 minutes. Cowork will create scheduled sessions through natural language — just tell it what you want and when. No cron syntax. No config files.


## The Heartbeat

With that many cowork sessions running, how do you know what needs your attention?

A heartbeat session following similar rules runs on its own schedule and checks the most recent state or output of all your other sessions. It reads each ones state file, looks for anything marked as "needs human," and sends you one summary: "Session A is stuck on X. Session B finished Y. Session C needs you to approve Z."

One glance. You know exactly what needs you and what doesn't. If everything's green, you hear nothing. The more urgent ones would have already been flagged.


## Staying in the Loop Without Being in Cowork

As of last week, you don't have to be sitting in front of Claude at your computer to stay connected.

Claude Dispatch lets you reach any existing session and create new ones from any device with the Claude app. Your session asks a question in Cowork, you get notified, you reply in Dispatch, and the answer goes back to the session. You never have to leave what you're doing. And context stays forced to be exact, and naturally separated, but always retrievable when needed.

## Why this matters

This solves the problem of one person managing many AI sessions without losing track of anything. Most of the time your sessions will just work quietly and the heartbeat will catch you up later. But if something is actually urgent, it reaches you immediately on whatever channel you use — no waiting for the next heartbeat.
