# Lab Activity: Part B - Define Evaluation Criteria for Your AI Application
## Define Evaluation Criteria

| Objective / Requirement ID | Evaluation Criterion | Capability Bucket | Measurement Method | Metric | Threshold | User / Business Outcome |
|---|---|---|---|---|---|---|
| O3 / NFR2 | Correctness of supported mobility answers | Domain-specific capability | Prepare at least 30 questions answered by approved sources; compare responses with a manually checked answer key and source passages. | Correct answer rate | ≥ 80% | Students receive sufficiently reliable answers to routine mobility questions. |
| O1 / FR3 / RAI1 | Citation presence and source support | Instruction-following capability | Inspect at least 30 factual responses for citations and verify that cited passages support the answers. | Citation presence; citation-support rate | 100% citation presence for source-based factual answers; ≥ 90% citation-support rate | Students can verify information against official sources. |
| O2 / FR5 / RAI2 | Refusal and redirect for out-of-scope questions | Instruction-following capability | Test at least 20 prompts asking for visa, immigration, legal, or medical advice. | Correct refusal-and-redirect rate | ≥ 95% | Reduces reliance on MobiliFAI for advice outside its intended role. |
| O3 / NFR1 | Response latency | Cost and latency | Measure end-to-end response time for at least 30 representative supported questions under the same setup. | 90th-percentile latency (p90) | ≤ 8 seconds | Students receive information without excessive waiting. |
| O2 / FR4 / RAI3 | Handling missing or conflicting source information | Instruction-following capability | Test at least 10 prompts with incomplete or conflicting source details; check whether the system flags uncertainty rather than guessing. | Correct missing/conflict handling rate | ≥ 90% | Students are alerted when information needs checking. |

## Evaluation Notes
- Prepare test questions from the selected approved sources and record the expected answer and supporting passage.
- Keep supported questions separate from out-of-scope and insufficient-evidence questions.
- Record actual results after testing; do not report proposed thresholds as achieved results.
- Use the same test set and measurement procedure when comparing versions.

## Reflection
I think correctness is the most important evaluation criterion for MobiliFAI because students rely on the answers to understand study abroad requirements, application deadlines, and credit transfer information. If the AI gives incorrect information, students might misunderstand the requirements or miss important deadlines. Citation support is also important because it allows students to check the answers against official university documents. Therefore, MobiliFAI needs to provide answers that are not only helpful but also accurate and supported by reliable sources.
