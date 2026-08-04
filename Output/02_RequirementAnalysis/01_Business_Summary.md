# 01 — Business Summary

**Project:** HealixCare HMS Automation POC
**Phase:** Requirement Analysis
**Scope:** Login (Authentication) + Patient Registration only. All other modules are explicitly out of scope.

---

## 1. Pre-Read Verification

Before analysis began, the following were confirmed:

| Check | Result |
|---|---|
| Documentation Audit outputs available | ✅ `Output/01_Document_Inventory.md` through `04_Required_Documents_For_POC.md` present and reviewed |
| Login source document accessible | ✅ `RequiementDoucments/Phase1/UserManagement&Authentication/HealixCare_FRD_UserManagement_Auth_v3_0.docx` — read in full (29 features, all tables extracted) |
| Patient Registration source document accessible | ✅ `RequiementDoucments/Phase1/Patient/HealixCare_FRD_Patient_Module_v1_0.docx` — read in full (25 features, all tables extracted) |
| Supporting document — Role Matrix and Permission Catalog | ✅ Read in full (needed for actors/permissions) |
| Supporting document — Solution Architecture Document | ✅ Read in full (needed for system/technical dependencies) |

No required document was missing or inaccessible. **No other module's documents were read or analyzed**, per the approved scope lock.

---

## 2. Scope Interpretation — Flagged for Your Confirmation

Both source FRDs cover more ground than the words "Login" and "Patient Registration" strictly denote. Rather than guess, this analysis draws an explicit line and asks you to confirm it (see `07_Client_Questions.md`, Q1 and Q2). Everything in this Requirement Analysis is scoped to the **left-hand column** below only.

### 2.1 Login — Interpreted Scope

| In Scope (analyzed) | Out of Scope (not analyzed — flagged, not assumed) |
|---|---|
| HC-USR-001 Login | HC-USR-003 to 008 Staff Account Management |
| HC-USR-002 Logout | HC-USR-009 View Login Activity Summary |
| HC-USR-010 Change Own Password | HC-USR-018 Role Permission Matrix (view/edit) |
| HC-USR-011 Admin Force Password Reset | HC-USR-019 to 024 Facility & Department Management |
| HC-USR-012 Request Password Reset Code | HC-USR-025 Configure Leave Allotments |
| HC-USR-013 Reset Password with Code | HC-USR-026 to 029 Audit Trail (Login, Write Event, Record/User History, CSV Export) |
| HC-USR-014 Two-Factor Setup | |
| HC-USR-015 Two-Factor Verify at Login | |
| HC-USR-016 Two-Factor Disable (Admin Recovery) | |
| HC-USR-017 Two-Factor Enforcement by Role | |

**Rationale:** "Login" most naturally maps to the authentication experience every staff member goes through (sign in, sign out, password self-service, 2FA) — not the admin back-office console for managing *other* users' accounts, facilities, or audit logs, which is a materially different (and much larger) surface. This interpretation is stated explicitly, not assumed silently — please confirm or correct it.

### 2.2 Patient Registration — Interpreted Scope

| In Scope (analyzed) | Out of Scope (not analyzed — flagged, not assumed) |
|---|---|
| HC-PAT-001 Full Patient Registration (7-Step Wizard) | HC-PAT-006 to 011 Patient Directory, Search, Filter, Sort |
| HC-PAT-002 Quick Patient Registration | HC-PAT-012 to 014 Banner, Details Drawer, Visit History |
| HC-PAT-003 Emergency Patient Registration | HC-PAT-015 Edit Patient Record |
| HC-PAT-004 Active Emergencies List | HC-PAT-016 Soft Delete Patient Record |
| HC-PAT-005 Patient QR Code (Generate/View/Download) | HC-PAT-018 to 021 Duplicate Candidates List, Merge, Dismiss, Merge History |
| HC-PAT-017 Pre-Registration Duplicate Check | |
| HC-PAT-022 to 025 ABHA-Assisted Registration | |

**Rationale:** HC-PAT-001–005 are the four registration flows plus their immediate on-screen consequence (QR code). HC-PAT-017 (duplicate warning) is included because it fires *inline*, mid-registration, in Step 2 of the wizard — it is part of the registration transaction, not a separate workflow. HC-PAT-022–025 (ABHA) are included because they are invoked *inline*, inside Step 5 of the same wizard. Directory/search, edit, delete, and the standalone admin-side duplicate-*merge* workflow are **post-registration** capabilities on already-existing records — a different use case from registering a new patient — and are excluded pending your confirmation.

---

## 3. Business Objective

