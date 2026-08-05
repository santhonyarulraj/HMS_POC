# 02 — Approved Project Scope

**Baseline Version:** RB-1.0

This is the scope boundary carried forward from Requirement Analysis (`Output/02_RequirementAnalysis/01_Business_Summary.md`, Section 2) and the RTM, frozen as-is for this baseline. **Note:** the boundary below reflects this project's working interpretation, confirmed at the Documentation Audit / Requirement Analysis / RTM review stages — but two scope-boundary questions (Q5, Q6) remain formally open with the client and are carried into `08_NonBlocking_Clarifications.md`. This baseline freezes the interpretation as the basis for Test Planning; it does not claim the boundary itself is beyond question.

---

## 1. In-Scope Modules

| Module | Requirement Count |
|---|---|
| Login | 10 |
| Patient Registration | 10 |
| **Total** | **20** |

## 2. Login — In Scope

Authentication experience only: sign in/out, password self-service, two-factor authentication.

- REQ-LOG-001 Login
- REQ-LOG-002 Logout
- REQ-LOG-003 Change Own Password
- REQ-LOG-004 Admin Force Password Reset
- REQ-LOG-005 Request Password Reset Code (Forgot Password)
- REQ-LOG-006 Reset Password with Code
- REQ-LOG-007 Two-Factor Setup (Authenticator App)
- REQ-LOG-008 Two-Factor Verify at Login
- REQ-LOG-009 Two-Factor Disable (Admin Recovery)
- REQ-LOG-010 Two-Factor Enforcement by Role

**Explicitly excluded** (same source FRD, not carried into this baseline): Staff Account Management, Session Activity view, RBAC Permission Matrix administration, Facility & Department Management, Leave Allotments, and Audit Trail (19 features of the User Management & Authentication FRD).

## 3. Patient Registration — In Scope

The three registration flows plus their immediate, inline consequences.

- REQ-PAT-001 Full Patient Registration (7-Step Wizard)
- REQ-PAT-002 Quick Patient Registration
- REQ-PAT-003 Emergency Patient Registration
- REQ-PAT-004 Active Emergencies List
- REQ-PAT-005 Patient QR Code — Generate, View, Download
- REQ-PAT-006 Pre-Registration Duplicate Check
- REQ-PAT-007 ABHA OTP Generation
- REQ-PAT-008 ABHA OTP Verification and Profile Fetch
- REQ-PAT-009 ABHA Search and Registration Pre-Fill
- REQ-PAT-010 ABHA Link and Profile Fetch for Existing Patient

**Explicitly excluded** (same source FRD, not carried into this baseline): Patient Directory/Search/Filter/Sort, Post-Registration Banner, Details Drawer, Visit History, Edit Patient Record, Soft Delete, and the standalone admin Duplicate Candidates/Merge/Dismiss/History workflow (15 features of the Patient Module FRD).

## 4. Out of Scope — All Other HMS Modules

Unchanged from the original POC scope lock: Appointments, Billing, EMR, Admin Settings, Queue & Check-in, Lab, Radiology, Pharmacy, IPD, OT, Insurance/TPA, Doctor Portal, Patient Portal, Advance Booking, Notifications, AI Assistant, HR/Payroll, Inventory/Purchase, MIS/Analytics, Telemedicine, and Control Plane (SaaS).

## 5. Open Scope Questions (Carried Forward, Not Resolved by This Baseline)

| Question | Where Tracked |
|---|---|
| Q5 — Confirm the "Login" boundary above | `08_NonBlocking_Clarifications.md` |
| Q6 — Confirm the "Patient Registration" boundary above | `08_NonBlocking_Clarifications.md` |
| Q7 — Confirm whether ABHA (REQ-PAT-007–010) is in scope for the first automation pass or deferred | `08_NonBlocking_Clarifications.md` |
| AM-06 — Whether REQ-PAT-010 belongs in "Patient Registration" at all (it acts on existing patients, not new registration) | `09_Requirement_Conflicts.md` is for true conflicts only; this ambiguity is tracked against REQ-PAT-010 in the RTM and repeated in `08_NonBlocking_Clarifications.md` |

This baseline includes all 20 requirements as currently interpreted; if the client's answers to Q5–Q7 change the boundary, that change is a baseline revision (RB-1.1), not a retroactive edit to RB-1.0.
