---
title: How I use agents for work
subtitle: Context is everything

# Summary for listings and search engines
summary: How I run 3-10 coding agents in parallel at work, and why the trick is making sure they already have the context.

# Link this post with a project
projects: []

# Date published
date: "2026-10-29T00:00:00Z"

# Date updated
lastmod: "2026-10-29T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Unlisted preview: rendered at its URL, but kept out of lists, the homepage and RSS.
# Delete `_build` and `private` below, and uncomment the tags and categories, to publish it properly.
_build:
  list: never
  render: always
# Keep it out of search engines while unlisted (Wowchemy adds a noindex meta).
private: true

# Show this page in the Featured widget?
featured: false

authors:
  - admin

# Tags and categories would list the post on their pages; restore them when publishing.
# tags:
#   - AI
#   - Agents
#   - Claude Code
#   - Productivity
#
# categories:
#   - AI
---

I've had both friends and colleagues ask how I'm using agents for work. It's easier to show than explain. Click the buttons in this animation to experience my typical morning:

{{< figframe src="/post-assets/how-i-use-agents/opener.html" title="Join standup, then launch a worker for each task" height="520" >}}

The above is genuinely how I work nowadays. Most days I'm working on 3-10 tasks in parallel. The trick isn't the agents or the harness. It's that, **by the time I ask, they already have the context.**

If you write software and want to get more out of your agents, here's how I've set this up.

## Context is everything

It doesn't matter how good the model is: if it doesn't get the correct information, it won't accomplish its task. My job these days is not to write code, but rather to ensure that my agents have access to the right context.

So let's talk about where my agents are getting their context from. This plot compares the sources before/after I had this setup described in this post, as well as its impact on my productivity:

{{< figframe src="/post-assets/how-i-use-agents/context-sources.html" title="What agents read each week, and merge requests I opened" height="560" >}}

### Brain

The most important piece of context in my setup is the brain. It's a text store where all agents are saving important information after every session. This turned out to be the most impactful source of context after I set up this system. In the first few weeks there's not much there so it's not super useful, but as it becomes populated the agents start relying on it more and more.

