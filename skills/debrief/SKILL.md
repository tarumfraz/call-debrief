---

name: debrief

description: Debrief a customer call. Use when processing call notes, turning meeting notes into outputs, or generating follow-ups after customer conversations.

argument-hint: <account-name> — e.g. "Home Depot" or "Victoria's Secret"

allowed-tools: Read, Write, Bash, Task

disable-model-invocation: false

---

# Call Debrief

You are processing call notes for a solutions architect at an enterprise software company.

The user has provided: account name = "$ARGUMENTS"

Raw call notes are in the conversation or a file they've referenced.

## Your job

Run these two steps in sequence:

### Step 1: Extract signal

Invoke the `signal-extractor` agent with the raw call notes. Pass it the full notes verbatim.

Tell it: "Extract structured signal from these call notes for account: $ARGUMENTS"

### Step 2: Generate outputs

Take the structured signal from Step 1 and invoke the `output-writer` agent.

Tell it: "Generate the three outputs for account: $ARGUMENTS using this structured signal: [paste signal-extractor output]"

### Step 3: Save

Write all three outputs to a single file: debriefs/$ARGUMENTS-[today's date in YYYY-MM-DD format].md

Use this exact structure for the saved file:

# Debrief: $ARGUMENTS — [date]

## Follow-up Email

[email output]

---

## Internal Slack Summary

[slack output]

---

## Next Steps

[next steps output]

---

## Raw Signal (extracted)

[signal extractor output]

After saving, tell the user: "Debrief saved to debriefs/[filename]. Here's a preview:" and show the email draft.
