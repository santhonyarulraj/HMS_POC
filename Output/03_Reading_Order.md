# 03 — Recommended Reading Order

**Project:** HealixCare HMS Automation POC
**Phase:** Documentation Audit

This order follows the documentation hierarchy the documents themselves declare (each FRD/Workflow doc has a "Relationship to Other Documents" section) — top-down from vision to module detail, with each module's FRD paired immediately with its corresponding workflow document where one exists.

## Tier 1 — Foundation (read first, always)

1. **Product_Vision_Document.docx** — the reference point every other document is evaluated against.
2. **HMS_Scope_and_Module_Roadmap.docx** — as-built module inventory and maturity (Complete/Partial/Stub). Critical: tells you which modules are stable enough to automate.
3. **Feature_Backlog_with_Priority.docx** — translates the roadmap into prioritized, traceable items.
4. **Solution_Architecture_Document.docx** — confirmed tech stack, modular monolith structure, deployment modes. Gives QA the technical shape of the system under test.
5. **Role_Matrix_and_Permission_Catalog.docx** — RBAC model and seeded roles. Required before designing any test-user matrix.
6. **HealixCare_HMS_Project_Management_Plan.docx** — governance/cadence context (lowest urgency of Tier 1; skim rather than deep-read).

## Tier 2 — Cross-Module Narrative

7. **End_to_End_Patient_Journey.docx** — the single document that walks the patient record across every module in order. Read this before any individual module FRD so each module's place in the larger flow is clear.

## Tier 3 — Module Detail (FRD paired with its Workflow doc, in patient-journey order)

8. **HealixCare_FRD_UserManagement_Auth_v3_0.docx** — read first among modules: login/session is a precondition for every other flow.
9. **HealixCare_FRD_Patient_Module_v1_0.docx** — patient registration is the entry point of the journey.
10. **HealixCare_FRD_Patient360_v1.0.docx** — read immediately after Patient, since it's the aggregated read view over the same entity.
11. **HealixCare_FRD_Appointments_Module_v1_0.docx**
12. **HealixCare_FRD_Queue_CheckIn_Module_v1_0.docx** — explicitly cross-referenced from the Appointments FRD; read as a pair.
13. **OPD_Workflow.docx** — ties scheduling → check-in/queue → consultation → billing together; read once the three FRDs above are understood.
14. **HealixCare_FRD_EMR_CoreClinical_v1_P1.docx**
15. **HealixCare_FRD_EMR_CoreClinical_v1_P2.docx** — read immediately after Part 1 (continuous feature numbering, same module).
16. **HealixCare_FRD_Billing_Module_v1_0.docx**
17. **Billing_and_Payment_Workflow.docx** — read as a pair with the Billing FRD.
18. **HealixCare_FRD_AdminSettings_v1_0.docx** — cross-cutting configuration consumed by nearly every other module; read last among FRDs since it makes most sense once you've seen what it configures.

## Tier 4 — Remaining Workflow-Only Modules (no FRD exists — context only)

These modules have a Workflow document but no acceptance-criteria-level FRD. Read only if/when the POC scope extends into them; otherwise they are optional for now (see `04_Required_Documents_For_POC.md`):

19. IPD_Workflow.docx
20. Emergency_Workflow.docx — note this document flags that no dedicated Emergency module exists in the codebase at all.
21. Lab_Workflow.docx
22. Radiology_Workflow.docx
23. Pharmacy_Workflow.docx
24. Insurance_TPA_Workflow.docx
25. Doctor_Patient_Portal_Workflows.docx
26. Inventory_and_Procurement_Workflow.docx

## Not Reviewed (Archived)

12 archived documents across 5 module folders were excluded from this audit per instruction and are not part of this reading order. Flag if any should be pulled in to resolve a version conflict.
