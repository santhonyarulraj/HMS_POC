# Master Test Scenarios — Login Module

**Project:** HealixCare Hospital Management System — Playwright TypeScript Automation POC
**Document:** Master Test Scenarios
**Version:** 1.0
**Date:** 2026-08-05
**Author:** AI Generated (Senior QA Architect role, Claude Code)
**Status:** Draft — Pending Client Review
**Standard Alignment:** IEEE 829-2008, ISO/IEC/IEEE 29119
**References:** Requirement Baseline RB-1.0; RTM (`Output/03_RequirementTraceability/`); Master Test Plan (`Output/05_TestPlan/`); User Management & Authentication FRD v3.0; Role Matrix and Permission Catalog; Solution Architecture Document; Global Execution Rules (`Prompts/00_Execution_Rules.md`)

---

## Naming Convention Note

Test Scenario IDs use the format `TS-LOGIN-NNN` (module code `LOGIN` per the Module Code Registry, `Prompts/00_Execution_Rules.md` §19), sequential across the module. **Requirement IDs referenced below remain exactly as frozen in RB-1.0 (`REQ-LOG-001`–`010`)** — see the Non-Blocking Observation in the phase completion summary regarding the `REQ-LOGIN` vs. `REQ-LOG` naming discrepancy between RB-1.0 and the newer Enterprise Naming Convention.

Module for every scenario in this document: **Login**. Not repeated per-scenario below to reduce redundancy.

---

## A. REQ-LOG-001 — Login (8 scenarios)

### TS-LOGIN-001 | Successful login with valid credentials (non-2FA-enforced role)
- **Requirement(s):** REQ-LOG-001 | **Feature:** Authentication | **Category:** Functional | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** A staff member with an active account and a role that does not enforce 2FA (doctor, receptionist, or pharmacist) logs in with correct credentials.
- **Preconditions:** An active, non-2FA-enrolled test account exists for a non-admin role; test environment reachable.
- **Test Objective:** Verify successful authentication establishes a session and loads the correct role-based permission map.
- **Expected Result:** User is taken directly to the dashboard; only the modules permitted by their role are visible; no error is shown.
- **Remarks:** Foundational smoke-tier scenario — first scenario that should pass before any other Login scenario is attempted.

### TS-LOGIN-002 | Successful password verification for a 2FA-enforced role triggers second-factor prompt
- **Requirement(s):** REQ-LOG-001, REQ-LOG-008 | **Feature:** Authentication | **Category:** Security (Authentication) | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** An admin account (2FA enforced by default per BR-L-18) submits correct credentials.
- **Preconditions:** Active admin test account with 2FA enforcement in effect.
- **Test Objective:** Verify the system does not grant a session on password alone when 2FA applies — it must present the second-factor prompt first.
- **Expected Result:** User is routed to the second-factor code entry screen; no dashboard access occurs until REQ-LOG-008 completes.
- **Remarks:** Execution blocked pending Blocking Clarification Q2 (TOTP secret / 2FA exemption strategy) — see `Output/05_TestPlan/12_Entry_Exit_Criteria.md`.

### TS-LOGIN-003 | Login blocked for a deactivated account
- **Requirement(s):** REQ-LOG-001 | **Feature:** Authentication | **Category:** Negative | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** A deactivated account attempts login with otherwise-correct credentials.
- **Preconditions:** A test account exists with `is_active = false`.
- **Test Objective:** Verify BR-L-02 — deactivated accounts cannot log in regardless of credential correctness.
- **Expected Result:** Login is refused with the message `"Account deactivated. Please contact your administrator."`
- **Remarks:** None.

### TS-LOGIN-004 | Forced-reset account is redirected to Change Password before dashboard access
- **Requirement(s):** REQ-LOG-001, REQ-LOG-003 | **Feature:** Authentication | **Category:** Functional | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An account with `force_reset = true` logs in with correct credentials.
- **Preconditions:** A test account exists with the forced-reset flag pre-set (via REQ-LOG-004 as a setup action, or seeded directly).
- **Test Objective:** Verify BR-L-03 — the user is interrupted with Change Password before reaching any other screen.
- **Expected Result:** User is redirected to the Change Password screen immediately after credential verification; dashboard is not reachable until the password is changed.
- **Remarks:** Precondition can be created via TS-LOGIN-017 (Admin Force Password Reset) as a fixture step.

