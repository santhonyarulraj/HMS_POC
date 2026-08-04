# 02 — Functional Requirements

**Scope:** Login (10 features) + Patient Registration (10 features) — per the interpreted scope in `01_Business_Summary.md`, Section 2. Feature IDs are preserved from the source FRDs for full traceability.

---

## A. Login Module (Source: User Management & Authentication FRD v3.0)

### A.1 Authentication

**HC-USR-001 | Login**
Staff authenticate with username + password. On success: secure server-managed session established, role-based permission map loaded. Blocks deactivated accounts. Routes forced-reset accounts to Change Password before dashboard. Triggers 2FA verification if enabled/enforced. Every attempt (success or failure) logged to the audit trail. Rate-limited to 5 attempts/minute per source address. Generic error message on failure (does not reveal which field was wrong).

**HC-USR-002 | Logout**
Sign-out available from every screen's user menu. Invalidates the session server-side immediately (blocklist); local session/permission data cleared; user returned to Login screen. A replayed invalidated session is refused.

### A.2 Password Management

**HC-USR-010 | Change Own Password**
Any authenticated staff member changes their own password. Requires current password. New password validated against complexity policy. Clears any forced-reset flag on success. **Known gap:** does not invalidate the user's other active sessions.

**HC-USR-011 | Admin Force Password Reset**
Admin sets a forced-reset flag on a target account (routes them to Change Password at next login) and/or sets a temporary password directly. Temporary password subject to the same complexity policy; never displayed after save.

**HC-USR-012 | Request Password Reset Code (Forgot Password)**
Unauthenticated user requests a reset via username or registered email. System emails a 6-digit code (email only — no SMS/WhatsApp/link). Valid 15 minutes, single-use. Identical confirmation message regardless of whether the account exists (no account enumeration). Rate-limited.

**HC-USR-013 | Reset Password with Code**
User submits the emailed 6-digit code + new password. Code validated for match/expiry/unused state. New password subject to complexity policy. On success, code consumed and user redirected to Login.

### A.3 Two-Factor Authentication

**HC-USR-014 | Two-Factor Setup (Authenticator App)**
Staff member enrolls in TOTP-based 2FA: QR code generated, paired app confirmed with a 6-digit code, 8 single-use backup codes generated and shown exactly once (with download option). Secret protected at rest, never redisplayed.

**HC-USR-015 | Two-Factor Verify at Login**
After password verification, 2FA-enabled/enforced accounts must supply a current 6-digit TOTP code (±1 time-step tolerance) or an unused backup code (consumed on use). 5 consecutive failures → 15-minute account lockout.

**HC-USR-016 | Two-Factor Disable (Admin Recovery)**
Admin clears a user's 2FA enrollment (e.g., after lost device) so the user can log in with password only and re-enroll later. Requires confirmation; recorded in the audit trail.

**HC-USR-017 | Two-Factor Enforcement by Role**
Admin toggles mandatory 2FA per system role (admin/doctor/receptionist/pharmacist). Default: admin ON, others OFF. Users of an enforced role without 2FA are walked through setup at next login before reaching the dashboard.

---

## B. Patient Registration Module (Source: Patient Module FRD v1.0)

### B.1 Registration Flows

**HC-PAT-001 | Full Patient Registration (7-Step Wizard)**
Structured wizard: Patient Info → Contact & Address → Category & Schemes → Medical Info → ID & Insurance → Consent & MLC → Review & Confirm. Only First Name, Last Name, and Phone are mandatory across the entire wizard — every other field (DOB, gender, category, blood group, Aadhaar, ABHA, consents, MLC, etc.) is optional. Cannot advance past a step with invalid mandatory data. On confirm: UHID auto-generated (`PAT+YYMM+XXXX`), QR code displayed, event audit-logged. Cancel discards unsaved data after confirmation.

**HC-PAT-002 | Quick Patient Registration**
Minimal dialog (First Name, Last Name, Phone mandatory; DOB/Gender optional) launched inline from Appointment Booking or Check-In. **Duplicate phone BLOCKS creation outright** (contrast with the wizard's warn-and-override behavior) and returns the existing patient for selection instead. Fails clearly if the acting user has no facility assignment.

**HC-PAT-003 | Emergency Patient Registration**
Rapid-capture dialog for unknown/urgent patients: Patient Name (mandatory, "Unknown" acceptable) and Chief Complaint (mandatory) are the only required fields; phone is optional. Captures triage priority (default P2), mode of arrival, emergency type, MLC/FIR. Sets Patient Type = "Emergency"; patient appears highlighted at the top of the list.

**HC-PAT-004 | Active Emergencies List**
Live, auto-refreshing list of patients from the emergency flow whose episode is still open — name, UHID, triage priority, type, mode of arrival, assigned doctor, elapsed time. Admin sees all facilities; other roles see their own facility only. Read-only.

**HC-PAT-005 | Patient QR Code — Generate, View, Download**
QR encoding the patient's UHID is auto-generated on successful registration (full or quick) and shown on the success screen. Retrievable later on demand from the patient row/drawer (loaded only when requested). Print and Download actions. Explicit "QR not available" state when none exists.

### B.2 Inline Registration Safeguard

**HC-PAT-017 | Pre-Registration Duplicate Check**
Fires when the user leaves the Phone field during the full wizard (Step 2). On a phone match within the facility, shows a warning banner (existing name + UHID) with three options: View Existing Record, Merge Records, Continue Anyway. Does **not** block — legitimate for family members sharing a phone. Silent when no match. (Contrast: the same underlying check *blocks* in Quick Registration, HC-PAT-002.)

### B.3 ABHA-Assisted Registration (Feature-Flagged, Off by Default)

**HC-PAT-022 | ABHA OTP Generation**
Within Step 5 of the wizard, staff enter the patient's ABDM-registered mobile and trigger an OTP. The OTP is delivered only to the patient's phone — never visible to staff. Requires the ABHA feature flag ON and ABDM credentials configured (both owned by the out-of-scope Admin Settings FRD).

**HC-PAT-023 | ABHA OTP Verification and Profile Fetch**
Staff enter the 6-digit OTP read out by the patient. On success, ABHA Number and Address auto-populate Step 5. Wrong/expired OTP shows a clear error with retry/regenerate.

**HC-PAT-024 | ABHA Search and Registration Pre-Fill**
Staff search the national registry by ABHA ID/address; a match pre-fills name/DOB/gender into the registration form (editable, not locked). No-match shows a clear message.

**HC-PAT-025 | ABHA Link and Profile Fetch for Existing Patient**
For an *already-registered* patient, admin/senior reception staff can fetch and link a verified ABHA profile after the fact. *(Included here for completeness since it shares feature-flag gating with 022–024, though strictly it acts on existing records — flag if this should move to the out-of-scope list.)*

---

## C. Requirements Explicitly Excluded from This Analysis

Per the scope interpretation in `01_Business_Summary.md`, the following in-FRD features were **not** analyzed and are not represented anywhere in this Requirement Analysis output: HC-USR-003 to 009, 018 to 029 (Staff Account Management, Session Activity, RBAC Matrix, Facility/Department Management, Leave Allotments, Audit Trail); HC-PAT-006 to 016, 018 to 021 (Directory/Search/Filter/Sort, Banner, Details Drawer, Visit History, Edit, Soft Delete, Duplicate Merge workflow). If Requirement Analysis needs to cover any of these, please say so explicitly — they will need a separate pass.
