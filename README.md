# ReviewLens AI - Product Requirements Document (Fall 2026)

_CS 490 Capstone Project_

## 1. Product Purpose and Vision

### 1.1 Product Purpose

Imagine a consultancy that specializes in Online Reputation Management (ORM). Their
business model relies on analyzing massive amounts of fragmented customer feedback to
offer strategic services to brands.

Without ReviewLens, they spend hours manually reading reviews to identify "pain points." They
need a rapid prototype of a Review Intelligence Portal that can ingest a product's digital
footprint and allow an analyst to "talk" to that data to find specific trends—without the AI
drifting into generalities or competitor data.

### 1.2 Product Vision

One coherent web app where a user can:

1. Authenticate and securely manage an account.
2. Create and manage review analysis targets.
3. Ingest customer reviews from one supported public review platform, or supply the review data in a practical supported format.
4. Review a clear summary of the ingested data before analysis.
5. Ask natural-language questions grounded exclusively in the ingested reviews.
6. Rely on explicit scope guards that reject unrelated questions and prevent cross-user data access.
7. Use the application through a production-style deployment supported by automated testing and CI/CD.

### 1.3 Product Positioning

This is a SaaS-style, multi-user Review Intelligence Portal with self-registration and strict per-user data isolation.

Each team must support **one** public review platform such as Amazon, Google Maps, G2, Capterra, or another approved source that:

1. Is publicly accessible for browsing.
2. Contains user-generated written reviews and ratings.

The application does not need to support multiple review platforms.

### 1.4 Using Modern Development Tools

Your goal is to build great software as a team. Development may include your use of AI tools (Claude Code,
Cursor, Codex, Copilot, etc.), which is expected.

In reviewing this project, much focus is on the professional quality and speed of delivery, the
judgment used when working with AI, and engineering as a team. These are all
vital traits for a professional software engineer.

### 1.5 Make this your own!!

This project is your canvas. The core requirements in the use cases set the baseline, but this is where
you can go above and beyond to stand out. Take this project in whatever direction proves your
unique value; whether that's through sophisticated prompt engineering, an elegant UI, a
particularly clever architectural choice, or engineering practices that show you can operate
on a production team.

### How To Use This Document

1. `README.md` defines the overall product.
2. Each sprint story file defines the work required for that sprint.
3. If there is a conflict between this document and a sprint story file, follow the sprint story file for the currently active sprint.

## 2. Primary UX Principle

The **Analysis Workspace** is the center of gravity.

The primary user flow is:

1. Select or create an analysis target.
2. Ingest review data.
3. Inspect the ingestion result summary.
4. Ask questions about the ingested reviews.

The user should always understand **what review dataset is currently being analyzed**. The application should make the active target, source platform, and ingestion state clear before and during Q&A.

Authentication, target management, and account controls support this workflow but should not dominate it.

## 3. Core Product Areas

1. Authentication and account security.
2. Analysis Target and Review Ingestion workflow.
3. Ingestion Result Summary.
4. Guardrailed Review Q&A.
5. Analysis target management and account controls.
6. Testing, CI/CD, and production deployment.

For Sprint 1, students should focus only on the Sprint 1 slice of this product. Not every product area is fully implemented in Sprint 1.

## 4. Functional Requirements

## 4.1 Authentication and Ownership

1. Users can register, login, logout, and reset password.
2. Protected application routes require authenticated access.
3. Backend services enforce data ownership boundaries for all user-scoped entities.
4. Cross-user read/write attempts are denied.
5. Review data belonging to one user must never be included in another user's AI context.

### Acceptance Criteria

1. An unauthenticated user cannot access protected analysis data or Q&A functionality.
2. User A cannot read, modify, delete, ingest into, or query User B's analysis targets or reviews.
3. Duplicate registration email is rejected.
4. Session invalidates cleanly on logout.
5. Authorization is enforced by the backend and is not dependent only on hidden UI controls or client-supplied ownership identifiers.

## 4.2 Analysis Targets and Review Ingestion

### Analysis Target Minimums

Each analysis target must include enough information to identify what is being analyzed.

Minimum target data:

1. User-defined target name or entity name.
2. Supported review platform.
3. Source URL or other source information required by the team's ingestion approach.
4. Owner identity.

### Ingestion Requirements

1. The application accepts a target URL from the team's supported platform and extracts relevant review data, **or** allows the user to supply that platform's review data in a practical supported format for analysis.
2. At minimum, successfully ingested reviews include:
   - rating
   - written review text
3. The system preserves enough source metadata to associate ingested reviews with the correct analysis target and ingestion operation.
4. Ingested data is persisted on the backend and scoped to the authenticated user.
5. Invalid, unsupported, incomplete, or failed ingestion attempts produce meaningful user-facing errors.
6. The application must not present fabricated review data as though it was collected from the selected source.

