# call-debrief

A Claude Code plugin for Technical Architects supporting strategic 
accounts through discovery calls, architecture reviews, and technical planning.

**Persona:** Enterprise Technical Architect — responsible for uncovering technical 
requirements, identifying implementation risks, and coordinating follow-up actions 
across the account team.

**Problem:** Customer calls surface critical technical signals — integration patterns, 
data ownership, security requirements, open commitments — that get buried in 
unstructured notes. When those signals are missed, the cost shows up later: 
implementation blockers, unclear ownership, incomplete follow-ups, and customers 
who feel like they weren't heard. Most post-call workflows are manual and 
inconsistent across the account team.

**What it does:** `call-debrief` transforms raw customer call notes into structured 
account team alignment artifacts using a three-agent workflow — extracting technical 
signal, reviewing for missing implementation-critical details, pausing for human 
confirmation if gaps are found, then generating three audience-specific outputs: 
a customer follow-up email, an internal Slack summary, and a tactical next steps 
document.