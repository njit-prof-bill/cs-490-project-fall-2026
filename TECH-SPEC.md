# ReviewLens AI - Technical Specifications (Fall 2026)

This technical specification is adapted from the Spring 2026 **ATS for Candidates** capstone project.

It serves the same purpose: to provide project-level engineering guardrails for student development, code review, and AI-assisted implementation. The architecture, domain model, AI requirements, testing expectations, and deployment standards have been revised for ReviewLens AI.

This document defines implementation standards. It does not prescribe a single technology stack or application architecture unless a requirement explicitly says otherwise.

## 1. Scope and Relationship to Other Specifications

Use project materials in this order:

1. **Active sprint requirements** define required sprint scope and grading expectations.
2. The **Product Requirements Document** defines overall product scope and cross-sprint direction.
3. **UX specifications** define user-experience direction.
4. These **technical specifications** define implementation standards and engineering guardrails.

If there is a conflict, follow the active sprint requirements for implementation scope and grading, then the Product Requirements Document for product intent.

These technical standards apply equally to human-written code, AI-generated code, AI-modified code, configuration, infrastructure, and test code.

AI-generated implementation is not exempt from architecture, security, testing, or quality requirements.

## 2. Architecture and Service Model

Before implementation begins, each team must establish a coherent architecture baseline.

### 2.1 Sprint 1 Architecture Baseline

Each team must prepare a short architecture baseline before substantial feature development begins.

At minimum, identify:

1. Frontend technology.
2. Backend technology.
3. Database or persistence technology.
4. Managed identity provider.
5. Review-ingestion approach.
6. LLM provider.
7. Deployment platform.
8. Core domain entities and conceptual relationships.
9. Main API or service boundaries.
10. Major frontend screens or operating surfaces.
11. Simple architecture diagram showing major application components and external services.

The baseline does not need to predict every later design decision. Teams may revise the architecture as they learn, but significant deviations should be intentional and explainable.

### 2.2 Reference Architecture Expectations

ReviewLens is a multi-user SaaS web application.

The architecture must provide:

1. A frontend application.
2. A backend application or service layer.
3. Persistent storage for user-owned application data.
4. Managed user authentication.
5. Backend authorization and ownership enforcement.
6. A review-ingestion boundary.
7. A review-data persistence and normalization boundary.
8. A Q&A orchestration boundary.
9. An LLM-provider integration boundary.
10. A deployment path suitable for public production use by Sprint 3.

The system should be organized around domain responsibilities rather than individual pages.

At minimum, maintain clear boundaries for:

1. Authentication and trusted user identity.
2. AnalysisTarget ownership and lifecycle.
3. Review ingestion and normalization.
4. Persisted review data.
5. Ingestion summary.
6. Review-grounded Q&A.
7. Scope enforcement and context construction.
8. Production deployment and operational verification.

### 2.3 External Service Boundaries

External services should be isolated behind application-defined service or adapter boundaries where practical.

This includes:

1. Managed identity providers.
2. Review-source APIs, scrapers, parsers, or import mechanisms.
3. LLM providers.
4. Hosted databases or storage services.
5. Other external infrastructure services.

Core application behavior should not be unnecessarily coupled to one provider's SDK or response format.

The team should be able to replace or control important external boundaries during automated testing.

## 3. Authentication, Data Ownership, and Authorization

ReviewLens is not a single-user application.

Authentication and authorization are separate responsibilities.

### 3.1 Authentication

Teams should use a managed identity provider.

Teams are not expected to implement their own password storage, password hashing, password-reset system, credential database, or equivalent identity infrastructure.

The application must derive authenticated user identity from trusted authentication context.

The backend must not trust a client-supplied user ID as proof of identity.

### 3.2 Ownership

Every user-owned domain operation must enforce ownership.

Required rules:

1. AnalysisTargets are user-owned.
2. Reviews must be associated with an AnalysisTarget.
3. Ingestion state must be associated with the correct AnalysisTarget.
4. Ingestion summaries must be derived within the ownership boundary of the active AnalysisTarget.
5. Q&A context must contain only review data the authenticated user is authorized to access.
6. Destructive operations must enforce the same ownership rules as read operations.
7. Cross-user access must be rejected by backend or persistence-layer authorization.

Frontend filtering and route guards are useful UX controls, but they are not authoritative security controls.

Backend ownership enforcement is authoritative.

### 3.3 AI Context Is a Security Boundary

Review content supplied to an LLM is protected application data.

The application must prevent:

1. Another user's reviews from entering the active AI context.
2. Reviews from a previously selected AnalysisTarget from remaining in context after the active target changes.
3. Client-supplied identifiers from bypassing ownership enforcement.

A response that happens not to reveal another user's data is not sufficient evidence of security.

The application should enforce the correct data boundary before model invocation.

## 4. Review Ingestion and Data Integrity

