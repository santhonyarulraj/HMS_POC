# 01 — Requirement Traceability Matrix (RTM)

**Project:** HealixCare HMS — Playwright TypeScript Automation POC
**Scope:** Login + Patient Registration only (per the interpreted scope confirmed in `Output/02_RequirementAnalysis/01_Business_Summary.md`, Section 2)
**Source of truth:** `Output/02_RequirementAnalysis/*` (approved) and `Output/01_Document_Inventory.md` through `04_Required_Documents_For_POC.md` (Documentation Audit). **No new requirements were identified or invented in this phase** — this RTM re-organizes and cross-references content already produced and approved in Phase 1.

---

## How to Read This Matrix

- **Requirement ID** is newly minted in this phase (`REQ-LOG-xxx` / `REQ-PAT-xxx`) to serve as the single stable identifier the rest of the QA lifecycle (scenarios, cases, scripts, defects) will reference. It is distinct from — but fully mapped to — the source FRD's own Feature ID (`HC-USR-xxx` / `HC-PAT-xxx`).
- **Source Page/Section:** the source documents are `.docx` files with no stable extracted page numbers; the FRD section heading and Feature ID are cited instead. This is noted explicitly rather than guessing a page number.
- **Priority**, **Automation Feasibility**, **Automation Complexity**, and **Automation Priority** are QA-judgment fields this phase is explicitly tasked with assigning (RTM Task items 7, 9, 10, 11) — they are analysis performed *on* the approved requirements, not new requirements. The rubric for each is stated below so the judgment is auditable.
- Any field with no available source information is marked **"Not Available"** rather than assumed, per instruction.
- Fields marked **"Clarification Required"** point to the specific question number in `Output/02_RequirementAnalysis/07_Client_Questions.md`.

### Priority Rubric
- **High** — core happy-path flow, security-critical, or a hard precondition for other in-scope requirements.
- **Medium** — supporting or self-service capability, used regularly but not on the primary happy path.
- **Low** — edge-case, admin-recovery, read-only/supporting view, or gated behind an off-by-default feature flag.

### Automation Feasibility Scale
- **Feasible** — can be automated now with standard Playwright techniques.
- **Partially Feasible** — the primary path is automatable; a sub-path is blocked pending a client answer (cited).
- **Blocked** — not automatable until a specific open question is resolved (cited).

### Automation Complexity Scale
- **Low** — single screen/dialog, few fields, no multi-step state.
- **Medium** — multi-field validation, conditional UI, or requires specific pre-seeded data.
- **High** — multi-step wizard, cross-system dependency (email/OTP/external registry), or heavy precondition setup.

---

## A. Login Module

### REQ-LOG-001

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-001 |
| Module | Login |
| Requirement Name | Login |
| Requirement Description | Staff authenticate with username + password; on success a secure server-managed session is established and the role-based permission map is loaded. Deactivated accounts are blocked; forced-reset accounts are redirected to Change Password; 2FA-enabled/enforced accounts are routed to second-factor verification. Every attempt is audit-logged. Rate-limited to 5 attempts/minute per source address. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.1 Authentication, Feature HC-USR-001 (no extracted page number — see note above) |
| Business Objective | Precondition for every staff action in the system; establishes the secured session all other in-scope and out-of-scope modules depend on. |
| Business Rule | BR-L-02, BR-L-03, BR-L-04, BR-L-05, BR-L-06, BR-L-19, BR-L-20 (`03_Business_Rules.md`) |
| Validation Rule | Username required; Password required; credentials must match active account; account must be active; rate limit ≤5/min (`04_Validation_Rules.md`, §A.2) |
| Priority | High |
| Dependency | HC-USR-015 (2FA when applicable); HC-USR-010/013 (forced-reset flow); HC-USR-026 Login Audit Trail (**out of scope**, logging side-effect only); lockout/rate-limit values owned by Admin Settings FRD HC-ADS-024 (**out of scope document**) |
| Automation Feasibility | Partially Feasible — feasible now for non-2FA-enforced roles (doctor, receptionist, pharmacist); blocked for the admin role pending Q2 (2FA exemption/TOTP strategy) |
| Automation Complexity | Low |
| Automation Priority | P1 (smoke tier per `08_Automation_Recommendation.md`) |
| Risk | AR-04 (session cookie not script-readable — must drive real UI or a supported API login), AR-05 (rate-limit could self-block CI on negative-path tests) |
| Assumption | Session mechanism is JWT via httpOnly/Secure/SameSite=Strict cookie (Solution Architecture Document, §6.1) |
| Clarification Required | Yes — Q2 (2FA exemption for admin test account), Q4 (role-model conflict affects which roles are guaranteed to exist) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Core precondition for all Patient Registration requirements (REQ-PAT-001 to 010). |