### TS-LOGIN-005 | Invalid credentials rejected with a generic, non-disclosing error message
- **Requirement(s):** REQ-LOG-001 | **Feature:** Authentication | **Category:** Negative | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** Login is attempted with (a) a valid username + wrong password, and (b) a nonexistent username + any password.
- **Preconditions:** None beyond a reachable environment.
- **Test Objective:** Verify BR-L-06 — the exact same error message appears in both cases, disclosing neither which field was wrong nor whether the account exists.
- **Expected Result:** `"Invalid username or password."` shown identically for both cases.
- **Remarks:** Should be scripted as two data-driven variants of one scenario to directly prove message identity, not just correctness.

### TS-LOGIN-006 | Mandatory field validation on the Login form
- **Requirement(s):** REQ-LOG-001 | **Feature:** Authentication | **Category:** Validation | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** Login is submitted with an empty Username field, then with an empty Password field.
- **Preconditions:** None.
- **Test Objective:** Verify both fields are enforced as mandatory per the Validation Rules Baseline.
- **Expected Result:** `"Required."` shown against the empty field in each case; no authentication attempt is made against the backend.
- **Remarks:** None.

### TS-LOGIN-007 | Rate limiting locks out repeated failed attempts
- **Requirement(s):** REQ-LOG-001 | **Feature:** Authentication | **Category:** Boundary | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** 5 consecutive failed login attempts are submitted from the same source within one minute, followed by a 6th attempt.
- **Preconditions:** None beyond a reachable environment; test must run in isolation to avoid interference from parallel CI jobs sharing a source address.
- **Test Objective:** Verify BR-L-05 — the boundary is exactly 5 attempts/minute, not 4 or 6.
- **Expected Result:** The 6th attempt (even with correct credentials) is refused with `"Too many attempts. Please try again in a minute."`
- **Remarks:** Risk AR-05 applies — must be paced/serialized in CI to avoid self-triggered lockouts affecting other tests. See `Output/05_TestPlan/11_Risk_and_Mitigation.md`.

### TS-LOGIN-008 | Session credential is not accessible to client-side scripts
- **Requirement(s):** REQ-LOG-001 | **Feature:** Authentication | **Category:** Security (Session Management) | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** After a successful login, inspect the session cookie's attributes and attempt to read it via client-side script execution.
- **Preconditions:** A successfully authenticated session (result of TS-LOGIN-001).
- **Test Objective:** Verify BR-L-19/NFR-001 — the session cookie is `httpOnly`, `Secure`, `SameSite=Strict`.
- **Expected Result:** Cookie attributes confirm all three flags; script-based read attempts return nothing.
- **Remarks:** This is a functional/behavioral security check, not a penetration test — consistent with `Output/05_TestPlan/06_Test_Levels_and_Test_Types.md` Section 3.

---

## B. REQ-LOG-002 — Logout (2 scenarios)

### TS-LOGIN-009 | Successful logout invalidates the session and returns to Login
- **Requirement(s):** REQ-LOG-002 | **Feature:** Authentication | **Category:** Functional | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An authenticated user selects Sign Out from the user menu.
- **Preconditions:** An active authenticated session.
- **Test Objective:** Verify BR-L-07 — the session is invalidated server-side, not just redirected client-side.
- **Expected Result:** User lands on the Login screen; locally held session/permission data is cleared.
- **Remarks:** None.

### TS-LOGIN-010 | A replayed, already-invalidated session is refused
- **Requirement(s):** REQ-LOG-002 | **Feature:** Authentication | **Category:** Security (Session Management) | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** After logout (TS-LOGIN-009), the previously-valid session credential is replayed against a protected endpoint/screen.
- **Preconditions:** A session that has just been invalidated via logout.
- **Test Objective:** Verify the invalidated credential is genuinely rejected server-side, per BR-L-07 and NFR-004.
- **Expected Result:** The replayed request is treated as unauthenticated; access is refused.
- **Remarks:** Requires an API-level or direct-request technique beyond pure UI interaction — a design point for the Test Case / Framework phase.

---

## C. REQ-LOG-003 — Change Own Password (7 scenarios)

### TS-LOGIN-011 | Successful password change with valid current password and compliant new password
- **Requirement(s):** REQ-LOG-003 | **Feature:** Password Management | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An authenticated user submits their correct current password and a new password meeting the complexity policy.
- **Preconditions:** Authenticated session; known current password.
- **Test Objective:** Verify the password is updated and the user can subsequently log in with the new value.
- **Expected Result:** `"Password changed successfully."`; new password authenticates on next login attempt.
- **Remarks:** None.