Review ingestion is a data-acquisition workflow, not an AI-generation workflow.

### 4.1 Ingestion Boundary

The application must define a clear boundary between:

1. Source-specific review input.
2. Application-owned normalized Review data.
3. Persisted ReviewLens analysis data.

ReviewLens may support a publicly accessible review URL, a practical supplied data format, or another instructor-approved mechanism.

Each team supports one review platform unless active sprint requirements state otherwise.

### 4.2 Canonical Review Representation

The application must normalize source-specific data into a stable application representation.

At minimum, a persisted Review must contain:

1. Review text.
2. Rating.

Teams should retain useful source metadata when available, such as reviewer display name, review date, source review identifier, source URL, or other platform-specific metadata.

The application should avoid allowing platform-specific source structure to leak throughout unrelated domain code.

### 4.3 Data Integrity Rules

The ingestion implementation must not:

1. Fabricate review text.
2. Fabricate ratings.
3. Report failed ingestion as successful.
4. Count rejected records as successfully ingested.
5. Destroy previously valid persisted data because a later external request fails.

Rejected, skipped, or unprocessable records must be handled consistently.

If partial ingestion is supported, the application must represent partial success distinctly from complete success.

### 4.4 Ingestion Summary Integrity

Ingestion summaries must be calculated from persisted application data.

Summary values must not be invented or interpreted by the LLM.

At minimum, the summary must be capable of reporting:

1. Active AnalysisTarget.
2. Ingestion result state.
3. Number of successfully ingested reviews.
4. Basic rating information.
5. Rejected or skipped records when applicable.

The summary should represent the same persisted dataset used by the Q&A workflow.

## 5. API and Error-Handling Standards

API and service behavior must be consistent.

Required standards:

1. Use consistent success and error behavior.
2. Use appropriate status codes or equivalent service-level outcomes.
3. Validation errors should identify invalid input clearly.
4. Authentication and authorization failures must be distinguishable from ordinary validation failures.
5. Avoid leaking internal stack traces to clients.
6. Log useful server-side diagnostic information.
7. Preserve request or operation context in logs when practical.

### 5.1 User-Facing Error Categories

ReviewLens should distinguish among:

1. Validation failure.
2. Authentication failure.
3. Authorization failure.
4. Review-ingestion failure.
5. Insufficient review evidence.
6. Scope-guard refusal.
7. LLM-provider failure.
8. Unexpected application failure.

These states should not collapse into one generic error condition.

### 5.2 User-Facing Error Quality

User-visible error messages should be understandable to an average application user.

Do not expose as normal user-facing behavior raw stack traces, database exception messages, provider SDK exceptions, authentication-provider internals, secret values, raw credentials, or unnecessary internal implementation details.

Technical diagnostic information belongs in server-side logs or equivalent operational tooling.

## 6. UI Implementation Standards

Teams may make their own design decisions, but interaction consistency is required.

Required UI standards:

1. Choose a coherent navigation pattern and use it consistently.
2. Keep forms, validation, status indicators, and actions predictable.
3. Reuse visual and interaction patterns where practical.
4. Optimize for desktop use while maintaining reasonable responsive behavior.
5. Avoid interfaces that require users to infer critical application state.

### 6.1 AnalysisTarget Context

The AnalysisTarget workspace is the primary operating surface for ReviewLens.

The active context should remain clear across review ingestion, ingestion summary, review data, and Q&A.

The user should always be able to determine:

1. Which AnalysisTarget is active.
2. Which review platform supplied the active data.
3. Which dataset is being analyzed.

### 6.2 Distinct Application States

The UI should distinguish important states such as:

1. No ingestion performed.
2. Successful ingestion.
3. Failed ingestion.
4. Partial ingestion when supported.
5. Normal grounded Q&A answer.
6. Insufficient review evidence.
7. Scope refusal.
8. LLM-service failure.

Do not rely on one generic success or failure treatment for all of these conditions.

## 7. AI Integration and Guardrail Standards

ReviewLens AI functionality is not general-purpose chat.

AI behavior must remain grounded in the authenticated user's active persisted review dataset.

### 7.1 Review-Grounded Context

Before sending a question to the model, ReviewLens must construct context from persisted review data belonging to the authenticated user and active AnalysisTarget.

The team must make an explicit architectural decision about context selection.

Possible approaches include:

1. Supplying all reviews when the dataset is small enough.
2. Selecting relevant reviews.
3. Search-based retrieval.
4. Retrieval-Augmented Generation.
5. Embeddings and vector search.
6. Another defensible approach.

A vector database, embeddings, or a specific RAG framework are not required.

The architecture should match the scale and complexity of the team's application.

### 7.2 System Prompt Guardrails

Every ReviewLens Q&A request must use a system prompt or equivalent high-priority instruction that clearly establishes:

1. Which AnalysisTarget is active.
2. Which review platform supplied the active dataset.
3. That answers must be based on supplied review evidence.
4. That unsupported information must not be invented.
5. That unrelated general-knowledge questions must be declined.
6. That questions about unsupported products, entities, locations, or review platforms must be declined.
7. That insufficient evidence must be acknowledged.

Prompt text or prompt construction logic must be reviewable by the development team.

Do not rely on undocumented manually configured prompt text that exists only in a hosted AI console.

### 7.3 AI Must Not Replace Deterministic Application Logic

Do not use the LLM to fabricate or determine values that should come from persisted application data.

Examples include:

1. Review count.
2. Rating average.
3. Rating distribution.
4. Ingestion success state.
5. Ownership.
6. Authorization.
7. Target identity.
8. Source metadata.

These are application responsibilities.

### 7.4 Model Boundary

The LLM provider should be accessed through a clear application boundary.

The model boundary should make it possible to:

1. Supply controlled responses during tests.
2. Capture or inspect application-supplied context during tests.
3. Test orchestration without paying for a live model call on every CI run.
4. Handle provider failure consistently.

Provider-specific logic should not unnecessarily leak across the rest of the application.

## 8. Testing Standards

Automated testing is mandatory and must verify meaningful behavior.

Coverage percentage alone is not sufficient.

### 8.1 Required Test Categories

For significant application behavior, include appropriate tests for:

1. Expected behavior.
2. Validation failure.
3. Error or exception behavior.
4. Edge and boundary cases.
5. Authentication.
6. Authorization and ownership.
7. Persistence behavior.
8. External-service failure where applicable.

### 8.2 ReviewLens Domain Testing

Important ReviewLens behavior includes:

1. Authentication and session protection.
2. AnalysisTarget ownership.
3. Cross-user access denial.
4. AnalysisTarget validation and persistence.
5. Review normalization.
6. Review persistence.
7. Ingestion success and failure.
8. Ingestion-summary calculations.
9. Review-context construction.
10. Grounded Q&A orchestration.
11. Insufficient-evidence handling.
12. Scope-guard behavior.
13. Active-target switching.
14. Stale-context prevention.
15. Cross-user AI-context isolation.
16. LLM-provider failure.
17. Production lifecycle behavior where applicable.

### 8.3 Deterministic AI Testing

Required CI tests must not depend on a live nondeterministic LLM returning one exact natural-language sentence.

Teams should use controlled techniques such as:

1. Mock model client.
2. Stubbed provider adapter.
3. Recorded response.
4. Dependency injection.
5. Equivalent deterministic test mechanism.

Tests should verify behavior such as:

1. Correct system instructions.
2. Correct active-target context.
3. Absence of wrong-target context.
4. Absence of cross-user context.
5. Correct handling of controlled model responses.
6. Correct scope classification or refusal behavior.

A live model may be used for supplemental evaluation, but live-model testing does not replace deterministic automated testing.

### 8.4 Scope-Guard Evaluation

The team must maintain representative scope-guard evaluation cases.

Include examples of:

1. Clearly in-scope review questions.
2. Relevant questions with insufficient evidence.
3. General world-knowledge questions.
4. Questions about another product or entity.
5. Questions about another review platform.

Evaluate required behavior rather than one exact natural-language sentence.

### 8.5 Story-Level Definition of Done for Testing

A story is not complete unless:

1. Relevant automated tests were added or updated.
2. Tests include negative or edge behavior where applicable.
3. Existing regression tests continue to pass.
4. Required CI checks pass.

## 9. CI/CD and Branch Policy Standards

Every team must maintain an automated quality gate.

### 9.1 Baseline CI Requirements

Pull requests must run:

1. Lint or equivalent static checks.
2. Application build.
3. Automated tests.

Failing required checks must block the team's normal merge workflow.

### 9.2 Sprint 2 Quality Ratchet

Sprint 2 additionally requires:

1. Required integration tests.
2. Changed-code coverage gate or equivalent policy.
3. Deterministic Q&A orchestration tests.
4. Scope-guard evaluation or automated guardrail tests.

### 9.3 Sprint 3 Quality Ratchet

Sprint 3 additionally requires:

1. Automated production deployment.
2. Security or dependency scanning appropriate to the selected stack.
3. Post-deployment health verification.
4. Automated production or production-equivalent smoke tests.
5. Migration or schema verification when applicable.

Deployment must be downstream of required quality gates.

A deployment path must not bypass required verification.

### 9.4 Branch Policy

Recommended branch policy:

1. Feature branches for story work.
2. Pull request review before merge.
3. No direct pushes to the protected release branch.
4. Required status checks before merge.

The protected release branch should remain deployable.

## 10. Deployment and Runtime Standards

ReviewLens must become publicly deployable by Sprint 3.

### 10.1 Production Deployment

