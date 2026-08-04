# 04 — Risks and Dependencies

Consolidates every risk (`AR-xx`) and missing-information item (`MR-xx`) from `Output/02_RequirementAnalysis/06_Gaps_and_Risks.md`, mapped to the specific Requirement IDs it affects. Also consolidates cross-requirement and cross-document dependencies. No new risks are introduced in this phase — this is a re-indexing of already-approved analysis.

---

## A. Risks Mapped to Requirements

| Risk ID | Risk | Affected Requirement IDs | Severity (per Req. Analysis) |
|---|---|---|---|
| AR-01 | 2FA may block fully automated login for the admin role (default enforcement ON) | REQ-LOG-001, REQ-LOG-008 | High |
| AR-02 | 2FA setup/verify cannot be end-to-end automated without a provisioned TOTP secret | REQ-LOG-007, REQ-LOG-008, REQ-LOG-009 (transitively) | Medium |
| AR-03 | Forgot-Password requires reading a real inbox; no test-mailbox strategy defined | REQ-LOG-005, REQ-LOG-006 | Medium |
| AR-04 | Session cookie is httpOnly/Secure/SameSite=Strict — not readable/forgeable by test code | REQ-LOG-001, REQ-LOG-002 (and indirectly every requirement that requires an authenticated session) | Medium |
| AR-05 | Rate limiting (5 login attempts/min) could self-block CI on negative-path tests | REQ-LOG-001 | Medium |
| AR-06 | Soft-delete-only data model — test patients are permanent and can pollute future duplicate-detection runs | REQ-PAT-001, REQ-PAT-002, REQ-PAT-003, REQ-PAT-006 | Medium–High |
| AR-07 | UHID and QR code are only known after successful registration — must be captured dynamically, not hardcoded | REQ-PAT-001, REQ-PAT-002, REQ-PAT-003, REQ-PAT-005 | Low |
| AR-08 | Documented "known gaps" mean some FRD "shall" statements don't match current build behavior — tests may "fail" for a pre-known, already-tracked reason | REQ-LOG-003 (session invalidation gap), REQ-PAT-001 (canonical-vs-alias gap) | High |
| AR-09 | ABHA flows depend on an external national registry (ABDM); no sandbox/mock confirmed; feature off by default | REQ-PAT-007, REQ-PAT-008, REQ-PAT-009, REQ-PAT-010 | Medium |
| AR-10 | Facility-scoping assertions need ≥2 seeded facilities to be meaningful | REQ-PAT-004 (and any future cross-facility scenario for REQ-PAT-001/002/003) | Low–Medium |

## B. Missing Information Mapped to Requirements

| Gap ID | Missing Item | Affected Requirement IDs |
|---|---|---|
| MR-01 | No test environment URL/deployment mode | All 20 (blocks everything) |
| MR-02 | No test/seed credentials; no confirmation of 2FA exemption strategy | All 20 (blocks everything indirectly, since Login is a precondition); most directly REQ-LOG-001, 004, 007, 008 |
| MR-03 | No test-data seeding specification (facilities/departments) | REQ-PAT-001, 002, 003, 004 |
| MR-04 | No exact format/character-class rules behind "valid email/PIN/Aadhaar" | REQ-PAT-001 |
| MR-05 | No confirmation of embedded visual mockups behind "Screen 1/2" captions | All 20, generally |
| MR-06 | No email-testing mechanism specified | REQ-LOG-005, REQ-LOG-006 |
| MR-07 | No TOTP secret provisioning strategy | REQ-LOG-007, REQ-LOG-008 |
| MR-08 | No confirmation of which "canonical vs. alias" UI path the live system actually routes to | REQ-PAT-001 |

## C. Dependency Graph (In-Scope Requirements Only)

```
REQ-LOG-001 (Login)
  ├─ depends on: none (entry point)
  ├─ required by: REQ-LOG-002, 003, 004, 009, 010; REQ-PAT-001, 002, 003, 004, 005, 006, 007, 008, 009, 010
  └─ conditionally invokes: REQ-LOG-008 (2FA verify, when applicable)

REQ-LOG-008 (2FA Verify at Login)
  ├─ depends on: REQ-LOG-001 (password step precedes it), REQ-LOG-007 (must be enrolled first)
  └─ required by: nothing directly, but gates REQ-LOG-001's completion for 2FA accounts

REQ-LOG-007 (2FA Setup) → REQ-LOG-008 (Verify) → REQ-LOG-009 (Disable, recovery path)
REQ-LOG-010 (2FA Enforcement by Role) → affects whether REQ-LOG-008 is mandatory for a given role
REQ-LOG-003 (Change Own Password) ⇄ REQ-LOG-004 (Admin Force Reset) — share the forced-reset flag
REQ-LOG-005 (Request Reset Code) → REQ-LOG-006 (Reset with Code) — strictly sequential, one flow

REQ-PAT-001 (Full Wizard)
  ├─ depends on: REQ-LOG-001
  ├─ contains inline: REQ-PAT-006 (duplicate check, Step 2), REQ-PAT-007/008/009 (ABHA, Step 5)
  └─ produces: REQ-PAT-005 (QR code, on success)

REQ-PAT-002 (Quick Registration)
  ├─ depends on: REQ-LOG-001
  ├─ contains inline: REQ-PAT-006 (duplicate check, blocking variant)
  └─ produces: REQ-PAT-005 (QR code, on success)

REQ-PAT-003 (Emergency Registration)
  ├─ depends on: REQ-LOG-001
  └─ produces: REQ-PAT-004 (Active Emergencies List entry)

REQ-PAT-010 (ABHA Link — Existing Patient)
  └─ depends on: an already-registered patient existing (i.e., a prior REQ-PAT-001/002/003 run) — this is a
     post-registration action, which is the basis of the AM-06 scope flag in the RTM
```

## D. Dependencies on Out-of-Scope Modules/Documents (Carried Forward)

| In-Scope Requirement | Depends On | Owning Document/Module (Out of Scope) |
|---|---|---|
| REQ-LOG-001, REQ-LOG-008 | Lockout duration, session timeout, attempt-limit numeric values | Admin Settings FRD, HC-ADS-024 |
| REQ-LOG-005 | SMTP configuration for OTP email delivery | Admin Settings FRD, HC-ADS-018 |
| REQ-PAT-007 to 010 | ABHA feature flag + ABDM credentials | Admin Settings FRD, HC-ADS-016, HC-ADS-012 |
| REQ-PAT-002 | Calling context (Appointment Booking / Check-In screens) | Appointments module, Queue & Check-in module (both out of scope) |
| REQ-PAT-003 | Downstream banner display (HC-PAT-012) | Out-of-scope UI feature — banner display itself not analyzed |
| REQ-PAT-006 | "Merge Records" option leads to standalone merge workflow | HC-PAT-019 (out of scope) |
| REQ-PAT-010 | "Complements" Edit Patient Record | HC-PAT-015 (out of scope) |

These are not blockers in themselves, but they mean full end-to-end verification of some in-scope requirements will touch configuration or UI this analysis has not reviewed. Test Plan should treat these boundaries explicitly (e.g., stub/assume the out-of-scope side, or explicitly extend scope with client sign-off).
