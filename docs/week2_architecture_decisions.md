# Lab Activity: Part C - Design and Justify Your V1 Architecture
## Identify Architecture Needs from Your Requirements

| Requirement ID | What does the requirement need? | Architecture Response |
|---|---|---|
| FR1 / FR2 | Answers must be based on approved mobility information. | A defined knowledge-source collection and retrieval of relevant passages. |
| FR3 / RAI1 | Answers need verifiable source references. | Preserve document title, URL, and page/section where available with retrieved passages. |
| FR4 / RAI3 | Missing or conflicting information must not be silently guessed. | Add a sufficiency/conflict check and an uncertainty response when evidence is inadequate. |
| FR5 / RAI2 | Out-of-scope questions must be declined and redirected. | Add a scope/safety control before answer generation and a post-generation check where feasible. |
| NFR1 | Responses should meet the latency target. | Keep the V1 flow simple, search only the defined collection, and measure end-to-end latency. |
| NFR2 / NFR3 | Answers should be correct and grounded. | Retrieve relevant passages and evaluate responses against a curated answer key and citation-support rubric. |
| NFR4 | Sources should be maintainable. | Keep source ingestion/update separate from the user-facing question-answer flow. |

## V1 Architecture Diagram
1. The student submits a question through the application interface.
2. The scope and safety control checks whether the question is within MobiliFAI’s supported scope. Requests involving restricted topics are refused or redirected to the appropriate official contact.
3. For an in-scope question, the document retrieval component searches approved knowledge sources and retrieves relevant passages with source metadata.
4. The AI response component drafts an answer based on the retrieved evidence.
5. The grounding and citation check verifies whether the answer is supported by the retrieved sources. If the evidence is insufficient or conflicting, the system provides an uncertainty notice instead of guessing.
6. The application returns the answer with citations, or provides an appropriate refusal, uncertainty notice, or redirection to the student.
7. During testing, the system is evaluated against the defined requirements using correctness, groundedness, response latency, and safety criteria.

## Architecture Decision Records (ADR)

| Decision | Requirement / Constraint | Alternatives Considered | Selected Option | Trade-off Accepted |
|---|---|---|---|---|
| ADR1 — Use approved knowledge sources with retrieval | FR1, FR2, FR3, NFR2, NFR3; information is spread across official documents and should be verifiable. | Generate from model alone; manually browse static pages; retrieve from a defined document collection. | Retrieve from the selected approved documents/resources and carry source metadata into answers. | Adds ingestion and retrieval work, but improves traceability and reduces dependence on unsupported model memory. |
| ADR2 — Add explicit scope and safety control | FR5, RAI2; visa, immigration, legal, and medical advice is outside scope. | Rely only on prompting; use a dedicated scope check and refusal/redirect behaviour. | Include an explicit scope/safety control in the question-answer flow. | May incorrectly refuse borderline questions and adds processing, but reduces unsafe out-of-scope responses. |
| ADR3 — Handle insufficient or conflicting evidence | FR4, RAI1, RAI3; sources may be outdated, incomplete, or inconsistent. | Always return the most likely answer; silently choose one source; flag uncertainty or conflict. | Return an uncertainty/conflict notice when evidence does not support a clear answer. | Some users may receive fewer direct answers, but the system avoids presenting guesses as confirmed requirements. |
| ADR4 — Keep V1 simple and technology-neutral | Undergraduate time/resources; Topic 2 asks for conceptual architecture rather than a final production stack. | Commit now to specific model/database/framework; describe components by capability. | Use technology-neutral components and add only those justified by requirements. | Technology choices remain open, but the design is flexible and easier to revise. |

## Initial Requirements Traceability Matrix

| Objective | Requirement | Evaluation Criterion | Architecture Element | Later Evidence |
|---|---|---|---|---|
| O1 — Design a document-grounded assistant | FR2, FR3 / NFR3 / RAI1 | Citation presence and source support | Approved knowledge sources, retrieval, source metadata, grounding/citation check | Curated test answers, source-match review, citation-support results |
| O2 — Develop retrieval, citations, and guardrails | FR5 / RAI2 | Refusal and redirect for out-of-scope queries | Scope/safety control and refusal/redirect response | Results from at least 20 out-of-scope test prompts |
| O3 — Evaluate correctness, groundedness, latency, and safety | NFR1, NFR2, NFR3, FR4 / RAI3 | Correct answer rate, p90 latency, and missing/conflict handling | Evaluation test set, latency measurement, uncertainty/conflict check | Test log, answer-key comparison, response-time and safety results |

## Final Architecture Review
- Each major component is justified by requirements.
- Approved sources and source metadata support grounding and citations.
- Scope/safety and insufficient-evidence handling are included.
- Evaluation is planned against a curated test set.
- The diagram is conceptual; no specific model, vector database, framework, or deployment platform is selected yet.

# Reflection
I think the most important trade-off was choosing a document-grounded AI approach instead of allowing the AI to answer freely. This decision was driven by the requirement for correctness and reliable citations, because students may rely on MobiliFAI for important information such as application deadlines and exchange requirements. The trade-off is that the system may not be able to answer every question, especially when the information is missing from the approved sources. However, I think this is acceptable because providing a limited but reliable answer is better than giving students incorrect information.
