# 04 — Required Documents for the Playwright TypeScript POC

**Project:** HealixCare HMS Automation POC
**Phase:** Documentation Audit

This document does **not** analyze requirements or propose test cases — that is the next phase. It only scopes which documents are needed to *start* that work responsibly, and what is still missing.

---

## 1. Recommended Module for the First Automation POC

**Recommendation: a two-step start — Login (User Management & Authentication) first, then Patient Registration.**

| Step | Module | Why |
|---|---|---|
| POC-0 | **Login / Authentication** | Smallest, most fully-specified flow (FRD v3.0, explicitly "code-verified"). Every other automated flow needs a logged-in session as a precondition, so this is the natural place to prove out the Playwright + Page Object Model framework, environment connectivity, and CI wiring before investing in a business-flow POC. |
| POC-1 | **Patient Registration** | Entry point of the actual patient journey; fully specified (7-step wizard, duplicate detection, UHID generation); well-bounded; every downstream module (Appointments, Billing, EMR) depends on a patient existing, so this is the natural second POC and unlocks everything after it. |

**Why not the other fully-specified modules first:**
- **Appointments** — largest module in the system (~85 functions); too large for a first POC, though an excellent third step once the framework is proven.
- **Billing** — the most business-rule-heavy module (GST, discounts, advance ledger); high complexity/risk for a first attempt.
- **EMR** — largest module overall (~123 endpoints, split across two FRDs); defer until the framework and patient/appointment data setup are proven.
- **Queue & Check-in, Admin Settings** — both are supporting/configuration modules that make more sense to automate once there's a patient and an appointment to check in or configure against.

**Why not the workflow-only modules (Lab, Radiology, Pharmacy, IPD, Insurance/TPA, Doctor/Patient Portal, Inventory):** no FRD exists for any of them — only process-level Workflow documents, which lack the acceptance-criteria, validation-message, and error-message detail needed to write precise Playwright assertions. Automating against a Workflow doc alone would mean guessing at exact UI text and edge-case behavior.

---

## 2. Minimum Document Set (Required to Start POC-0 + POC-1)

| Document | Role in the POC |
|---|---|
| `RequiementDoucments/Phase1/UserManagement&Authentication/HealixCare_FRD_UserManagement_Auth_v3_0.docx` | Primary spec for POC-0 (login, 2FA, session, error/success messages). |
| `RequiementDoucments/Phase1/Patient/HealixCare_FRD_Patient_Module_v1_0.docx` | Primary spec for POC-1 (registration wizard, validations, UHID generation, duplicate handling). |
| `RequiementDoucments/Core_Business/Role_Matrix_and_Permission_Catalog.docx` | Defines the 4 seeded roles and their permissions — needed to build the test-user matrix and decide which role logs in to perform which action. |
| `RequiementDoucments/Solution_Architecture_Document.docx` | Confirms tech stack, deployment mode (on-prem vs SaaS), and app structure — needed to understand what environment the tests will target (see Section 4 — the actual environment URL is still missing). |

## 3. Recommended Supporting Documents (Not Blocking, but Should Be Skimmed)

| Document | Why It Helps |
|---|---|
| `Product_Vision_Document.docx` | Fast orientation to the domain and non-goals. |
| `HMS_Scope_and_Module_Roadmap.docx` | Confirms Patient and User Management are both "Complete" maturity (not Partial/Stub) — worth double-checking before automating against them. |
| `Workflow/End_to_End_Patient_Journey.docx` | Shows where registration and login sit in the larger flow — useful when the POC scope grows beyond POC-0/POC-1. |
| `Phase1/Patient 360/HealixCare_FRD_Patient360_v1.0.docx` | Optional verification point — after registering a patient, Patient 360° is a convenient read-only screen to assert the new record appears correctly. |

## 4. Documents That Can Be Skipped For Now

Everything not listed above is **deferred, not discarded** — revisit as POC scope expands to later modules:

- All other Phase 1 FRDs: Appointments, Queue & Check-in, EMR (Part 1 & 2), Billing, Admin Settings.
- All Workflow documents except End-to-End Patient Journey: OPD, IPD, Emergency, Lab, Radiology, Pharmacy, Billing & Payment, Insurance/TPA, Doctor/Patient Portal, Inventory & Procurement.
- `Feature_Backlog_with_Priority.docx` — prioritization context, not needed to script tests for two already-agreed modules.
- `HealixCare_HMS_Project_Management_Plan.docx` — governance/cadence, not implementation detail.

---

## 5. Missing Documents — Recommend Requesting From Client/Stakeholder

These are referenced *by name* inside the existing documents (in their "Relationship to Other Documents" sections) but do not exist anywhere in the workspace:

| Missing Document | Referenced By | Why It Matters for Automation |
|---|---|---|
| **Test environment details** (URL, deployment mode of the instance under test, network/VPN access) | *(not referenced — a hard gap)* | Without this, no Playwright test can be pointed at anything. **This is the single most urgent missing item.** |
| **Test/seed credentials per role**, and how 2FA (OTP via SMS/WhatsApp) will be handled for automated login | *(not referenced — a hard gap)* | Login FRD confirms 2FA can be enforced per role; automation needs either a test-account exemption or an OTP-interception strategy before POC-0 can run end-to-end. |
| Application Architecture Document | Solution Architecture Doc | Expands the router/service/repository layering — useful if the team ever needs to seed data directly via API instead of through the UI. |
| Integration Architecture Document | Solution Architecture Doc, Insurance/TPA Workflow | Details Razorpay, TPA, ABHA, AI Assistant, notification integrations — relevant once tests touch payments or notifications. |
| Security and Compliance Architecture | Solution Architecture Doc, Role Matrix | Expands JWT/2FA/PHI encryption posture — relevant to the 2FA-handling question above. |
| Deployment Architecture for SaaS and On-Premise | Solution Architecture Doc | Would supply the exact environment topology/URLs referenced as the gap above. |
| Data Architecture and Master Data Strategy | Solution Architecture Doc, End-to-End Patient Journey | Would clarify what master/reference data (facilities, departments, service items) needs to exist before registration/booking tests can run realistically. |
| Audit and Logging Design | Role Matrix | Only relevant if audit-trail assertions are added later. |
| API Catalog | Feature Backlog, Role Matrix | Would materially speed up test data setup/teardown via API calls instead of UI actions. |
| Module Interface Contracts | Feature Backlog | Only relevant once automation spans multiple modules that need to integrate. |
| Backlog Workbook (companion `.xlsx`) | Project Management Plan, Feature Backlog | Declared the actual single source of truth for status/priority and the live 14-item risk register — the `.docx` Feature Backlog is described as *derived from* this workbook, which is not present. |

**FRD coverage gap** — no FRD exists for these modules (Workflow docs only, or nothing at all): Lab, Radiology, Pharmacy, IPD, OT, Insurance/TPA, Doctor Portal, Patient Portal, Advance Booking, Notifications, AI Assistant, ABHA, Audit Trail, HR/Staff, Payroll, Inventory/Purchase, MIS/Analytics, Telemedicine, Control Plane (SaaS). Not blocking for POC-0/POC-1, but should be requested before automation scope extends into any of them.

---

## 6. Assumptions and Open Questions for the Client

1. **Assumed** the folder `RequiementDoucments` (as named in the workspace) is the same folder the audit brief calls `RequirementDocuments` — please confirm.
2. **Assumed** Archive subfolders can be safely ignored for POC scoping (per instruction received mid-audit) — flag if any archived version should override its active counterpart.
3. **Open question:** do the "Screen 1 / Screen 2" captions in the FRDs correspond to actual embedded mockups, Figma links, or design files anywhere, or are they narrative-only? This affects how much visual/UI verification (vs. behavioral verification) the POC can realistically include.
4. **Open question:** which specific environment (on-premise instance vs. SaaS tenant) should the POC target, and is it currently reachable?
5. **Open question:** for the two recommended POC modules, can 2FA be disabled or bypassed for a dedicated QA/automation test account, or must the automation handle live OTP delivery?

---

**Status:** Documentation Audit phase complete. Awaiting your review of the four output documents before proceeding to Phase 2 (Requirement Analysis).
