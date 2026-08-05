# 03 — Approved Requirements Register

**Baseline Version:** RB-1.0 | **Status:** All 20 requirements below are **APPROVED FOR TESTING**

Full field-level detail (Business Objective, Dependency, Automation Feasibility/Complexity/Priority, Risk, Assumption, Clarification Required, and reserved lifecycle placeholders) is preserved unmodified in `Output/03_RequirementTraceability/01_Requirement_Traceability_Matrix.md` — this register is the frozen, at-a-glance index confirming approval status for each.

---

## A. Login Module

| Requirement ID | Requirement Name | Priority | Business Rule(s) | Validation Rule(s) Present? | Automation Feasibility | Baseline Status |
|---|---|---|---|---|---|---|
| REQ-LOG-001 | Login | High | BR-L-02 to 06, 19, 20 | Yes | Partially Feasible | ✅ Approved for Testing |
| REQ-LOG-002 | Logout | High | BR-L-07 | No field-level rule (behavioral only) | Feasible | ✅ Approved for Testing |
| REQ-LOG-003 | Change Own Password | Medium | BR-L-08, 09, 10 | Yes | Feasible | ✅ Approved for Testing |
| REQ-LOG-004 | Admin Force Password Reset | Medium | BR-L-08 | Yes | Feasible (with precondition) | ✅ Approved for Testing |
| REQ-LOG-005 | Request Password Reset Code (Forgot Password) | Medium | BR-L-11, 12, 13 | Yes | Partially Feasible | ✅ Approved for Testing |
| REQ-LOG-006 | Reset Password with Code | Medium | BR-L-08, 12 | Yes | Partially Feasible | ✅ Approved for Testing |
| REQ-LOG-007 | Two-Factor Setup (Authenticator App) | Medium | BR-L-14, 15 | Yes | Blocked | ✅ Approved for Testing |
| REQ-LOG-008 | Two-Factor Verify at Login | High | BR-L-04, 16, 17 | Yes | Blocked | ✅ Approved for Testing |
| REQ-LOG-009 | Two-Factor Disable (Admin Recovery) | Low | BR-X-01 (general audit rule only) | No field-level rule (confirmation-gated only) | Feasible (conditional) | ✅ Approved for Testing |
| REQ-LOG-010 | Two-Factor Enforcement by Role | Medium | BR-L-18 | No field-level rule (toggle UI) | Feasible (toggle only) | ✅ Approved for Testing |

## B. Patient Registration Module

| Requirement ID | Requirement Name | Priority | Business Rule(s) | Validation Rule(s) Present? | Automation Feasibility | Baseline Status |
|---|---|---|---|---|---|---|
| REQ-PAT-001 | Full Patient Registration (7-Step Wizard) | High | BR-P-01, 02, 03, 04, 09, 10, 11, 12, 17, 18 | Yes | Feasible | ✅ Approved for Testing |
| REQ-PAT-002 | Quick Patient Registration | High | BR-P-01, 04, 05, 08, 09, 10 | Yes | Feasible | ✅ Approved for Testing |
| REQ-PAT-003 | Emergency Patient Registration | High | BR-P-01, 06, 07, 08, 09, 10 | Yes | Feasible | ✅ Approved for Testing |
| REQ-PAT-004 | Active Emergencies List | Low | Facility-scoping (NFR-003, general) | No field-level rule (read-only view) | Feasible | ✅ Approved for Testing |
| REQ-PAT-005 | Patient QR Code — Generate, View, Download | Medium | BR-P-12 | No field-level rule (generation/display behavior) | Feasible (partial — payload decode is a design choice) | ✅ Approved for Testing |
| REQ-PAT-006 | Pre-Registration Duplicate Check | High | BR-P-04 | Yes | Feasible (needs seeded data) | ✅ Approved for Testing |
| REQ-PAT-007 | ABHA OTP Generation | Low | BR-P-13, 14 | Yes | Blocked | ✅ Approved for Testing |
| REQ-PAT-008 | ABHA OTP Verification and Profile Fetch | Low | BR-P-15 | Yes | Blocked | ✅ Approved for Testing |
| REQ-PAT-009 | ABHA Search and Registration Pre-Fill | Low | BR-P-13, 15 | Yes | Blocked | ✅ Approved for Testing |
| REQ-PAT-010 | ABHA Link and Profile Fetch for Existing Patient | Low | BR-P-13 | No field-level rule beyond one error message | Blocked | ✅ Approved for Testing (scope itself flagged — see AM-06) |

---

## Register Totals

| Metric | Count |
|---|---|
| Total Requirements | 20 |
| Approved for Testing | 20 |
| Rejected | 0 |
| High Priority | 7 |
| Medium Priority | 7 |
| Low Priority | 6 |
| Requirements with at least one field-level Validation Rule | 14 |
| Requirements with behavioral-only rules (no field-level validation) | 6 (REQ-LOG-002, 009, 010; REQ-PAT-004, 005, 010) |

The 6 "behavioral-only" requirements are not gaps — each was individually verified during RTM construction to have an honest "no field-level validation applies" determination (e.g., a read-only list, a toggle, a confirmation dialog), not a missed field.
