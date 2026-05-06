# call-debrief

A Claude Code plugin for enterprise solutions architects who run 3–5 customer calls a week and need to turn raw call notes into polished outputs — without spending 45 minutes per call doing it manually.

**Persona:** Solutions architect at an enterprise software company managing named accounts (retail, financial services, manufacturing, etc.)

**Problem it solves:** After every customer call, an SA has fragmented notes and three things to produce: a follow-up email to the customer, an internal Slack summary for the team, and a personal next steps doc. This currently takes 30–60 minutes per call and happens inconsistently. This plugin does it in under 2 minutes.

---

## What it produces

Given raw call notes for an account, `/call-debrief:debrief` generates:

1. **Follow-up email** — professional, personalized, ready to send

2. **Internal Slack summary** — 3–5 sentences, casual tone, for your team channel  

3. **Next steps doc** — structured checklist with ownership and timeframes

Everything is auto-saved to `debriefs/[account]-[date].md` in your working directory.

---

## Install in under 5 minutes

### Prerequisites

- Claude Code installed. Check: `claude --version`

- If not installed: `npm install -g @anthropic-ai/claude-code`

### Install

```bash

git clone https://github.com/[your-username]/call-debrief.git

cd call-debrief

claude --plugin-dir .

When Claude Code opens, type /help and look for call-debrief:debrief. If you see it, you're ready.
Try it immediately
/call-debrief:debrief Acme Corp

Call notes:

- Spoke with Sarah (VP Eng) and Marcus (Platform Arch)

- Main pain: their current segmentation tool requires a data team request for every new audience — takes 3 weeks

- They're evaluating us vs Competitor X — decision by end of quarter

- We committed to sending a technical comparison doc by Friday

- Sarah asked about real-time event processing — we said we'd follow up with a reference architecture

- Next call scheduled for 2 weeks out

You should see three outputs generated and a file saved to debriefs/Acme Corp-[today's date].md.


How it works
/call-debrief:debrief [account name]

           ↓

      debrief skill

      (orchestrates)

           ↓

  signal-extractor          output-writer

  (notes → clean signal)    (signal → 3 outputs)

           ↓                        ↓

               PostToolUse hook

        (creates debriefs/ dir, logs each save)

                        ↓

        debriefs/[account]-[date].md| Component | Type | Why it's here |
|---|---|---|
| `debrief` | Skill | Entry point. Receives account name and notes, orchestrates
the two agents in sequence. |
| `signal-extractor` | Agent | Parses raw notes into clean structured facts.
Read-only — no write access needed. |
| `output-writer` | Agent | Takes clean signal and generates three audiencespecific outputs. |
| `PostToolUse` hook | Hook | Guarantees `debriefs/` exists on any machine.
Without this, fresh clone installs fail on the first write. |
---
## What I'd do with more time
1. **Account context injection** — read an `accounts/[name].md` file at debrief
time so the output-writer already knows the account's industry, tech stack, and
relationship history before writing. Currently the agent works only from call
notes; richer context produces sharper outputs.
2. **Three-agent architecture** — split output-writer into three focused agents
(one per output type). Each would have a tighter system prompt and wouldn't
context-switch between professional email tone and casual Slack tone in the same
session.
3. **Slack MCP integration** — add an optional `.mcp.json` pointing to the Slack
MCP so the Slack summary can be posted directly with one confirmation prompt.
Made optional so the core workflow never breaks on installs without Slack auth
configured.







