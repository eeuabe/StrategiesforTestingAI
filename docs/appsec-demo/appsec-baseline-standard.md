# AppSec Baseline Standard (Demo)

This baseline is intentionally lightweight for talk and workshop demonstrations.

## Severity Model

- Critical: Easily exploitable and high impact to confidentiality, integrity, or availability
- High: Exploitable with meaningful impact and low to moderate attacker effort
- Medium: Exploitable under constrained conditions or moderate impact
- Low: Hard to exploit or low impact, still worth addressing

## Controls

### AS-01 Input and Prompt Safety

Requirement:

- User-controlled input must be validated for expected format and length.
- Prompt construction must include defensive instructions and avoid direct trust of user directives.

Evidence examples:

- Request validation logic exists and is enforced.
- Prompt assembly includes clear instruction hierarchy and sanitization or filtering.

### AS-02 Output Encoding and Rendering Safety

Requirement:

- Untrusted data must be encoded before rendering in browser contexts.
- Unsafe HTML insertion patterns must be avoided.

Evidence examples:

- Rendering uses safe APIs such as `textContent` where possible.
- HTML-escaping is consistently applied for all untrusted fields.

### AS-03 Authentication and Authorization for Sensitive Actions

Requirement:

- State-changing or administrative endpoints require authentication.
- Authorization checks enforce least privilege.

Evidence examples:

- Sensitive routes validate identity and role before execution.
- No client-only trust assumptions for privileged actions.

### AS-04 Error Handling and Data Exposure

Requirement:

- User-facing errors must not leak internals.
- Logs must avoid sensitive data and secrets.

Evidence examples:

- Generic error messages returned to clients.
- Stack traces and sensitive fields redacted from routine logs.

### AS-05 Security Configuration Hardening

Requirement:

- CORS must be restricted to trusted origins.
- Debug mode must be disabled outside development.
- Rate limiting should protect exposed endpoints.

Evidence examples:

- Explicit CORS allowlist is configured.
- Environment-specific secure defaults are present.
- Endpoint throttling is implemented for abuse-prone routes.

## Evaluation Decision

- Pass: No unresolved Critical or High findings, and control evidence exists for all applicable controls.
- Pass with Warnings: No unresolved Critical findings, but one or more High findings or significant verification gaps remain.
- Fail: Any unresolved Critical finding or material control gaps with clear exploit path.
