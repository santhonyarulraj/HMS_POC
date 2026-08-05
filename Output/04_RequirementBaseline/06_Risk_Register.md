# 06 — Risk Register

**Baseline Version:** RB-1.0 | **Source:** `Output/02_RequirementAnalysis/06_Gaps_and_Risks.md` §C and `Output/03_RequirementTraceability/04_Risks_and_Dependencies.md` (approved, unmodified) | **Total Risks Frozen: 10**

All risks remain **Open/Unresolved** as of this baseline — freezing the baseline documents current risk exposure, it does not mitigate or close any risk.

---

## Risk Register

| Risk ID | Risk | Severity | Affected Requirement(s) | Status |
|---|---|---|---|---|
| AR-01 | 2FA may block fully automated login for the admin role (default enforcement ON) | High | REQ-LOG-001, REQ-LOG-008 | Open |
| AR-02 | 2FA setup/verify cannot be end-to-end automated without a provisioned TOTP secret | Medium | REQ-LOG-007, REQ-LOG-008, REQ-LOG-009 | Open |
| AR-03 | Forgot-Password requires reading a real inbox; no test-mailbox strategy defined | Medium | REQ-LOG-005, REQ-LOG-006 | Open |
| AR-04 | Session cookie is httpOnly/Secure/SameSite=Strict — not readable/forgeable by test code | Medium | REQ-LOG-001, REQ-LOG-002 (and indirectly all requirements needing an authenticated session) | Open |
| AR-05 | Rate limiting (5 login attempts/min) could self-block CI on negative-path tests | Medium | REQ-LOG-001 | Open |
| AR-06 | Soft-delete-only data model — test patients are permanent and can pollute future duplicate-detection runs | Medium-High | REQ-PAT-001, REQ-PAT-002, REQ-PAT-003, REQ-PAT-006 | Open |
| AR-07 | UHID and QR code are only known after successful registration — must be captured dynamically | Low | REQ-PAT-001, REQ-PAT-002, REQ-PAT-003, REQ-PAT-005 | Open |
| AR-08 | Documented "known gaps" mean some FRD "shall" statements don't match current build behavior | High | REQ-LOG-003 (session invalidation gap), REQ-PAT-001 (canonical-vs-alias gap) | Open |
| AR-09 | ABHA flows depend on an external national registry (ABDM); no sandbox/mock confirmed; feature off by default | Medium | REQ-PAT-007, REQ-PAT-008, REQ-PAT-009, REQ-PAT-010 | Open |
| AR-10 | Facility-scoping assertions need ≥2 seeded facilities to be meaningful | Low-Medium | REQ-PAT-004 (and any future cross-facility scenario) | Open |

## Severity Distribution

| Severity | Count | Risk IDs |
|---|---|---|
| High | 2 | AR-01, AR-08 |
| Medium-High | 1 | AR-06 |
| Medium | 5 | AR-02, AR-03, AR-04, AR-05, AR-09 |
| Low / Low-Medium | 2 | AR-07, AR-10 |

## Highest-Priority Risks for Test Planning Attention

1. **AR-08** (known-gap vs. specified behavior) and **AR-01** (2FA blocking admin login) are the two High-severity risks. Both require a **client decision** (Q3 and Q2, respectively — see `07_Blocking_Clarifications.md`) before Test Planning can proceed with confidence on the affected requirements.
2. **AR-06** (permanent test data) is Medium-High and structural — it will affect every Patient Registration test run for the life of this POC unless a data-isolation strategy (Q9) is adopted early.

No risk in this register has been closed, mitigated, or accepted as of this baseline — all 10 remain open inputs to Test Plan design.