### TS-LOGIN-012 | Incorrect current password is rejected
- **Requirement(s):** REQ-LOG-003 | **Feature:** Password Management | **Category:** Negative | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An authenticated user submits an incorrect current password.
- **Preconditions:** Authenticated session.
- **Test Objective:** Verify the change is refused without modifying the stored password.
- **Expected Result:** `"Current password is incorrect."`
- **Remarks:** None.

### TS-LOGIN-013 | New password complexity boundary validation
- **Requirement(s):** REQ-LOG-003 | **Feature:** Password Management | **Category:** Boundary | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** New password values are tested at the complexity boundary: exactly 7 characters (fail), exactly 8 characters meeting all classes (pass), missing uppercase only, missing digit only, missing special character only.
- **Preconditions:** Authenticated session; correct current password.
- **Test Objective:** Verify BR-L-08 is enforced precisely at its stated thresholds, not approximately.
- **Expected Result:** Sub-8-character or missing-class values rejected with the complexity message; the exactly-compliant value succeeds.
- **Remarks:** Strong data-driven scenario — five data variants under one scenario.

### TS-LOGIN-014 | Confirm New Password mismatch is rejected
- **Requirement(s):** REQ-LOG-003 | **Feature:** Password Management | **Category:** Validation | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** New Password and Confirm New Password fields are submitted with different values.
- **Preconditions:** Authenticated session; correct current password.
- **Test Objective:** Verify the two fields must match before the change is accepted.
- **Expected Result:** `"Passwords do not match."`
- **Remarks:** None.

### TS-LOGIN-015 | Successful password change clears a pending forced-reset flag
- **Requirement(s):** REQ-LOG-003, REQ-LOG-001 | **Feature:** Password Management | **Category:** Functional | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** A user with `force_reset = true` completes a compliant password change.
- **Preconditions:** Test account with forced-reset flag set (e.g., via TS-LOGIN-017).
- **Test Objective:** Verify BR-L-09 — the flag clears and subsequent logins no longer redirect to Change Password.
- **Expected Result:** After the change, a fresh login (TS-LOGIN-001 pattern) reaches the dashboard directly, with no forced-reset interstitial.
- **Remarks:** This scenario closes the loop opened by TS-LOGIN-004.

### TS-LOGIN-016 | Live password complexity checklist reflects rule compliance as the user types
- **Requirement(s):** REQ-LOG-003 | **Feature:** Password Management | **Category:** Usability | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** As a new password value is typed character by character, the on-screen complexity checklist is observed.
- **Preconditions:** Authenticated session; on the Change Password screen.
- **Test Objective:** Verify each complexity rule ticks/unticks live as it becomes satisfied/unsatisfied (Interface Requirement, source FRD HC-USR-010).
- **Expected Result:** Checklist state matches the current input's actual compliance at every keystroke tested.
- **Remarks:** UI-behavior scenario — does not depend on any open blocker.

### TS-LOGIN-017 | Known gap — other active sessions remain valid after a password change
- **Requirement(s):** REQ-LOG-003 | **Feature:** Password Management | **Category:** Reliability | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** A user has two concurrent sessions (e.g., two browser contexts); one session changes the password; the other session's continued validity is checked.
- **Preconditions:** Two active sessions for the same test account.
- **Test Objective:** Document actual current behavior for BR-L-10 — this is a **known, already-tracked gap**, not a hypothesis.
- **Expected Result (current, documented behavior):** The second session remains valid and usable after the password change in the other session.
- **Remarks:** **Gated by Blocking Q3.** Per `Output/05_TestPlan/05_Test_Strategy.md` §6, this scenario should assert current behavior and reference BR-L-10 rather than be logged as a new defect if confirmed. Do not finalize the expected result in Test Case design until Q3 is answered.

---

## D. REQ-LOG-004 — Admin Force Password Reset (4 scenarios)

### TS-LOGIN-018 | Admin sets the forced-reset flag on a target account
- **Requirement(s):** REQ-LOG-004 | **Feature:** Password Management | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** An admin applies Force Password Reset to a target staff account without setting a temporary password.
- **Preconditions:** Authenticated admin session; target account exists.
- **Test Objective:** Verify the flag is applied and the confirmation message shown.
- **Expected Result:** `"Password reset applied. The user must change their password at next login."`
- **Remarks:** Requires an authenticated admin session — currently gated by Blocking Q2 (AR-01).

