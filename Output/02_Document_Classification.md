# 02 — Document Classification

**Project:** HealixCare HMS Automation POC
**Phase:** Documentation Audit

Each active document is classified into one primary category per the taxonomy given in the audit brief (Business, Functional, Workflow, Technical, Architecture, Project Management, UI/UX, API, Database, Other). Where a document meaningfully spans two categories, a secondary category is noted.

---

## Business

| Document | Notes |
|---|---|
| Product_Vision_Document.docx | Top of the documentation hierarchy — vision, problem statement, 3-year goals, non-goals. Everything else is evaluated against it. |
| HMS_Scope_and_Module_Roadmap.docx | As-built module inventory + maturity (Complete/Partial/Stub) + forward roadmap (Phase 2/Enterprise). Directly tells us which modules are safe to automate now. |
| Feature_Backlog_with_Priority.docx | Prioritized backlog derived from the roadmap gap analysis. *Secondary: Project Management.* |

## Technical (Security / Access Control)

| Document | Notes |
|---|---|
| Role_Matrix_and_Permission_Catalog.docx | RBAC model: seeded roles, permission flags (view/create/edit/delete), facility scoping, 2FA/session policy. *Secondary: Functional* — this is essential input for designing test users/roles for automation. |

## Project Management

| Document | Notes |
|---|---|
| HealixCare_HMS_Project_Management_Plan.docx | Milestones, backlog governance, RACI, risk register (pointer to companion workbook), cadence. Governance context, not implementation detail. |

## Architecture

| Document | Notes |
|---|---|
| Solution_Architecture_Document.docx | Confirmed tech stack, modular monolith structure (`backend/modules/` vs legacy `backend/routers/`), multi-tenancy (on-prem vs SaaS), auth/JWT, PHI encryption. References five other architecture documents (Application, Integration, Security & Compliance, Deployment, Data Architecture) — **none of which are present in the workspace** (see `04_Required_Documents_For_POC.md`, Missing Documents). |

## Functional (Functional Requirements Documents — Phase 1)

| Document | Module Covered |
|---|---|
| HealixCare_FRD_AdminSettings_v1_0.docx | Admin Settings (hospital profile, billing/GST config, security policy, notifications, feature flags, etc.) |
| HealixCare_FRD_Appointments_Module_v1_0.docx | Appointments (booking, lifecycle, calendars, availability, waitlist, reminders, overbooking, analytics) |
| HealixCare_FRD_Billing_Module_v1_0.docx | Billing & Payments (invoicing, GST, discounts, advance ledger, estimates, shift closure, reports) |
| HealixCare_FRD_Queue_CheckIn_Module_v1_0.docx | Queue & Check-in (front-desk worklist, vitals capture, live token queue, TV display) |
| HealixCare_FRD_EMR_CoreClinical_v1_P1.docx | EMR Part 1 (consultations, vitals, prescriptions, diagnoses, allergies, CPOE, CDS) |
| HealixCare_FRD_EMR_CoreClinical_v1_P2.docx | EMR Part 2 (templates, consents, care plans, referrals, discharge summaries) |
| HealixCare_FRD_Patient360_v1.0.docx | Patient 360° (unified read-only patient view aggregating all modules) |
| HealixCare_FRD_Patient_Module_v1_0.docx | Patient (registration flows, directory/search, duplicate detection/merge, ABHA linking) |
| HealixCare_FRD_UserManagement_Auth_v3_0.docx | User Management & Authentication (login, 2FA, RBAC matrix, facilities/departments, audit) |

All nine are written to the same rigorous format: User Story → Acceptance Criteria → Prototypes/Screens → Data Specifications → Validations → Use Cases → Error/Success Messages → Interface Requirements → Dependencies → User Roles and Permissions. This format is directly usable as a source for Playwright test-case derivation (acceptance criteria ≈ assertions; error/success messages ≈ expected UI text; user roles ≈ test-user matrix).

**Coverage gap:** only 7 of ~29 modules cataloged in the Roadmap have an FRD at this level of detail. See `04_Required_Documents_For_POC.md` for the full missing-FRD list.

## Workflow

| Document | Notes |
|---|---|
| End_to_End_Patient_Journey.docx | Cross-module narrative — the one document that ties all other workflows together chronologically. |
| OPD_Workflow.docx | Scheduling → check-in/queue → EMR consultation → order fan-out → billing handoff. |
| IPD_Workflow.docx | Admission → bed assignment → nursing-care loop → transfer → discharge. |
| Emergency_Workflow.docx | **Flags its own gap:** no dedicated Emergency backend module exists; this document describes emergency handling as a usage pattern of Patient Registration + IPD/OPD, not a standalone module. |
| Lab_Workflow.docx | Order → result entry; no FRD exists for this module (workflow-level detail only). |
| Radiology_Workflow.docx | Order → report, incl. QA/peer-review; no FRD exists for this module. |
| Pharmacy_Workflow.docx | E-prescription → dispensing; no FRD exists for this module. |
| Billing_and_Payment_Workflow.docx | Charge aggregation → invoice → settlement (incl. Razorpay two-path confirmation). Complements the Billing FRD. |
| Insurance_TPA_Workflow.docx | Pre-authorization → claim; no FRD exists for this module. |
| Doctor_Patient_Portal_Workflows.docx | Both portals' capabilities and the patient/doctor authorization boundary; no FRD exists for either portal. |
| Inventory_and_Procurement_Workflow.docx | Stock monitoring → requisition → goods receipt; no FRD exists for this module. |

Every Workflow document is explicitly grounded in source code paths (e.g., `backend/modules/billing/`) and each ends with a **Known Gaps** section — these gap notes are valuable for scoping realistic (vs. aspirational) test coverage.

## Database

No standalone database/schema document exists. Each FRD's "Data Specifications" section references a per-feature "Database Schema" table, and the Solution Architecture Document mentions a 121-table PostgreSQL schema and a separate "Data Architecture and Master Data Strategy" document — **that document is not present in the workspace.**

## API

No standalone API catalog/OpenAPI spec exists. Individual endpoints are referenced inline throughout the FRDs and Workflow docs (e.g., `GET /api/v1/patients/{patient_id}/360`), and an "API Catalog" document is referenced by both the Feature Backlog and the Role Matrix as a related document — **it is not present in the workspace.**

## UI/UX

No standalone UI/UX design document, wireframe set, or style guide exists. FRDs describe screens narratively ("Screen 1: Booking form...") under a "Prototypes/Screens" heading; whether actual visual mockups are embedded behind these captions was **not verified** in this audit (see `01_Document_Inventory.md`, Section 1).

## Other

- `.DS_Store` files (macOS metadata, not documentation — no classification needed, excluded from all further analysis).
- Archive folders (12 documents) — excluded from classification per instruction; not reviewed.

---

## Summary Table

| Category | Count | Documents Present? |
|---|---|---|
| Business | 3 | Yes |
| Technical | 1 | Yes |
| Project Management | 1 | Yes |
| Architecture | 1 (+5 referenced, absent) | Partial |
| Functional | 9 (of ~29 modules) | Partial |
| Workflow | 11 | Yes |
| Database | 0 (schema info embedded only) | **Missing as standalone doc** |
| API | 0 (endpoints embedded only) | **Missing as standalone doc** |
| UI/UX | 0 (unverified inline captions only) | **Missing / unverified** |
| Other | 0 | — |
