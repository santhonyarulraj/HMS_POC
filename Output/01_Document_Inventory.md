# 01 — Document Inventory

**Project:** HealixCare HMS Automation POC
**Phase:** Documentation Audit
**Prepared by:** Principal QA Architect (Claude Code)
**Date:** 2026-08-04

---

## 1. Scope Note and Assumption

The automation prompt (`Prompts/01_Document_Audit.md`) refers to a folder named `RequirementDocuments`. The actual folder present in the workspace is named **`RequiementDoucments`** (note the transposed/misspelled characters). This inventory treats `RequiementDoucments` as the intended target folder. **Assumption:** this is a typo in the original folder name, not a second, separate folder — no folder named `RequirementDocuments` exists in the workspace. Please confirm this is correct.

Per explicit instruction received during this audit, **all `Archive` subfolders were excluded from detailed analysis** and are only reported at summary/count level (Section 3). If any archived document later proves relevant (e.g., to resolve a conflict between versions), it can be pulled in on request.

All documents are Microsoft Word `.docx` binary files. They cannot be read directly as text — content was extracted programmatically (Python + `python-docx`) to perform this audit. Extraction covered document body paragraphs and detected the presence/count of embedded tables; it did **not** verify embedded images, diagrams, or figure artwork (the FRDs reference "Screen 1", "Figure 1", etc. as captions — whether actual mockup images/diagrams are embedded behind those captions was not independently confirmed). This is flagged as an open item in `04_Required_Documents_For_POC.md`.

---

## 2. Complete Inventory — Active Documents (26 files)

### 2.1 Core Business (`RequiementDoucments/Core_Business/`)

| # | Document | Size | Version | Owner (per doc) |
|---|----------|------|---------|------------------|
| 1 | Product_Vision_Document.docx | 14.3 KB | 1.0 | Product Management |
| 2 | HMS_Scope_and_Module_Roadmap.docx | 15.6 KB | 1.0 | Product Management |
| 3 | Feature_Backlog_with_Priority.docx | 14.3 KB | 1.0 | Product Management |
| 4 | Role_Matrix_and_Permission_Catalog.docx | 14.6 KB | 1.0 | Engineering / Security |

### 2.2 Project Management (`RequiementDoucments/`)

| # | Document | Size | Version | Owner (per doc) |
|---|----------|------|---------|------------------|
| 5 | HealixCare_HMS_Project_Management_Plan.docx | 17.4 KB | — | — |

### 2.3 Architecture (`RequiementDoucments/`)

| # | Document | Size | Version | Owner (per doc) |
|---|----------|------|---------|------------------|
| 6 | Solution_Architecture_Document.docx | 23.6 KB | 1.0 | Engineering / Architecture |

### 2.4 Functional Requirements — Phase 1 (`RequiementDoucments/Phase1/`)

| # | Document | Folder | Size | Version |
|---|----------|--------|------|---------|
| 7 | HealixCare_FRD_AdminSettings_v1_0.docx | AdminSettings | 80.2 KB | 1.0 |
| 8 | HealixCare_FRD_Appointments_Module_v1_0.docx | Appoitnment&Scheduling | 93.1 KB | 1.0 |
| 9 | HealixCare_FRD_Billing_Module_v1_0.docx | Billing | 56.7 KB | 1.0 |
| 10 | HealixCare_FRD_Queue_CheckIn_Module_v1_0.docx | Checkin&Queue | 35.9 KB | 1.0 |
| 11 | HealixCare_FRD_EMR_CoreClinical_v1_P1.docx | EMR | 52.6 KB | 1.0 (Part 1 of 2) |
| 12 | HealixCare_FRD_EMR_CoreClinical_v1_P2.docx | EMR | 42.6 KB | 1.0 (Part 2 of 2) |
| 13 | HealixCare_FRD_Patient360_v1.0.docx | Patient 360 | 28.7 KB | 1.0 |
| 14 | HealixCare_FRD_Patient_Module_v1_0.docx | Patient | 53.7 KB | 1.0 |
| 15 | HealixCare_FRD_UserManagement_Auth_v3_0.docx | UserManagement&Authentication | 60.3 KB | 3.0 (supersedes v2.0) |

**Note:** These 9 FRDs cover only **7 of the ~29 modules** cataloged in the HMS Scope and Module Roadmap (Admin Settings, Appointments, Billing, Queue/Check-in, EMR, Patient, Patient 360, User Management — EMR and Patient/Patient 360 are closely related but distinct FRDs). See `04_Required_Documents_For_POC.md` for the full list of modules with **no FRD at all**.

### 2.5 Workflow (`RequiementDoucments/Workflow/`)

| # | Document | Size |
|---|----------|------|
| 16 | End_to_End_Patient_Journey.docx | 17.7 KB |
| 17 | OPD_Workflow.docx | 16.2 KB |
| 18 | IPD_Workflow.docx | 16.1 KB |
| 19 | Emergency_Workflow.docx | 16.2 KB |
| 20 | Lab_Workflow.docx | 15.3 KB |
| 21 | Radiology_Workflow.docx | 15.2 KB |
| 22 | Pharmacy_Workflow.docx | 15.8 KB |
| 23 | Billing_and_Payment_Workflow.docx | 15.7 KB |
| 24 | Insurance_TPA_Workflow.docx | 15.6 KB |
| 25 | Doctor_Patient_Portal_Workflows.docx | 15.9 KB |
| 26 | Inventory_and_Procurement_Workflow.docx | 15.3 KB |

*(11 workflow documents. Total active, non-archived documents across sections 2.1–2.5: 4 + 1 + 1 + 9 + 11 = **26**.)*

---

## 3. Archive Folders (Excluded from Detailed Audit)

Per instruction, these are reported at count-only level:

| Module Folder | Archived Documents |
|---|---|
| Appoitnment&Scheduling/Archive | 3 |
| Billing/Archive | 3 |
| EMR/Archive | 1 |
| Patient/Archive | 3 |
| UserManagement&Authentication/Archive | 2 |
| **Total archived** | **12** |

No archived document content was read or analyzed for this audit.

---

## 4. Non-`.docx` Files Observed

- `.DS_Store` files present in several Phase1 subfolders (macOS filesystem artifacts) — not project documentation, no action needed.

## 5. Access Issues

No documents were inaccessible. All 25 active `.docx` files opened and extracted successfully once UTF-8 output encoding was applied (an initial extraction pass hit console encoding errors on special characters such as `→`, `°`, `₹` — re-run with UTF-8 resolved this; no content was lost).
