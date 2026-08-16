# ReviewLens AI - UX and Interface Guidance (Fall 2026)

This UX guidance is adapted from the Spring 2026 **ATS for Candidates** capstone project.

It serves the same purpose: to provide high-level experience, consistency, and interface guardrails while leaving teams room to make thoughtful design decisions.

This is not a component checklist and it is not a design template. Teams may choose their own layout, visual style, and interaction patterns, but those decisions must produce one coherent product from beginning to end.

## 1. How To Use These UX Specifications

Use project materials in this order:

1. **Active sprint requirements** define required sprint scope and grading expectations.
2. The **Product Requirements Document** defines overall product direction.
3. These **UX specifications** define user-experience standards and consistency expectations.
4. The **technical specifications** define implementation standards and engineering guardrails.

If there is a conflict, follow the active sprint requirements for implementation scope.

The most important idea is simple:

> The application should feel like one product, not three sprint submissions stitched together.

Users should not feel that they are moving between unrelated screens, disconnected workflows, or inconsistent interaction patterns.

---

## 2. Product Experience Direction

ReviewLens is an analysis-centered application.

Everything in the interface should help a user move from identifying what they want to analyze to understanding the available review evidence.

The primary product flow is:

**AnalysisTarget → Ingestion → Dataset Summary → Review Q&A**

When a user enters the application, they should be able to understand:

1. What entities they are analyzing.
2. Which AnalysisTarget is active.
3. Which review platform supplied the active data.
4. Whether usable review data exists.
5. What the ingestion result means.
6. What dataset the summary represents.
7. What dataset ReviewLens is using for Q&A.

The application should reduce ambiguity around the analysis context.

ReviewLens should feel like a focused review-intelligence product, not a general-purpose chatbot.

---

## 3. Navigation Expectations

Each team may choose its navigation pattern.

Acceptable approaches include:

1. Top navigation.
2. Left navigation.
3. Hybrid navigation.
4. Another coherent design.

What is not acceptable is changing navigation paradigms arbitrarily across the application.

If the application begins with one approach, major screens and workflows should follow that approach consistently.

Navigation should make it easy to:

1. Find existing AnalysisTargets.
2. Create a new AnalysisTarget.
3. Open the active AnalysisTarget workspace.
4. Move between major product areas without losing context.
5. Access account or settings functionality when applicable.

Not every final destination must appear in every sprint. Follow the active sprint requirements for required functionality.

Moving between major application areas should not cause the user to lose track of the active AnalysisTarget.

---

## 4. AnalysisTarget Workspace

The AnalysisTarget workspace is the primary operating surface for ReviewLens and should receive the most design attention.

This is where the user should be able to understand and work with the active analysis context.

The workspace should make clear:

1. Target or entity name.
2. Review platform.
3. Source or source reference.
4. Ingestion state.
5. Current review dataset.
6. Ingestion summary.
7. Available Q&A functionality.

Teams may choose how this workspace is organized.

Possible approaches include:

1. Tabs.
2. Panels.
3. Cards.
4. Sections.
5. Split views.
6. Another coherent model.

The important requirement is that ingestion, summary, and Q&A feel like parts of one analysis workflow rather than unrelated application screens.

The user should not need to infer which AnalysisTarget or review dataset is currently being analyzed.

---

## 5. AnalysisTarget Management Experience

Creating, reopening, switching, and managing AnalysisTargets should be straightforward.

The interface should make it easy to:

1. Create a new AnalysisTarget.
2. Identify the entity being analyzed.
3. Identify the supported review platform.
4. Identify the review source.
5. Reopen an existing target.
6. Switch between owned targets.
7. Understand which target is active.
8. Delete an owned target when that functionality is available.

Target creation should use clear validation and meaningful feedback.

Invalid input should not create confusing partial state.

Switching AnalysisTargets should produce a visible and understandable change in the active application context.

The user should never have to guess whether they are still viewing or analyzing the prior target.

---

## 6. Review Ingestion Experience

Review ingestion should not feel like a black box.

When ingestion begins, the user should understand:

1. What source is being processed.
2. Which AnalysisTarget will receive the resulting review data.
3. Whether processing is underway.
4. Whether ingestion succeeds.
5. Whether ingestion fails.
6. Whether ingestion partially succeeds, when supported.
7. Whether records were skipped or rejected.
8. Whether the resulting dataset is ready for analysis.

The application should clearly distinguish important states such as:

1. No ingestion has occurred.
2. Ingestion is processing.
3. Ingestion succeeded.
4. Ingestion failed.
5. Ingestion partially succeeded.

The user should not click an ingestion action and then be left wondering whether data was collected or whether ReviewLens is ready to answer questions.

### 6.1 Ingestion Result Summary

The ingestion summary should communicate enough information to give the user confidence in the active dataset.

At minimum, the user should be able to understand:

1. Which AnalysisTarget the summary represents.
2. How many reviews were successfully ingested.
3. Basic rating information.
4. Whether records were rejected or skipped when applicable.
5. Whether the active dataset is usable for Q&A.

Summary information should be easy to scan.

The UX should not force the user to inspect raw review rows merely to determine whether ingestion worked.

---

## 7. Review Intelligence and Q&A Experience

ReviewLens Q&A should feel like analysis of the active review dataset, not open-ended AI chat.

The interface should maintain visible context around:

1. The active AnalysisTarget.
2. The review platform.
3. The active dataset.
4. Whether review data is available.
5. Whether the system is ready to answer review-grounded questions.

