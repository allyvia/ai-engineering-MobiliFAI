# Lab Activity: Part A - Refine Project Framing, Define Application Scope, and Write Requirements
## Section 0 — Project Framing & Application Scope Checkpoint

| Field | Content |
|---|---|
| **Problem Evidence** | **Evidence 1:** A study of Erasmus student mobility across seven European countries identified lack of information about exchange programmes and difficulty finding a suitable institution or programme as barriers to participation. This suggests clearer information may help students explore suitable opportunities (Souto-Otero et al., 2013). **Citation:** Souto-Otero, M., Huisman, J., Beerkens, M., de Wit, H., & Vujić, S. (2013). Barriers to international student mobility: Evidence from the Erasmus program. *Educational Researcher, 42*(2), 70–77. https://doi.org/10.3102/0013189X12466696. <br>**Evidence 2:** A qualitative study of nine UUM undergraduates participating in a semester-long mobility programme in Indonesia identified unclear credit-transfer information, administrative coordination difficulties, and inadequate pre-departure guidance (Abdul Aziz et al., 2025). **Citation:** Abdul Aziz, M. N., Suppiah, S. M., & Mohd Yusoff, N. (2025). *Across borders and classrooms: Exploring curriculum through student mobility*. *International Journal of Islamic Theology & Civilization, 3*(4), 192–203. https://doi.org/10.5281/zenodo.17429731.|
| **Existing Approaches** | **1. Human academic advising:** UTM International and faculty advisors can provide contextual, personalised guidance, but access depends on staff availability and office hours. <br>**2. Static university webpages, forms, and PDFs:** UTM International pages, the FAI Academic Handbook, and pre-departure materials provide official information, but the information is spread across sources and can be difficult to navigate or become outdated. <br>**3. General-purpose AI assistants (e.g., ChatGPT and Claude):** Support flexible natural-language questions, but may not be grounded in relevant official UTM or host-university documents and may generate incorrect deadlines, rules, or eligibility criteria. |
| **Project / Engineering Gap** | Existing approaches are fragmented across multiple pages and documents, depend partly on staff availability, and may contain conflicting or outdated information. MobiliFAI will answer questions from a defined collection of official sources, provide citations, flag detected source conflicts or insufficient evidence, and decline requests outside its supported scope. |
| **Objective O1** | To design a document-grounded AI assistant for outbound student mobility based on a curated collection of approximately 5–7 official documents and webpages. |
| **Objective O2** | To develop the assistant with retrieval, citations, and guardrails that refuse visa, immigration, legal, and medical questions. |
| **Objective O3 (if relevant)** | To evaluate answer correctness, groundedness, latency, and safety. |
| **Intended Users** | FAI students planning to study abroad or join an exchange programme. |
| **Supported Tasks** | Answer questions about outbound mobility opportunities; explain application procedures, eligibility, deadlines, and pre-departure steps when supported by sources; summarise relevant academic and credit-related requirements; provide scholarship information where covered by the selected sources; cite supporting sources; and indicate when information is missing or conflicting. |
| **Information / Knowledge Domain** | Outbound student mobility, including UTM mobility procedures, FAI academic requirements relevant to mobility, pre-departure preparation, and selected exchange or scholarship opportunities described by the approved sources. |
| **Operating Context** | A student submits a question through a simple chat-style interface. The assistant searches the selected official documents and webpages and returns a concise answer with source references. The initial version is an informational support tool, not an official approval or application system. |
| **Important Constraints** | The initial knowledge base will contain approximately 5–7 selected official documents/webpages, with the final list confirmed during implementation. Sources may change, become outdated, or contain inconsistent details. Performance depends on the quality of text extracted from PDFs and webpages. The project has undergraduate time and resource limits. The assistant cannot confirm individual eligibility or replace advice from UTM International, faculty staff, or host universities. |
| **Explicit Out-of-Scope Tasks** | Providing visa, immigration, legal, or medical advice; making official decisions about admission, mobility selection, scholarship awards, credit transfer, or prerequisite waivers; submitting applications or contacting universities on a student's behalf; guaranteeing that a deadline or policy is current without source confirmation; and presenting unsupported facts as confirmed. |
| **Evaluation Focus** | Determine whether MobiliFAI answers supported outbound-mobility questions correctly and with verifiable citations, responds within an acceptable time, identifies missing or conflicting evidence, and safely declines out-of-scope questions. Evaluation will use curated questions, source checking, latency measurements, and out-of-scope test prompts. |

### Initial Data Sources (working list; approximately 5–7 sources)

