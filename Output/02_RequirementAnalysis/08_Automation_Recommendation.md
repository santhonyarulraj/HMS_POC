# 08 — Automation Approach Recommendation

**Scope note:** this is a recommended *approach*, not a Test Plan, Test Scenarios, or scripts — those are explicitly the next phases and are not started here.

---

## 1. Framework and Structure

- **Page Object Model**, one POM per screen/dialog rather than per feature ID, since several features share a screen (e.g., HC-USR-012/013 are two steps of one Forgot-Password flow; HC-PAT-001's seven wizard steps are naturally one `PatientRegistrationWizard` POM with step-scoped methods, not seven separate POMs).
- Recommended top-level POM set for this scope:
  - `LoginPage` (HC-USR-001, HC-USR-015 second-factor prompt)
  - `ChangePasswordPage` (HC-USR-010)
  - `ForgotPasswordPage` + `ResetPasswordPage` (HC-USR-012/013)
  - `TwoFactorSetupPage` (HC-USR-014)
  - `PatientRegistrationWizard` (HC-PAT-001, with step-scoped methods, including the inline duplicate-warning banner and inline ABHA sub-flow if in scope)
  - `QuickRegisterDialog` (HC-PAT-002)
  - `EmergencyRegisterDialog` (HC-PAT-003)
  - `PatientQrDialog` (HC-PAT-005)
- Use Playwright's `storageState` pattern: authenticate once per role via a real UI login in a setup step, persist the storage state, and reuse it for Patient Registration specs so every registration test doesn't re-exercise the full login flow. The **Login test suite itself** should still always drive the real UI end-to-end, since login correctness is the point of that suite.

## 2. Test-User Matrix

Based on the confirmed 4-role model (pending Q4's resolution on the custom-role conflict):

| Role | Needed For | 2FA Consideration |
|---|---|---|
| Admin | HC-USR-011 (force reset), full-facility registration checks | Enforced by default (BR-L-18) — needs Q2 resolved before any admin-role test can log in |
| Receptionist | Primary actor for all three registration flows | Not enforced by default |
| Doctor | Login-only coverage (no registration use case named) | Not enforced by default |
| Pharmacist | Login-only coverage (no registration use case named) | Not enforced by default |

Recommend provisioning at least **one dedicated automation account per role**, plus **one deliberately deactivated account** (for BR-L-02) and **one account with a forced-reset flag pre-set** (for BR-L-03) as fixture/setup data — these are stateful preconditions that can't be created through the UI mid-test without an admin action first.

## 3. Test Layering (Risk-Based)

1. **Smoke tier** (candidate for CI-per-PR): valid login for each role, valid Full Wizard registration, valid Quick Registration, valid Emergency Registration. Fast, high-value, low flake risk.
2. **Validation tier:** the field-level rules in `04_Validation_Rules.md` — highly data-driven, one parameterized test iterating the (field, condition, message) table per screen rather than one test per row.
3. **Business-rule tier:** the differential behaviors in `03_Business_Rules.md` that are easy to get backwards — duplicate-phone *warn* (wizard) vs. *block* (quick reg) is the standout example; deactivated-account login block; forced-reset redirect; 2FA lockout after 5 failures.
4. **Known-gap-aware tier:** once Q3 is answered, a small explicit set of tests documenting current (not necessarily ideal) behavior for the flagged gaps, clearly commented/tagged as testing documented-current-state so a future fix doesn't silently "break" them without review.

Recommend **not** interleaving negative-login-attempt tests (wrong password, lockout) too densely in the same CI window, given the 5-attempts/minute rate limit (AR-05) — group and pace them, or use a dedicated source IP/runner if available.

## 4. Explicitly Recommend Deferring (Pending Client Input)

- **2FA end-to-end positive-path automation** (HC-USR-014/015 full setup-and-verify) — until a TOTP-secret provisioning strategy exists (Q2). Until then, cover login for non-2FA-enforced roles only.
- **Forgot Password end-to-end** (HC-USR-012/013) — until a test-mailbox solution exists (Q8).
- **ABHA-assisted registration** (HC-PAT-022–025) — until Q7 (in/out of first pass) and ABDM sandbox availability are resolved. If deferred, the Full Wizard tests should simply skip Step 5's ABHA sub-flow, since it's optional (ABHA ID is not a mandatory field).

## 5. Test Data Strategy

- Every patient created during test runs is **permanent** (soft-delete only, BR-P-16) — recommend a naming convention (e.g., `AUTOTEST_` prefix on First/Last Name) so automation-created patients are identifiable and excludable from production-like reporting, pending Q9.
- Capture UHID and QR code **dynamically** from each test's own success response/screen — never hardcode expected UHID values, and assert the *format* (`PAT` + 4-digit year-month + 4 digits) rather than a literal string (AR-07).
- Duplicate-phone test scenarios need **deliberately pre-seeded** existing patients with known phone numbers to reliably trigger BR-P-04 — don't rely on incidental collisions from prior automated runs.

## 6. Out of Scope for This Recommendation

Per the approved lock, no automation approach is proposed for any module beyond Login and Patient Registration, and no CI/pipeline architecture decisions are made here — those belong to later phases once the Test Plan defines the actual test inventory.

---

**Status:** Requirement Analysis complete. Awaiting your review before proceeding to Phase 2 (Test Plan) — per your instructions, this phase does not proceed further without your explicit go-ahead.
