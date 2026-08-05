# Scenario Coverage Summary — Login Module

**Project:** HealixCare Hospital Management System — Playwright TypeScript Automation POC
**Document:** Scenario Coverage Summary
**Version:** 1.0
**Date:** 2026-08-05
**Author:** AI Generated (Senior QA Architect role, Claude Code)
**Status:** Draft — Pending Client Review
**References:** `Master_Test_Scenarios.md` (this folder); Requirement Baseline RB-1.0

---

## 1. Overall Statistics

| Metric | Value |
|---|---|
| Total Scenarios | 55 |
| Requirements Covered | 10 of 10 (100%) |
| Requirements With Zero Scenarios | 0 |
| Duplicate Scenarios | 0 (validated by unique-ID scan) |
| Orphan Scenarios (no valid Requirement ID) | 0 (validated — every scenario cites at least one REQ-LOG ID confirmed to exist in RB-1.0/RTM) |

## 2. Category Breakdown

| Category | Count | % of Total |
|---|---|---|
| Functional | 20 | 36% |
| Negative | 8 | 15% |
| Validation | 6 | 11% |
| Boundary | 6 | 11% |
| Security (all subtypes) | 8 | 15% |
| — Security (Authentication) | 3 | |
| — Security (Session Management) | 2 | |
| — Security (Authorization) | 2 | |
| — Security (general/behavioral) | 1 | |
| Usability | 4 | 7% |
| Reliability | 3 | 5% |
| **Total** | **55** | **100%** |

## 3. Priority Breakdown

| Priority | Count | % of Total |
|---|---|---|
| High | 16 | 29% |
| Medium | 29 | 53% |
| Low | 10 | 18% |

## 4. Automation Assessment

| Metric | Count | % of Total |
|---|---|---|
| Automation Candidate: Yes | 55 | 100% |
| Automation Candidate: No | 0 | 0% |
| Automation Priority: High | 15 | 27% |
| Automation Priority: Medium | 21 | 38% |
| Automation Priority: Low | 19 | 35% |

**Note on "100% Automation Candidate":** this reflects that every scenario is *technically* automatable in Playwright — consistent with the project's prior finding (`Output/03_RequirementTraceability/03_Automation_Coverage.md`, Section E) that no requirement in this module is exclusively manual. It does **not** mean every scenario can execute today — 26 of the 55 scenarios (47%) are explicitly marked "Blocked pending Q2" (2FA/TOTP) or "Blocked pending Q8" (email test-mailbox) in their Remarks field. See Section 6 below.

## 5. Requirement Coverage Matrix

| Requirement ID | Requirement Name | Scenario Count | Functional | Negative | Boundary | Validation | Security | Usability | Reliability |
|---|---|---|---|---|---|---|---|---|---|
| REQ-LOG-001 | Login | 8 | 3 | 2 | 1 | 1 | 1 | 0 | 0 |
| REQ-LOG-002 | Logout | 2 | 1 | 0 | 0 | 0 | 1 | 0 | 0 |
| REQ-LOG-003 | Change Own Password | 7 | 2 | 1 | 1 | 1 | 0 | 1 | 1 |
| REQ-LOG-004 | Admin Force Password Reset | 4 | 3 | 0 | 0 | 1 | 0 | 0 | 0 |
| REQ-LOG-005 | Request Password Reset Code | 4 | 1 | 0 | 1 | 1 | 1 | 0 | 0 |
| REQ-LOG-006 | Reset Password with Code | 7 | 1 | 2 | 1 | 2 | 0 | 0 | 1 |
| REQ-LOG-007 | Two-Factor Setup | 7 | 3 | 1 | 0 | 0 | 1 | 2 | 0 |
| REQ-LOG-008 | Two-Factor Verify at Login | 8 | 1 | 2 | 2 | 0 | 2 | 1 | 0 |
| REQ-LOG-009 | Two-Factor Disable (Admin Recovery) | 3 | 2 | 0 | 0 | 0 | 0 | 0 | 1 |
| REQ-LOG-010 | Two-Factor Enforcement by Role | 5 | 3 | 0 | 0 | 0 | 2 | 0 | 0 |
| **Total** | | **55** | **20** | **8** | **6** | **6** | **8** | **4** | **3** |

## 6. Execution Readiness Cross-Reference

| Blocker | Scenarios Affected | Count |
|---|---|---|
| Blocking Q2 (2FA / TOTP secret provisioning) | TS-LOGIN-002, 018–021, 033, 036–038, 040–042, 045–046, 048–054 | 21 |
| Blocking Q8 (test-mailbox for email OTP) | TS-LOGIN-022, 026, 030–032 | 5 |
| Neither (executable once environment/credentials exist — Q1/Q2 baseline access only) | All remaining scenarios | 29 |

*(Some scenarios appear in both the Q2 list above and are also gated more generally by baseline environment access — this table reflects the specific, named additional blocker beyond general environment/credential access already covered by Q1/Q2 at the Test Plan level.)*

## 7. Validation Performed (Per Phase Requirements)

| Check | Result |
|---|---|
| Every Requirement ID referenced exists in RTM/RB-1.0 | ✅ Confirmed — all references are to REQ-LOG-001–010, cross-checked against `Output/03_RequirementTraceability/02_Requirement_ID_Catalog.md` |
| No orphan scenarios | ✅ Confirmed — every scenario has ≥1 valid Requirement ID |
| No duplicate scenarios | ✅ Confirmed — 55 unique `TS-LOGIN-NNN` IDs, sequential 001–055, no gaps or repeats |
| Login Module remains the only implementation scope | ✅ Confirmed — zero scenarios reference Patient Registration or any other module |
| Every scenario suitable for future Playwright automation | ✅ Confirmed — see Section 4; "suitable for automation" is distinct from "executable today" |

## 8. Relationship to Future Phases

Each scenario in `Master_Test_Scenarios.md` is a candidate for one or more Test Cases in the next phase (`Output/07_TestCases/`, per naming convention `TC-LOGIN-NNN`). No Test Cases, Test Data, or Playwright scripts have been generated in this phase.
