---
title: My modes of working with agents
subtitle: Stepping back as far as the stakes allow

# Summary for listings and search engines
summary: Three ways I work with coding agents, from being in every conversation to only merging PRs, and how I choose between them.

# Link this post with a project
projects: []

# Date published
date: "2026-11-20T00:00:00Z"

# Date updated
lastmod: "2026-11-20T00:00:00Z"

# Is this an unpublished draft?
draft: false

# Show this page in the Featured widget?
featured: false

authors:
  - admin

tags:
  - AI
  - Agents
  - Claude Code
  - Productivity

categories:
  - AI
---

Recently I wrote about [how I use agents for software work](/post/2026-10-how-i-use-agents/). In this follow-up, I want to dive into more details of my workflow. I'll walk through three modes of working I use, and how I pick between them. Each mode has a different level of involvement from me.

You don't need to read the full previous post, but you should know that I use a "brain session", which is just a Claude Code/Codex/Pi instance that is capable of launching more instances. For more details, my full setup is public in [brain-public](https://github.com/dcferreira/brain-public).

To make my descriptions more concrete, this post follows the story of my (imaginary) friend Pascal, who is opening a tech-forward bakery called Boolangerie. Since I work with computers, obviously I'm the friend he calls for anything computer, but luckily he pays me with fresh croissants!

## Worker mode

{{< story >}}
My friend Pascal has the bakery almost ready to go: he opens in a week!

He needs a domain name, email, website, and a social media strategy, quickly.
{{< /story >}}

I open my main brain session, and prompt it:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/1-brain-workers.html" title="The brain starts three workers" height="360" caption="This is interactive: click ⏎ to send each prompt." >}}

The brain session creates one new session for each of the tasks, and primes it with some context it already has: Pascal had sent me an email with some pictures for the website, so they have some context on Boolangerie. Each of those sessions is what we call a "worker": they're a Claude Code or a Codex or a Pi agent.

Each of the workers suggests a plan of action, and I go to each and review, and from then on do a normal interactive session with it. I spend the next few days switching between these workers until the work is done.

{{< figframe src="/post-assets/my-modes-of-working-with-agents/2-the-workers.html" title="Going round the workers" height="290" >}}

{{< when >}}
It's a one-off task, or the first task of a new project. I find I can keep track of up to 5 workers like this, but if doing it long enough I typically feel [AI brain fry](https://www.euronews.com/2026/03/10/ai-brain-fry-why-your-brain-feels-fatigued-after-using-ai-chatbots-at-work). If you have more than 3, I'd recommend moving to the manager mode.
{{< /when >}}

## Manager mode

{{< story >}}
The launch has been a success! Pascal has more customers than he can handle, and he's asking for my help managing it.

He wants a user area on the website for regular customers, with the ability to order for pickup. Every time he speaks to me he has more requests: we need customers to be able to select pick-up time slots, we need to send them reminders when they pre-order, we need to set up a loyalty stamp, we need them to have order history, and they need to be able to pay online.

Ah and the existing customers have started complaining about missing information on the website.
{{< /story >}}

I open my brain session, and prompt it:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/3-brain-manager.html" title="The brain starts a manager" height="370" caption="This is interactive: click ⏎ to send each prompt." >}}

Then I have a "manager worker" that I can interact with. This manager is in charge of keeping track of what needs doing, and keeping me informed on the status of tasks. It's a smart ticket manager, which also helps me break down the tasks. I tell it to spin up some workers to do these initial tasks:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/4-manager-workers.html" title="The manager starts the first workers" height="460" caption="This is interactive: click ⏎ to send each prompt." >}}

I switch from worker to worker and move the tasks along. Before I finish them, Pascal comes in with the next requests. I go to the manager, break the problems down into tasks, and get new workers for all of them.

{{< figframe src="/post-assets/my-modes-of-working-with-agents/5-manager-more.html" title="New requests come in" height="420" caption="This is interactive: click ⏎ to send each prompt." >}}

This is too much information for me all at once, so the manager is keeping me sane by helping me select where to put my attention. The manager has a dashboard to help me:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/6-dashboard.html" title="The manager's dashboard" height="560" >}}

