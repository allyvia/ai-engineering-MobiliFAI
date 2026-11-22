# Week 4 — Data Quality & Bias Report (MobiliFAI)

**Student:** Sang Yen Ling | A24AI0126 | **Audit date:** 22 Nov 2026 | **Labels:** Observed / Inferred risk / N.V. (Not Verified)

## 1. Project and Data Overview

| Item | Detail |
|---|---|
| Project | MobiliFAI: a document-grounded assistant that answers FAI students' outbound mobility questions with citations (retrieval + generation, Weeks 5+). |
| Two requirements audited | **FR3 / RAI1** (every substantive answer cites a source: needs source title, URL and page per chunk). **FR4 / RAI3** (flag outdated or conflicting information: needs a reliable freshness profile and date signals per source). |
| Pathway | A: PDF/TXT (web pages stored as `.txt` snapshots). |
| Audited output | Part B artefact `data/processed/prepared_text.jsonl` (511 chunks, 7 sources) with `data/processed/source_profile.json`. Lineage: `data/week3_manifest.json`. Part C copy `data/pipeline_v1/documents/prepared_text.jsonl` is byte-identical (Observed: `cmp`), matching `pipeline_manifest.json`. |
| Sources | S01 FAI Handbook 2025/26 (245 chunks), S02 Pre-Departure Briefing (13), S03 UTM Calendar 2026/27 (11), S04 Academic Rules 2022, Bahasa Malaysia (226), S05 Mobility Portal snapshot (8), S06 JASSO (3), S07 SISU China Link (5). |
| Inspection unit / sample | Unit = chunk. **Population checks:** all 511 chunks (automated). **Spot-check sample:** 15 chunks (S01 x6, S02 x2, S03 x2, S04 x3 chosen at random, seed 4, length >100 chars, plus 2 known problem chunks S01-0001 and S01-0222) compared with a fresh `pypdf` extraction of the same page. S05–S07 are 16 chunks from three short snapshots (checked as a population). **Limit:** a small sample says nothing about all pages; random draw did not cover every source page. |
| Source / access status | Source files match the manifest hashes (7/7, Observed). Rights/permission: **N.V.** for all sources (S01 and S04 carry copyright notices). |

*Checkpoint A1:* if page or URL is missing, FR3 citations cannot be verified; if freshness is wrong, FR4/RAI3 would present an expired rule as current.

## 2. Five-Dimension Quality Scorecard

| Dimension | Finding (Observed unless stated) | Evidence | Impact on task | Decision |
|---|---|---|---|---|
| Completeness | All 7 sources and all 63 S01 pages are present, but S01 PDF pp.53–56 (printed 48–51, the 2025/26 calendar) have no text layer and survive only as 4 near-empty chunks (e.g. S01-0222 = `4 8`). 6 chunks are under 30 characters (4 in S01, S04-0011 `II`, S04-0025). | `source_profile.json` empty_pages = [53–56]; chunk lengths | 2025/26 dates cannot be answered from S01; near-empty chunks may be retrieved as noise (FR1/FR4). | **Warning** |
| Consistency | 241 of 245 S01 chunks (98%) are letter-spaced (`U T M K L i s i n i t s ...`; >50% one-character tokens), against 0 of 266 chunks in other sources. A fresh `pypdf` run on the same file (hash matches) gives normal text (S01 p.10). 237 of 511 chunks (46%, S03 and S04) have `source_url = "not recorded"`. Manifest paths (`pdf/...`, `web/...`) differ from repo paths (`data/*.pdf`, `data/web/*.txt`). | Whole-population scan; S01 p.10 comparison | The main handbook source is badly formatted for retrieval; citations for S03/S04 have no URL (FR3). Cause of spacing: **N.V.** (inferred: Colab environment or cleaning step). | **Review** |
| Accuracy | 15/15 spot-checked chunks match the source page once whitespace and hyphens are ignored, so characters are faithful. However the heuristic `signals` fail on S01: 0 of 245 chunks tagged `credit` or `deadline`, although the credit exemption/transfer text exists (S01-0206 to S01-0210, PDF p.47), while S04 has 53 `credit` chunks. | Spot-check list in Appendix; S01-0206..0210 `topics = []` | Tag-based filtering would miss the handbook's credit rules (FR1/FR4). | **Warning** |
| Timeliness | S01 is valid for session 2025/26, which ended on 4 Oct 2026 (printed p.49 calendar); `source_profile.json` still shows S01 `possibly_outdated = false`. Flagged outdated: S02 (undated), S04 (2022 ed.), S05 (2022 listings), S06 (page "as of Jan 2024"). Current: S03 (2026/27) and S07 (2026-01-28). | `source_profile.json`; handbook calendar | An expired session could be presented as current (RAI3). | **Review** |
| Uniqueness | 0 duplicate `record_id` and 0 duplicate chunk texts (normalised) among 511 chunks. The Part B and Part C `prepared_text.jsonl` files are identical by design (exception, not a defect). | Whole-population hash scan | Duplicate retrieval hits are unlikely from repeated chunks. | **Pass** |

