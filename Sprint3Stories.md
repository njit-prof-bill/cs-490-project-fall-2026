# ReviewLens AI - Sprint 3 Stories (Fall 2026)

This is the Sprint 3 backlog for the ReviewLens AI capstone project.

Story writing format for Jira:

- Story ID
- Title
- Outcome — what must be true when complete
- Technical Guidance — implementation boundaries and expectations
- Rules — applicable canonical business rules

Sprint 3 focus: saved-analysis lifecycle, persistent Q&A history, review refresh, evidence exploration, export, a student-designed product extension, and public delivery.

Instructor-defined stories: 20.

Each team must also define and add the backlog stories required to implement its approved student-designed product feature.

Sprint 1 and Sprint 2 requirements remain in force unless explicitly changed here.

## How To Read This Sprint

1. These active sprint requirements define required Sprint 3 scope and grading expectations.
2. The Product Requirements Document defines the overall product direction.
3. UX specifications define the intended user experience.
4. Technical specifications define project-level engineering guardrails.
5. Teams should make reasonable engineering decisions where implementation details are unspecified.
6. The objective of Sprint 3 is to complete ReviewLens as a useful product, not to build unnecessary production infrastructure.
7. In addition to the 20 instructor-defined stories, each team must design one substantial ReviewLens product feature, obtain instructor approval, decompose the approved feature into backlog stories, and implement those stories during Sprint 3.

## Sprint 3 Demonstration Goal

The final ReviewLens application should demonstrate this complete workflow:

1. A user accesses ReviewLens at a public URL and authenticates.
2. The user reopens a previously saved analysis from the analysis sidebar.
3. The analysis has a meaningful human-readable name.
4. Previously persisted review data and Q&A history are available.
5. The user can rename or Save As an analysis.
6. Grounded answers include useful supporting review evidence.
7. The user can inspect and filter the underlying review dataset.
8. The user can refresh or re-ingest an existing analysis without corrupting the current dataset.
9. The user can export review data to CSV.
10. The user can export the analysis to Markdown.
11. The user can delete an analysis they own.
12. A second authenticated user remains isolated from the first user's data.
13. The team demonstrates its approved student-designed product feature as an integrated part of ReviewLens.
14. The complete workflow operates from the publicly deployed application.

The demo should present these capabilities as one coherent product experience rather than as a sequence of Jira stories.

---

# CI/CD and Testing Requirements

All Sprint 1 and Sprint 2 CI/CD and testing requirements remain mandatory.

Sprint 3 does **not** add a new infrastructure-heavy CI/CD ratchet.

Every pull request must continue to run the team's established required checks, including:

1. Lint or equivalent static checks.
2. Application build.
3. Automated unit tests.
4. Required integration tests.
5. Changed-code coverage gate or equivalent mechanism.
6. Required AI orchestration and scope-guard tests.
7. Required checks must pass before merge through the normal protected-branch workflow.

## Sprint 3 Test Baseline

1. Every Sprint 3 feature story, including team-authored stories for the student-designed product feature, must include appropriate automated test updates.
2. Sprint 1 and Sprint 2 regression tests must continue to pass.
3. Saved-analysis lifecycle tests must verify user ownership.
4. Q&A-history tests must verify correct AnalysisTarget association.
5. Re-ingestion tests must verify dataset integrity and failure safety.
6. Evidence tests must verify that cited reviews belong to the active dataset.
7. Export tests should verify content and ownership boundaries.
8. Public deployment does not require a specialized release-gate workflow, production smoke-test suite, dedicated health endpoint, security-scanning platform, or advanced deployment strategy unless the team chooses to add one.

## Definition of Done Additions

A Sprint 3 story is not complete until:

1. The story outcome works in the running application.
2. Required automated tests pass locally.
3. Required automated tests pass in CI.
4. Sprint 1 and Sprint 2 regression tests continue to pass.
5. Ownership and user-isolation rules remain intact.
6. Q&A grounding and scope-guard behavior remain intact.
7. The pull request contains useful test evidence.
8. Production-facing functionality works from the public deployment when applicable.

---

# Canonical Business Rules

## S3-BR Saved Analysis Lifecycle