The Q&A interaction should be simple and focused.

At minimum, it should support:

1. Natural-language question input.
2. Clear submit action.
3. Processing or loading state.
4. Response presentation.
5. Meaningful failure state.

Persistent multi-session chat history is not required unless active sprint requirements say otherwise.

### 7.1 Distinct Q&A Outcomes

The interface should distinguish among four fundamentally different outcomes:

1. **Grounded answer**  
   ReviewLens can answer from the active reviews.

2. **Insufficient evidence**  
   The question is relevant, but the available reviews do not support a reliable answer.

3. **Out-of-scope refusal**  
   The question is unrelated to the active review dataset or asks about unsupported entities or platforms.

4. **AI/service failure**  
   The Q&A operation could not be completed because of a technical or external-service problem.

These should not be treated as interchangeable generic chatbot responses.

The user should be able to understand whether ReviewLens:

1. Found evidence.
2. Did not find enough evidence.
3. Refused the request because it was outside scope.
4. Failed because the service could not complete the request.

### 7.2 Make the Analysis Boundary Visible

ReviewLens should make its boundaries understandable before users repeatedly discover them through failed questions.

The interface should reinforce that ReviewLens answers questions about:

1. The active AnalysisTarget.
2. The active persisted review dataset.

The application should not visually imply that it is a general-purpose assistant.

---

## 8. Context Switching and State Clarity

Changing AnalysisTargets changes the meaning of the analysis.

The UX must make that transition obvious.

After switching from one target to another:

1. The new target should be clearly identified.
2. The ingestion summary should correspond to the new target.
3. The review dataset should correspond to the new target.
4. Q&A should clearly operate in the new target's context.
5. Prior-target state should not appear to remain active accidentally.

Teams may choose how to handle prior Q&A content.

Possible approaches include:

1. Clearing prior Q&A when the target changes.
2. Keeping Q&A separated by AnalysisTarget.
3. Labeling prior Q&A clearly as belonging to a different target.
4. Another coherent design.

The exact design is flexible.

The guardrail is not:

> Never show historical Q&A.

The guardrail is:

> Never allow the user to reasonably believe that an answer belongs to one AnalysisTarget when it was generated from another.

---

## 9. Error and Failure Experience

ReviewLens depends on multiple application and external-service boundaries, so failure states are part of the normal user experience.

The interface should distinguish among:

1. Invalid AnalysisTarget input.
2. Authentication or session failure.
3. Authorization denial.
4. Review-source or ingestion failure.
5. Partial or incomplete ingestion.
6. No review data available.
7. LLM-provider failure.
8. Unexpected application failure.

Errors should be expressed in user terms rather than implementation terms.

Do not present normal users with:

1. Raw stack traces.
2. Database exceptions.
3. Provider SDK errors.
4. Internal authentication-provider messages.
5. Secret values.
6. Unnecessary technical diagnostics.

Where practical, failure messages should also communicate what the user can do next.

Examples include:

1. Correct invalid input.
2. Retry ingestion later.
3. Complete ingestion before asking questions.
4. Ask a question about the active reviews.
5. Try the AI operation again when the provider is available.

---

## 10. Visual Language

The visual direction should be modern, student-friendly, and professional.

The application should feel current without becoming noisy or overly decorative.

Teams may choose their own design style, but they should establish a visual system early and apply it consistently.

Typography, spacing, color, and component styling should be deliberate.

Repeated UI elements should look and behave as though they belong to the same product family.

This includes:

1. Buttons.
2. Forms.
3. Cards and panels.
4. Navigation.
5. Status indicators.
6. Loading states.
7. Validation.
8. Error states.
9. Ingestion states.
10. Q&A responses.

ReviewLens should look like a modern analysis product rather than a collection of developer tools or an unstyled generic chatbot.

---

## 11. Responsive Behavior

ReviewLens is desktop-first.

The primary workflows are expected to be used primarily on desktop and laptop computers, and teams should optimize those experiences first.

At the same time, the application should remain usable and understandable on tablets and mobile devices.

Responsive support should be intentional rather than an afterthought.

Core workflows should not become unusable merely because the screen is smaller.

A Progressive Web App implementation is optional.

It may be included if a team chooses to invest in it, but it is not required.

---

## 12. Production Experience Expectations

By the final sprint, ReviewLens should feel like a production application rather than a development environment exposed to the internet.

The public application should:

1. Load into a coherent product experience.
2. Preserve the navigation and interaction patterns established earlier.
3. Use deliberate loading states.
4. Handle missing or unavailable external services gracefully.
5. Avoid exposing developer-only debug information.
6. Avoid exposing raw errors or configuration details.
7. Preserve user context and persisted data across sessions.
8. Continue to make the active AnalysisTarget clear.

Production deployment should not introduce an entirely different user experience from the development application.

The application should feel like the same product throughout the semester.

---

## 13. UX Quality Standard

The primary UX quality goals are:

### Consistency

Repeated actions, controls, states, and interaction patterns should behave the same way throughout the application.

### Clarity

Users should understand:

1. Which AnalysisTarget is active.
2. What review data is available.
3. What action they can take.
4. What just happened.
5. Whether ReviewLens answered, lacked evidence, refused the question, or encountered a technical failure.

### Flow

Users should be able to move naturally through:

**Target selection → Ingestion → Dataset understanding → Review Q&A**

without becoming lost or losing context.

If a team makes creative layout and interaction choices while preserving consistency, clarity, and flow, that is exactly the kind of result this project should produce.
