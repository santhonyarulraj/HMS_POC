# Phase 3 – Enterprise Test Scenario Generation

Follow all rules defined in:

Prompts/00_Execution_Rules.md

---

## Objective

Generate Enterprise Test Scenarios for the approved POC scope.

Project:
HealixCare Hospital Management System

Current POC Scope:
- Login Module Only

Future Scope:
- Patient Registration (Out of Scope for this phase)

---

## Mandatory Inputs

Use only approved project artifacts.

Required Inputs:

- Documentation Audit
- Requirement Analysis
- Requirement Traceability Matrix (RTM)
- Requirement Baseline RB-1.0
- Master Test Plan
- Login FRD
- Solution Architecture Document
- Role Matrix

Do not use assumptions that are not supported by the approved documentation.

---

## Test Scenario Standard

Generate Test Scenarios following:

- IEEE 829
- ISO/IEC/IEEE 29119
- Enterprise QA Best Practices

Every Test Scenario must be traceable to one or more approved Requirement IDs.

---

## Output

Create the folder if it does not exist:

Output/06_TestScenarios/

Generate the following files:

1. Master_Test_Scenarios.md
2. Scenario_Coverage_Summary.md

---

## Scenario Categories

Generate scenarios covering:

### Functional
- Positive
- Negative
- Boundary
- Validation
- Business Rules

### Security
- Authentication
- Authorization
- Invalid Access
- Session Management

### Usability
- Error Messages
- Mandatory Fields
- Field Behaviour
- Keyboard Navigation

### Reliability
- Session Timeout
- Concurrent Login (if supported)
- Recovery Scenarios

---

## Scenario Template

Each scenario must contain:

- Scenario ID
- Requirement ID(s)
- Module
- Feature
- Scenario Name
- Scenario Description
- Preconditions
- Test Objective
- Expected Result
- Priority
- Automation Candidate (Yes/No)
- Automation Priority (High/Medium/Low)
- Remarks

---

## Validation

Before completing the phase, verify:

- Every Requirement ID exists in the RTM.
- No orphan scenarios exist.
- No duplicate scenarios exist.
- Login Module remains the only implementation scope.
- Every scenario is suitable for future Playwright automation.

---

## Completion Criteria

When complete, provide:

1. Executive Summary

2. Statistics

- Total Scenarios
- Positive Scenarios
- Negative Scenarios
- Security Scenarios
- Automation Candidates
- High Priority Scenarios

3. Requirement Coverage Summary

4. Generated File List

5. Risks or Non-Blocking Observations

Do NOT generate Test Cases.

Do NOT generate Test Data.

Do NOT generate Playwright scripts.

Stop after completing the Test Scenario phase and wait for my review.