---
title: agent-pawl
summary: Makes AI coding agents follow the steps you want, in the order you want. A state-machine workflow engine for Claude Code.
tags:
  - AI Agents
  - LLMs
date: "2026-09-18T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

url_code: "https://github.com/dcferreira/agent-pawl"
url_pdf: ""
url_slides: ""
url_video: ""

slides: ""
---

`pawl` ensures your AI coding agents follow the steps you want, in the order you want.

You write a workflow as a YAML state machine, and `pawl` drives the agent through it, not moving on until a step's check passes.
Deterministic steps run as scripts, agentic steps are handed to subagents, and postconditions are checked by `pawl` itself,
so the agent can't simply declare the job done.

It ships as a [Claude Code](https://claude.com/claude-code) plugin with a bundled Go binary, a skill and enforcement hooks.
Check it out [on GitHub](https://github.com/dcferreira/agent-pawl).
