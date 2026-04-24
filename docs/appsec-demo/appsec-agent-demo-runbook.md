# AppSec Agent Demo Runbook (60 Minutes)

This runbook is designed for a live talk that demonstrates custom agent setup, vulnerability analysis, and remediation guidance.

## Demo Goal

Show how custom agents and skills can:

1. Analyze an application for security vulnerabilities
2. Prioritize findings using a standard
3. Propose and optionally apply minimal remediation patches
4. Re-validate against the same baseline

## Prerequisites

- Repository opened in VS Code
- Copilot chat available
- Customization files present under `.github/`
- Baseline standard available at `docs/appsec-demo/appsec-baseline-standard.md`
- Speaker cue card available at `docs/appsec-demo/appsec-speaker-cheat-sheet.md`

Optional scripted workflow:

- `python appsec_runtime_remediation_demo.py --mode assess`
- `python appsec_runtime_remediation_demo.py --mode apply`
- `python appsec_runtime_remediation_demo.py --mode rollback`

## Timeboxed Agenda

### 1. Setup and framing (5 minutes)

- Explain the three assets:
  - custom agents
  - skills
  - baseline standard
- Show where these files live in the repo.

### 2. Discovery and triage (15 minutes)

Prompt example:

"Use AppSec Triage Agent to scan app and static/js for top 5 findings. Map each to docs/appsec-demo/appsec-baseline-standard.md controls and mark uncertain items as needs verification."

Expected output shape:

- severity-ordered findings
- evidence per finding
- control mapping
- prioritized fix queue

### 3. Patch suggestions (15 minutes)

Prompt example:

"Use Remediation Coach Agent for findings F-001 and F-003. Provide minimal patch suggestions with validation and rollback notes."

Apply-mode prompt example:

"Use Remediation Coach Agent in apply mode for findings F-001 and F-003. Make minimal reversible edits, then list changed files and rollback commands."

Expected output shape:

- patch objective
- diff-style suggestion or applied edit summary
- validation checklist
- residual risk

### 4. Live apply and validation (10 minutes)

Prompt example:

"Run runtime-remediation for the same findings. Confirm controls addressed, summarize changed files, and provide rollback commands."

Expected output shape:

- changed files
- control mapping
- validation summary
- rollback commands

### 5. Re-validation (5 minutes)

Prompt example:

"Run remediation-validation for F-001 and F-003 against docs/appsec-demo/appsec-baseline-standard.md and report ready or not ready decision."

Expected output shape:

- per-finding validation status
- unresolved items
- residual risk summary
- readiness decision

### 6. Q and A buffer (15 minutes)

- Discuss strengths and limitations
- Emphasize human review in secure SDLC

## Presenter Notes

- Keep claims conservative and evidence-based.
- Avoid exploit walkthroughs; focus on risk reduction.
- If results vary during live run, use the output schema as anchor.