## 3. Validation Findings

| Rule | Condition | Scope / result | Action on failure |
|---|---|---|---|
| **V1** (FR3/RAI1) | Each chunk has `source_id`, a non-empty text, a `source_url` starting with `http`, and an integer `page` for PDF sources (S01–S04). Web snapshots (S05–S07) have no pages, so URL is their locator. | 511 chunks: **274 pass, 237 fail** (S03 11, S04 226: URL "not recorded"). PDF chunks without page: 0. The 16 web chunks with `page = null` are valid exceptions, not defects. | Hard fail for final citations: record the official URL (S04: `ugs.utm.my/academic-rules/` is named by the handbook; S03 URL **N.V.**) or cite as "local file, title + page" until then. |
| **V2** (FR4/RAI3) | A source whose stated validity has ended, or whose listings predate 2026, must have `possibly_outdated = true`. | 7 sources: **6 pass, 1 fail** (S01: session ended 4 Oct 2026 but flag is false). | Review: add a manual `valid_until` field in the catalogue, which overrides the date heuristic. |

## 4. Duplicate, Anomaly and Coverage Findings

- **Anomalies (Observed):** letter-spaced S01 text (241 chunks); 6 chunks under 30 characters; S01 calendar pages without text. 29% of chunks start in lowercase (mid-sentence proxy, already noted in Week 3).
- **Coverage 1: current deadlines/dates (FR1, FR4).** Needed: current UTM exchange deadlines. Found: only 13 of 511 chunks (2.5%) have a date in 2026 or later (S03 11, S01 1, S07 1); the 2022 portal deadlines (S05, 7 chunks) are expired; no source gives a current UTM exchange deadline. Effect: deadline questions must return "not found / confirm with UTM International", not a guess.
- **Coverage 2: credit transfer (FR1, FR4).** Needed: English credit transfer rules. Found: 53 `credit` chunks come from S04 (Malay, 2022 edition); S01's English credit text is present but untagged (spacing). Effect: English questions may retrieve Malay text or miss the handbook rule.
- **Limit:** coverage was counted from heuristic tags and dates, not from a manual read of every chunk.

## 5. Bias Assessment

**Pipeline-induced / representation risk (Inferred risk, not proven unfairness).** Source decision: S01 was extracted as letter-spaced text and S04 is Malay → regex tags and keywords find few English credit/deadline chunks in S01 → retrieval may favour the Malay 2022 regulation or the stale 2022 portal over the current handbook rule → students receive outdated or incomplete credit/deadline answers. *Representation:* destination coverage is only Japan (S06, 3 chunks) and China (S07, 5 chunks); other destinations (e.g. Korea) are absent (**N.V.** whether students need them). **Controls:** re-extract S01 and re-run tagging; add bilingual keywords or a multilingual embedding model later; state source and date in answers; add Korea or other sources only if the requirement list needs them. No claim is made about system accuracy or fairness before Topic 5+ evaluation.