In practice, this is an [Obsidian](https://obsidian.md/) vault (that is, a bunch of markdown files), the capability to index it (I'm using [qmd](https://github.com/tobi/qmd)), and some rules to get all Claude Code sessions to write to it.

### Meetings

I have very few meetings myself, but find that the context in them is typically very important. I find it hard to remember all the details of what we speak about in a meeting, but agents excel at that. Despite it not being a very prominent source of information in the plot above, I'd rank its importance as 2nd only to the brain itself.

In my setup I'm using [Krisp](https://krisp.ai/) to record/transcribe meetings, and then use its [MCP service](https://help.krisp.ai/hc/en-us/articles/25396920405148-Krisp-MCP#h_01M2SV0QW7001SFH0FNHMGPG00) to pull them into my vault. Fortunately for me I work remotely, so I know all meetings will go through my computer; if you work in person this might be hard to replicate.

### Logs

For bug fixing, logs are the most important source. If the agents can see the bugs happening, then you don't need to explain what's actually happening.

I've been using [Datadog](https://www.datadoghq.com/), which also has an [MCP service](https://docs.datadoghq.com/mcp_server/). I find that the agents sometimes use Kubernetes commands directly also, for cases where something is happening live rather than a retrospective investigation.

### Communications

Then there's the messaging platforms and docs. I use Slack to communicate with colleagues, Gmail for customers, and Notion as a wiki. All of these have pretty good MCP connections. I find agents are surprisingly good at searching Slack in particular, they often find things I couldn't myself.

### Linking them together

As an example, a customer asked me to make some changes to how our code was matching names. This was one of many requests in a call with them, a small change for which I didn't memorize all the details. After the meeting, the summary and tasks were added to the brain.

The next day I started an agent with "make the changes to ignore middle names like we talked in yesterday's call", and the agent went to work. The agent knew the summarized information was second-hand, so went straight to the source call and found all the details. It turns out the rules were overly simplified in the summary that was saved in the brain! They were complex: *first/last name alternative spellings were meant to be flagged for human review, when completely different they should be rejected, and only when they completely match (except for the middle name which would ignored) should they be accepted*.

Not only did I not tell the agent all of this, but I'd forgotten all of those details. I checked later and they did match the agreed rules during the 1 minute or so that we discussed in the call. I didn't give the agent the rules, I just made sure it could find them!

## A typical work day

### Before standup

I open up a new session and call a skill: an agent summarizes what I did yesterday, so I have an easy reference to read from during the standup. I find having some writing to look at helps me not forget topics I want to talk about. I also found—as I started doing more tasks per day—that I kept forgetting some things I had done.

{{< figframe src="/post-assets/how-i-use-agents/1-before-standup.html" title="Before standup" height="460" >}}

### After standup

Typically in a standup I'll talk about what I'm doing today, and colleagues make requests of me. So after the call I have a todo list for the day, and I unleash my agents! I do this not only for programming tasks, but for almost all tasks: if I need to send a plot to Alice with the week's data, I open a session so I don't forget it. Each task has one dedicated agent.

*This is the "magic moment" from the opener of this post.*

{{< figframe src="/post-assets/how-i-use-agents/2-standup.html" title="Standup" height="480" >}}

### During the day

Most of my time is context switching between the multiple tasks. I check what an agent needs from me, do what it takes to unblock it, and move on to the next agent. In practice I'm micromanaging all these agents, I'm choosing designs, I'm confirming bugs, I'm running commands myself and reporting results. **Each agent gets a bit of my attention, instead of one task getting all of it.**

To make it a bit easier to manage, I use tmux with some Claude Code hooks that change the tmux session name to include a 🔔 for the sessions that need me.

{{< figframe src="/post-assets/how-i-use-agents/3-the-day.html" title="The day" height="460" >}}

### Wrapping up tasks

When each agent finishes, they are "collected" and "torn down". This just means that they write up a summary of the session (including what new information they learned) into the brain, and then clean up all the temporary files they left. This is often git worktrees, or some scripts that will never be needed again.

Without this step, none of the above would work. **These summaries are the most important part of the brain setup!** They're what make the "pre-standup" skill work, and then everything else from there.

I typically have one "brain" session which is in charge of doing this, and there are some skills on how to do it since it's such a common process.

{{< figframe src="/post-assets/how-i-use-agents/4-wrap-up.html" title="Wrap up" height="470" >}}

### Over the days

Those systems described above all work on a daily basis. However, projects don't typically work at day-scale, so I have a few more tricks:

1. **Backlog:** In the vault, there's a backlog file. The sessions know to write there whenever there's some follow-up work that will need doing. It doesn't get done automatically, but it's there so we can get to it when appropriate.
2. **Weekly goals:** When planning what to do each day, it's useful to have goals that are one level higher. Therefore each week I plan out weekly goals (usually together with my team), and then when planning each day I have an easy reference point for what to prioritize.
3. **Dreaming:** All the sessions store what went wrong in their summary. Every now and then, I have an agent go over these problems and address them. They're typically changes to the setup itself. One example: my machine has `ls` aliased to [colorls](https://github.com/athityakumar/colorls), and therefore the scripts Claude made with `ls` broke in unexpected ways. All agents were having to run into the issue and work around it, until one dreaming session noticed it and decided to put that note in `CLAUDE.md`.

## Prior art and inspiration

Nothing in here is revolutionary, this is just how I've been working these days. The concept of the "2nd brain" was quite popular on Twitter and was introduced to me by Matt Camp at Unitary. I took the general ideas and adjusted them to my setup and ways of working.

I'm not aware of any general framework doing all of this automatically right now. I think it likely is possible to do one, but I find it hard to adopt other people's workflows precisely as they use it. If you're like me, I think a custom setup like described here is the only way to go.

## Conclusion

Everything in this post is about context. Having the right context allows you to live one level of abstraction higher. I don't promise that working like this leads to tasks getting done faster than manually, but it will allow you to do more tasks in the same time. With this workflow, **I'm working on 3-10 tasks simultaneously at any given time.**

I'm sharing my setup as a repo [here](https://github.com/dcferreira/brain-public). It includes the skills I use and a little bit on how to use them. It also includes more systems than those described here. I'm planning to write some follow-up posts: how I manage workers, when to trust agents, and forcing agents to follow my instructions (hint: this is just [pawl](https://github.com/dcferreira/agent-pawl)).

I don't recommend you install those specific skills, but rather take ideas from it and start building your own. Have an agent read over it and recommend one or two ideas to start using in your workflows.

We're back at the vi configs era, but now we "config" them in English!