### TS-LOGIN-019 | Admin sets a temporary password directly
- **Requirement(s):** REQ-LOG-004 | **Feature:** Password Management | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** An admin sets a compliant temporary password for a target account.
- **Preconditions:** Authenticated admin session; target account exists.
- **Test Objective:** Verify the temporary password is stored (as a hash) and never redisplayed after save.
- **Expected Result:** Success message shown; the temporary password value does not reappear anywhere in the UI after save.
- **Remarks:** Gated by Q2, same as TS-LOGIN-018.

### TS-LOGIN-020 | Temporary password complexity validation
- **Requirement(s):** REQ-LOG-004 | **Feature:** Password Management | **Category:** Validation | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** Admin submits a non-compliant temporary password.
- **Preconditions:** Authenticated admin session.
- **Test Objective:** Verify the same complexity policy (BR-L-08) applies to admin-set temporary passwords.
- **Expected Result:** Complexity error message shown; no change applied.
- **Remarks:** Gated by Q2.

### TS-LOGIN-021 | Force-reset action correctly triggers the login-time interstitial for the target user
- **Requirement(s):** REQ-LOG-004, REQ-LOG-001 | **Feature:** Password Management | **Category:** Functional | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** End-to-end: admin applies a forced reset (TS-LOGIN-018), then the target user logs in.
- **Preconditions:** Admin session; target account.
- **Test Objective:** Verify the admin-side action correctly produces the user-side behavior already covered in TS-LOGIN-004 — proving the two features integrate correctly.
- **Expected Result:** Target user is redirected to Change Password on their next login.
- **Remarks:** Cross-requirement integration scenario; gated by Q2 for the admin-side setup step.

---

## E. REQ-LOG-005 — Request Password Reset Code (Forgot Password) (4 scenarios)

### TS-LOGIN-022 | Valid reset code request
- **Requirement(s):** REQ-LOG-005 | **Feature:** Password Management | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** A user submits their username (or email) on the Forgot Password screen.
- **Preconditions:** A registered test account with a valid email address.
- **Test Objective:** Verify the neutral confirmation is shown and (where verifiable) a code is dispatched.
- **Expected Result:** `"If an account exists for the details entered, a reset code has been emailed."`
- **Remarks:** Full email-delivery verification blocked pending Q8 (no test-mailbox strategy); the UI-visible confirmation step itself is not blocked.

### TS-LOGIN-023 | Neutral confirmation message regardless of account existence
- **Requirement(s):** REQ-LOG-005 | **Feature:** Password Management | **Category:** Security (Authentication) | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** The same request is submitted twice: once with a real registered username, once with a fabricated, non-existent username.
- **Preconditions:** None beyond a reachable environment.
- **Test Objective:** Verify BR-L-13 — no account-enumeration signal is given via message differences.
- **Expected Result:** Identical confirmation message in both cases.
- **Remarks:** High-value security scenario; fully automatable without email access, since only the UI response is asserted.

### TS-LOGIN-024 | Mandatory field validation on the Forgot Password form
- **Requirement(s):** REQ-LOG-005 | **Feature:** Password Management | **Category:** Validation | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** The Forgot Password form is submitted with an empty Username/Email field.
- **Preconditions:** None.
- **Test Objective:** Verify the field is mandatory.
- **Expected Result:** `"Required."`
- **Remarks:** None.

### TS-LOGIN-025 | Repeated reset requests are rate-limited
- **Requirement(s):** REQ-LOG-005 | **Feature:** Password Management | **Category:** Boundary | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Multiple reset-code requests are submitted in rapid succession for the same account.
- **Preconditions:** A registered test account.
- **Test Objective:** Verify the stated rate-limiting behavior (HC-USR-012 AC7) engages.
- **Expected Result:** Requests beyond the (unspecified in the FRD) limit are throttled or rejected.
- **Remarks:** The FRD does not state the exact numeric threshold for this rate limit — flag for Test Case-level clarification if the exact number is needed; the *existence* of throttling is what this scenario verifies.

---

## F. REQ-LOG-006 — Reset Password with Code (7 scenarios)

### TS-LOGIN-026 | Successful password reset with a valid code
- **Requirement(s):** REQ-LOG-006 | **Feature:** Password Management | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** A valid, unexpired, unused 6-digit code is submitted with a compliant new password.
- **Preconditions:** A freshly-issued valid reset code for a test account.
- **Test Objective:** Verify the password updates and the code is consumed.
- **Expected Result:** `"Password reset successfully. Please log in with your new password."`; new password authenticates; the same code cannot be reused.
- **Remarks:** Blocked pending Q8 (needs a real emailed code) — negative-path variants below do not share this blocker.