1. **S3-BR-001**: Every saved analysis must have a human-readable display name.
2. **S3-BR-002**: Analysis names are user-editable and do not change the underlying source URL or review data.
3. **S3-BR-003**: A returning authenticated user must be able to reopen their saved analyses.
4. **S3-BR-004**: Saved analyses must remain isolated by authenticated owner.
5. **S3-BR-005**: Save As must create a separate analysis identity rather than silently renaming the original.
6. **S3-BR-006**: Future changes to a Save As copy must not mutate the original analysis.
7. **S3-BR-007**: A user may delete only analyses they own.
8. **S3-BR-008**: Deletion must handle associated ReviewLens data consistently according to the team's documented persistence design.

## S3-BR Q&A History

1. **S3-BR-009**: Questions and ReviewLens responses may be persisted as part of an analysis.
2. **S3-BR-010**: Persisted Q&A history must remain associated with the correct AnalysisTarget.
3. **S3-BR-011**: Reopening an analysis must restore its persisted Q&A history when history exists.
4. **S3-BR-012**: Clearing Q&A history must not delete the AnalysisTarget or its review dataset.
5. **S3-BR-013**: Q&A history from one analysis must not appear under another analysis.

## S3-BR Re-Ingestion and Refresh

1. **S3-BR-014**: A user may refresh or re-ingest an existing owned analysis.
2. **S3-BR-015**: The application must clearly identify the current review dataset as the deduplicated union of previously persisted reviews and successfully fetched reviews, unless the provider explicitly confirms a complete replacement dataset.
3. **S3-BR-016**: A failed refresh must not destroy or change the last known-good review dataset.
4. **S3-BR-017**: Re-ingestion must not silently create duplicate review records representing the same source review.
5. **S3-BR-018**: The team's duplicate strategy must be deterministic and testable.
6. **S3-BR-019**: A successful refresh must update the ingestion summary to represent the accumulated current dataset, including newly discovered reviews and previously persisted reviews retained from a partial or throttled response.

## S3-BR Evidence and Review Exploration

1. **S3-BR-020**: Grounded Q&A answers must be able to present supporting evidence from the active review dataset.
2. **S3-BR-021**: Supporting evidence must refer only to reviews belonging to the active AnalysisTarget.
3. **S3-BR-022**: Supporting evidence must not fabricate review text or reviewer metadata.
4. **S3-BR-023**: The user must be able to inspect more than the small ingestion-preview sample.
5. **S3-BR-024**: Review filtering must operate on the active AnalysisTarget's persisted review data.
6. **S3-BR-025**: ReviewLens must not present a numerical confidence score unless the team has a defensible and documented method for calculating it.

## S3-BR Export

1. **S3-BR-026**: A user may export only data belonging to analyses they own.
2. **S3-BR-027**: CSV export must represent the active persisted review dataset.
3. **S3-BR-028**: Markdown export must provide a human-readable representation of the saved analysis.
4. **S3-BR-029**: Exported files must use meaningful filenames.
5. **S3-BR-030**: Export content must not silently include another user's review or Q&A data.

## S3-BR Public Delivery

1. **S3-BR-031**: ReviewLens must be reachable at a public web URL for the final demonstration.
2. **S3-BR-032**: The public deployment must preserve authentication, ownership, ingestion, Q&A grounding, scope guards, saved-analysis behavior, and export functionality.
3. **S3-BR-033**: Production secrets must not be committed to source control or exposed to browser-delivered code.
4. **S3-BR-034**: A normal managed-platform deployment is sufficient. Specialized production operations infrastructure is not required.

---

# A. Saved Analysis Lifecycle

## S3-001 - Name and Save an Analysis

**Outcome:**
A user can save an analysis with a short human-readable name that is shown in the analysis sidebar.

**Technical Guidance:**

The display name should be independent of the full review-source URL.

The application may suggest an initial name based on the detected entity or source, but the user must be able to choose a meaningful name.

The saved record should preserve at least:

1. Analysis name.
2. Owner.
3. Source URL or source reference.
4. AnalysisTarget identity.
5. Current review dataset association.

**Rules:** S3-BR-001 through S3-BR-004

## S3-002 - Rename a Saved Analysis

**Outcome:**
A user can rename an analysis they own without changing its review source, review dataset, or Q&A data.

**Technical Guidance:**

Rename should update only the user-facing analysis name.

Validation should reject empty or otherwise unusable names.

Another user must not be able to rename the analysis.

**Rules:** S3-BR-001 through S3-BR-004

## S3-003 - Save As a Separate Analysis

**Outcome:**
A user can create a separate saved analysis based on the current analysis and assign the copy a new name.

**Technical Guidance:**

Save As must create a new analysis identity.

