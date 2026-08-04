# 05 — Requirement-to-Automation Map

This is the forward-looking skeleton that later phases (Test Plan, Test Scenarios, Test Cases, Automation, Execution, Defect Management) will populate. **Every cell below is a reserved placeholder** — no Test Scenario, Test Case, or Automation Script is created in this phase, per instruction. This table is what makes every requirement "traceable throughout the complete QA lifecycle" (RTM Task 8) — as later phases produce artifacts, they get logged back into the corresponding row here.

---

## A. Login Module

| Requirement ID | Future Scenario ID | Future Test Case ID | Future Script ID | Execution Status | Defect Reference |
|---|---|---|---|---|---|
| REQ-LOG-001 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-002 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-003 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-004 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-005 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-006 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-007 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-008 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-009 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-LOG-010 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |

## B. Patient Registration Module

| Requirement ID | Future Scenario ID | Future Test Case ID | Future Script ID | Execution Status | Defect Reference |
|---|---|---|---|---|---|
| REQ-PAT-001 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-002 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-003 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-004 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-005 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-006 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-007 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-008 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-009 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |
| REQ-PAT-010 | Not Yet Assigned | Not Yet Assigned | Not Yet Assigned | Not Started | Not Available |

---

## C. Planned ID Conventions for Future Phases

To keep this map consistent once later phases start filling it in, the following naming conventions are proposed (not created yet — for your awareness/approval when those phases begin):

| Artifact | Proposed ID Format | Example |
|---|---|---|
| Test Scenario | `SCN-{REQ-ID}-{seq}` | `SCN-REQ-LOG-001-01` |
| Test Case | `TC-{REQ-ID}-{seq}` | `TC-REQ-LOG-001-01` |
| Automation Script | `AUTO-{REQ-ID}-{seq}` | `AUTO-REQ-LOG-001-01` |
| Defect | Whatever your defect tracker natively assigns (e.g., Jira key) — recorded here as-is, not reformatted | `HMS-1234` |

## D. Execution Status Legend (Reserved Values for Future Use)

| Value | Meaning |
|---|---|
| Not Started | No Test Case exists yet for this requirement (current state — all 20) |
| Blocked | A Test Case exists but cannot run (e.g., pending an open question in `07_Client_Questions.md`) |
| Pass | Automated or manual execution completed successfully |
| Fail | Execution completed and found a defect — see Defect Reference |
| Skipped | Deliberately not run this cycle (e.g., deferred ABHA scenarios) |

---

**Current state of this map: 20/20 rows fully unpopulated (100% "Not Started"), as expected — this phase's job was to build the traceable skeleton, not fill it in.**
