# Sprint 3 Demo Grading Rubric (Fall 2026)

_ReviewLens AI - CS 490 Capstone_

## 1. Purpose and Scoring Model

This rubric is used to evaluate the Sprint 3 ReviewLens AI demonstration.

The student-facing `Sprint3Demo.md` defines what teams are expected to prepare and demonstrate. This rubric is primarily an instructor scoring tool.

### Objective Checklist

There are **20 objective checklist items**.

Each item is scored:

- **0 — Not demonstrated / substantially incorrect**
- **1 — Partially demonstrated / materially incomplete**
- **2 — Fully demonstrated / meets the requirement**

Objective subtotal:

`20 items × 2 points = 40 points maximum`

### Subjective Engineering and Presentation Score

Award **0 to 10 points** based on the complete demonstration.

Consider:

1. Technical understanding.
2. Product and architecture quality.
3. Ability to explain implementation decisions and tradeoffs.
4. Understanding of persistence and lifecycle behavior.
5. Understanding of evidence, data integrity, and ownership boundaries.
6. Understanding of the student-designed feature and why it belongs in ReviewLens.
7. Quality of the team's story decomposition for the student-designed feature.
8. Ability to answer questions accurately and concisely.
9. Quality and organization of the presentation.
10. Evidence that multiple team members understand the completed system.

Suggested scale:

- **0-2:** weak technical understanding
- **3-4:** limited understanding
- **5-6:** competent understanding
- **7-8:** strong technical reasoning
- **9-10:** excellent engineering understanding and presentation

Base score:

`Objective Checklist (0-40) + Subjective Score (0-10) = 50 points maximum before deductions`

---

## 2. Demo Bug / Fault Deductions

Subtract **1 point for each confirmed bug or fault** exposed during the demo.

There is no fixed cap.

A confirmed bug/fault includes:

1. Crash, unhandled runtime error, or broken required flow.
2. Behavior that clearly contradicts a Sprint 1, Sprint 2, or Sprint 3 requirement.
3. Required evidence that should exist but is unavailable when requested.
4. User-visible error handling that exposes raw exceptions, stack traces, credentials, provider errors, or other technical failure information not meaningful to an average user.
5. Saved-analysis behavior that unexpectedly changes or corrupts another analysis.
6. Q&A history that appears under the wrong analysis.
7. Re-ingestion that duplicates reviews incorrectly, changes valid data after a failed refresh, or leaves the current dataset ambiguous.
8. Supporting evidence that does not belong to the active AnalysisTarget or does not correspond to persisted review data.
9. Filtering or export that returns incorrect, stale, or unintended data.
10. Student-designed feature behavior that contradicts the approved proposal or corrupts existing ReviewLens behavior.
11. Public deployment behavior that differs materially from the required completed ReviewLens workflow.
12. A previously demonstrated valid workflow that breaks later in the same demo.

A deliberately demonstrated negative case is not a bug when the application handles the failure correctly.

---

## 3. Demo Readiness Deduction

Deduct **0 to 5 points** for avoidable lack of preparation.

- **0:** Fully prepared; no meaningful avoidable delays.
- **-1:** Minor preparation issue.
- **-2:** Noticeable preparation problems or unnecessary delays.
- **-3:** Significant preparation deficiencies consume meaningful demo time.
- **-4:** Major readiness problems prevent several required items from being demonstrated efficiently.
- **-5:** Team is substantially unprepared.

Readiness deductions may apply when the public application, production account, saved analyses, Q&A history, review data, refresh input, export evidence, student-designed feature, approved Discord proposal, GitHub Project stories, tests, CI evidence, or other expected demo prerequisites were not prepared in advance.

A temporary third-party review-source or LLM outage by itself is not a readiness problem when the team has prepared appropriate fallback evidence and continues the demo efficiently.

---

## 4. Objective Checklist

Score each item `0`, `1`, or `2`.

### A. Saved Analysis Lifecycle

| ID  | Criterion                                                          | Evidence to Observe                                                                                                                                     | Sprint 3 Story Coverage | Score |
| --- | ------------------------------------------------------------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ----- |
| C01 | Returning user can reopen a saved analysis                         | Earlier-session analysis restores its human-readable name, source/entity context, current review dataset, ingestion summary, and persisted Q&A history  | S3-001, S3-004, S3-007  |       |
| C02 | User can rename a saved analysis correctly                         | Display name changes while source URL, review dataset, and analysis identity remain unchanged                                                           | S3-002                  |       |
| C03 | Save As creates an independent analysis                            | Copy has a separate identity and expected persisted state; later changes to the copy do not mutate the original                                         | S3-003                  |       |
| C04 | Persisted Q&A history remains associated with the correct analysis | Previously saved question/response pairs return in order, and switching analyses changes the visible history appropriately                              | S3-006, S3-007          |       |
| C05 | User can delete an owned analysis safely                           | Confirmation is clear, deleted analysis disappears from normal retrieval, and associated-data behavior is consistent with the team's persistence design | S3-005                  |       |

