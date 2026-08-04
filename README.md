# HealixCare HMS — Automation POC

## 1. Project Overview

HealixCare HMS is an existing, feature-complete Hospital Management System (modular monolith, FastAPI + PostgreSQL backend, React frontend), covering registration, scheduling, EMR, billing, and more across on-premise and SaaS deployment modes. This repository is a **Proof of Concept (POC)** for automating regression coverage of HealixCare HMS using **Playwright with TypeScript**, following the Page Object Model.

The POC follows a phased, documentation-first approach: no automation is written until requirements have been audited, analyzed, and turned into an approved test plan and test scenarios.

## 2. Objectives

- Validate a Playwright + TypeScript + Page Object Model framework against a real, running HMS build.
- Prove out end-to-end automated coverage for two foundational flows: **Login** and **Patient Registration**.
- Establish reusable patterns (page objects, fixtures, test data, CI wiring) that later phases can extend to additional modules (Appointments, Billing, EMR, etc.) without rework.
- Produce professional, reviewable documentation at every phase — audit, requirement analysis, test plan, test scenarios — before any code is written.

## 3. Folder Structure

```
HMS_POC/
├── Prompts/                        # Phase-by-phase task prompts driving this POC
│   ├── 01_Document_Audit.md
│   ├── 02_Requirement_Analysis.md
│   ├── 03_Test_Plan.md
│   └── 04_Test_Scenarios.md
├── RequiementDoucments/             # Source requirement documents (business, functional, workflow)
│   ├── Core_Business/               # Vision, roadmap, backlog, role matrix
│   ├── Phase1/                      # Functional Requirements Documents (FRDs) per module
│   └── Workflow/                    # Cross-module and per-module workflow documents
├── Output/                          # Deliverables produced by each POC phase
│   ├── 01_Document_Inventory.md
│   ├── 02_Document_Classification.md
│   ├── 03_Reading_Order.md
│   └── 04_Required_Documents_For_POC.md
├── Claude.md                        # Project instructions / working principles for the AI-assisted workflow
├── .gitignore
└── README.md
```

> Automation scaffolding (`tests/`, `pages/`, `fixtures/`, `playwright.config.ts`, `package.json`, `tsconfig.json`) does not exist yet — it will be added starting from the Automation phase, once the Test Plan and Test Scenarios are approved.

## 4. Technology Stack

| Layer | Choice |
|---|---|
| Automation Framework | Playwright |
| Language | TypeScript |
| Design Pattern | Page Object Model (POM) |
| Runtime | Node.js |
| System Under Test | HealixCare HMS (FastAPI backend, React frontend, PostgreSQL) |
| Documentation | Markdown |

*(Test runner/reporter and CI tooling to be confirmed when the automation scaffolding is set up.)*

## 5. Current Phase

**Phase 1 — Documentation Audit: ✅ Complete and approved.**

Scope for this POC has been locked to two modules:
- **Login (User Management & Authentication)**
- **Patient Registration**

All other modules (Appointments, Billing, EMR, Admin Settings, Queue & Check-in, Lab, Radiology, Pharmacy, IPD, Insurance/TPA, and the Doctor/Patient Portals) are explicitly **out of scope** for this POC.

**Phase 2 — Requirement Analysis: not yet started**, pending explicit go-ahead.

## 6. Roadmap

| Phase | Status |
|---|---|
| 1. Documentation Audit | ✅ Complete |
| 2. Requirement Analysis | ⏳ Not started |
| 3. Test Plan | ⏳ Not started |
| 4. Test Scenarios | ⏳ Not started |
| 5. Playwright Automation (Login + Patient Registration) | ⏳ Not started |
| 6. CI Integration | ⏳ Not started |
| 7. Scope expansion to additional modules | ⏳ Future |

## 7. How to Run

Automation scaffolding does not exist yet. Once it is added, this section will be updated with concrete steps. Expected shape:

```bash
# Install dependencies
npm install

# Configure environment (base URL, test credentials) in .env — see .env.example
cp .env.example .env

# Run all tests
npx playwright test

# View the HTML report
npx playwright show-report
```

## 8. Future Enhancements

- CI pipeline (GitHub Actions) running the Playwright suite on every Pull Request.
- Expand automated coverage to additional modules per the HMS Scope and Module Roadmap (Appointments, Billing, EMR, etc.), once each has an approved Test Plan.
- API-level test data setup/teardown once an API Catalog is available (currently a documented gap — see `Output/04_Required_Documents_For_POC.md`).
- Visual regression testing, contingent on confirming whether UI mockups referenced in the FRDs exist as usable design artifacts.