The dashboard tells me which worker I need to look at, so my day is still context switching between the multiple workers.

{{< when >}}
Whenever you have an ongoing project that will have multiple tasks.

This severely alleviates the cognitive load when doing many small tasks: you still keep both high and low-level decisions, but you're released from keeping track of everything at once.

*I got the dashboard idea from Thariq Shihipar's episode on the [Latent Space podcast](https://latent.space/p/thariq). Everything else in this post is agnostic to the harness you use, but the dashboard is specifically using Claude Code.*
{{< /when >}}

## PM + Tech Lead mode

{{< story >}}
Pascal has been keeping the staff shifts on a whiteboard, and managing his stock in a notebook. Last week one of the bakers thought he was off and didn't show up, and on Saturday they ran out of flour!

I'll build him a simple web app to help keep track of stock and shifts. My day job has been busy recently, so I can't give it much attention, but the app is only for Pascal's staff. If it breaks, the worst case is they fall back to the whiteboard and notebook for a day 🤷‍♂️.
{{< /story >}}

I open my brain session, and prompt it:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/7-brain-pm-tech-lead.html" title="The brain starts a PM and a tech lead" height="470" caption="This is interactive: click ⏎ to send each prompt." >}}

This is the most complicated setup, and it's also the most hands-off. I speak only with the "project manager worker", whose job is to orchestrate the work. The "tech lead worker" is in charge of making decisions on which tech to use, how to break the problem down into tasks, and verifying the work of the other workers. The PM ensures the work moves along, but the tech lead is making the actual decisions.

This animation shows how I interact with the PM:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/8-pm-session.html" title="Talking to the PM in its session" height="560" caption="This is interactive: click ⏎ to send each prompt." >}}

In this setup, I'm not interacting with any of the workers directly. I set direction, like a CEO would in a real company, and unblock the workers as needed (need new context, access to a website, installing some tool, etc). This is the most hands-off I work, but even in this case I do keep track of which PRs are being opened and usually set a requirement that I need to review them. I find this to be a very good way to reliably set up long tasks (think 5+ hours without my interaction).

Doing it like this allows me to work on multiple projects at the same time, without really exhausting me. I'm abstracting myself from the low-level decisions completely, but am still in charge of the high-level ones.

{{< when >}}
Whenever you have a big task with lots of parts, with low cost of failure. Internal tools and greenfield projects fit this. I find having an Opus 5.5 for a tech lead makes the outcome quite reliable, because it's incredibly thorough at verifying the work. But I haven't been brave enough to use these techniques for products that are actually used by a big audience.
{{< /when >}}

## What's next?

The ideas presented above can be tweaked to fit whatever work styles you prefer. I made up the roles like project manager and tech lead for concepts that make sense to me, but you could easily add a QA worker or a product manager worker, or whatever else you can think of. **You can and should customize it to your world.**

However, changing the roles only gets you so far. In all the examples above, we have a single developer looking over all of those agents, and if there's a 2nd person who wants to work with the same agents, they just can't.

The next step comes from scaling the humans: you want a setup where your colleagues are using the same agents as you. People have been calling these [software factories](https://simonwillison.net/2026/Feb/7/software-factory/). If you want to learn more, check out [warp.dev's post](https://www.warp.dev/articles/modern-software-factory-tools), or reach out to me because I've built one at [Unitary](https://unitary.ai/platform).

## Close

This post describes some ways of working I've been using myself, and they cover a range of autonomy for the agents. Choosing the appropriate mode for each task is not a simple problem, but I'd summarize the decision as a trade-off between speed and robustness of development: **if bugs have low real-world impact, prioritize speed**. If you're flying a plane, any bug puts lives at risk, so be very hands-on. If you're doing a game for your friends, bugs are at worst a minor annoyance. Most work falls somewhere in the middle.

In practice, I'm generally working with a mix of those modes:

{{< figframe src="/post-assets/my-modes-of-working-with-agents/9-in-practice.html" title="In practice: all three modes at once" height="370" >}}

As in the previous post, you can check the details of how I'm implementing these modes of work in my [public repo](https://github.com/dcferreira/brain-public), but don't copy them directly. Instead, I recommend you take inspiration from it and make your own skills that fit your working style.