The new analysis should begin with the current saved state of the source analysis, including the current persisted review dataset.

If Q&A history is already implemented, the team may copy or omit the history, but the chosen behavior must be clear and consistent.

After Save As:

1. Renaming the copy does not rename the original.
2. Deleting the copy does not delete the original.
3. Refreshing/re-ingesting the copy does not mutate the original.

**Rules:** S3-BR-005, S3-BR-006

## S3-004 - Reopen a Saved Analysis

**Outcome:**
A returning authenticated user can reopen an analysis created during an earlier session.

**Technical Guidance:**

Reopening should restore the current ReviewLens workspace for that analysis.

At minimum, restore:

1. Analysis name.
2. Source/entity context.
3. Current ingestion state.
4. Current persisted review dataset.
5. Ingestion summary.

Persisted Q&A history is addressed separately.

**Rules:** S3-BR-003, S3-BR-004

## S3-005 - Delete an Owned Analysis

**Outcome:**
A user can delete an analysis they own.

**Technical Guidance:**

The UI should clearly communicate destructive intent and request confirmation.

The team must define how associated records are handled.

Possible designs include:

1. Cascading deletion.
2. Soft deletion.
3. Another consistent documented strategy.

Tests must verify:

1. Owner can delete the analysis.
2. Deleted analysis no longer appears in normal retrieval.
3. Another user cannot delete the analysis.

**Rules:** S3-BR-007, S3-BR-008

---

# B. Persistent Q&A History

## S3-006 - Persist Questions and ReviewLens Responses

**Outcome:**
Questions and ReviewLens responses can be stored as part of the saved analysis.

**Technical Guidance:**

Persist enough information to reconstruct the user-visible analysis history.

At minimum, consider:

1. Question text.
2. ReviewLens response.
3. Creation timestamp.
4. AnalysisTarget association.

If supporting evidence is persisted with the response, retain stable references where practical rather than duplicating large amounts of review text unnecessarily.

**Rules:** S3-BR-009, S3-BR-010

## S3-007 - Restore Q&A History When Reopening an Analysis

**Outcome:**
When a user reopens a saved analysis, its persisted Q&A history is restored in the correct order.

**Technical Guidance:**

History must belong to the active analysis.

Switching analyses must replace the visible history with the newly selected analysis's history.

Tests should use clearly distinguishable questions across multiple AnalysisTargets.

**Rules:** S3-BR-010, S3-BR-011, S3-BR-013

## S3-008 - Clear Q&A History

**Outcome:**
A user can clear the Q&A history for an analysis without deleting the analysis or its review dataset.

**Technical Guidance:**

The UI should clearly distinguish:

- Clear Q&A History
- Delete Analysis

Clearing history must not remove:

1. AnalysisTarget.
2. Source URL.
3. Persisted reviews.
4. Ingestion summary.

**Rules:** S3-BR-012

---

# C. Review Refresh and Re-Ingestion

## S3-009 - Re-Ingest an Existing Analysis

**Outcome:**
A user can initiate a new ingestion operation for an existing owned analysis.

**Technical Guidance:**

The user should not need to create a second AnalysisTarget merely to obtain newer review data.

The application should record enough ingestion state to distinguish the new attempt from the previously successful dataset.

Because the review provider may throttle responses or return only a partial batch, re-ingestion must merge newly fetched reviews into the existing persisted dataset. Reviews already collected must remain current when they are absent from a later partial response.

The application may remove or deactivate previously collected reviews only when the provider explicitly confirms that the response represents the complete source dataset.

**Rules:** S3-BR-014, S3-BR-015

## S3-010 - Promote a Successful Refresh to the Current Dataset

**Outcome:**
After successful re-ingestion, ReviewLens clearly treats the resulting review dataset as the current dataset for summary, browsing, Q&A, and export.

**Technical Guidance:**

After a successful refresh, the current dataset is the deduplicated union of previously persisted reviews and newly fetched reviews. Existing reviews with the same stable identity should be reconciled with updated source fields; reviews absent from a partial response must remain current.

After successful refresh:

1. Ingestion summary reflects the current data.
2. Review browsing reflects the current data.
3. Q&A uses the current data.
4. Export uses the current data.

5. The ingestion summary reflects the accumulated current dataset, including previously collected reviews retained because the provider response was partial or throttled.

**Rules:** S3-BR-015, S3-BR-019

## S3-011 - Prevent Duplicate Reviews During Re-Ingestion