## 6. Privacy, Rights and Governance

| Question | Record |
|---|---|
| Provenance / version | Manifest has 7 records with SHA-256 for sources and prepared output (hashes verified 7/7). Versions: S01 2025/26, S03 2026/27, S04 16th ed. 2022, S05 snapshot 2026-11-16, S06 "as of 2024-01-31", S07 2026-01-28, S02 undated (**N.V.**). |
| Rights / permission | **N.V. for all 7.** S01 and S04 carry copyright notices. **Observed:** the GitHub repository can be cloned anonymously (public) and contains the raw PDFs (S01 about 20 MB, S02, S03, S04) and the prepared text, which conflicts with `week3_data_inventory.md` ("keep repo private; do not publish raw files"). |
| Privacy / sensitive data | Observed: organisational contacts only (`globaleducation@utm.my`, `oisa@shisu.edu.cn`, a few office phone numbers in S02/S04). No student records. The handbook lists staff names; a reliable PII scan of S01 is **N.V.** because letter-spacing breaks pattern matching. |
| Consent / collection basis | Not applicable to personal data (organisational public documents, no student data collected). Permission to reuse is the open point above. |
| Maintenance | Owner: student. Reviewer: lecturer / UTM International for source status. Revalidate when a source hash changes, a new session starts (Oct) or a source is replaced (NFR4). |

**Required safeguard:** make the repository private, or remove the raw PDFs (S01–S04) from the repository and its history, and commit only URLs and permitted derived files.

## 7. Mitigation and Impact on Requirements

| # | Priority improvement | Who / when | Requirements affected | Risk that remains |
|---|---|---|---|---|
| 1 | Make repo private or remove raw PDFs and history; obtain or record permission status for S01/S04. | Student, before Week 5 commit | Governance, RAI1 | Permission may be refused → S01/S04 excluded |
| 2 | Re-extract S01 (check `pypdf` version, test `pdftotext`/OCR for pp.53–56), re-tag signals, add `valid_until` and S03/S04 URLs; re-run V1/V2. | Student, before indexing (Week 5) | FR3, FR4, RAI1, RAI3, NFR4 | OCR may be imperfect; S04 stays Malay/2022 |

## 8. Quality-Gate Decision

**HOLD** for `data/processed/prepared_text.jsonl` as input to indexing and for public sharing. Scope: all 7 sources, Pathway A, at the Week 4 stage.

- **Evidence:** (1) 98% of S01 chunks are letter-spaced and untagged; (2) rights are N.V. while the repo is public with raw copyrighted PDFs; (3) V1 fails on 237 of 511 chunks (46%) and V2 fails on S01.
- **Conditions for release (GO WITH CONDITIONS):** actions 1 and 2 above done and V1/V2 re-run with documented results.
- **Residual risks:** stale sources (S02, S04–S06) stay in the corpus and must be flagged in answers; no current UTM exchange deadline exists in any source; coverage is limited to Japan and China.
- **Re-review trigger:** a new S01 extraction, any change in a source hash, a new session calendar, or a rights decision.

## Appendix: Evidence

- Spot-check (chunk, page): S01-0062 p24, S01-0083 p29, S01-0027 p12, S01-0191 p43, S01-0107 p32, S01-0129 p34, S02-0003 p3, S02-0002 p2, S03-0002 p1, S03-0001 p1, S04-0145 p56, S04-0204 p76, S04-0105 p42, S01-0001 p1, S01-0222 p53 → 15/15 match after ignoring whitespace and hyphens.
- Checks were run with a short additional Python script on the committed files (no new pipeline, no embeddings or retrieval). Evidence trail: `data/processed/prepared_text.jsonl`, `data/processed/source_profile.json`, `data/week3_manifest.json`, `notebooks/week3_pipeline.ipynb` (extractor: `pypdf`).