### REQ-LOG-002

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-002 |
| Module | Login |
| Requirement Name | Logout |
| Requirement Description | Sign-out available from every screen's user menu; invalidates the session server-side immediately via blocklist; clears local session/permission data; returns user to Login; a replayed invalidated session is refused. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.1 Authentication, Feature HC-USR-002 |
| Business Objective | Guarantees a session cannot be reused after the user ends it — core session-security guarantee. |
| Business Rule | BR-L-07 (`03_Business_Rules.md`) |
| Validation Rule | None field-level — behavioral only (session invalidation) |
| Priority | High |
| Dependency | HC-USR-001 (Login); server-side invalidation store (Solution Architecture §6.1) |
| Automation Feasibility | Feasible |
| Automation Complexity | Low |
| Automation Priority | P1 |
| Risk | None specific beyond general session-handling risk (AR-04) |
| Assumption | None beyond §A of `05_Assumptions.md` |
| Clarification Required | No |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Verifying true server-side invalidation (not just UI redirect) requires an API-level check — a design point for Test Plan. |

### REQ-LOG-003

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-003 |
| Module | Login |
| Requirement Name | Change Own Password |
| Requirement Description | Any authenticated staff member changes their own password by supplying the current password and a new password meeting the complexity policy. Clears any forced-reset flag on success. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.4 Password Management, Feature HC-USR-010 |
| Business Objective | Self-service credential hygiene; also the mechanism that satisfies a forced-reset requirement (REQ-LOG-004). |
| Business Rule | BR-L-08, BR-L-09, BR-L-10 (`03_Business_Rules.md`) |
| Validation Rule | Current Password must match; New Password complexity; Confirm New Password must match (`04_Validation_Rules.md`, §A.2) |
| Priority | Medium |
| Dependency | Complexity policy (§3.3 of source FRD); forced-reset flow shared with HC-USR-001 and HC-USR-011 |
| Automation Feasibility | Feasible |
| Automation Complexity | Low |
| Automation Priority | P2 |
| Risk | AR-08 — known gap: does not invalidate the user's other active sessions (BR-L-10); asserting FRD "shall" language here vs. current behavior needs Q3 resolved first |
| Assumption | Password complexity policy applies identically across all password-set operations (§3.3 of source FRD) |
| Clarification Required | Yes — Q3 (assert current vs. specified behavior for the session-invalidation gap) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-LOG-004

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-004 |
| Module | Login |
| Requirement Name | Admin Force Password Reset |
| Requirement Description | Admin sets a forced-reset flag on a target account (routing them to Change Password at next login) and/or sets a temporary password directly, subject to the complexity policy. Temporary password never displayed after save. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.4 Password Management, Feature HC-USR-011 |
| Business Objective | Lets an admin replace compromised/default credentials for another user without requiring the user's cooperation. |
| Business Rule | BR-L-08 (`03_Business_Rules.md`) |
| Validation Rule | Temporary Password complexity, when provided (`04_Validation_Rules.md`, §A.2) |
| Priority | Medium |
| Dependency | HC-USR-001 (login redirect on forced-reset flag); HC-USR-010 (change flow clears the flag) |
| Automation Feasibility | Feasible, but requires an authenticated admin session first (see REQ-LOG-001 admin-login blocker, Q2) |
| Automation Complexity | Medium (multi-account setup: acting admin + target user) |
| Automation Priority | P2 |
| Risk | Depends transitively on AR-01 (admin 2FA blocker) to even reach this screen |
| Assumption | None beyond §A of `05_Assumptions.md` |
| Clarification Required | No new question beyond Q2 (already tracked) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Useful as a **setup/fixture** action (creating a forced-reset precondition for REQ-LOG-001 AC6 test), not only as a feature under test in its own right. |

