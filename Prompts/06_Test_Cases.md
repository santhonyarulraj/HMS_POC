# Phase 4 – Enterprise Test Case Generation

Follow all rules defined in:

Prompts/00_Execution_Rules.md

---

## Objective

Generate Enterprise Test Cases for the approved POC scope.

Project:
HealixCare Hospital Management System

Current POC Scope:
- Login Module Only

Future Scope:
- Patient Registration (Out of Scope)

---

## Mandatory Inputs

Use only approved project artifacts.

Required Inputs:

- Documentation Audit
- Requirement Analysis
- Requirement Traceability Matrix (RTM)
- Requirement Baseline RB-1.0
- Master Test Plan
- Master Test Scenarios
- Login FRD
- Solution Architecture Document
- Role Matrix

Do not introduce new requirements or assumptions.

---

## Test Case Standard

Generate test cases aligned with:

- IEEE 829
- ISO/IEC/IEEE 29119
- Enterprise QA Best Practices

Every Test Case must trace back to:

Requirement
→ Test Scenario
→ Test Case

Every Test Case must be automation-ready for Playwright.

---

## Output

Create the folder if it does not exist:

Output/07_TestCases/

Generate the following deliverables:

1. Login_Test_Cases.xlsx
2. Test_Case_Summary.md
3. Test_Coverage_Report.md

---

## Excel Structure

The Excel workbook shall contain at least the following columns:

- Test Case ID
- Requirement ID
- Scenario ID
- Module
- Feature
- Test Case Title
- Test Objective
- Preconditions
- Test Steps
- Test Data
- Expected Result
- Priority
- Severity
- Automation Candidate
- Automation Priority
- Execution Type (Manual / Automated)
- Status
- Remarks

---

## Test Case Coverage

Generate test cases covering:

- Positive
- Negative
- Boundary
- Validation
- Business Rules
- Security
- Session Management
- Error Handling
- Authorization
- Input Validation

---

## Automation Readiness

Each test case must clearly indicate:

- Suitable for Playwright (Yes/No)
- Automation Priority
- Any prerequisite test data
- Any environment dependency

---

## Validation

Before completing the phase:

Verify:

- Every Requirement ID exists.
- Every Scenario ID exists.
- No duplicate Test Cases.
- No orphan Test Cases.
- All Test Cases are traceable.
- Login Module remains the only implementation scope.

---

## Completion Criteria

When complete, provide:

1. Executive Summary

2. Statistics

- Total Test Cases
- Positive
- Negative
- Security
- Automation Candidates
- High Priority

3. Requirement Coverage

4. Scenario Coverage

5. Generated File List

6. Risks / Non-blocking Observations

Do NOT generate Playwright scripts.

Do NOT generate Test Execution.

Stop after completing the Test Case phase and wait for user review.Start Phase 4 – Enterprise Test Case Generation.

Follow:

- Prompts/00_Execution_Rules.md
- Prompts/06_Test_Cases.md

All prerequisite phases are complete and approved.

Current implementation scope is Login Module only.

Generate all Test Case deliverables defined in the prompt.

Generate the Excel workbook and supporting markdown documents.

Ensure every Test Case is directly traceable to:
Requirement → Scenario → Test Case

Ensure every Test Case is automation-ready for future Playwright implementation.

Do not generate Playwright scripts.

Stop after completing the Test Case phase and wait for my review.