### Acceptance Criteria

1. A user can create an analysis target for the team's supported platform.
2. A user can successfully ingest or import a valid review dataset.
3. Persisted reviews remain associated with the correct authenticated user and analysis target.
4. Invalid source input or unusable review data is rejected or reported clearly.
5. A user cannot ingest into or modify another user's analysis target.
6. The source platform and target being analyzed remain identifiable after ingestion.

## 4.3 Ingestion Result Summary

After ingestion, the application must give the user a clear summary of what was successfully collected.

The summary exists to give the user confidence that the dataset is accurate enough, sufficiently complete, and ready for analysis.

At minimum, the summary should communicate:

1. Analysis target and source platform.
2. Number of reviews successfully ingested.
3. Basic rating information appropriate to the source data.
4. Any reviews or records that were skipped, rejected, or could not be processed, when applicable.
5. The state or result of the ingestion operation.

The exact presentation may be text-based, tabular, visual, or a combination.

Summary values must be derived from the ingested data rather than generated speculatively by an LLM.

### Acceptance Criteria

1. The user can distinguish a successful ingestion from a partial or failed ingestion.
2. Displayed counts and rating summaries agree with persisted review data.
3. The summary identifies the dataset that will be used by Q&A.
4. Partial ingestion does not silently appear to be complete.
5. The user can proceed from the completed summary into review analysis.

## 4.4 Guardrailed Q&A Interface

### Interactive Review Q&A

1. The application provides a user-facing conversational interface.
2. Users can ask natural-language questions about the currently selected ingested review dataset.
3. Answers must be grounded in the review data available to that authenticated user and selected analysis target.
4. If the ingested reviews do not support an answer, the application should say so rather than inventing information.

### Scope Guard Enforcement

The Q&A experience must explicitly reject questions outside the active review dataset.

Out-of-scope requests include:

1. Questions about another review platform when that platform is not the active source.
2. Questions about another product or entity that is not represented in the selected review dataset.
3. General world-knowledge questions unrelated to the reviews.
4. Requests that would require another user's review data.

Scope enforcement should be driven primarily by the LLM system prompt and the application context supplied to the model.

Supporting application logic may reinforce the scope boundary, but it must not replace a clear prompt-level scope definition.

### Acceptance Criteria

1. An in-scope question receives an answer based on the ingested reviews.
2. An out-of-scope question receives a clear, graceful refusal or scope reminder.
3. A general-knowledge question unrelated to the review dataset is declined.
4. The assistant does not claim knowledge of reviews that were not part of the active dataset.
5. Changing the selected analysis target changes the review context used by Q&A.
6. Cross-user review data is never exposed through the Q&A interface.

## 4.5 Analysis Target Management

1. Users can view the analysis targets that belong to their account.
2. Users can open a target to review its ingestion state and enter the Analysis Workspace.
3. Users can delete or otherwise remove targets that they own.
4. Target-management operations enforce backend ownership rules.
5. The application provides a coherent path for updating or re-running ingestion when supported by the team's design.

### Acceptance Criteria

1. A user's target list contains only targets owned by that user.
2. Opening a target loads the correct persisted review context.
3. Deleting or updating a target does not affect another user's data.
4. The UI clearly indicates which target is active during ingestion, summary, and Q&A.

## 5. Non-Functional Requirements

1. **Security:** authentication, backend authorization, per-user data isolation, protected secrets, and safe handling of untrusted input are mandatory.
2. **Reliability:** normal authentication, ingestion, summary, and Q&A paths should execute without crashes or silent data corruption.
3. **AI grounding:** responses should remain constrained to the active ingested review dataset and should identify when the data is insufficient.
4. **Usability:** navigation, ingestion status, loading states, failures, and AI scope refusals should be clear to the user.
5. **Accessibility:** major workflows should provide reasonable keyboard support, labels, semantic controls, and contrast.
6. **Performance:** the application should be responsive enough for normal course-scale use and class demonstration.
7. **Deployability:** the application must be designed to run in a hosted production-style environment rather than only on a developer workstation.
8. **Cost awareness:** teams are responsible for their use of hosted infrastructure, scraping services, model APIs, and other paid external services.

## 6. CI/CD and Testing Standards

These standards apply across all sprints, with ratcheting expectations.

Testing is a first-class project requirement. A feature is not complete merely because it works once in the browser.

### Sprint 1 Baseline

1. Pull-request CI runs:
   - lint/static checks
   - build
   - unit tests
2. Failing required checks block merge.
3. Sprint stories include happy-path and non-happy-path automated tests.
4. Authentication and ownership rules have explicit negative tests.
5. Secrets and environment-specific configuration are not committed to source control.

### Sprint 2 Ratchet