### REQ-LOG-005

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-005 |
| Module | Login |
| Requirement Name | Request Password Reset Code (Forgot Password) |
| Requirement Description | Unauthenticated user requests a reset via username or registered email; system emails a 6-digit code (email only), valid 15 minutes, single-use. Identical confirmation message whether or not the account exists. Rate-limited. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.4 Password Management, Feature HC-USR-012 |
| Business Objective | Allows account recovery without admin intervention while preventing account enumeration. |
| Business Rule | BR-L-11, BR-L-12, BR-L-13 (`03_Business_Rules.md`) |
| Validation Rule | Username or Email required (`04_Validation_Rules.md`, §A.2) |
| Priority | Medium |
| Dependency | Email delivery via SMTP configuration owned by Admin Settings FRD HC-ADS-018 (**out of scope document**); continues into HC-USR-013 |
| Automation Feasibility | Partially Feasible — the request step and its neutral confirmation message are testable without email access; full loop blocked pending Q8 |
| Automation Complexity | Medium |
| Automation Priority | P3 |
| Risk | AR-03 (no test-mailbox strategy defined) |
| Assumption | SMTP is configured and functioning in the test environment (unverified — Admin Settings FRD is out of scope) |
| Clarification Required | Yes — Q8 (test-mailbox solution) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-LOG-006

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-006 |
| Module | Login |
| Requirement Name | Reset Password with Code |
| Requirement Description | User submits the emailed 6-digit code plus a new password; code validated for match/expiry/unused state; new password subject to complexity policy; on success the code is consumed and the user redirected to Login. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.4 Password Management, Feature HC-USR-013 |
| Business Objective | Completes the self-service account-recovery loop started by REQ-LOG-005. |
| Business Rule | BR-L-08, BR-L-12 (`03_Business_Rules.md`) |
| Validation Rule | Reset Code validity; New Password complexity; Confirm New Password match (`04_Validation_Rules.md`, §A.2) |
| Priority | Medium |
| Dependency | HC-USR-012 (code issuance); complexity policy (§3.3 of source FRD) |
| Automation Feasibility | Partially Feasible — negative-path (invalid/expired code) testable without email access; positive-path blocked pending Q8 |
| Automation Complexity | Medium |
| Automation Priority | P3 |
| Risk | AR-03 (same test-mailbox gap as REQ-LOG-005) |
| Assumption | None beyond §A of `05_Assumptions.md` |
| Clarification Required | Yes — Q8 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-LOG-007

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-007 |
| Module | Login |
| Requirement Name | Two-Factor Setup (Authenticator App) |
| Requirement Description | Staff member enrolls in TOTP-based 2FA: QR code generated, paired app confirmed with a 6-digit code, 8 single-use backup codes generated and shown exactly once (with download). Secret protected at rest, never redisplayed. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.5 Two-Factor Authentication, Feature HC-USR-014 |
| Business Objective | Enables the second-factor security control that REQ-LOG-008 subsequently enforces. |
| Business Rule | BR-L-14, BR-L-15 (`03_Business_Rules.md`) |
| Validation Rule | Confirmation Code must be current and valid (`04_Validation_Rules.md`, §A.2) |
| Priority | Medium |
| Dependency | HC-USR-015 (verification at login), HC-USR-016 (admin disable), HC-USR-017 (enforcement) |
| Automation Feasibility | Blocked — requires reading the QR-encoded TOTP secret programmatically to generate a valid confirmation code; no provisioning strategy defined |
| Automation Complexity | High |
| Automation Priority | P3 |
| Risk | AR-02 (no TOTP secret provisioning strategy) |
| Assumption | TOTP standard, 30-second step (§4 Assumptions of source FRD) |
| Clarification Required | Yes — Q2 (TOTP secret provisioning) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-LOG-008

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-008 |
| Module | Login |
| Requirement Name | Two-Factor Verify at Login |
| Requirement Description | After password verification, 2FA-enabled/enforced accounts must supply a current 6-digit TOTP code (±1 time-step tolerance) or an unused backup code. 5 consecutive failures locks the account for 15 minutes. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.5 Two-Factor Authentication, Feature HC-USR-015 |
| Business Objective | Enforces the second-factor security gate for high-privilege/enforced accounts, directly gating the default-ON admin role. |
| Business Rule | BR-L-04, BR-L-16, BR-L-17 (`03_Business_Rules.md`) |
| Validation Rule | Authentication Code validity; ≤5 consecutive failures (`04_Validation_Rules.md`, §A.2) |
| Priority | High |
| Dependency | HC-USR-001 (password step precedes this); HC-USR-014 (setup); lockout duration owned by Admin Settings FRD HC-ADS-024 (**out of scope document**) |
| Automation Feasibility | Blocked — same TOTP provisioning gap as REQ-LOG-007; this is the requirement that actually blocks admin-role login end-to-end (see REQ-LOG-001) |
| Automation Complexity | High |
| Automation Priority | P2 (high business priority, but currently blocked — see `06_Gaps_and_Risks.md` AR-01) |
| Risk | AR-01, AR-02 |
| Assumption | None beyond §A of `05_Assumptions.md` |
| Clarification Required | Yes — Q2 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Highest-impact blocked requirement in the Login module — resolving Q2 unblocks both this and admin-role coverage across the entire POC. |

