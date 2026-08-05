# Test Coverage Report — Login Module

**Project:** HealixCare Hospital Management System — Playwright TypeScript Automation POC
**Document:** Test Coverage Report
**Version:** 1.0
**Date:** 2026-08-05
**Author:** AI Generated (Senior QA Architect role, Claude Code)
**Status:** Draft — Pending Client Review
**Standard Alignment:** IEEE 829-2008, ISO/IEC/IEEE 29119
**References:** Requirement Baseline RB-1.0; `Output/03_RequirementTraceability/`; `Output/06_TestScenarios/`; `Login_Test_Cases.xlsx`

---

## 1. Purpose

This report closes the traceability chain for the Login module through the Test Case layer: **Requirement → Test Scenario → Test Case**, per `Prompts/00_Execution_Rules.md` §8. It also records the validation checks performed before this phase was marked complete.

## 2. Full Traceability Matrix (55 Rows)

| Requirement ID | Scenario ID | Test Case ID | Category | Status |
|---|---|---|---|---|
| REQ-LOG-001 | TS-LOGIN-001 | TC-LOGIN-001 | Functional | Ready - Not Executed |
| REQ-LOG-001; REQ-LOG-008 | TS-LOGIN-002 | TC-LOGIN-002 | Security | Blocked - Pending Q2 |
| REQ-LOG-001 | TS-LOGIN-003 | TC-LOGIN-003 | Negative | Ready - Not Executed |
| REQ-LOG-001; REQ-LOG-003 | TS-LOGIN-004 | TC-LOGIN-004 | Functional | Ready - Not Executed |
| REQ-LOG-001 | TS-LOGIN-005 | TC-LOGIN-005 | Negative | Ready - Not Executed |
| REQ-LOG-001 | TS-LOGIN-006 | TC-LOGIN-006 | Validation | Ready - Not Executed |
| REQ-LOG-001 | TS-LOGIN-007 | TC-LOGIN-007 | Boundary | Ready - Not Executed |
| REQ-LOG-001 | TS-LOGIN-008 | TC-LOGIN-008 | Security | Ready - Not Executed |
| REQ-LOG-002 | TS-LOGIN-009 | TC-LOGIN-009 | Functional | Ready - Not Executed |
| REQ-LOG-002 | TS-LOGIN-010 | TC-LOGIN-010 | Security | Ready - Not Executed |
| REQ-LOG-003 | TS-LOGIN-011 | TC-LOGIN-011 | Functional | Ready - Not Executed |
| REQ-LOG-003 | TS-LOGIN-012 | TC-LOGIN-012 | Negative | Ready - Not Executed |
| REQ-LOG-003 | TS-LOGIN-013 | TC-LOGIN-013 | Boundary | Ready - Not Executed |
| REQ-LOG-003 | TS-LOGIN-014 | TC-LOGIN-014 | Validation | Ready - Not Executed |
| REQ-LOG-003; REQ-LOG-001 | TS-LOGIN-015 | TC-LOGIN-015 | Functional | Ready - Not Executed |
| REQ-LOG-003 | TS-LOGIN-016 | TC-LOGIN-016 | Usability | Ready - Not Executed |
| REQ-LOG-003 | TS-LOGIN-017 | TC-LOGIN-017 | Reliability | Blocked - Pending Q3 |
| REQ-LOG-004 | TS-LOGIN-018 | TC-LOGIN-018 | Functional | Blocked - Pending Q2 |
| REQ-LOG-004 | TS-LOGIN-019 | TC-LOGIN-019 | Functional | Blocked - Pending Q2 |
| REQ-LOG-004 | TS-LOGIN-020 | TC-LOGIN-020 | Validation | Blocked - Pending Q2 |
| REQ-LOG-004; REQ-LOG-001 | TS-LOGIN-021 | TC-LOGIN-021 | Functional | Blocked - Pending Q2 |
| REQ-LOG-005 | TS-LOGIN-022 | TC-LOGIN-022 | Functional | Blocked - Pending Q8 |
| REQ-LOG-005 | TS-LOGIN-023 | TC-LOGIN-023 | Security | Ready - Not Executed |
| REQ-LOG-005 | TS-LOGIN-024 | TC-LOGIN-024 | Validation | Ready - Not Executed |
| REQ-LOG-005 | TS-LOGIN-025 | TC-LOGIN-025 | Boundary | Ready - Not Executed |
| REQ-LOG-006 | TS-LOGIN-026 | TC-LOGIN-026 | Functional | Blocked - Pending Q8 |
| REQ-LOG-006 | TS-LOGIN-027 | TC-LOGIN-027 | Boundary | Blocked - Pending Q8 |
| REQ-LOG-006 | TS-LOGIN-028 | TC-LOGIN-028 | Negative | Blocked - Pending Q8 |
| REQ-LOG-006 | TS-LOGIN-029 | TC-LOGIN-029 | Negative | Ready - Not Executed |
| REQ-LOG-006 | TS-LOGIN-030 | TC-LOGIN-030 | Validation | Blocked - Pending Q8 |
| REQ-LOG-006 | TS-LOGIN-031 | TC-LOGIN-031 | Validation | Blocked - Pending Q8 |
| REQ-LOG-005; REQ-LOG-006 | TS-LOGIN-032 | TC-LOGIN-032 | Reliability | Blocked - Pending Q8 |
| REQ-LOG-007 | TS-LOGIN-033 | TC-LOGIN-033 | Functional | Blocked - Pending Q2 |
| REQ-LOG-007 | TS-LOGIN-034 | TC-LOGIN-034 | Functional | Ready - Not Executed |
| REQ-LOG-007 | TS-LOGIN-035 | TC-LOGIN-035 | Negative | Ready - Not Executed |
| REQ-LOG-007 | TS-LOGIN-036 | TC-LOGIN-036 | Functional | Blocked - Pending Q2 |
| REQ-LOG-007 | TS-LOGIN-037 | TC-LOGIN-037 | Usability | Blocked - Pending Q2 |
| REQ-LOG-007 | TS-LOGIN-038 | TC-LOGIN-038 | Security | Blocked - Pending Q2 |
| REQ-LOG-007 | TS-LOGIN-039 | TC-LOGIN-039 | Usability | Ready - Not Executed |
| REQ-LOG-008 | TS-LOGIN-040 | TC-LOGIN-040 | Functional | Blocked - Pending Q2 |
| REQ-LOG-008 | TS-LOGIN-041 | TC-LOGIN-041 | Functional | Blocked - Pending Q2 |
| REQ-LOG-008 | TS-LOGIN-042 | TC-LOGIN-042 | Boundary | Blocked - Pending Q2 |
| REQ-LOG-008 | TS-LOGIN-043 | TC-LOGIN-043 | Negative | Blocked - Pending Q2 |
| REQ-LOG-008 | TS-LOGIN-044 | TC-LOGIN-044 | Boundary | Blocked - Pending Q2 |
| REQ-LOG-008 | TS-LOGIN-045 | TC-LOGIN-045 | Negative | Blocked - Pending Q2 |
| REQ-LOG-008; REQ-LOG-001 | TS-LOGIN-046 | TC-LOGIN-046 | Security | Blocked - Pending Q2 |
| REQ-LOG-008 | TS-LOGIN-047 | TC-LOGIN-047 | Usability | Blocked - Pending Q2 |
| REQ-LOG-009 | TS-LOGIN-048 | TC-LOGIN-048 | Reliability | Blocked - Pending Q2 |
| REQ-LOG-009; REQ-LOG-001 | TS-LOGIN-049 | TC-LOGIN-049 | Functional | Blocked - Pending Q2 |
| REQ-LOG-009; REQ-LOG-010 | TS-LOGIN-050 | TC-LOGIN-050 | Functional | Blocked - Pending Q2 |
| REQ-LOG-010 | TS-LOGIN-051 | TC-LOGIN-051 | Functional | Blocked - Pending Q2 |
| REQ-LOG-010 | TS-LOGIN-052 | TC-LOGIN-052 | Functional | Blocked - Pending Q2 |
| REQ-LOG-010; REQ-LOG-007 | TS-LOGIN-053 | TC-LOGIN-053 | Functional | Blocked - Pending Q2 |
| REQ-LOG-010 | TS-LOGIN-054 | TC-LOGIN-054 | Security | Blocked - Pending Q2 |
| REQ-LOG-010 | TS-LOGIN-055 | TC-LOGIN-055 | Security | Ready - Not Executed |

