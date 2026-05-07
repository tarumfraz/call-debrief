# call-debrief

A Claude Code plugin for Technical Architects running customer discovery, architecture, and implementation planning calls.

---

## The problem

Customer calls usually contain a mix of technical requirements, open questions, stakeholder concerns, and next steps buried inside messy notes.

The problem is not writing follow-up emails. The problem is preserving the important signal after the call ends so the broader account team stays aligned and technical gaps are surfaced early.

Most teams do this manually today. The process is inconsistent, details get lost, and follow-ups depend heavily on whoever took the notes.

---

## What it does

`call-debrief` takes raw customer call notes and turns them into structured account team follow-ups.

The workflow is intentionally split across three agents:

```text
signal-extractor → technical-reviewer → output-writer
```

### signal-extractor

Reads raw notes and pulls out structured signal:
- customer pain points
- technical dependencies
- commitments
- stakeholders
- open questions
- next steps

### technical-reviewer

Reviews the notes for missing technical context that should probably be clarified before follow-up.

Examples:
- unclear integration ownership
- missing authentication details
- unresolved data questions
- undefined success metrics
- vague implementation timelines

### output-writer

Generates three audience-specific outputs:
- customer follow-up email
- internal Slack summary
- next steps document

If the reviewer finds unresolved technical gaps, the workflow pauses before generating the customer email. You can either add more context or continue anyway.

Everything saves automatically to:

```text
debriefs/[account]-[date].md
```

---

## Who it's for

Built for technical field teams supporting complex customer accounts.

The workflow is aimed at Technical Architects who:
- run multiple customer calls per week
- coordinate across account teams
- need consistent follow-ups
- do not want technical details buried in meeting notes

---

## Why this workflow matters

Customer discovery calls directly influence architecture decisions, implementation timelines, and account strategy.

Small details missed during follow-up can turn into larger problems later:
- unclear ownership
- incomplete requirements
- missed commitments
- unresolved technical assumptions

The goal of this plugin is to make post-call alignment more structured and repeatable without adding more process overhead to the account team.

---

# Install in under 5 minutes

## Prerequisites

Verify Claude Code is installed:

```bash
claude --version
```

If needed:

```bash
npm install -g @anthropic-ai/claude-code
```

---

## Clone and launch

```bash
git clone https://github.com/tarumfraz/call-debrief.git
cd call-debrief
claude --plugin-dir .
```

Inside Claude Code:

```text
/help
```

You should see:

```text
/call-debrief:debrief
```

---

# Quickstart

Run:

```text
/call-debrief:debrief Momentum Brands
```

Then paste customer notes.

---

# Sample customer notes

```text
- Call with Rachel Kim (Director of Digital Engineering) and Tom Osei (Enterprise Architect)
- They want Agentforce for post-purchase customer support
- Service Cloud is live but all case routing is manual
- OMS data lives in an external system
- Authentication pattern not discussed
- Data Cloud not in current contract
- No PII handling discussion for order data
- Success metric not defined
- Timeline roughly 4 months before holiday peak
- Next steps: send Agentforce Actions overview and confirm OMS integration pattern
```

---

# What happens next

1. `signal-extractor` parses the notes into a structured format
2. `technical-reviewer` surfaces missing technical details
3. Workflow pauses if unresolved gaps are found
4. `output-writer` generates:
   - customer email
   - Slack summary
   - next steps doc
5. Final output saves to `debriefs/`

---

# Example outputs

## Customer email

Short customer-ready follow-up with:
- summary of discussion
- agreed next steps
- open questions
- ownership

## Slack summary

Internal account team update with:
- what happened
- biggest concerns
- next actions
- support needed

## Next steps document

Structured checklist with:
- account team actions
- customer actions
- unresolved gaps
- follow-up items

---

# Plugin architecture

```text
call-debrief/
├── .claude-plugin/
│   └── plugin.json
├── skills/
│   └── debrief/
│       └── SKILL.md
├── agents/
│   ├── signal-extractor.md
│   ├── technical-reviewer.md
│   └── output-writer.md
├── hooks/
│   └── hooks.json
└── README.md
```

See `architecture.html` for a full visualization of the workflow, agent boundaries, hook behavior, and generated outputs.

---

# Why the workflow is split into agents

The workflow is intentionally split into separate agents so extraction, review, and writing remain isolated responsibilities with different instructions and tool access.

| Component | Purpose |
|---|---|
| `signal-extractor` | Focused only on extraction so the output stays structured and consistent |
| `technical-reviewer` | Reviews the extracted signal and flags missing technical context |
| `output-writer` | Generates audience-specific follow-up artifacts |
| `debrief` skill | Orchestrates the workflow and handles sequencing |
| `PostToolUse` hook | Ensures `debriefs/` exists and logs saves automatically |

---

# Why the hook exists

The plugin generates runtime output files on fresh installs.

The `PostToolUse` hook:
- guarantees the `debriefs/` directory exists
- prevents first-run write failures
- appends save events to `debriefs/.log`

This keeps the workflow consistent across machines without requiring manual setup.

---

# Testing quickly

After running the sample command:

Verify the generated file exists:

```bash
cat debriefs/Momentum\ Brands-$(date +%Y-%m-%d).md
```

Verify the hook log:

```bash
cat debriefs/.log
```

---

# Build pattern

This plugin is one example of a broader enterprise workflow pattern:

```text
raw operational input
→ structured extraction
→ review/checkpoint
→ audience-specific outputs
```

The same structure could be reused for:
- RFP workflows
- architecture review prep
- escalation summaries
- pilot readiness reviews
- integration discovery
- AI workshop follow-ups

---

# What I'd do with more time

1. Add optional account context injection from `accounts/[name].md` so outputs can reference existing architecture, stakeholders, and account history alongside the current call notes.

2. Split `output-writer` into three smaller output-specific agents so each output type can stay tightly scoped to its audience and formatting rules.

3. Add optional Slack MCP support so the internal summary can post directly into account team channels after confirmation.

---

# Author

Tarum Fraz  
github.com/tarumfraz