### REQ-LOG-009

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-009 |
| Module | Login |
| Requirement Name | Two-Factor Disable (Admin Recovery) |
| Requirement Description | Admin clears a user's 2FA enrollment (e.g., lost device) so the user can log in with password only and re-enroll later. Requires confirmation; recorded in the audit trail. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.5 Two-Factor Authentication, Feature HC-USR-016 |
| Business Objective | Account-recovery path when a user loses their second-factor device. |
| Business Rule | Derived from feature description — no numbered BR-L item beyond general audit rule BR-X-01 (`03_Business_Rules.md`) |
| Validation Rule | None field-level — confirmation-gated action only |
| Priority | Low |
| Dependency | HC-USR-014 (re-enrolment), HC-USR-017 (forced re-enrolment when role-enforced), HC-USR-027 Write Event Audit Trail (**out of scope**, logging side-effect only) |
| Automation Feasibility | Feasible only if a 2FA-enabled test account already exists — indirectly blocked by the same gap as REQ-LOG-007 |
| Automation Complexity | Medium |
| Automation Priority | P3 |
| Risk | Inherits AR-02 indirectly (needs a 2FA-enrolled account to disable) |
| Assumption | None beyond §A of `05_Assumptions.md` |
| Clarification Required | No new question beyond Q2 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-LOG-010

| Field | Value |
|---|---|
| Requirement ID | REQ-LOG-010 |
| Module | Login |
| Requirement Name | Two-Factor Enforcement by Role |
| Requirement Description | Admin toggles mandatory 2FA per system role (admin/doctor/receptionist/pharmacist). Default: admin ON, others OFF. Users of an enforced role without 2FA are walked through setup at next login. |
| Requirement Source | User Management & Authentication FRD v3.0 |
| Source Page/Section | §2.5 Two-Factor Authentication, Feature HC-USR-017 |
| Business Objective | Lets the hospital mandate stronger authentication for selected roles hospital-wide. |
| Business Rule | BR-L-18 (`03_Business_Rules.md`) |
| Validation Rule | None field-level — toggle-per-role UI, no format validation |
| Priority | Medium |
| Dependency | HC-USR-014/015 (setup and verify); storage reconciliation with Admin Settings FRD HC-ADS-024 (**out of scope document**) — the source FRD itself flags a possible divergence between the two config stores as a defect if it occurs |
| Automation Feasibility | Feasible for the toggle action itself; verifying the *effect* (forced setup at next login) requires a second test account and login cycle |
| Automation Complexity | Medium |
| Automation Priority | P3 |
| Risk | AM-03 (role-model conflict — this feature explicitly assumes exactly 4 fixed roles, which is the disputed fact) |
| Assumption | Exactly four system roles exist (§1.4 of source FRD) — **directly implicated by the Role Matrix conflict, AM-03** |
| Clarification Required | Yes — Q4 (role-model conflict) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

---

## B. Patient Registration Module

