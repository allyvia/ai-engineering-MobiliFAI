# MobiliFAI
 
> A 24/7 AI assistant that answers FAI students' outbound study abroad and exchange questions, with citations from official documents.
 
## Problem Statement
FAI students struggle to navigate study abroad requirements because critical details on deadlines, credits and more are scattered across various UTM and host university websites. A 24/7 AI assistant solves this by instantly providing accurate answers cited directly from official documents.
 
## Evidence of the Problem
- Evidence 1: A study of Erasmus student mobility across seven European countries identified lack of information about exchange programmes and difficulty finding a suitable institution or programme as barriers to student participation. This suggests that clearer information about outbound opportunities and their requirements may help students explore suitable programmes (Souto-Otero et al., 2013). Source / citation: Souto-Otero, M., Huisman, J., Beerkens, M., de Wit, H., & Vujić, S. (2013). Barriers to international student mobility: Evidence from the Erasmus program. *Educational Researcher, 42*(2), 70–77. https://doi.org/10.3102/0013189X12466696
- Evidence 2: A qualitative study of nine UUM undergraduates participating in a semester-long mobility programme in Indonesia identified unclear credit-transfer information, administrative coordination difficulties, and inadequate practical pre-departure guidance. One participant specifically recommended updating credit-transfer information on the university website to reduce confusion (Abdul Aziz et al., 2025). Source / citation: Abdul Aziz, M. N., Suppiah, S. M., & Mohd Yusoff, N. (2025). *Across borders and classrooms: Exploring curriculum through student mobility*. *International Journal of Islamic Theology & Civilization, 3*(4), 192–203. https://doi.org/10.5281/zenodo.17429731
- Evidence 3: UTM International provides separate resources for outbound mobility programmes and their application requirements. This supports the need for a centralised assistant to help FAI students find relevant opportunities and understand application procedures. Source / citation: UTM International. (n.d.). UTM Mobility Portal. https://international.utm.my/utm-mobility-portal-2/
## Initial Related Work / Existing Approaches
- Study / solution 1: Human academic advising for outbound mobility (e.g., UTM International office and academic advisors). Source / citation: UTM International and Faculty advising services. Key limitation: Contextual and personalized, but availability is strictly limited by office hours, and published schedules often conflict, making immediate access difficult.
- Study / solution 2: Static university web pages, online forms, and official PDF documents. Source / citation: UTM International pages, FAI Academic Handbook, and Pre-Departure Briefing. Key limitation: Highly authoritative but fragmented across multiple platforms; information can be difficult to navigate and is partly outdated.
- Study / solution 3: General-purpose conversational AI assistants (e.g., ChatGPT, Claude). Source / citation: Standard web/chat interfaces. Key limitation: Flexible and accessible, but lacks grounding in specific UTM or host university official documents, often leading to invented deadlines, rules, or eligibility criteria (hallucinations).
## Engineering Gap / Limitation
Existing approaches are fragmented across many pages and documents, depend on staff availability, and contain conflicting or outdated information. There is no tool that answers a student's question from a fixed set of official documents, with a citation, a warning when sources conflict, and a refusal for questions outside the documents.
 
## Project Objectives
- O1: To design a document-grounded AI assistant for outbound student mobility based on five official documents.
- O2: To develop the assistant with retrieval, citations and guardrails that refuse visa, legal and medical questions.
- O3: To evaluate the assistant in terms of answer correctness, groundedness, latency and safety.
## Target Users
FAI students planning to study abroad or join an exchange programme.
 
## Data Sources
- Source 1: FAI Academic Handbook 2025–2026 (PDF) — https://drive.google.com/file/d/1coXIUom2QVI_YlUtQ0CjFeNkZU2xOPy3/view
- Source 2: UTM International Outbound Pre-Departure Briefing (PDF) — https://international.utm.my/wp-content/uploads/2023/05/UTM-OUTBOUND-PREDEPARTURE-BRIEFING.pdf
- Source 3: UTM Outbound Mobility Portal — https://international.utm.my/utm-mobility-portal-2/
- Source 4: JASSO Student Exchange Support Program — https://www.jasso.go.jp/en/ryugaku/scholarship_j/ukeire.html
- Source 5: China Link Scholarship 2026 Application Guide, Shanghai International Studies University — https://www.oisa.shisu.edu.cn/index.php/en/index/newscontent/cid/130/id/663.html
## Weekly Progress
| Week | Focus | Deliverable | Status |
|------|-------|-------------|--------|
| W1 | System map + project scope | README + docs | ☑ |
