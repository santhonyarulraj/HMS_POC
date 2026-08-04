# 07 — Clarification Questions for the Client

Consolidated from `06_Gaps_and_Risks.md`. Grouped by priority — **Blocking** items must be answered before Phase 3 (Test Plan) can start meaningfully; **Scoping** items shape how large Phase 3 is; **Design** items can be answered slightly later but should be flagged now.

---

## Blocking — Required Before Test Plan Can Start

**Q1. Test environment.** What URL(s) should Playwright target, and is the environment currently reachable? Is it running in `onpremise` or `saas` deployment mode?

**Q2. Test credentials and 2FA.** Can you provide (or allow us to seed) dedicated test accounts for each of the four roles (admin, doctor, receptionist, pharmacist)? For the admin role specifically — 2FA is enforced by default (BR-L-18) — can 2FA be disabled for a QA/automation account, or should we provision a TOTP secret so tests can generate valid codes programmatically?

**Q3. Current vs. specified behavior for known gaps.** Both FRDs document specific defects in the current build (e.g., password change does not invalidate other sessions; account creation currently has no auth check; a secondary "alias" function under-enforces permissions on patient edit). Should automated tests assert the **FRD's stated "shall" requirement** (which will currently fail) or the **documented current behavior** (which passes today but may not match intended behavior)? We recommend testing current behavior now and flagging the FRD-vs-build gap separately, rather than writing tests that fail on day one for a known, already-tracked reason — but this is your call.

**Q4. Role Matrix contradiction.** We found a direct conflict between two source documents: the Login FRD states exactly four fixed system roles with no custom-role support; the Role Matrix and Permission Catalog document states hospital admins can create custom roles. Which is correct for the environment we'll be testing against?

---

## Scoping — Shapes the Size of Phase 3

**Q5. "Login" scope boundary.** We've interpreted "Login" as the authentication experience only — sign in/out, password self-service, 2FA (10 features: HC-USR-001/002/010–017). This excludes the admin back-office console for Staff Management, the RBAC permission matrix, Facility/Department management, Leave Allotments, and Audit Trails (19 features), which live in the same FRD document. **Please confirm this boundary, or tell us to include some/all of the excluded features.**

**Q6. "Patient Registration" scope boundary.** We've interpreted "Patient Registration" as the three registration flows plus their immediate consequences (10 features: HC-PAT-001–005, 017, 022–025 — including the inline duplicate-phone check and the inline ABHA workflow, since both fire *during* the registration wizard). This excludes the Patient Directory/Search screens, Edit/Delete, and the standalone admin duplicate-*merge* workflow (15 features), which act on already-existing patients rather than registering new ones. **Please confirm this boundary.**

**Q7. ABHA in scope or deferred?** ABHA-assisted registration (HC-PAT-022–025) is feature-flagged off by default and depends on an external national registry (ABDM) plus configuration owned by the out-of-scope Admin Settings FRD. Should this be included in the POC's first automation pass, or deferred until an ABDM sandbox/mock is available?

---

## Design — Needed Before Scripting, Can Follow Slightly Later

**Q8. Email testing for Forgot Password.** HC-USR-012/013 requires reading a real emailed OTP. Do you have a test-mailbox solution in mind (e.g., Mailosaur, Ethereal, a shared IMAP inbox), or should this flow be deferred/stubbed for the POC?

**Q9. Test data isolation and cleanup.** No patient record is ever hard-deleted (soft-delete/merge-flag only), so every patient created by automated tests will persist permanently and could eventually trigger duplicate-detection warnings against other test-created patients. Should we use a dedicated test facility, a naming convention (e.g., an `AUTOTEST_` prefix) to mark automation-created patients, a periodic database-level reset, or some combination?

**Q10. Number of seeded facilities.** Facility-scoping is enforced server-side for both modules (e.g., non-admin staff only see/register their own facility's patients). How many facilities exist (or can be seeded) in the test environment? At least two are needed to test cross-facility isolation meaningfully.

**Q11. Field-format precision.** Several validations (email format, PIN code, Aadhaar) are specified only as "valid format" in the FRDs without an exact pattern. Is there an authoritative regex/format spec we should use for boundary-value test design, or should we infer patterns from the FRD's own examples (e.g., 10-digit phone, 12-digit Aadhaar)?

**Q12. Visual mockups.** Do the "Screen 1 / Screen 2" captions throughout both FRDs correspond to actual embedded mockups, Figma links, or design files anywhere, or are they narrative-only? This affects our confidence in exact field labels/order beyond what the text describes.

---

## Summary

12 questions: 4 blocking, 3 scoping, 5 design. We recommend resolving Q1–Q4 first — everything else can be answered in parallel with early Test Plan drafting once those are settled.