**Outcome:**
Refreshing an analysis does not silently create duplicate Review records for the same source review.

**Technical Guidance:**

Prefer stable source review identifiers when available.

When a source does not provide stable identifiers, the team may use another deterministic strategy such as a normalized fingerprint derived from stable review attributes.

The duplicate strategy should be documented and automatically tested.

Re-ingestion must not create a second record for an existing source review, even when the same review is returned in multiple refresh batches.

**Rules:** S3-BR-017, S3-BR-018

## S3-012 - Preserve Last Known-Good Data When Refresh Fails

**Outcome:**
If re-ingestion fails, the previously successful review dataset remains available and remains the current usable dataset.

**Technical Guidance:**

A failed refresh should record a failed ingestion attempt without replacing valid persisted review data with an empty or partial accidental result. A failed refresh must not change the current review set.

The user should receive a meaningful failure state.

**Rules:** S3-BR-016

---

# D. Evidence and Review Exploration

## S3-013 - Show Supporting Evidence with Grounded Answers

**Outcome:**
A grounded ReviewLens answer displays representative review evidence supporting the analysis.

**Technical Guidance:**

Supporting evidence should come from the active persisted review dataset.

A reasonable response may show two to five representative review excerpts.

The application should preserve enough identity information to verify that cited evidence corresponds to real persisted Review records.

Do not fabricate quotations or reviewer metadata.

**Rules:** S3-BR-020 through S3-BR-022

## S3-014 - View Matching or Supporting Reviews

**Outcome:**
A user can inspect the larger set of reviews identified as relevant to a grounded answer when such a set exists.

**Technical Guidance:**

The exact implementation depends on the team's Q&A context strategy.

Examples include:

1. Reviews retrieved for the question.
2. Reviews cited by a structured model response.
3. Reviews selected by a search/ranking layer.

The UI should clearly distinguish these from the complete review dataset.

**Rules:** S3-BR-020 through S3-BR-024

## S3-015 - Browse the Complete Review Dataset

**Outcome:**
A user can inspect the complete current persisted review dataset for the active analysis.

**Technical Guidance:**

A table, list, card view, or another coherent design is acceptable.

At minimum, expose useful review information such as:

1. Rating.
2. Review text.
3. Date when available.
4. Reviewer/display name when available.

Pagination or incremental loading is acceptable for larger datasets.

**Rules:** S3-BR-023, S3-BR-024

## S3-016 - Filter the Active Review Dataset

**Outcome:**
A user can apply rating and date filters to the active analysis and see only matching reviews throughout the review-browsing experience.

**Technical Guidance:**

The filtering interface must support:

1. A minimum rating.
2. A maximum rating.
3. A start date when review dates are available.
4. An end date when review dates are available.

The minimum and maximum rating fields define an inclusive rating range. For example:

- Minimum `1`, maximum `2` shows one- and two-star reviews.
- Minimum `4`, maximum `5` shows four- and five-star reviews.
- Maximum `2` shows reviews rated two stars or below.
- Minimum `4` shows reviews rated four stars or above.

The start and end dates define an inclusive date range. Reviews without a source date must not match a date filter.

When multiple filters are provided, they must be combined using AND semantics. A review must satisfy every active filter to be included.

Filters must operate against the active AnalysisTarget's current persisted review dataset. They must not query another AnalysisTarget, another ingestion dataset, or another user's data.

The user must be able to:

1. Review pending filter selections before applying them.
2. Apply the selected filters.
3. Cancel pending changes without changing the current results.
4. Clear all active filters and return to the complete current dataset.

After filters are applied:

1. The Review Preview must display only matching reviews.
2. The complete review browser must display only matching reviews.
3. The displayed matching-review count must update.
4. Pagination or incremental loading, if used, must operate on the filtered result set.
5. The active filter state must be visually apparent.
6. A zero-result state must clearly indicate that no reviews match the selected filters and must not be presented as an ingestion failure.

The filtering operation must not mutate persisted reviews, the AnalysisTarget, ingestion history, Q&A history, or the underlying current dataset.

Automated tests must verify:

1. Minimum-rating filtering.
2. Maximum-rating filtering.
3. Combined minimum and maximum rating filtering.
4. Inclusive date-range filtering.
5. Combined rating and date filtering.
6. Clearing filters.
7. Zero matching results.
8. Filtered results remain scoped to the active AnalysisTarget.
9. Cross-user filtering is denied or isolated according to the application's ownership rules.

The team may add search or other filters after the required behavior is complete.