### REQ-PAT-001

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-001 |
| Module | Patient Registration |
| Requirement Name | Full Patient Registration (7-Step Wizard) |
| Requirement Description | Structured 7-step wizard (Patient Info → Contact & Address → Category & Schemes → Medical Info → ID & Insurance → Consent & MLC → Review & Confirm). Only First Name, Last Name, and Phone are mandatory across all steps. On confirm: UHID auto-generated (`PAT+YYMM+XXXX`), QR code shown, event audit-logged. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.1 Patient Registration, Feature HC-PAT-001 |
| Business Objective | Primary, most complete entry point of the patient journey — every downstream clinical/financial record traces back to the UHID created here. |
| Business Rule | BR-P-01, BR-P-02, BR-P-03, BR-P-04, BR-P-09, BR-P-10, BR-P-11, BR-P-12, BR-P-17, BR-P-18 (`03_Business_Rules.md`) |
| Validation Rule | First/Last Name required; Phone format; Email format; PIN Code format; Aadhaar format; duplicate-phone warning (`04_Validation_Rules.md`, §B.2) |
| Priority | High |
| Dependency | REQ-LOG-001 (Login is a hard precondition); HC-PAT-005 (QR on success); HC-PAT-017 (duplicate check, in scope as REQ-PAT-006); HC-PAT-022–024 (ABHA in Step 5, in scope as REQ-PAT-007 to 009) |
| Automation Feasibility | Feasible |
| Automation Complexity | High (7-step multi-field wizard with conditional sections) |
| Automation Priority | P1 |
| Risk | AR-06 (permanent test data — soft delete only), AR-07 (UHID/QR must be captured dynamically), AR-08 (canonical-vs-alias known gap, NFR-007) |
| Assumption | UHID format is system-generated and never user-entered (Assumptions §B.1 of `05_Assumptions.md`); photo capture (Step 1) is a non-functional placeholder |
| Clarification Required | Yes — Q6 (does "Patient Registration" scope include this feature's full field set as described, confirmed already) and Q3/MR-08 (canonical vs. alias creation path) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Largest single requirement in scope — likely candidate for the most Test Scenarios/Cases in Phase 3. |

### REQ-PAT-002

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-002 |
| Module | Patient Registration |
| Requirement Name | Quick Patient Registration |
| Requirement Description | Minimal dialog (First Name, Last Name, Phone mandatory) launched inline from Appointment Booking or Check-In (both out of scope as calling contexts, but the dialog itself is in scope). Duplicate phone **blocks** creation outright and returns the existing patient. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.1 Patient Registration, Feature HC-PAT-002 |
| Business Objective | Fast-path registration that doesn't interrupt a booking/check-in workflow. |
| Business Rule | BR-P-01, BR-P-04, BR-P-05, BR-P-08, BR-P-09, BR-P-10 (`03_Business_Rules.md`) |
| Validation Rule | First/Last Name/Phone required; Phone format; duplicate-phone block; facility-assignment check (`04_Validation_Rules.md`, §B.2) |
| Priority | High |
| Dependency | REQ-LOG-001 (Login); called from Appointments/Queue & Check-in (**out of scope modules** — the calling screens are not tested, only the dialog itself); contrasts directly with REQ-PAT-006 (duplicate check) |
| Automation Feasibility | Feasible — note: the FRD describes this dialog as launched from Appointments/Check-In screens (out of scope); a direct entry point for isolated testing needs to be confirmed in Test Plan |
| Automation Complexity | Low |
| Automation Priority | P1 |
| Risk | AR-06, AR-07 |
| Assumption | None beyond §B.1 of `05_Assumptions.md` |
| Clarification Required | No new question — inherits Q6 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Test Plan will need to determine how to reach this dialog without exercising the out-of-scope Appointments/Check-In modules end-to-end. |

### REQ-PAT-003

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-003 |
| Module | Patient Registration |
| Requirement Name | Emergency Patient Registration |
| Requirement Description | Rapid-capture dialog for unknown/urgent patients: Patient Name (mandatory, "Unknown" accepted) and Chief Complaint (mandatory) are the only required fields; phone optional. Captures triage priority (default P2), mode of arrival, emergency type, MLC/FIR. Sets Patient Type = "Emergency". |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.1 Patient Registration, Feature HC-PAT-003 |
| Business Objective | Enables immediate care documentation for unknown/urgent patients without blocking on full registration; supports medico-legal (MLC) compliance capture. |
| Business Rule | BR-P-01, BR-P-06, BR-P-07, BR-P-08, BR-P-09, BR-P-10 (`03_Business_Rules.md`) |
| Validation Rule | Patient Name required ("Unknown" accepted); Chief Complaint required; facility-assignment check (`04_Validation_Rules.md`, §B.2) |
| Priority | High |
| Dependency | REQ-LOG-001 (Login); feeds REQ-PAT-004 (Active Emergencies List) |
| Automation Feasibility | Feasible |
| Automation Complexity | Medium (conditional MLC fields, multiple dropdowns) |
| Automation Priority | P1 |
| Risk | AR-06, AR-07 |
| Assumption | Emergency registrations intentionally capture minimum data only (Assumptions §B.2, item 7 of `05_Assumptions.md`) |
| Clarification Required | No new question — inherits Q6 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-PAT-004

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-004 |
| Module | Patient Registration |
| Requirement Name | Active Emergencies List |
| Requirement Description | Live, auto-refreshing, read-only list of patients from the emergency flow whose episode is still open. Admin sees all facilities; other roles see their own facility only. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.1 Patient Registration, Feature HC-PAT-004 |
| Business Objective | Gives care teams live visibility of who needs urgent attention — the verification counterpart to REQ-PAT-003. |
| Business Rule | Facility-scoping rule per NFR-003 of source FRD (referenced generally; no dedicated BR-P item) |
| Validation Rule | None field-level — read-only view |
| Priority | Low |
| Dependency | Populated entirely by REQ-PAT-003 |
| Automation Feasibility | Feasible |
| Automation Complexity | Low |
| Automation Priority | P3 |
| Risk | Solution Architecture confirms polling, not push (§C.2 of `05_Assumptions.md`) — auto-refresh assertions must account for polling interval, not assume instant update |
| Assumption | None beyond §B.1 of `05_Assumptions.md` |
| Clarification Required | No new question — inherits Q6, Q10 (facility count) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Best exercised as a post-condition check immediately after a REQ-PAT-003 test, not as a fully independent scenario. |

### REQ-PAT-005

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-005 |
| Module | Patient Registration |
| Requirement Name | Patient QR Code — Generate, View, Download |
| Requirement Description | QR encoding the patient's UHID is auto-generated on successful registration (full or quick) and shown on the success screen; retrievable later on demand. Print and Download actions. Explicit "QR not available" state when none exists. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.1 Patient Registration, Feature HC-PAT-005 |
| Business Objective | Enables physical ID-card printing and fast QR-based patient lookup at future check-in. |
| Business Rule | BR-P-12 (`03_Business_Rules.md`) |
| Validation Rule | None field-level — generation/display/download behavior only |
| Priority | Medium |
| Dependency | Patient must already exist (REQ-PAT-001 or REQ-PAT-002); consumed by out-of-scope QR-scan-locate (HC-PAT-008) and Appointments check-in |
| Automation Feasibility | Feasible for presence/download-triggers-file assertions; **not feasible** to assert the QR's decoded payload without a QR-decoding utility in the test stack |
| Automation Complexity | Low–Medium |
| Automation Priority | P2 |
| Risk | Decoding the QR image to verify it encodes the correct UHID (rather than just asserting an image/file exists) is a tooling decision for Test Plan |
| Assumption | None beyond §B.1 of `05_Assumptions.md` |
| Clarification Required | No new question — inherits Q6, Q12 (whether a QR-decode-level assertion is expected) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-PAT-006

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-006 |
| Module | Patient Registration |
| Requirement Name | Pre-Registration Duplicate Check |
| Requirement Description | Fires when the user leaves the Phone field during the Full Wizard (Step 2). On a phone match within the facility, shows a warning banner with three options: View Existing Record, Merge Records, Continue Anyway. Does not block. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.4 Duplicate Detection and Patient Merge, Feature HC-PAT-017 |
| Business Objective | Prevents accidental duplicate patient records at the point of entry while still allowing legitimate shared-phone cases (e.g., family members). |
| Business Rule | BR-P-04 (`03_Business_Rules.md`) — the central differential rule of this module |
| Validation Rule | Duplicate-phone warning message (`04_Validation_Rules.md`, §B.2) |
| Priority | High |
| Dependency | REQ-PAT-001 (fires inside the wizard); REQ-PAT-002 (same underlying check, blocking variant); "Merge Records" option leads to HC-PAT-019 (**out of scope**) |
| Automation Feasibility | Feasible, but requires a **deliberately pre-seeded** existing patient with a known phone number to reliably trigger — cannot rely on incidental data |
| Automation Complexity | Medium |
| Automation Priority | P1 (high business value — the warn-vs-block distinction, BR-P-04, is the single easiest business rule to implement backwards) |
| Risk | AR-06 (test-created duplicates could compound over repeated runs if not isolated) |
| Assumption | Duplicate detection triggers on exact phone match within facility only (no fuzzy matching described for this specific inline check) |
| Clarification Required | No new question — inherits Q9 (test data isolation, since this requirement specifically needs seeded duplicate data) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | "View Existing Record" and "Merge Records" options navigate toward out-of-scope screens (Patient Details Drawer, Merge workflow) — Test Plan should decide whether to verify only that the banner appears/the button is clickable, or follow through (which would touch out-of-scope UI). |

### REQ-PAT-007

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-007 |
| Module | Patient Registration |
| Requirement Name | ABHA OTP Generation |
| Requirement Description | Within Step 5 of the wizard, staff enter the patient's ABDM-registered mobile and trigger an OTP, delivered only to the patient's phone (never visible to staff). Requires the ABHA feature flag ON and ABDM credentials configured. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.5 ABHA-Assisted Registration, Feature HC-PAT-022 |
| Business Objective | Begins linking the patient's national health ID (Ayushman Bharat) during registration. |
| Business Rule | BR-P-13, BR-P-14 (`03_Business_Rules.md`) |
| Validation Rule | Mobile number format (`04_Validation_Rules.md`, §B.2) |
| Priority | Low (feature-flagged off by default) |
| Dependency | Gated by Admin Settings FRD HC-ADS-016 (feature flag) and HC-ADS-012 (ABDM credentials) — **both out of scope documents**; continues into REQ-PAT-008 |
| Automation Feasibility | Blocked — feature is off by default and depends on external ABDM registry availability |
| Automation Complexity | High |
| Automation Priority | P3 / candidate for deferral |
| Risk | AR-09 (external ABDM dependency, no sandbox/mock confirmed) |
| Assumption | ABHA is off by default (Assumptions §B.2, item 5 of `05_Assumptions.md`) |
| Clarification Required | Yes — Q7 (in scope for first pass or deferred?) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-PAT-008

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-008 |
| Module | Patient Registration |
| Requirement Name | ABHA OTP Verification and Profile Fetch |
| Requirement Description | Staff enter the 6-digit OTP read out by the patient. On success, ABHA Number and Address auto-populate Step 5. Wrong/expired OTP shows a clear error with retry/regenerate. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.5 ABHA-Assisted Registration, Feature HC-PAT-023 |
| Business Objective | Completes ABHA linkage begun in REQ-PAT-007, verified national health data auto-fills the registration. |
| Business Rule | BR-P-15 (`03_Business_Rules.md`) |
| Validation Rule | OTP validity (6 digits, matches transaction) (`04_Validation_Rules.md`, §B.2) |
| Priority | Low |
| Dependency | REQ-PAT-007 |
| Automation Feasibility | Blocked — same dependency chain as REQ-PAT-007; additionally requires reading the OTP from a simulated "patient" channel, which has no defined mechanism |
| Automation Complexity | High |
| Automation Priority | P3 / candidate for deferral |
| Risk | AR-09 |
| Assumption | None beyond §B.2 of `05_Assumptions.md` |
| Clarification Required | Yes — Q7 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | None |

### REQ-PAT-009

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-009 |
| Module | Patient Registration |
| Requirement Name | ABHA Search and Registration Pre-Fill |
| Requirement Description | Staff search the national registry by ABHA ID/address; a match pre-fills name/DOB/gender into the registration form (editable, not locked). No-match shows a clear message. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.5 ABHA-Assisted Registration, Feature HC-PAT-024 |
| Business Objective | Alternative ABHA linkage path not requiring an OTP round-trip — speeds registration for patients who already know their ABHA ID. |
| Business Rule | BR-P-13, BR-P-15 (`03_Business_Rules.md`) |
| Validation Rule | Query (ABHA ID/Address) required; no-match message (`04_Validation_Rules.md`, §B.2) |
| Priority | Low |
| Dependency | Gated as per REQ-PAT-007; feeds REQ-PAT-001 (Step 5) |
| Automation Feasibility | Blocked — same external registry dependency as REQ-PAT-007/008 |
| Automation Complexity | High |
| Automation Priority | P3 / candidate for deferral |
| Risk | AR-09 |
| Assumption | None beyond §B.2 of `05_Assumptions.md` |
| Clarification Required | Yes — Q7 |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | The "no-match" negative path may be testable even without ABDM sandbox access, since it doesn't require a real successful registry response — worth revisiting once Q7 is answered. |

### REQ-PAT-010

| Field | Value |
|---|---|
| Requirement ID | REQ-PAT-010 |
| Module | Patient Registration |
| Requirement Name | ABHA Link and Profile Fetch for Existing Patient |
| Requirement Description | For an already-registered patient, admin/senior reception staff fetch and link a verified ABHA profile after the fact. |
| Requirement Source | Patient Module FRD v1.0 |
| Source Page/Section | §2.5 ABHA-Assisted Registration, Feature HC-PAT-025 |
| Business Objective | Keeps a patient's national health ID connected even if it wasn't linked at initial registration. |
| Business Rule | BR-P-13 (`03_Business_Rules.md`) |
| Validation Rule | None field-level beyond registry-unreachable error message |
| Priority | Low |
| Dependency | Gated as per REQ-PAT-007; complements out-of-scope HC-PAT-015 (Edit Patient) |
| Automation Feasibility | Blocked — same external registry dependency; additionally requires an *existing* patient record as precondition |
| Automation Complexity | High |
| Automation Priority | P3 / candidate for deferral |
| Risk | AR-09; AM-06 (this feature acts on existing records, not new registration — its inclusion in "Patient Registration" scope is itself flagged as ambiguous) |
| Assumption | None beyond §B.2 of `05_Assumptions.md` |
| Clarification Required | Yes — Q7, and Q6/AM-06 (whether this feature belongs in "Patient Registration" scope at all, since it acts on existing patients) |
| Future Scenario ID | Not Yet Assigned |
| Future Test Case ID | Not Yet Assigned |
| Future Script ID | Not Yet Assigned |
| Execution Status | Not Started |
| Defect Reference | Not Available |
| Comments | Strongest candidate in the entire matrix for being re-scoped out of "Patient Registration" — recommend confirming with Q6/AM-06 before Test Plan. |

---

## C. Requirement Conflicts

### CONFLICT-001 — Custom Role Support

| Field | Value |
|---|---|
| Conflicting Requirements | REQ-LOG-001, REQ-LOG-010 (and, transitively, the role-based facility-scoping behavior underlying every requirement in this matrix) |
| Source A | User Management & Authentication FRD v3.0, §1.4: *"Exactly four system roles exist... custom role creation is not supported."* (repeated in the FRD's own Assumptions §4, item 1) |
| Source B | Role Matrix and Permission Catalog document, §3: *"Any number of roles can exist — four are seeded by default... and hospital admins can create custom roles through the roles.py router."* |
| Nature of Conflict | Direct factual contradiction about whether the live system supports custom roles beyond the four seeded ones. |
| Impact | Determines whether the test-user matrix needs to plan only for 4 fixed roles, or must also handle custom-role creation/assignment as a first-class scenario. |
| Resolution Status | Unresolved — tracked as Q4 in `Output/02_RequirementAnalysis/07_Client_Questions.md` |

*(No other genuine cross-document conflicts were identified. Items previously logged as "ambiguities" in `06_Gaps_and_Risks.md` §B that concern a single document's own internal scope wording — not two documents contradicting each other — are represented above as per-requirement "Clarification Required" entries, not as Requirement Conflicts.)*