Required production standards include:

1. Publicly reachable application URL.
2. Automated deployment through CI/CD.
3. Environment-specific configuration.
4. Protected production secrets.
5. Persistent production storage.
6. Visible deployment success or failure.

Normal deployment should not require copying files manually to a server, running undocumented commands from one developer's laptop, or editing application source code to change environments.

### 10.2 Production Configuration and Secrets

Production secrets must not:

1. Be committed to source control.
2. Be embedded in browser-delivered code.
3. Be printed in normal application logs.
4. Be displayed during demonstrations.

Development, automated test, and production configuration must be separable without editing source code for every deployment.

### 10.3 Health and Smoke Verification

A hosting provider reporting successful deployment is not sufficient evidence that ReviewLens is operational.

Every production deployment must be followed by automated verification.

At minimum:

1. Health verification should confirm that the deployed application responds successfully.
2. Smoke tests should verify a small set of critical production behavior.
3. Failed required post-deployment verification should produce a failed release result.

Smoke tests should be safe to run repeatedly, small and targeted, non-destructive to real user data, and focused on deployed behavior.

### 10.4 Runtime Failure Handling

The production application must handle important external failures deliberately.

Examples include:

1. Review-source failure.
2. Ingestion-service failure.
3. LLM-provider failure.
4. Database connectivity failure.
5. Missing critical configuration.

Failure must not result in fabricated successful application behavior.

Logs should provide enough information to diagnose important failures without exposing protected secrets.

## 11. Data Model Guardrails

The physical database design is a team decision.

The minimum conceptual model includes:

1. **User** or trusted authenticated identity.
2. **AnalysisTarget**.
3. **IngestionRun** or equivalent ingestion-state representation.
4. **Review**.
5. Optional Q&A session/message structures if required by the team's design.

### 11.1 Core Relationships

At minimum:

1. An AnalysisTarget belongs to one authenticated user.
2. Reviews belong to an AnalysisTarget.
3. Ingestion state belongs to or references an AnalysisTarget.
4. Ingestion summaries are derived from Review data associated with the active AnalysisTarget.
5. Q&A context is derived from Reviews associated with the active AnalysisTarget.
6. User ownership must remain resolvable through the application data model.

### 11.2 Persistence Guardrails

Teams should:

1. Preserve referential integrity.
2. Avoid unnecessary duplicate canonical data.
3. Maintain useful created/updated timestamps where relevant.
4. Define deletion behavior for AnalysisTargets and associated data.
5. Define duplicate-handling behavior for ingestion when applicable.
6. Use repeatable schema or migration artifacts when the selected persistence technology requires them.

If re-ingestion is supported, the team must define how the new result relates to the prior dataset.

## 12. Jira Traceability Rules

Jira stories are execution units.

These technical specifications are the project-level engineering standards contract.

Required traceability practice:

1. Each story should identify relevant technical-specification sections.
2. Each story should identify applicable canonical business rules from the active sprint requirements.
3. Story Definition of Done should include relevant test and ownership/security expectations.
4. Pull requests should reference the associated story.
5. Pull requests should include useful test evidence.

Useful Jira metadata may include:

1. `Technical Specification Sections`
2. `Test Evidence`
3. `Security/Ownership Impact`
4. `Canonical Rule IDs`

For AI-related work, implementation notes or test evidence should identify material changes to prompt behavior, context construction, retrieval strategy, model/provider boundary, or guardrail behavior.

## 13. Sprint Application Guidance

These technical specifications apply throughout the project, but the required emphasis increases by sprint.

### Sprint 1

Primary technical focus:

1. Managed authentication.
2. Backend ownership and authorization.
3. AnalysisTarget foundation.
4. Review ingestion.
5. Review normalization and persistence.
6. Error handling.
7. CI/testing baseline.

### Sprint 2

Primary technical focus:

1. Ingestion-summary integrity.
2. Review-grounded Q&A.
3. Prompt-level scope guard.
4. Insufficient-evidence behavior.
5. AnalysisTarget context switching.
6. Stale-context prevention.
7. Cross-user AI-context isolation.
8. Integration testing.
9. Changed-code coverage.
10. Deterministic AI testing.

### Sprint 3

Primary technical focus:

1. Automated production deployment.
2. Production configuration and secret handling.
3. Production lifecycle behavior.
4. External-service failure handling.
5. Persistence and migration safety.
6. Health verification.
7. Production smoke testing.
8. Security/dependency scanning.
9. Production regression of authentication, ownership, ingestion, Q&A, and scope behavior.

## 14. Final Engineering Standard

The team is building one product, not a sequence of disconnected demos.

Engineering decisions must preserve:

1. Product coherence.
2. User and data isolation.
3. Grounded, testable behavior.
4. Deployable production quality.

If a shortcut conflicts with these principles, it is the wrong shortcut.