1. **FAI Academic Handbook 2025–2026 (PDF)** — academic regulations and programme requirements. https://drive.google.com/file/d/1coXIUom2QVI_YlUtQ0CjFeNkZU2xOPy3/view
2. **UTM International Outbound Pre-Departure Briefing (PDF)** — outbound mobility procedures, preparation steps, and pre-departure requirements. https://international.utm.my/wp-content/uploads/2023/05/UTM-OUTBOUND-PREDEPARTURE-BRIEFING.pdf
3. **UTM Outbound Mobility Portal** — mobility programmes, participating universities, application procedures, eligibility, and deadlines. https://international.utm.my/utm-mobility-portal-2/
4. **JASSO Student Exchange Support Program** — exchange scholarship information, eligibility, and funding details where applicable. https://www.jasso.go.jp/en/ryugaku/scholarship_j/ukeire.html
5. **China Link Scholarship 2026 Application Guide, Shanghai International Studies University** — short-term study/research opportunities, eligibility, funding, and application procedures. https://www.oisa.shisu.edu.cn/index.php/en/index/newscontent/cid/130/id/663.html

The five entries above are the current starting list, not a strict limit. The final knowledge base may include approximately 5–7 sources if additional official documents directly support the project scope. Any added source should be relevant, authoritative, and recorded with its title and URL.

## Section 1 — Functional Requirements

| ID | Requirement — The system SHALL | Scope Boundary — SHALL NOT (where relevant) |
|---|---|---|
| FR1 | Answer questions about outbound mobility procedures, eligibility, deadlines, and pre-departure requirements when present in approved sources. | Invent requirements, deadlines, or eligibility details not supported by the sources. |
| FR2 | Retrieve relevant information from the defined collection of selected official documents/resources to formulate an answer. | Treat unrestricted web content or model memory as an approved source for project-specific rules. |
| FR3 | Provide a source reference for each substantive answer based on approved documents/resources. | Present unsupported information as official or provide a citation that does not support the answer. |
| FR4 | Indicate when approved sources do not provide enough information or relevant sources appear to conflict. | Silently choose between conflicting requirements or fabricate a resolution. |
| FR5 | Decline visa, immigration, legal, and medical questions, and redirect users to UTM International or the relevant official authority. | Provide personalised visa, legal, or medical advice, or make official mobility, scholarship, admission, or credit-transfer decisions. |

## Section 2 — Non-Functional Requirements

| ID | Quality Attribute | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| NFR1 | Performance | Return an answer for a typical supported query within an acceptable response time. | Measure end-to-end latency for at least 30 representative queries under the same test setup. | 90th-percentile response latency (p90) | ≤ 8 seconds |
| NFR2 | Correctness | Answers to supported questions shall be factually consistent with approved sources. | Compare answers against a manually prepared answer key and source passages. | Correct answer rate | ≥ 80% |
| NFR3 | Groundedness | Factual claims about mobility requirements shall be supported by cited source material. | Review at least 30 answers and check material claims against cited sources. | Supported-claim rate | ≥ 90% |
| NFR4 | Maintainability | A changed or replacement source shall be updateable without redesigning unrelated components. | Replace/update one source in a controlled test and time the workflow. | Time to update and re-index one source | ≤ 15 minutes |

## Section 3 — Responsible AI Requirements

| ID | Responsible AI Risk | Requirement | Measurement Method | Metric | Threshold |
|---|---|---|---|---|---|
| RAI1 | Hallucination / Transparency | Provide citations that support factual answers and state when evidence is insufficient. | Review at least 30 responses for citation presence and support. | Citation presence; citation-support rate | 100% citation presence for source-based factual answers; ≥ 90% citation-support rate |
| RAI2 | Safety and Scope | Refuse visa, immigration, legal, and medical advice requests and redirect users to an appropriate official authority. | Test at least 20 out-of-scope prompts across these categories. | Correct refusal-and-redirect rate | ≥ 95% |
| RAI3 | Outdated or Conflicting Information | Flag detected conflicts between approved sources or state that information needs confirmation when sources do not establish a clear answer. | Test cases with known missing or conflicting information. | Correct conflict/insufficient-evidence handling rate | ≥ 90% |

## Reflection
I think RAI1 may be the most difficult Responsible AI requirement to measure because having a citation does not always mean the answer is correct. The AI might provide a source but still misunderstand some information or include details that are not actually stated in the document. Therefore, I need to check whether the answer really matches the cited source, rather than just checking if a citation is provided. This can be challenging because it requires careful checking of the answers and their sources, especially when the information is unclear or complicated.