## 3. Validation Checks Performed

| Check | Method | Result |
|---|---|---|
| Every Requirement ID exists | Cross-checked all `REQ-LOG-*` references against `Output/03_RequirementTraceability/02_Requirement_ID_Catalog.md` | ✅ Pass — all references resolve to the 10 baselined Login requirements; no reference to any out-of-scope or non-existent ID |
| Every Scenario ID exists | Cross-checked all `TS-LOGIN-*` references against `Output/06_TestScenarios/Master_Test_Scenarios.md` | ✅ Pass — all 55 Scenario IDs resolve |
| No duplicate Test Cases | Programmatic uniqueness check on all 55 `TC-LOGIN-*` IDs | ✅ Pass — 55 unique IDs, 0 duplicates |
| No orphan Test Cases | Every Test Case cites ≥1 valid Requirement ID and exactly 1 valid Scenario ID | ✅ Pass |
| No orphan Scenarios | Every one of the 55 approved Scenarios has exactly one Test Case | ✅ Pass — 55/55 (100%) |
| All Test Cases traceable | Full chain Requirement → Scenario → Test Case confirmed for all 55 rows (Section 2 above) | ✅ Pass |
| Login Module remains the only implementation scope | Scanned all 55 rows for any Patient Registration or other-module reference | ✅ Pass — zero out-of-scope references |