### TS-LOGIN-027 | Expired code is rejected
- **Requirement(s):** REQ-LOG-006 | **Feature:** Password Management | **Category:** Boundary | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** A code older than its 15-minute validity window is submitted.
- **Preconditions:** A reset code whose issuance time can be controlled or simulated (e.g., via time manipulation or a pre-aged fixture).
- **Test Objective:** Verify BR-L-12's 15-minute boundary is enforced precisely.
- **Expected Result:** `"Invalid or expired code."`
- **Remarks:** Achieving a precisely-aged code may require database-level fixture control — a design point for Test Case/Test Data phases.

### TS-LOGIN-028 | Already-used code is rejected
- **Requirement(s):** REQ-LOG-006 | **Feature:** Password Management | **Category:** Negative | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** A code that has already been successfully used once (per TS-LOGIN-026) is submitted again.
- **Preconditions:** A previously-consumed reset code.
- **Test Objective:** Verify single-use enforcement.
- **Expected Result:** `"Invalid or expired code."`
- **Remarks:** Depends on TS-LOGIN-026 having run first, or an equivalent fixture.

### TS-LOGIN-029 | Invalid (incorrect) code is rejected
- **Requirement(s):** REQ-LOG-006 | **Feature:** Password Management | **Category:** Negative | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An arbitrary, never-issued 6-digit code is submitted.
- **Preconditions:** None beyond a reachable environment.
- **Test Objective:** Verify incorrect codes are rejected.
- **Expected Result:** `"Invalid or expired code."`
- **Remarks:** Fully automatable without email access — no real code needed for this negative case.

### TS-LOGIN-030 | New password complexity validation on reset
- **Requirement(s):** REQ-LOG-006 | **Feature:** Password Management | **Category:** Validation | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** A valid code is paired with a non-compliant new password.
- **Preconditions:** A valid reset code.
- **Test Objective:** Verify BR-L-08 applies identically in this entry point.
- **Expected Result:** Complexity error message shown; password not changed.
- **Remarks:** Blocked pending Q8 (needs a valid code).

### TS-LOGIN-031 | Confirm New Password mismatch on reset
- **Requirement(s):** REQ-LOG-006 | **Feature:** Password Management | **Category:** Validation | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** A valid code is paired with mismatched New Password / Confirm New Password values.
- **Preconditions:** A valid reset code.
- **Test Objective:** Verify the confirm-match rule applies identically in this entry point.
- **Expected Result:** `"Passwords do not match."`
- **Remarks:** Blocked pending Q8.

### TS-LOGIN-032 | Account recovery restores login ability after credential loss
- **Requirement(s):** REQ-LOG-005, REQ-LOG-006 | **Feature:** Password Management | **Category:** Reliability | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** End-to-end recovery journey: a user who cannot recall their password requests a code, retrieves it, and successfully logs in with the reset password.
- **Preconditions:** A registered test account; test-mailbox access.
- **Test Objective:** Verify the recovery journey as a whole restores full login ability, not just that individual steps succeed in isolation.
- **Expected Result:** User reaches the dashboard using the newly-set password.
- **Remarks:** The genuine end-to-end Reliability/Recovery scenario for this module; blocked pending Q8.

---

## G. REQ-LOG-007 — Two-Factor Setup (Authenticator App) (7 scenarios)

### TS-LOGIN-033 | Successful 2FA setup with a valid confirmation code
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** A user initiates 2FA setup, and confirms with a valid current 6-digit code generated from the displayed secret.
- **Preconditions:** Authenticated session; a way to programmatically generate a valid TOTP code from the setup secret.
- **Test Objective:** Verify the QR/secret is generated and setup completes only on valid confirmation.
- **Expected Result:** `"Two-factor authentication enabled successfully."`; account's 2FA-enabled flag is set.
- **Remarks:** **Blocked pending Q2** (TOTP secret provisioning strategy) — see AR-02.

### TS-LOGIN-034 | Setup is not marked complete until a valid code is entered
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** A user views the QR/secret but navigates away without submitting a confirmation code.
- **Preconditions:** Authenticated session.
- **Test Objective:** Verify BR-L-14 — 2FA is not enabled until proof of possession is given.
- **Expected Result:** Account's 2FA-enabled flag remains false; login continues to not require a second factor.
- **Remarks:** Does not require a valid TOTP code — automatable now.