### B. Evidence-Backed Analysis and Review Exploration

| ID  | Criterion                                           | Evidence to Observe                                                                                                                        | Sprint 3 Story Coverage | Score |
| --- | --------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------ | ----------------------- | ----- |
| C06 | Grounded Q&A displays supporting review evidence    | Answer includes representative persisted reviews that support the response and belong to the active AnalysisTarget                         | S3-013                  |       |
| C07 | User can inspect supporting or matching reviews     | Application exposes the broader relevant review set when available and distinguishes it from the complete dataset                          | S3-014                  |       |
| C08 | User can browse the complete current review dataset | Complete active dataset is inspectable with useful review fields rather than only a small preview                                          | S3-015                  |       |
| C09 | Rating and date filters work correctly              | Demonstrated filters change the visible result set correctly, preserve active-analysis scope, and handle available date data appropriately | S3-016                  |       |

### C. Review Refresh and Data Integrity

| ID  | Criterion                                                 | Evidence to Observe                                                                            | Sprint 3 Story Coverage | Score |
| --- | --------------------------------------------------------- | ---------------------------------------------------------------------------------------------- | ----------------------- | ----- |
| C10 | Existing analysis can be refreshed/re-ingested            | User initiates a new ingestion attempt without creating a new AnalysisTarget                   | S3-009                  |       |
| C11 | Successful refresh updates the current dataset coherently | Ingestion summary and active review data reflect the accumulated current dataset after refresh | S3-010                  |       |
| C12 | Re-ingestion prevents duplicate source reviews            | Team demonstrates or shows test evidence for a deterministic duplicate-prevention strategy     | S3-011                  |       |
| C13 | Failed refresh preserves the last known-good dataset      | Failure evidence shows that valid persisted review data remains unchanged and usable           | S3-012                  |       |

### D. Export and Public Delivery

| ID  | Criterion                                            | Evidence to Observe                                                                                                                          | Sprint 3 Story Coverage | Score |
| --- | ---------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------- | ----- |
| C14 | Active review dataset exports correctly to CSV       | Download contains representative current persisted review data and a meaningful filename                                                     | S3-017, S3-019          |       |
| C15 | Saved analysis exports correctly to Markdown         | Download contains useful analysis context, summary, Q&A history when present, supporting evidence when applicable, and a meaningful filename | S3-018, S3-019          |       |
| C16 | Complete ReviewLens application is publicly deployed | Stable public URL supports the required Sprint 3 ReviewLens workflow demonstrated during the presentation                                    | S3-020                  |       |

### E. Student-Designed Product Extension

| ID  | Criterion                                                                                                                       | Evidence to Observe                                                                                                                                      | Sprint 3 Story Coverage        | Score |
| --- | ------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------ | ----- |
| C17 | Student-designed feature solves a meaningful ReviewLens user problem and is integrated into the product                         | Demonstrated workflow matches the approved Discord proposal and appears as a coherent ReviewLens capability rather than a disconnected page or prototype | Team-authored Sprint 3 stories |       |
| C18 | Student-designed feature demonstrates full-stack engineering depth                                                              | Feature includes meaningful frontend/UX behavior and corresponding backend/API or service logic                                                          | Team-authored Sprint 3 stories |       |
| C19 | Student-designed feature introduces meaningful persisted state or data relationships and preserves required security boundaries | Demonstration and explanation show persistent data/data-model impact plus appropriate authentication, ownership, or security behavior                    | Team-authored Sprint 3 stories |       |

### F. Story Decomposition, Tests, and CI

| ID  | Criterion                                                                                                          | Evidence to Observe                                                                                                                                                               | Sprint 3 Story Coverage                                              | Score |
| --- | ------------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------- | ----- |
| C20 | Team-authored feature stories, representative automated tests, and CI evidence support the completed Sprint 3 work | GitHub Project contains sensible approved-feature story decomposition; representative tests include a non-happy-path feature case; recent CI run passes established quality gates | Student-designed feature requirement and Sprint 3 Definition of Done |       |