## 4. Requirement Coverage Summary

All 10 of 10 Login requirements have Test Case coverage. See `Test_Case_Summary.md`, Section 7, for exact per-requirement counts (range: 2 to 12 Test Cases per requirement, reflecting each requirement's actual complexity as already captured in the RTM's Automation Complexity ratings).

## 5. Scenario Coverage Summary

**100% (55 of 55)** approved Test Scenarios have a corresponding Test Case. This is a deliberate 1:1 design choice (see `Test_Case_Summary.md`, Section 1) — data-driven scenarios are represented as a single Test Case containing a multi-row data table, not fragmented into additional Test Case IDs.

## 6. Blocked Test Cases — Consolidated by Blocker

| Blocker | Test Case Count | Test Case IDs |
|---|---|---|
| Q2 (2FA / TOTP secret provisioning) | 24 | TC-LOGIN-002, 018–021, 033, 036–038, 040–054 |
| Q3 (known-gap testing policy) | 1 | TC-LOGIN-017 |
| Q8 (email test-mailbox) | 7 | TC-LOGIN-022, 026, 027, 028, 030, 031, 032 |
| **Total Blocked** | **32** | |
| **Ready — Not Executed** | **23** | All remaining Test Case IDs |

*(Counts programmatically verified against the full traceability table in Section 2 — 24 + 1 + 7 = 32, matching the "Total Blocked" figure exactly.)*

## 7. Conclusion

Traceability from Requirement Baseline RB-1.0 through Test Scenarios to Test Cases is complete and internally validated for the Login module. No Test Case was invented without a source Scenario; no Scenario was left uncovered. This report, together with `Login_Test_Cases.xlsx` and `Test_Case_Summary.md`, constitutes the complete Phase 4 deliverable set.