1. Keep all Sprint 1 requirements.
2. Add a changed-code coverage gate.
3. Add integration tests for critical backend boundaries, including ingestion and persistence.
4. Add automated tests for the Q&A orchestration layer.
5. Add a guardrail evaluation set containing both:
   - in-scope review questions
   - out-of-scope questions that must be declined
6. Tests should verify behavior rather than depend on one exact natural-language response.
7. Deterministic CI tests should mock or isolate nondeterministic external LLM behavior where practical. Live-model evaluation may be run separately when needed.

### Sprint 3 Ratchet

1. Keep all Sprint 1 and Sprint 2 requirements.
2. Add automated deployment from the team's protected main branch or equivalent release branch after required checks pass.
3. Add post-deployment health checks.
4. Add smoke tests for the deployed critical path:
   - authentication
   - access to a user-owned target
   - ingestion or access to ingested data
   - ingestion summary
   - Q&A
   - out-of-scope guardrail behavior
5. Add migration or schema-change verification when the application uses persistent storage.
6. Add dependency/security scanning appropriate to the team's technology stack.
7. A failed verification step must fail visibly rather than silently producing a broken release.

## 7. Domain Model (High Level)

Minimum persistent entities:

1. User
2. AnalysisTarget
3. IngestionRun
4. Review

Recommended responsibilities:

### User

Represents the authenticated account that owns application data.

### AnalysisTarget

Represents the product, business, location, or other entity whose reviews are being analyzed.

Minimum information includes:

1. owner
2. target/entity name
3. supported platform
4. source URL or equivalent source reference

### IngestionRun

Represents an attempt to collect or import review data for an AnalysisTarget.

Useful information includes:

1. target
2. start/completion timestamps
3. status
4. success/failure counts
5. source or ingestion metadata

### Review

Represents a single ingested customer review.

Minimum information includes:

1. ingestion/target relationship
2. rating
3. review text
4. source metadata when available

Optional persistent entities may include `ChatSession` and `ChatMessage` if the team chooses to persist conversation history.

### Required Relationships

1. User 1:N AnalysisTarget
2. AnalysisTarget 1:N IngestionRun
3. AnalysisTarget 1:N Review, directly or through IngestionRun
4. IngestionRun 1:N Review when the design preserves ingestion history

Ownership is transitive: reviews and ingestion data are accessible only through an AnalysisTarget owned by the authenticated user.

## 8. Sprint Themes and Delivery Order

Themes:

1. **Sprint 1: Authentication, Analysis Targets, and Ingestion Baseline**
   - multi-user authentication
   - ownership/security foundation
   - target creation
   - initial review ingestion
   - CI and unit-test baseline
2. **Sprint 2: Ingestion Intelligence and Guardrailed Q&A**
   - ingestion result summary
   - review-grounded conversational analysis
   - scope guard enforcement
   - integration testing and quality ratchet
3. **Sprint 3: Production Deployment and Engineering Hardening**
   - cloud/hosted deployment
   - automated CI/CD
   - post-deployment verification
   - security and reliability hardening
   - end-to-end smoke coverage

## 9. Student Execution Guidance

1. Treat each sprint story file as authoritative for that sprint.
2. Use canonical business rule IDs in the sprint file when implementing and testing stories.
3. Keep Jira tickets one-to-one with stories.
4. Provide explicit test evidence for each completed story.
5. Do not implement later-sprint features early unless explicitly approved.
6. AI-assisted development tools are permitted and expected, but students remain responsible for understanding, reviewing, testing, and securing the code they submit.
7. Maintain complete AI-assisted development transcripts in an `/ai-transcripts` directory when transcripts are available from the tools being used. Do not intentionally remove dead ends or failed approaches from those transcripts.
8. Document the team's chosen review platform and important implementation assumptions.
9. The core requirements are the baseline. Teams may extend the product after required behavior is working and tested.
10. Do not substitute fake integrations, hard-coded AI responses, or fabricated review data for required end-to-end behavior.
11. Use professional judgment for implementation details that are not explicitly specified.

## 10. Definition of Done (Project Level)

A team is done when:

1. A user can register, login, logout, and securely access the application.
2. Multiple users can use the same deployed application with strict per-user data isolation.
3. A user can create an analysis target and ingest or practically import real review data from the team's supported public review platform.
4. The application provides a trustworthy ingestion result summary.
5. The user can ask questions that are answered from the active ingested reviews.
6. The Q&A system explicitly declines questions outside the active review scope.
7. Authentication, authorization, ingestion, summary, Q&A, and scope-guard behaviors are test-backed.
8. CI quality gates pass for the relevant sprint.
9. The application is deployed at a publicly reachable URL, with protected functionality requiring authentication.
10. Deployment is performed through the team's CI/CD process and verified with post-deployment checks.
11. The complete source code is maintained in the team's GitHub repository.
12. Required project documentation and AI development transcripts are present and reviewable.
13. The deployed application is stable enough for an end-to-end class demonstration.