### TS-LOGIN-035 | Invalid confirmation code is rejected during setup
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Negative | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An arbitrary, incorrect 6-digit code is submitted during setup.
- **Preconditions:** Authenticated session; setup screen reached.
- **Test Objective:** Verify incorrect codes are rejected and setup does not complete.
- **Expected Result:** `"Invalid code. Please try again."`
- **Remarks:** Does not require a valid TOTP secret — fully automatable now.

### TS-LOGIN-036 | Exactly 8 single-use backup codes are generated at setup
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Following successful setup, the backup codes screen is inspected.
- **Preconditions:** Completed 2FA setup (TS-LOGIN-033).
- **Test Objective:** Verify BR-L-15 — exactly 8 codes, shown once.
- **Expected Result:** 8 distinct codes displayed; codes are not retrievable again after leaving the screen.
- **Remarks:** Blocked pending Q2 (depends on TS-LOGIN-033).

### TS-LOGIN-037 | Backup codes are downloadable
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Usability | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** The Download action on the backup-codes screen is exercised.
- **Preconditions:** Completed 2FA setup.
- **Test Objective:** Verify a downloadable file containing the codes is produced.
- **Expected Result:** File download is triggered and contains the 8 displayed codes.
- **Remarks:** Blocked pending Q2.

### TS-LOGIN-038 | 2FA secret is never redisplayed after setup
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Security | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** After setup completes, the user revisits the 2FA settings screen.
- **Preconditions:** Completed 2FA setup.
- **Test Objective:** Verify the raw secret value is not shown again anywhere in the UI or API responses.
- **Expected Result:** Secret is absent from all subsequent screens/responses; only a masked/enabled indicator is shown.
- **Remarks:** Blocked pending Q2.

### TS-LOGIN-039 | Six-digit code entry auto-advances across input boxes
- **Requirement(s):** REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Usability | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Digits are typed one at a time into the 6-digit confirmation code entry field.
- **Preconditions:** Authenticated session; on the 2FA setup confirmation screen.
- **Test Objective:** Verify the Interface Requirement — focus auto-advances to the next box as each digit is entered.
- **Expected Result:** Focus moves correctly across all 6 positions; backspace behavior works as expected.
- **Remarks:** Pure UI-interaction scenario; does not require a valid code — automatable now.

---

## H. REQ-LOG-008 — Two-Factor Verify at Login (8 scenarios)

### TS-LOGIN-040 | Successful verification with a valid current TOTP code
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Following password verification for a 2FA-enabled account, a valid current TOTP code is submitted.
- **Preconditions:** A 2FA-enrolled test account with a known secret.
- **Test Objective:** Verify full session is granted only after this step succeeds.
- **Expected Result:** User reaches the dashboard.
- **Remarks:** **Blocked pending Q2.**

### TS-LOGIN-041 | Successful verification with an unused backup code
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** A valid, unused backup code is submitted in place of a TOTP code.
- **Preconditions:** A 2FA-enrolled account with known, unused backup codes.
- **Test Objective:** Verify backup-code login succeeds and the code is consumed.
- **Expected Result:** User reaches the dashboard; the same backup code fails if reused (see TS-LOGIN-044).
- **Remarks:** Blocked pending Q2.

### TS-LOGIN-042 | TOTP clock-skew tolerance is honored
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Boundary | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** A code generated one 30-second time-step before/after the current server step is submitted.
- **Preconditions:** A 2FA-enrolled account with known secret; ability to generate a time-step-offset code.
- **Test Objective:** Verify BR-L-16's ±1 step tolerance boundary precisely (accept at ±1 step, reject at ±2 steps).
- **Expected Result:** ±1 step codes accepted; ±2 step codes rejected.
- **Remarks:** Blocked pending Q2; also technically intricate — flag for careful Test Case design once unblocked.

### TS-LOGIN-043 | Invalid second-factor code is rejected
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Negative | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** An arbitrary, incorrect 6-digit code is submitted at the second-factor prompt.
- **Preconditions:** A password-verified session awaiting second factor (does not require a real 2FA-enrolled account if a test account with 2FA enforced can reach this prompt).
- **Test Objective:** Verify incorrect codes are rejected without granting a session.
- **Expected Result:** `"Invalid code. Please try again."`
- **Remarks:** Does not require a valid TOTP secret — automatable now for accounts that can reach the prompt (admin role, subject to Q2 for reaching the prompt at all).

