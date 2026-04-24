# AppSec Live Demo Script

This is a presenter script, not an automated script. It is intended for a live walkthrough in VS Code.

## Goal

Show, in order:

1. What custom agents are
2. What instructions files are
3. What workspace `copilot-instructions.md` does
4. What skills are
5. What the baseline standard does
6. How to explicitly invoke the right agent and skill
7. How to review proposed results
8. How to accept or reject runtime code changes
9. How to validate and, if needed, roll back

## Important clarification

The `.instructions.md` file does not run by itself.

It acts as a rules layer for Copilot. In the live demo, you should:

1. Show the instructions file on screen
2. Explain what behavior it is supposed to shape
3. Then explicitly select the custom agent and send the prompt

## Demo setup

Keep these files ready in open tabs:

- `.github/copilot-instructions.md`
- `.github/instructions/appsec-demo.instructions.md`
- `.github/agents/appsec-triage.agent.md`
- `.github/agents/remediation-coach.agent.md`
- `.github/skills/vulnerability-discovery/SKILL.md`
- `.github/skills/runtime-remediation/SKILL.md`
- `docs/appsec-demo/appsec-baseline-standard.md`
- `app/main.py`
- `app/rag_pipeline.py`

## Step 1: Explain the architecture

What to open:

- `.github/copilot-instructions.md`
- `.github/agents/appsec-triage.agent.md`
- `.github/instructions/appsec-demo.instructions.md`
- `.github/skills/vulnerability-discovery/SKILL.md`
- `docs/appsec-demo/appsec-baseline-standard.md`

What to say:

- "The workspace copilot-instructions file provides always-on AppSec guardrails for this repository."
- "The agent file defines the specialist role."
- "The instructions file defines the rules of engagement."
- "The skill file defines a reusable workflow."
- "The baseline standard is the policy source of truth the assistant output must map to."
- "Together they turn a general assistant into a domain-specific AppSec teammate."
- "In this demo, I am the orchestrator choosing which specialist agent to invoke at each step."

## Step 2: Explain the baseline standard

What to open:

- `docs/appsec-demo/appsec-baseline-standard.md`

What to say:

- "This is the standard the agent is using to judge findings."
- "Instead of vague opinions, I want every finding tied to a specific control."
- "That makes the output explainable and reviewable."

## Step 3: Explain how invocation works

What to say:

- "I do not rely on Copilot guessing which agent to use."
- "For demos, I explicitly select the custom agent in the agent picker."
- "If supported, I also invoke the skill explicitly using slash syntax."
- "This means I am orchestrating the workflow manually for clarity."
- "The next logical step would be to build a custom orchestrator agent that decides when to call triage, remediation, and validation agents automatically."

What to do:

1. Open Copilot Chat.
2. Select `AppSec Triage Agent` in the agent picker.
3. Keep `appsec-demo.instructions.md` visible or added as context.

## Step 4: Run discovery and triage

What to type:

`Run /vulnerability-discovery for app and static/js with max 5 findings. Map each finding to docs/appsec-demo/appsec-baseline-standard.md controls and mark uncertain items as needs verification.`

What to say while it runs:

- "This prompt is going to the custom AppSec Triage Agent."
- "The skill gives it a repeatable vulnerability discovery workflow."
- "The instructions file tells it how to format and prioritize findings."

What to point out in the result:

- severity ordering
- evidence references
- control IDs
- confirmed vs needs verification

## Step 5: Review the findings like a human

What to say:

- "I do not blindly accept the output."
- "I check whether the evidence is concrete and whether the control mapping makes sense."
- "This is human-in-the-loop review, not autonomous security scanning."

What to do:

1. Pick 2 findings that are easy to explain.
2. Open the referenced code files.
3. Show the relevant code.
4. Confirm that the finding appears reasonable.

Suggested example files:

- `app/main.py`
- `app/rag_pipeline.py`

## Step 6: Switch to remediation agent

What to do:

1. In Copilot Chat, switch the agent picker to `Remediation Coach Agent`.
2. Optionally show `.github/agents/remediation-coach.agent.md` on screen.

What to say:

- "Now I’m switching from analysis to remediation."
- "This second agent has a different job: propose minimal, reviewable changes."

## Step 7: Get patch suggestions first

What to type:

`Run /secure-patch-suggestion for findings F-001 and F-003. Provide minimal patch suggestions with validation and rollback notes.`

What to say:

- "I like to start in suggestion mode before allowing edits."
- "This lets me review the shape of the fix first."

What to point out:

- minimal blast radius
- validation checklist
- rollback note

## Step 8: Move to apply mode

What to type:

`Run /runtime-remediation in apply mode for findings F-001 and F-003. Make minimal reversible edits, then list changed files and rollback commands.`

What to say before accepting anything:

- "At this point Copilot can propose edits, but I still review the diff."
- "I can accept, reject, or partially accept changes."
- "That review step is where engineering judgment still matters."

What to do:

1. Review the proposed edits in the diff.
2. Accept or reject them deliberately.
3. Call out why a small patch is easier to trust than a broad refactor.

## Step 9: Validate the remediations

What to type:

`Run /remediation-validation for F-001 and F-003 against docs/appsec-demo/appsec-baseline-standard.md and report resolved, partially resolved, or unresolved status.`

What to say:

- "A fix is not complete just because code changed."
- "I want the same baseline standard used again for post-fix validation."

What to point out:

- resolved vs partially resolved
- residual risk
- next action if anything remains open

## Step 10: Show rollback and safety

Option A: interactive explanation

What to say:

- "If I do not like the changes, I can reject them before they land."
- "If I accepted them for demo purposes, I can still roll them back."

Option B: scripted fallback

What to type in terminal:

1. `python appsec_runtime_remediation_demo.py --mode assess`
2. `python appsec_runtime_remediation_demo.py --mode apply`
3. `python appsec_runtime_remediation_demo.py --mode rollback`

What to say:

- "This scripted path is here for repeatability during rehearsals or if live prompting drifts."

## Step 11: Close with the mental model

What to say:

- "Copilot instructions and instruction files guide assistant behavior."
- "The baseline standard defines the policy criteria and controls."
- "Agent file: who does the work."
- "Instructions file: the rules for how to do the work."
- "Skill file: the repeatable procedure."
- "Prompting plus review is how I steer the system and keep a human in control."
- "In this version of the demo, I am the orchestrator."
- "The next evolution is an orchestrator agent that manages the specialist agents and skills as a coordinated workflow."

## Minimal 5-minute version

If time is short, do only this:

1. Show `appsec-demo.instructions.md`
2. Show one agent file and one skill file
3. Select `AppSec Triage Agent`
4. Run:
   `Run /vulnerability-discovery for app and static/js with max 3 findings. Map each to docs/appsec-demo/appsec-baseline-standard.md.`
5. Switch to `Remediation Coach Agent`
6. Run:
   `Run /secure-patch-suggestion for F-001. Provide minimal patch suggestions with validation and rollback notes.`

## Presenter reminder

Do not describe the instructions file as something you execute directly.
Describe it as a context and behavior layer that Copilot uses while you explicitly choose the agent and prompt.