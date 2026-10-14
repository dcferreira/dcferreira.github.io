---
title: Budgebell
summary: An agent-first, fully-local desktop app that nudges you through habits, managed by your AI agent over MCP.
tags:
  - AI Agents
  - Webdev
date: "2026-09-15T00:00:00Z"

# Optional external URL for project (replaces project detail page).
external_link: ""

url_code: "https://github.com/dcferreira/budgebell"
url_pdf: ""
url_slides: ""
url_video: ""

slides: ""
---

An agent-first desktop app that nudges you through habits (movement drills first, general habits too), shows you what to do, and logs what you actually did.

It's designed to be driven by an AI agent rather than by hand: a bundled local [MCP](https://modelcontextprotocol.io) server lets your agent add, update and
disable habits, decide what you get nudged about, and read back the log of what you did or skipped.

Everything stays on your machine: habit definitions, media and logs, with no network calls, no telemetry and no cloud (the MCP server is local too).
Built with Tauri and Svelte, with [releases](https://github.com/dcferreira/budgebell/releases) for macOS, Linux and Windows.
