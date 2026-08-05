# Test Case Summary — Login Module

**Project:** HealixCare Hospital Management System — Playwright TypeScript Automation POC
**Document:** Test Case Summary
**Version:** 1.0
**Date:** 2026-08-05
**Author:** AI Generated (Senior QA Architect role, Claude Code)
**Status:** Draft — Pending Client Review
**Standard Alignment:** IEEE 829-2008, ISO/IEC/IEEE 29119
**References:** `Login_Test_Cases.xlsx` (this folder, source of truth for full detail); `Output/06_TestScenarios/Master_Test_Scenarios.md`; Requirement Baseline RB-1.0

---

## 1. Overview

This document summarizes **55 Enterprise Test Cases** covering the Login module, generated in a strict **1:1 mapping to the 55 approved Test Scenarios** (`TS-LOGIN-001`–`055` → `TC-LOGIN-001`–`055`). Each Test Case operationalizes its source Scenario into concrete Test Steps, Test Data, and an Expected Result, ready for Playwright implementation. Full detail for every field lives in `Login_Test_Cases.xlsx`; this document reports the roll-up statistics and traceability confirmation.

**Design choice on data-driven scenarios:** where a Test Scenario specified multiple data variants (e.g., password complexity boundaries, mandatory-field checks on two fields), the corresponding Test Case captures all variants as a structured data table within its own Test Steps/Test Data fields, rather than being split into separate Test Case IDs. This models how these will become parameterized Playwright tests (`test.each`) and keeps 1:1 Requirement→Scenario→Case traceability exact and auditable.

## 2. Total Test Cases: 55

## 3. Category Breakdown (Carried Forward From Approved Test Scenarios)

| Category | Count | % |
|---|---|---|
| Functional (includes "Positive") | 20 | 36% |
| Negative | 8 | 15% |
| Security (Authentication/Authorization/Session Management) | 8 | 15% |
| Validation | 6 | 11% |
| Boundary | 6 | 11% |
| Usability | 4 | 7% |
| Reliability | 3 | 5% |
| **Total** | **55** | **100%** |

## 4. Priority Breakdown

| Priority | Count |
|---|---|
| High | 16 |
| Medium | 29 |
| Low | 10 |

## 5. Severity Breakdown (New at This Phase — per Test Plan Severity Matrix)

| Severity | Count | Definition (from `Output/05_TestPlan/10_Defect_Management.md`) |
|---|---|---|
| Critical | 5 | Security control fails, or the module is entirely unusable if this breaks |
| High | 16 | Core functional flow broken, no workaround |
| Medium | 28 | Functional defect with a workaround, or incorrect-but-non-blocking message |
| Low | 6 | Cosmetic / non-functional wording or UI issue |

## 6. Automation Readiness

| Metric | Value |
|---|---|
| Automation Candidate: Yes | 55 (100%) |
| Automation Candidate: No | 0 |
| Automation Priority: High | 16 |
| Automation Priority: Medium | 17 |
| Automation Priority: Low | 22 |
| Status: Ready — Not Executed | 23 (42%) |
| Status: Blocked — Pending Q2 (2FA/TOTP) | 24 |
| Status: Blocked — Pending Q3 (known-gap policy) | 1 |
| Status: Blocked — Pending Q8 (email test-mailbox) | 7 |
| **Total Blocked** | **32 (58%)** |

Every Test Case is a Playwright automation candidate (consistent with the project's established finding that no Login requirement is exclusively manual). "Blocked" reflects current **execution* readiness — tied to the same 4 Blocking Clarifications tracked since RB-1.0 — not a judgment that the case is unsuited to automation.

## 7. Requirement Coverage (Test Case Count per Requirement)

| Requirement ID | Test Case Count |
|---|---|
| REQ-LOG-001 | 12 |
| REQ-LOG-002 | 2 |
| REQ-LOG-003 | 8 |
| REQ-LOG-004 | 4 |
| REQ-LOG-005 | 5 |
| REQ-LOG-006 | 7 |
| REQ-LOG-007 | 8 |
| REQ-LOG-008 | 9 |
| REQ-LOG-009 | 3 |
| REQ-LOG-010 | 6 |

*(Counts sum to 64, not 55, because several Test Cases legitimately trace to more than one Requirement ID for cross-requirement integration points — e.g., TC-LOGIN-004 verifies both REQ-LOG-001's redirect behavior and REQ-LOG-003's forced-reset mechanics. Every requirement has at least 2 Test Cases; all 10 of 10 Login requirements are covered.)*

## 8. Scenario Coverage

**55 of 55 approved Test Scenarios (100%) have exactly one corresponding Test Case.** No orphan Test Cases exist (every case cites a valid Scenario ID); no orphan Scenarios remain uncovered. See `Test_Coverage_Report.md` for the full traceability matrix.

## 9. Feature Breakdown

| Feature | Test Case Count |
|---|---|
| Authentication | 10 |
| Password Management | 21 |
| Two-Factor Authentication | 24 |

## 10. Generated Deliverables

| File | Contents |
|---|---|
| `Login_Test_Cases.xlsx` | Full 55-row × 18-column workbook — source of truth for every field |
| `Test_Case_Summary.md` | This document |
| `Test_Coverage_Report.md` | Requirement → Scenario → Test Case traceability matrix and validation results |