**Rules:** S3-BR-024

---

# E. Export

## S3-017 - Export the Active Review Dataset to CSV

**Outcome:**
A user can download the active analysis's current persisted review dataset as CSV.

**Technical Guidance:**

Export useful fields that actually exist in the team's canonical Review model.

Examples include:

1. Rating.
2. Review text.
3. Review date.
4. Reviewer/display name.
5. Source review identifier.
6. Source URL.

Do not fabricate unavailable values merely to fill columns.

**Rules:** S3-BR-026, S3-BR-027, S3-BR-030

## S3-018 - Export the Analysis to Markdown

**Outcome:**
A user can download a human-readable Markdown representation of the saved analysis.

**Technical Guidance:**

The export should include useful context such as:

1. Analysis name.
2. Entity/source information.
3. Ingestion summary.
4. Q&A history when present.
5. Supporting evidence when associated with persisted Q&A.

The exact presentation is a team design decision.

**Rules:** S3-BR-026, S3-BR-028, S3-BR-030

## S3-019 - Use Meaningful Export Names and Metadata

**Outcome:**
Exported files are easy for a user to identify outside ReviewLens.

**Technical Guidance:**

Use filenames based on the analysis name and export type rather than generic names such as `export.csv`.

Examples:

- `blue-bottle-mint-plaza-reviews.csv`
- `blue-bottle-mint-plaza-analysis.md`

Include useful generated/exported timestamps or source metadata inside the exported content when appropriate.

**Rules:** S3-BR-029

---

# F. Public Delivery

## S3-020 - Deploy the Complete ReviewLens Application Publicly

**Outcome:**
The completed ReviewLens application is available at a stable public web URL for the final demonstration.

**Technical Guidance:**

A normal managed-platform deployment is sufficient.

The public deployment must preserve:

1. Authentication.
2. User ownership and isolation.
3. Saved-analysis lifecycle.
4. Ingestion and refresh.
5. Ingestion summary.
6. Review browsing.
7. Review-grounded Q&A.
8. Scope guards.
9. Q&A history.
10. Export.

Production secrets must be supplied through appropriate environment or platform configuration rather than committed to source control.

Specialized production operations infrastructure is **not required**.

Teams are not required to add:

1. Kubernetes.
2. Blue/green deployment.
3. Canary deployment.
4. Multi-region infrastructure.
5. A dedicated production smoke-test suite.
6. A custom health-check framework.
7. A separate release-gate workflow.
8. Advanced observability infrastructure.
9. Automated rollback.
10. A security-scanning platform beyond existing course/project expectations.

**Rules:** S3-BR-031 through S3-BR-034

---

# G. Student-Designed Product Extension

In addition to the 20 instructor-defined Sprint 3 stories, each team must design and implement one substantial new ReviewLens product feature.

This requirement is intentionally not decomposed into instructor-authored stories. Part of the Sprint 3 assignment is for the team to define the feature, obtain approval, and turn the approved feature into appropriate backlog stories.

## Product Feature Requirement

The feature must solve a plausible ReviewLens user problem that is not already satisfied by the required Sprint 3 stories.

An acceptable feature must:

1. Add a meaningful user-facing workflow or capability.
2. Require new backend behavior or business logic.
3. Introduce new persisted application state or a meaningful new relationship among persisted data.
4. Preserve ReviewLens authentication, ownership, and user-isolation requirements.
5. Include meaningful automated tests, including at least one non-happy-path behavior.
6. Integrate naturally into the existing ReviewLens product rather than appearing as a disconnected demonstration page.
7. Require coordinated changes across multiple application layers.

The expected implementation should normally affect:

1. User experience and frontend behavior.
2. Backend/API or service behavior.
3. Persistence or data relationships.
4. Automated tests.

The purpose of this requirement is to give each team responsibility for making a small but meaningful product decision and carrying that decision through requirements, design, implementation, persistence, security, testing, and demonstration.

## Feature Proposal and Instructor Approval

Before implementation begins, the team must post a short feature proposal in its **team Discord channel**.

The proposal must describe:

1. The user problem the feature will solve.
2. The intended user workflow.
3. The expected UX changes.
4. The backend/API or service behavior required.
5. The new persisted state or data relationships required.
6. Authentication, ownership, or security implications.
7. The major automated tests the team expects to need.

The feature must receive **instructor approval in the team Discord channel before implementation begins**.