### TS-LOGIN-044 | 5 consecutive failed 2FA attempts locks the account for 15 minutes
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Boundary | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** 5 consecutive incorrect codes are submitted at the second-factor prompt, followed by a 6th (even if correct).
- **Preconditions:** A password-verified session awaiting second factor.
- **Test Objective:** Verify BR-L-17's exact 5-attempt boundary.
- **Expected Result:** After the 5th failure: `"Too many failed attempts. Your account is locked for 15 minutes."`; the 6th attempt (even valid) is refused during lockout.
- **Remarks:** Does not require a valid code to trigger (uses 5 wrong codes) — automatable now for accounts reaching the prompt.

### TS-LOGIN-045 | A reused backup code is rejected
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Negative | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** A backup code already consumed in TS-LOGIN-041 is submitted again.
- **Preconditions:** A previously-used backup code.
- **Test Objective:** Verify single-use enforcement for backup codes.
- **Expected Result:** `"Invalid code. Please try again."`
- **Remarks:** Blocked pending Q2 (depends on TS-LOGIN-041 having run).

### TS-LOGIN-046 | Full session is granted only after both factors succeed
- **Requirement(s):** REQ-LOG-008, REQ-LOG-001 | **Feature:** Two-Factor Authentication | **Category:** Security (Authentication) | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** After correct password entry but before second-factor submission, an attempt is made to access a protected screen directly (e.g., by URL).
- **Preconditions:** A password-verified, second-factor-pending session state.
- **Test Objective:** Verify no partial session/access exists between the two factors.
- **Expected Result:** Protected screens remain inaccessible until the second factor succeeds.
- **Remarks:** Important security-boundary scenario — the "no partial trust" guarantee. Achievable for accounts that can reach this intermediate state; execution timing/access depends on Q2 for full end-to-end coverage.

### TS-LOGIN-047 | Six-digit code entry auto-advances at the login second-factor prompt
- **Requirement(s):** REQ-LOG-008 | **Feature:** Two-Factor Authentication | **Category:** Usability | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Same UI-behavior check as TS-LOGIN-039, applied at the login-time second-factor prompt rather than the setup screen.
- **Preconditions:** A password-verified session awaiting second factor.
- **Test Objective:** Verify consistent auto-advance behavior across both 6-digit entry contexts.
- **Expected Result:** Focus auto-advances correctly across all 6 positions.
- **Remarks:** Pure UI-interaction scenario, automatable now.

---

## I. REQ-LOG-009 — Two-Factor Disable (Admin Recovery) (3 scenarios)

### TS-LOGIN-048 | Admin disables 2FA for a user who lost their device
- **Requirement(s):** REQ-LOG-009 | **Feature:** Two-Factor Authentication | **Category:** Reliability | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** An admin applies the Disable 2FA action against a target user's account.
- **Preconditions:** Authenticated admin session; target account has 2FA enrolled.
- **Test Objective:** Verify this is the account-recovery path for lost-device scenarios (the requirement's own name — "Admin Recovery").
- **Expected Result:** `"Two-factor authentication disabled for this user."`; target account's 2FA-enabled flag clears.
- **Remarks:** Blocked pending Q2 for both the admin session and the pre-existing 2FA-enrolled target account.

### TS-LOGIN-049 | User logs in with password only after admin-initiated 2FA disable
- **Requirement(s):** REQ-LOG-009, REQ-LOG-001 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Following TS-LOGIN-048, the affected user logs in with just their password.
- **Preconditions:** A recently-disabled 2FA account.
- **Test Objective:** Verify no second-factor prompt appears post-disable.
- **Expected Result:** User reaches the dashboard without a 2FA prompt.
- **Remarks:** Blocked pending Q2 (depends on TS-LOGIN-048).

### TS-LOGIN-050 | Forced re-enrollment applies if the account's role has 2FA enforcement on
- **Requirement(s):** REQ-LOG-009, REQ-LOG-010 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Low | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** An admin's own 2FA is disabled (or a role-enforced user's is), then that user logs in again.
- **Preconditions:** A disabled-2FA account whose role has enforcement ON.
- **Test Objective:** Verify BR-L (2FA re-enrollment forced for enforced roles) applies correctly after a disable.
- **Expected Result:** User is walked through 2FA setup again before reaching the dashboard.
- **Remarks:** Blocked pending Q2; cross-requirement with REQ-LOG-010.

---

## J. REQ-LOG-010 — Two-Factor Enforcement by Role (5 scenarios)

