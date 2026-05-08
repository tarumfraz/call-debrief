---
name: debrief
description: Debrief a customer call. Use when processing call notes, turning meeting notes into structured outputs, or generating account team alignment artifacts after customer conversations.
argument-hint: <account-name> — e.g. "Momentum Brands" or "Duskline Commerce"
allowed-tools: Read, Write, Bash, Task
---

# 📞 Call Debrief

You are supporting an Enterprise Technical Architect who has just completed a customer call.

The user has provided: account name = "$ARGUMENTS"
Raw call notes are in the conversation or a file they have referenced.

---

## Your job

Run these three steps in sequence. Do not skip steps. Do not combine steps.

---

### 🔍 Step 1: Extract signal

Invoke the `signal-extractor` agent with the raw call notes. If the notes are in a file, read the file first and paste the full text content inline into the prompt — do not pass the file path.

Tell it:
"Extract structured signal from these call notes for account: $ARGUMENTS

[paste full notes content here — not a file path, the actual text]"

Wait for the signal-extractor to complete before continuing.

---

### 🔎 Step 2: Technical review

Take the COMPLETE text output from the signal-extractor agent in Step 1 — every line of it — and pass it directly to the `technical-reviewer` agent. Do not summarize it. Do not truncate it. Copy it verbatim.

Tell the technical-reviewer agent:
"Here is the extracted signal from a customer call for account: $ARGUMENTS. Review it for missing technical details.

[INSERT COMPLETE SIGNAL-EXTRACTOR OUTPUT VERBATIM HERE]"

Show the full TECHNICAL REVIEW output to the user.

Then check: does the ⚠️ Missing or 🔴 Blockers section contain any items?

**If yes — pause here.** Say exactly this:

"⚠️ I found [X] technical gaps and [X] blockers from this call before writing the customer email. Would you like to add context now, or proceed with gaps noted as open items in the next steps doc?"

Wait for the user's response before continuing to Step 3.

**If no gaps and no blockers** — continue to Step 3 automatically.

---

### ✍️ Step 3: Generate outputs

Invoke the `output-writer` agent.

Pass it all three of the following:
- The raw call notes
- The extracted signal from Step 1
- The full technical review from Step 2 including all flagged gaps and blockers

Tell it:
"Generate the three outputs for account: $ARGUMENTS.

Here are the raw call notes: [raw call notes]

Here is the extracted signal: [signal-extractor output]

Here is the technical review including flagged gaps: [technical-reviewer output]

Ensure all flagged gaps appear under 🔴 Technical gaps flagged in the next steps doc. Do not drop any gaps."

Wait for the output-writer to complete before continuing.

---

### 💾 Step 4: Save

Write all outputs to a single file: debriefs/$ARGUMENTS-[today's date in YYYY-MM-DD format].md

Use this exact structure:

---
# 📞 Debrief: $ARGUMENTS — [date]

## 📧 Follow-up Email
[email output]

---

## 💬 Internal Slack Summary
[slack output]

---

## 📋 Next Steps
[next steps output]

---

## 🔍 Technical Review
[full technical-reviewer output]

---

## 🗒️ Raw Signal
[signal-extractor output]

---

After saving, tell the user:

"✅ Debrief saved to debriefs/$ARGUMENTS-[date].md

Here's your follow-up email draft:"

Then show the email output.