The instructor may approve the proposal as written, request changes, or reject a proposal that is too small, too large, insufficiently integrated, or otherwise inappropriate for the sprint.

If the team later changes the feature materially from the approved proposal, the revised scope should be posted in the team Discord channel for approval before the team continues with the changed design.

## Team-Authored Backlog Stories

After the feature is approved, the team must decompose it into appropriate Sprint 3 backlog stories and add those stories to the team's GitHub Project.

A single catch-all story such as `Implement custom feature` is not sufficient.

The team-authored stories should:

1. Describe observable outcomes rather than vague implementation tasks.
2. Divide the feature into sensible units of work.
3. Make dependencies or sequencing clear where they matter.
4. Include testable acceptance criteria or equivalent completion expectations.
5. Identify important technical, persistence, ownership, or security constraints.
6. Be detailed enough that another team member can understand what must be delivered.

Teams should use the same general level of specificity as the instructor-defined Sprint 3 stories.

The number of stories is not prescribed. The decomposition should reflect the actual scope and architecture of the approved feature rather than artificially splitting work to produce a particular story count.

## Example Features

The following examples illustrate the expected level of product and engineering scope.

Teams are **encouraged to propose an original feature of their own design**. The examples are provided to establish the expected scope, not to define preferred solutions.

### Example 1 - Secure Read-Only Analysis Sharing

Allow the owner of an analysis to create a read-only share link for that analysis.

A complete implementation might include:

1. UX for creating, viewing, and revoking a share.
2. A persisted share/grant record associated with the owned analysis.
3. A secure share token or equivalent access mechanism.
4. Backend authorization that exposes only the shared analysis.
5. Read-only behavior for the recipient.
6. Handling of revoked, invalid, or expired shares if expiration is supported.
7. Automated tests for authorized, revoked, invalid, and cross-user behavior.

### Example 2 - Review Annotations and Tags

Allow a user to create personal tags and annotations for reviews within an analysis.

A complete implementation might include:

1. UX for creating, editing, and deleting tags or annotations.
2. Persisted tag and annotation data.
3. Relationships between tags, annotations, reviews, and the owning user.
4. Filtering or finding reviews by user-created tags.
5. Ownership enforcement so one user cannot access another user's annotations.
6. Automated tests for persistence, ownership, update/delete behavior, and filtering.

Simply adding a single notes field without a meaningful lifecycle or supporting workflow would not satisfy this example.

### Example 3 - Saved Analysis Comparison

Allow a user to select two analyses they own, create a named comparison, and reopen that comparison later.

A complete implementation might include:

1. UX for selecting analyses and creating a comparison.
2. A persisted Comparison entity or equivalent relationship between analyses.
3. Backend logic for retrieving and presenting useful information from both datasets.
4. Authorization across both referenced analyses.
5. Reopen and delete behavior for saved comparisons.
6. Defined behavior when one of the referenced analyses becomes unavailable.
7. Automated tests covering persistence, ownership, lifecycle behavior, and comparison results.

## Features That Are Too Small By Themselves

The following changes do **not** satisfy the student-designed product feature requirement by themselves:

1. Dark mode, color themes, or primarily cosmetic changes.
2. Adding another simple sort option.
3. Adding another simple filter to the existing review filter feature.
4. Adding another export format.
5. Adding a single field to an existing record.
6. Adding a static dashboard card, chart, or informational panel.
7. A UI-only enhancement with no meaningful backend behavior.
8. A backend-only refactor with no new user capability.
9. Calling an additional third-party API without introducing meaningful ReviewLens product behavior.

A team may include changes like these as part of a larger approved feature, but they are not substantial enough to satisfy this requirement on their own.

---

# Sprint 3 Explicitly Out of Scope

The following functionality is not required for Sprint 3:

1. Multiple review platforms.
2. Public sharing of analyses, except when approved as the team's student-designed product feature.
3. Collaboration between users.
4. Organization/team accounts.
5. General-purpose AI chat.
6. Advanced analytics dashboards.
7. Sentiment-analysis pipelines beyond the Q&A experience.
8. Fine-tuned or custom-trained models.
9. A required vector database or RAG framework.
10. Numerical AI confidence scoring without a defensible methodology.
11. PDF report generation.
12. Enterprise-scale production infrastructure.
13. Advanced deployment strategies.
14. Full observability platforms.
15. Complex historical comparison of multiple ingestion runs.

Teams should prioritize finishing the ReviewLens product experience, preserving correctness, and delivering the completed application at a public URL.
