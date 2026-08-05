# 01 — Requirement Baseline

**Project:** HealixCare HMS — Playwright TypeScript Automation POC
**Requirement Baseline Version:** **RB-1.0**
**Status:** FROZEN
**Scope:** Login (10 requirements) + Patient Registration (10 requirements) = 20 requirements total

---

## 1. Purpose of This Baseline

This document freezes the set of requirements approved through Phase 1 (Requirement Analysis) and Phase 1.5 (Requirement Traceability Matrix) as the **single official source for all future testing activity** — Test Planning, Test Scenarios, Test Cases, and Automation. From this point forward:

- Test Plan and all downstream phases must trace back to a Requirement ID in this baseline.
- No requirement in this baseline may be silently altered — any future change requires a new, versioned baseline (e.g., RB-1.1) with a documented reason.
- Open questions and known risks are **frozen as-is, not resolved** by this exercise — this baseline documents the honest current state, including its open items, rather than presenting false completeness.

## 2. Inputs Used (and Only These)

Per instruction, this baseline was built exclusively from already-approved artifacts. No other document was consulted or introduced:

| Source | Location |
|---|---|
| Documentation Audit | `Output/01_Document_Inventory.md` through `Output/04_Required_Documents_For_POC.md` |
| Requirement Analysis | `Output/02_RequirementAnalysis/01_Business_Summary.md` through `08_Automation_Recommendation.md` |
| Requirement Traceability Matrix (RTM) | `Output/03_RequirementTraceability/01_Requirement_Traceability_Matrix.md` through `05_Requirement_to_Automation_Map.md` |

## 3. Pre-Freeze Verification Performed

| Verification | Result |
|---|---|
| Every approved requirement exists in the RTM | ✅ Confirmed — all 20 requirements (REQ-LOG-001 to 010, REQ-PAT-001 to 010) present with full field sets |
| Requirement IDs are unique | ✅ Confirmed — re-validated against `02_Requirement_ID_Catalog.md` Section C (0 duplicates, 1:1 source mapping) |
| Every requirement has: Business Rule, Validation Rule, Priority, Dependency, Automation Feasibility | ✅ Confirmed for all 20 — see `03_Approved_Requirements.md`. Where a field's honest value is "no field-level rule applies" (e.g., read-only views), this is recorded explicitly as such, not left blank |
| Assumptions reviewed | ✅ 17 source-verified assumptions consolidated — see `05_Assumptions_Register.md` |
| Risks reviewed | ✅ 10 automation risks consolidated — see `06_Risk_Register.md` |
| Clarification questions reviewed and split | ✅ 12 questions split into 4 Blocking (`07_Blocking_Clarifications.md`) and 8 Non-Blocking (`08_NonBlocking_Clarifications.md`) |
| Requirement conflicts identified | ✅ 1 conflict — see `09_Requirement_Conflicts.md` |
| Final project scope confirmed | ✅ See `02_Approved_Project_Scope.md` |

## 4. Baseline Declaration

All **20 requirements** listed in `03_Approved_Requirements.md` are hereby marked:

> **APPROVED FOR TESTING**

This status means: the requirement is well-formed, traceable, and sufficiently specified to begin Test Plan design. It does **not** mean every open question about it has been answered — where a requirement carries an open Blocking or Non-Blocking clarification, that linkage is preserved in its RTM record and repeated in this baseline's supporting registers, so Test Planning inherits full visibility rather than a false "all clear."

**Rejected requirements: 0.** No requirement was found incomplete, contradictory in itself, or unfit to proceed during this verification pass.

## 5. Change Control From This Point Forward

Per instruction, this phase does not modify any approved requirement, Requirement ID, business rule, or prior-phase output. If a future phase (Test Planning or later) discovers that a requirement needs correction, that correction must be raised as a new baseline revision (RB-1.1, RB-2.0, etc.) with a documented change reason — not a silent edit to RB-1.0's record.

## 6. Companion Documents in This Baseline

| File | Contents |
|---|---|
| `02_Approved_Project_Scope.md` | Final confirmed in-scope / out-of-scope boundary for both modules |
| `03_Approved_Requirements.md` | The 20-requirement register, each marked Approved for Testing |
| `04_Business_Rules_Baseline.md` | All 41 frozen business rules (21 Login, 18 Patient Registration, 2 cross-cutting) |
| `05_Assumptions_Register.md` | All 17 source-verified assumptions plus system dependencies |
| `06_Risk_Register.md` | All 10 automation risks, mapped to affected requirements |
| `07_Blocking_Clarifications.md` | The 4 questions that must be answered before Test Planning can start meaningfully |
| `08_NonBlocking_Clarifications.md` | The 8 questions that can be resolved in parallel with early Test Planning |
| `09_Requirement_Conflicts.md` | The 1 unresolved cross-document conflict |
| `10_Baseline_Approval_Report.md` | Formal approval report with statistics and Automation Readiness Assessment |
