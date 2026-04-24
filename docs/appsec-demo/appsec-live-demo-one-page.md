# AppSec Live Demo One-Page
Use this as a presenter cheat sheet during the live talk.

## Segment 1: Show the building blocks
Screen:
- `.github/copilot-instructions.md`
- `.github/agents/appsec-triage.agent.md`
- `.github/instructions/appsec-demo.instructions.md`
- `.github/skills/vulnerability-discovery/SKILL.md`
- `docs/appsec-demo/appsec-baseline-standard.md`

Say:
- "The workspace copilot-instructions file provides always-on guardrails for this repo."
- "The agent file defines the specialist role."
- "The instructions file defines the rules of engagement."
- "The skill file defines the repeatable workflow."
- "The baseline standard is the policy source of truth those components should follow."
- "Together they turn a general assistant into an AppSec teammate."
- "In this demo, I am acting as the orchestrator by deciding which specialist agent to call and when."

## Segment 2: Show the standard
Screen:
- `docs/appsec-demo/appsec-baseline-standard.md`

Say:
- "This is the baseline standard the agent will use."
- "Every finding should map to a named control, not just opinion."

## Segment 3: Explain invocation

Screen:
- Copilot Chat with agent picker visible

Say:
- "Do not rely on Copilot guessing the right agent."
- "Explicitly select the custom agent and explicitly invoke the skill."
- "The instructions file shapes behavior, but it is not something I execute directly."
- "Right now I am orchestrating the flow manually; the next step would be to create a custom orchestrator agent that coordinates these specialists for me."

Type:
- Select `AppSec Triage Agent`

## Segment 4: Run discovery

Screen:
- Copilot Chat
- Keep `appsec-demo.instructions.md` visible if possible

Say:
- "Now I’m sending a discovery task to the triage agent."
- "The skill gives it a repeatable AppSec workflow."

Type:
`Run /vulnerability-discovery for app and static/js with max 5 findings. Map each finding to docs/appsec-demo/appsec-baseline-standard.md controls and mark uncertain items as needs verification.`

## Segment 5: Review findings

Screen:
- Copilot result
- Open referenced code in `app/main.py` or `app/rag_pipeline.py`

Say:
- "I review the evidence before trusting the result."
- "This is human-in-the-loop analysis, not autonomous security auditing."

## Segment 6: Switch to remediation

Screen:
- Copilot Chat
- `.github/agents/remediation-coach.agent.md`

Say:
- "Now I’m switching from analysis to remediation."
- "This second agent is optimized for minimal, reviewable changes."

Type:
- Select `Remediation Coach Agent`

## Segment 7: Get patch suggestions first

Screen:
- Copilot Chat

Say:
- "I start in Ask mode so I can review the fix shape before any edits happen."

Type:
`Run /secure-patch-suggestion for findings F-001 and F-003. Provide minimal patch suggestions with validation and rollback notes.`

## Segment 8: Apply changes with review

Screen:
- Copilot Chat
- Diff/editor review UI

Say:
- "I now switch chat mode from Ask to Agent so edits and diffs can be applied."
- "Now I’m allowing controlled runtime edits."
- "I still review the diff and choose whether to accept or reject the changes."
- "That review step is the safety boundary."

Type:
- Switch chat mode to `Agent` (or your custom agent mode), keep `Remediation Coach Agent` selected
`Run /runtime-remediation in apply mode for findings F-001 and F-003. Make minimal reversible edits, then list changed files and rollback commands.`

## Segment 9: Validate the fixes

Screen:
- Copilot Chat

Say:
- "Code changed is not the same as risk resolved."
- "I want the same standard used again for re-validation."

Type:
`Run /remediation-validation for F-001 and F-003 against docs/appsec-demo/appsec-baseline-standard.md and report resolved, partially resolved, or unresolved status.`

## Segment 10: Explain accept, reject, rollback

Screen:
- Diff view or terminal

Say:
- "If I do not like the changes, I reject them before they land."
- "If I accept them for demo purposes, I can still roll them back."
- "That keeps the demo safe and repeatable."

Optional fallback:

1. `python appsec_runtime_remediation_demo.py --mode assess`
2. `python appsec_runtime_remediation_demo.py --mode apply`
3. `python appsec_runtime_remediation_demo.py --mode rollback`

## Closing line

Say:
- "Copilot instructions and instruction files shape assistant behavior."
- "The baseline standard defines policy and control criteria."
- "Agent file: who does the work."
- "Instructions file: the rules for how it should work."
- "Skill file: the repeatable procedure."
- "My prompt and review process keep the human in control."
- "Today I am the orchestrator; the next maturity step is building an orchestrator agent that routes between triage, remediation, and validation automatically."