### TS-LOGIN-051 | Admin toggles 2FA enforcement for a role
- **Requirement(s):** REQ-LOG-010 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** An admin switches enforcement ON for the Doctor role (default OFF).
- **Preconditions:** Authenticated admin session.
- **Test Objective:** Verify the toggle applies and is confirmed to the admin.
- **Expected Result:** `"Two-factor enforcement updated."`
- **Remarks:** Requires admin session — gated by Q2.

### TS-LOGIN-052 | Default enforcement state matches specification
- **Requirement(s):** REQ-LOG-010 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** On a freshly-provisioned environment (or an unmodified one), the enforcement settings screen is inspected.
- **Preconditions:** Authenticated admin session; environment with unmodified defaults.
- **Test Objective:** Verify BR-L-18 — admin ON, doctor/receptionist/pharmacist OFF, out of the box.
- **Expected Result:** Toggle states match exactly.
- **Remarks:** Gated by Q2; also depends on the test environment not having been pre-modified (an environment assumption to confirm during Q1 setup).

### TS-LOGIN-053 | User of a newly-enforced role is forced through 2FA setup at next login
- **Requirement(s):** REQ-LOG-010, REQ-LOG-007 | **Feature:** Two-Factor Authentication | **Category:** Functional | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Low
- **Description:** Following TS-LOGIN-051 (Doctor enforcement turned ON), a doctor account without 2FA logs in.
- **Preconditions:** Doctor-role enforcement ON; a doctor test account without existing 2FA enrollment.
- **Test Objective:** Verify the user is walked through setup before reaching the dashboard, per HC-USR-017 AC3.
- **Expected Result:** User is redirected into the 2FA setup flow (REQ-LOG-007) rather than the dashboard.
- **Remarks:** Cross-requirement scenario; the redirect itself is automatable, but full setup completion is blocked pending Q2.

### TS-LOGIN-054 | Enforcement settings require admin authorization
- **Requirement(s):** REQ-LOG-010 | **Feature:** Two-Factor Authentication | **Category:** Security (Authorization) | **Priority:** Medium | **Automation Candidate:** Yes | **Automation Priority:** Medium
- **Description:** The 2FA enforcement settings screen/action is accessed by an admin session.
- **Preconditions:** Authenticated admin session.
- **Test Objective:** Confirm the positive authorization case as a baseline for TS-LOGIN-055.
- **Expected Result:** Admin can view and modify enforcement settings.
- **Remarks:** Gated by Q2.

### TS-LOGIN-055 | Non-admin roles are denied access to enforcement settings
- **Requirement(s):** REQ-LOG-010 | **Feature:** Two-Factor Authentication | **Category:** Security (Authorization) | **Priority:** High | **Automation Candidate:** Yes | **Automation Priority:** High
- **Description:** A non-admin session (doctor, receptionist, or pharmacist) attempts to reach the 2FA enforcement settings screen/action directly.
- **Preconditions:** Authenticated non-admin session.
- **Test Objective:** Verify the "Admin only" access restriction (HC-USR-017 "User Roles and Permissions") is enforced, not merely hidden in the UI.
- **Expected Result:** Access is refused (ideally at the API/server level, not just UI hiding) — direct navigation/requests should also be denied.
- **Remarks:** High-value authorization scenario; automatable now using any non-admin role (no 2FA blocker applies since non-admin roles are not 2FA-enforced by default).

---

## Summary of Scenario Counts by Requirement

| Requirement ID | Scenario Count | Scenario ID Range |
|---|---|---|
| REQ-LOG-001 | 8 | TS-LOGIN-001 to 008 |
| REQ-LOG-002 | 2 | TS-LOGIN-009 to 010 |
| REQ-LOG-003 | 7 | TS-LOGIN-011 to 017 |
| REQ-LOG-004 | 4 | TS-LOGIN-018 to 021 |
| REQ-LOG-005 | 4 | TS-LOGIN-022 to 025 |
| REQ-LOG-006 | 7 | TS-LOGIN-026 to 032 |
| REQ-LOG-007 | 7 | TS-LOGIN-033 to 039 |
| REQ-LOG-008 | 8 | TS-LOGIN-040 to 047 |
| REQ-LOG-009 | 3 | TS-LOGIN-048 to 050 |
| REQ-LOG-010 | 5 | TS-LOGIN-051 to 055 |
| **Total** | **55** | **TS-LOGIN-001 to 055** |

*(Several scenarios reference a secondary Requirement ID for cross-requirement integration points — each is still counted once, against its primary requirement, in the table above. Full coverage cross-tabulation is in `Scenario_Coverage_Summary.md`.)*
