# 09 — Requirement Conflicts

**Baseline Version:** RB-1.0 | **Source:** `Output/03_RequirementTraceability/01_Requirement_Traceability_Matrix.md`, Section C (approved, unmodified) | **Total Unresolved Conflicts: 1**

A "conflict" here means two independent source documents make contradictory factual claims about the same system behavior — distinct from a single document's own scope ambiguity (those are tracked as clarification questions, not conflicts).

---

## CONFLICT-001 — Custom Role Support

| Field | Value |
|---|---|
| Conflicting Requirements | REQ-LOG-001, REQ-LOG-010 (and, transitively, the role-based facility-scoping behavior underlying every requirement in this baseline) |
| Source A | User Management & Authentication FRD v3.0, §1.4: *"Exactly four system roles exist... custom role creation is not supported."* (repeated in the FRD's own Assumptions §4, item 1) |
| Source B | Role Matrix and Permission Catalog document, §3: *"Any number of roles can exist — four are seeded by default... and hospital admins can create custom roles through the roles.py router."* |
| Nature of Conflict | Direct factual contradiction about whether the live system supports custom roles beyond the four seeded ones. |
| Impact | Determines whether the test-user matrix needs to plan only for 4 fixed roles, or must also handle custom-role creation/assignment as a first-class scenario. |
| Resolution Status | **Unresolved as of RB-1.0** — tracked as Q4 in `07_Blocking_Clarifications.md` |
| Baseline Treatment | Both REQ-LOG-001 and REQ-LOG-010 remain **Approved for Testing** despite this open conflict — the conflict does not invalidate either requirement, it means the test-user matrix design for both must wait on Q4's answer before being finalized into Test Cases. |

No other cross-document conflicts were identified during RTM construction or this baseline verification pass. All other open items in this project are single-document scope ambiguities or documented current-vs-ideal behavior gaps, and are tracked in `08_NonBlocking_Clarifications.md` / `06_Risk_Register.md` respectively, not here.
