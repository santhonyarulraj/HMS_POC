# 08 — Non-Blocking Clarifications

**Baseline Version:** RB-1.0 | **Source:** `Output/02_RequirementAnalysis/07_Client_Questions.md`, "Scoping" and "Design" sections (approved, unmodified) | **Total: 8**

These questions can be resolved in parallel with early Test Plan drafting — they do not need to be answered before Phase 2 begins, but should be tracked to closure before the requirements they touch are finalized into Test Cases.

---

## Scoping (3)

**Q5. "Login" scope boundary.** We've interpreted "Login" as the authentication experience only (10 features: HC-USR-001/002/010–017). This excludes Staff Management, RBAC matrix administration, Facility/Department management, Leave Allotments, and Audit Trails (19 features in the same FRD). Please confirm this boundary.
*Affects: overall Login module scope (`02_Approved_Project_Scope.md`, Section 2).*

**Q6. "Patient Registration" scope boundary.** We've interpreted "Patient Registration" as the three registration flows plus inline duplicate-check and inline ABHA (10 features). This excludes Directory/Search, Edit/Delete, and the standalone admin duplicate-merge workflow (15 features in the same FRD). Please confirm this boundary.
*Affects: overall Patient Registration module scope (`02_Approved_Project_Scope.md`, Section 3); also see AM-06 below regarding REQ-PAT-010 specifically.*

**Q7. ABHA in scope or deferred?** ABHA-assisted registration (REQ-PAT-007–010) is feature-flagged off by default and depends on an external national registry (ABDM). Should this be included in the first automation pass, or deferred?
*Affects: REQ-PAT-007, 008, 009, 010 — all 4 are currently "Blocked" in the Automation Coverage assessment pending this answer.*

## Design (5)

**Q8. Email testing for Forgot Password.** REQ-LOG-005/006 requires reading a real emailed OTP. Do you have a test-mailbox solution in mind, or should this flow be deferred/stubbed?

**Q9. Test data isolation and cleanup.** No patient record is ever hard-deleted, so every patient created by automated tests persists permanently. Should we use a dedicated test facility, a naming convention, a periodic database-level reset, or a combination?
*Affects: REQ-PAT-001, 002, 003, 006 most directly (see Risk AR-06).*

**Q10. Number of seeded facilities.** How many facilities exist (or can be seeded) in the test environment? At least two are needed for cross-facility isolation testing.
*Affects: REQ-PAT-004 and any future cross-facility scenario.*

**Q11. Field-format precision.** Several validations (email, PIN code, Aadhaar format) are specified only as "valid format" without an exact pattern. Is there an authoritative regex, or should patterns be inferred from the FRD's own examples?
*Affects: REQ-PAT-001 boundary-value test design.*

**Q12. Visual mockups.** Do the "Screen 1/Screen 2" captions in both FRDs correspond to actual embedded mockups or design files, or are they narrative-only?
*Affects: locator/field-order confidence across all 20 requirements generally.*

---

## Ambiguity Carried Alongside (Not a Formal Numbered Question, Tracked for Completeness)

**AM-06.** REQ-PAT-010 (ABHA link for an existing patient) is grouped under "ABHA-Assisted Registration" in the source FRD, but its own acceptance criteria describe acting on an already-registered patient — closer to profile management than registration. This is the strongest candidate in the baseline for being re-scoped out of "Patient Registration" once Q6/Q7 are answered.

---

## Summary

3 scoping + 5 design = 8 non-blocking questions, plus 1 carried ambiguity (AM-06). None currently prevents Test Plan from starting on the requirements they don't touch, but each should be closed before its associated requirement(s) move from "Approved for Testing" to a finalized Test Case.
