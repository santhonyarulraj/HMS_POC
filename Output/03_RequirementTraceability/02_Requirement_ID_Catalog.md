# 02 — Requirement ID Catalog

Quick-lookup index of every Requirement ID minted in this phase, mapped to its source Feature ID. Full detail for each lives in `01_Requirement_Traceability_Matrix.md`.

---

## A. Login Module (10 Requirements)

| Requirement ID | Requirement Name | Source Feature ID | Priority |
|---|---|---|---|
| REQ-LOG-001 | Login | HC-USR-001 | High |
| REQ-LOG-002 | Logout | HC-USR-002 | High |
| REQ-LOG-003 | Change Own Password | HC-USR-010 | Medium |
| REQ-LOG-004 | Admin Force Password Reset | HC-USR-011 | Medium |
| REQ-LOG-005 | Request Password Reset Code (Forgot Password) | HC-USR-012 | Medium |
| REQ-LOG-006 | Reset Password with Code | HC-USR-013 | Medium |
| REQ-LOG-007 | Two-Factor Setup (Authenticator App) | HC-USR-014 | Medium |
| REQ-LOG-008 | Two-Factor Verify at Login | HC-USR-015 | High |
| REQ-LOG-009 | Two-Factor Disable (Admin Recovery) | HC-USR-016 | Low |
| REQ-LOG-010 | Two-Factor Enforcement by Role | HC-USR-017 | Medium |

## B. Patient Registration Module (10 Requirements)

| Requirement ID | Requirement Name | Source Feature ID | Priority |
|---|---|---|---|
| REQ-PAT-001 | Full Patient Registration (7-Step Wizard) | HC-PAT-001 | High |
| REQ-PAT-002 | Quick Patient Registration | HC-PAT-002 | High |
| REQ-PAT-003 | Emergency Patient Registration | HC-PAT-003 | High |
| REQ-PAT-004 | Active Emergencies List | HC-PAT-004 | Low |
| REQ-PAT-005 | Patient QR Code — Generate, View, Download | HC-PAT-005 | Medium |
| REQ-PAT-006 | Pre-Registration Duplicate Check | HC-PAT-017 | High |
| REQ-PAT-007 | ABHA OTP Generation | HC-PAT-022 | Low |
| REQ-PAT-008 | ABHA OTP Verification and Profile Fetch | HC-PAT-023 | Low |
| REQ-PAT-009 | ABHA Search and Registration Pre-Fill | HC-PAT-024 | Low |
| REQ-PAT-010 | ABHA Link and Profile Fetch for Existing Patient | HC-PAT-025 | Low |

---

## C. Uniqueness Validation

**Total Requirement IDs minted: 20**

| Check | Method | Result |
|---|---|---|
| No duplicate Requirement IDs | Visual scan + sequential numbering per module prefix (`REQ-LOG-001`…`010`, `REQ-PAT-001`…`010`), each minted exactly once against exactly one source Feature ID | ✅ Pass — 20 unique IDs, 0 duplicates |
| No duplicate source Feature ID mapped to two Requirement IDs | Cross-checked against `Output/02_RequirementAnalysis/02_Functional_Requirements.md`, which lists 20 in-scope Feature IDs (10 `HC-USR-xxx` + 10 `HC-PAT-xxx`) | ✅ Pass — 1:1 mapping, no source feature claimed by more than one Requirement ID |
| Every in-scope Requirement Analysis feature has a Requirement ID | Cross-checked against §A and §B of `02_Functional_Requirements.md` | ✅ Pass — all 20 in-scope features (§A.1–A.3, §B.1–B.3 of that document) are represented; the explicitly-excluded features (§C of that document) intentionally have no Requirement ID, as they are out of this phase's scope |
| No Requirement ID represents an invented/new requirement | Every row in `01_Requirement_Traceability_Matrix.md` cites a source Feature ID and FRD section — none was authored without a source citation | ✅ Pass |

## D. ID Scheme Reference

```
REQ-LOG-NNN   → Login module, sequential 001–010, source-ordered by FRD section (Authentication → Password Management → Two-Factor Authentication)
REQ-PAT-NNN   → Patient Registration module, sequential 001–010, source-ordered by FRD section (Registration Flows → Inline Duplicate Check → ABHA-Assisted Registration)
```

This scheme is stable for the remainder of the QA lifecycle — Test Scenarios, Test Cases, Automation Scripts, and Defects raised in later phases should reference these IDs, not the underlying FRD Feature IDs directly, so that traceability survives even if a future FRD revision renumbers its own features.
