# 03 — Automation Coverage

Consolidated automation-readiness view across all 20 in-scope requirements. Feasibility/Complexity/Priority definitions are stated in `01_Requirement_Traceability_Matrix.md`, "How to Read This Matrix."

---

## A. Coverage Table

| Requirement ID | Automation Feasibility | Complexity | Automation Priority | Primary Blocker (if any) |
|---|---|---|---|---|
| REQ-LOG-001 | Partially Feasible | Low | P1 | Q2 — 2FA exemption (admin role only) |
| REQ-LOG-002 | Feasible | Low | P1 | None |
| REQ-LOG-003 | Feasible | Low | P2 | None |
| REQ-LOG-004 | Feasible | Medium | P2 | Transitively Q2 (needs admin login) |
| REQ-LOG-005 | Partially Feasible | Medium | P3 | Q8 — test mailbox |
| REQ-LOG-006 | Partially Feasible | Medium | P3 | Q8 — test mailbox |
| REQ-LOG-007 | Blocked | High | P3 | Q2 — TOTP secret provisioning |
| REQ-LOG-008 | Blocked | High | P2 | Q2 — TOTP secret provisioning |
| REQ-LOG-009 | Feasible* | Medium | P3 | *Depends on REQ-LOG-007 being unblocked first |
| REQ-LOG-010 | Feasible | Medium | P3 | None (toggle itself); Q4 affects role-model assumptions |
| REQ-PAT-001 | Feasible | High | P1 | None |
| REQ-PAT-002 | Feasible | Low | P1 | None |
| REQ-PAT-003 | Feasible | Medium | P1 | None |
| REQ-PAT-004 | Feasible | Low | P3 | None |
| REQ-PAT-005 | Feasible | Low–Medium | P2 | None (QR payload decode is a design choice, not a blocker) |
| REQ-PAT-006 | Feasible | Medium | P1 | None (needs seeded duplicate data — a setup task, not a blocker) |
| REQ-PAT-007 | Blocked | High | P3 | Q7 — ABHA in/out of scope; external ABDM dependency |
| REQ-PAT-008 | Blocked | High | P3 | Q7 — same as REQ-PAT-007 |
| REQ-PAT-009 | Blocked | High | P3 | Q7 — same as REQ-PAT-007 |
| REQ-PAT-010 | Blocked | High | P3 | Q7 — same as REQ-PAT-007; also AM-06 scope question |

---

## B. Summary Counts

| Category | Count | Requirement IDs |
|---|---|---|
| **Feasible now** | 11 | REQ-LOG-002, 003, 004, 009, 010; REQ-PAT-001, 002, 003, 004, 005, 006 |
| **Partially Feasible** (primary path works, sub-path blocked) | 3 | REQ-LOG-001, 005, 006 |
| **Blocked** (cannot automate until a question is resolved) | 6 | REQ-LOG-007, 008; REQ-PAT-007, 008, 009, 010 |
| **Total** | 20 | — |

## C. Automation Priority Distribution

| Automation Priority | Count | Requirement IDs |
|---|---|---|
| P1 | 6 | REQ-LOG-001, 002; REQ-PAT-001, 002, 003, 006 |
| P2 | 4 | REQ-LOG-003, 004, 008; REQ-PAT-005 |
| P3 | 10 | REQ-LOG-005, 006, 007, 009, 010; REQ-PAT-004, 007, 008, 009, 010 |
| **Total** | 20 | — |

## D. What "Blocked" Means Here

None of the 6 Blocked requirements are infeasible *in principle* — every blocker traces to a specific open client question (Q2, Q7, Q8) rather than a technical dead end. Once those are answered, this table should be revisited; no requirement is being recommended for permanent exclusion from automation on technical grounds alone. The one partial exception is **REQ-PAT-010**, which also carries a scope question (AM-06) independent of the ABHA technical blocker.

## E. Manual-Only Candidates

No requirement in this matrix is assessed as **exclusively manual** (i.e., impossible to automate even after blockers are resolved). This reflects that both modules were deliberately chosen in the Documentation Audit phase for being well-specified, UI-driven, deterministic flows. If, after Test Plan design, any requirement proves impractical to automate reliably (e.g., true QR-payload decoding, or live ABDM sandbox behavior), that determination belongs to Phase 3 (Test Plan) — this phase does not make that call.
