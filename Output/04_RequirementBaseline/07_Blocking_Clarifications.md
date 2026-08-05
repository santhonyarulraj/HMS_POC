# 07 — Blocking Clarifications

**Baseline Version:** RB-1.0 | **Source:** `Output/02_RequirementAnalysis/07_Client_Questions.md`, "Blocking" section (approved, unmodified) | **Total: 4**

These 4 questions must be answered before Test Planning can start meaningfully. They are reproduced verbatim from the approved Requirement Analysis — no rewording.

---

**Q1. Test environment.** What URL(s) should Playwright target, and is the environment currently reachable? Is it running in `onpremise` or `saas` deployment mode?
*Affects: all 20 requirements (nothing can be tested without an environment).*

**Q2. Test credentials and 2FA.** Can you provide (or allow us to seed) dedicated test accounts for each of the four roles (admin, doctor, receptionist, pharmacist)? For the admin role specifically — 2FA is enforced by default (BR-L-18) — can 2FA be disabled for a QA/automation account, or should we provision a TOTP secret so tests can generate valid codes programmatically?
*Affects: REQ-LOG-001, 004, 007, 008, 009 directly; all 20 requirements indirectly (Login is a hard precondition for everything).*

**Q3. Current vs. specified behavior for known gaps.** Both FRDs document specific defects in the current build (e.g., password change does not invalidate other sessions; account creation currently has no auth check; a secondary "alias" function under-enforces permissions on patient edit). Should automated tests assert the **FRD's stated "shall" requirement** (which will currently fail) or the **documented current behavior** (which passes today but may not match intended behavior)?
*Affects: REQ-LOG-003 (BR-L-10), REQ-PAT-001 (BR-P-18) directly, and sets precedent for how every future "known gap" in later phases should be handled.*

**Q4. Role Matrix contradiction.** A direct conflict exists between two source documents: the Login FRD states exactly four fixed system roles with no custom-role support; the Role Matrix and Permission Catalog document states hospital admins can create custom roles. Which is correct for the environment we'll be testing against?
*Affects: REQ-LOG-001, REQ-LOG-010 directly; the entire test-user matrix design. See `09_Requirement_Conflicts.md`, CONFLICT-001.*

---

## Recommended Resolution Order

All 4 should be resolved together before Test Plan begins, but if sequencing is necessary: **Q1 and Q2 first** (nothing can run without an environment and credentials), then **Q4** (determines the shape of the test-user matrix), then **Q3** (determines expected-result design for the specific requirements it touches).