HealixCare HMS already exists as a feature-complete, code-verified Hospital Management System. The business objective of **this POC** is not to specify new functionality but to **prove that Playwright + TypeScript automation can reliably validate the two most foundational, highest-traffic flows in the system**:

1. **Login** — the precondition for every other action any staff member takes in the system, across all four roles (admin, doctor, receptionist, pharmacist).
2. **Patient Registration** — the entry point of the entire patient journey; every downstream clinical and financial record (appointments, EMR, billing, lab, pharmacy) traces back to a UHID created here.

Both modules are marked **"Complete" maturity** in the HMS Scope and Module Roadmap (per the Documentation Audit), making them the lowest-risk starting point for establishing automation patterns (Page Object Model, test-user matrix, CI wiring) that later phases can extend to other modules.

---

## 4. End-to-End Business Workflow (In-Scope Portion)

```
[Staff opens application]
        │
        ▼
  LOGIN (HC-USR-001)
   ├─ username + password
   ├─ [if forced-reset flag] → Change Password (HC-USR-010) → retry login
   ├─ [if 2FA enabled/enforced] → Two-Factor Verify (HC-USR-015)
   │      ├─ authenticator code, or
   │      └─ backup code (single-use)
   └─ → Dashboard (session established)
        │
        ▼
  [Receptionist/Admin navigates to Patients]
        │
        ▼
  PATIENT REGISTRATION — one of three entry flows:
   ├─ Full 7-Step Wizard (HC-PAT-001)             — complete demographic/medical/consent capture
   │    ├─ Step 2: duplicate-phone WARNING (HC-PAT-017) → Continue Anyway / View Existing / Merge
   │    └─ Step 5: optional ABHA OTP verify / search-prefill (HC-PAT-022–025)
   ├─ Quick Registration (HC-PAT-002)              — name + phone only, inline during booking/check-in
   │    └─ duplicate-phone BLOCKS outright (different rule than the wizard)
   └─ Emergency Registration (HC-PAT-003)          — name + chief complaint only, triage capture
        │
        ▼
  On success: UHID auto-generated (PAT+YYMM+XXXX) → QR Code shown/printable (HC-PAT-005)
        │
        ▼
  [Patient now exists — Appointments/Billing/EMR can proceed — out of scope for this POC]
        │
        ▼
  LOGOUT (HC-USR-002) — session invalidated server-side
```

---

## 5. Actors

| Actor | Role in Login | Role in Patient Registration |
|---|---|---|
| **Admin** | Logs in like any staff member; also performs Force Password Reset (HC-USR-011) on other accounts and can Disable 2FA for recovery (HC-USR-016, out of core scope) | Can perform all registration flows; admin scope spans all facilities |
| **Doctor** | Logs in; subject to 2FA enforcement (default OFF for this role) | Not a typical actor for registration (clinical role) — no FRD restriction found preventing it, but no use case describes it either |
| **Receptionist** | Logs in; subject to 2FA enforcement (default OFF) | **Primary actor** for all three registration flows (explicitly named in every HC-PAT-001/002/003 user story) |
| **Pharmacist** | Logs in; subject to 2FA enforcement (default OFF) | Not described as a registration actor in any use case |
| **ER Nurse** | Logs in (as whichever system role their account maps to — the FRD does not define a distinct "nurse" login role) | **Named actor** for Emergency Registration (HC-PAT-003) specifically |
| **Patient** (external, unauthenticated) | Not a system actor for Login — receives the ABHA OTP on their own phone during registration (HC-PAT-022/023), but never logs into the staff-facing system | Subject of the registration record — not a data-entry actor |
| **System / Server** | Authenticates credentials, issues/validates JWT session, enforces rate limits and lockouts, records login audit trail | Auto-generates UHID, auto-generates QR code, runs pre-registration duplicate check |
| **National ABDM/ABHA Registry** (external system) | Not involved in Login | External dependency for ABHA OTP delivery and profile search (HC-PAT-022–025) — feature-flagged, off by default |

**Note:** the FRD confirms **exactly four system roles** (admin, doctor, receptionist, pharmacist) with **no custom role creation** — see `06_Gaps_and_Risks.md` for a direct contradiction found against the Role Matrix and Permission Catalog document on this exact point.

---

## 6. Summary

Both modules are well-specified at the acceptance-criteria level, with quoted UI text, explicit validation tables, and documented "known gaps" that distinguish current (as-built) behavior from ideal (specified) behavior — a level of detail directly usable for Playwright assertions. The main open items are **scope-boundary confirmation** (Section 2 above) and a handful of **conflicts/gaps** detailed in `06_Gaps_and_Risks.md` and `07_Client_Questions.md`.
