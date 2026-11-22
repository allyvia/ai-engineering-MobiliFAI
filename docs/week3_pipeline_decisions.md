# Week 3 — Pipeline Decisions, Stage Contracts and Traceability

## Requirements traced
| ID | Why it needs data preparation |
|---|---|
| FR1 / FR2 | Answers come only from the selected official documents → ingest 7 fixed sources |
| FR3 / RAI1 | Citations need title, URL and **page** → page must survive chunking |
| FR4 / RAI3 | Missing/outdated/conflicting info must be flagged → per-source freshness profile + empty-page flags |
| NFR4 | One source must be re-ingestable → catalogue + hashes + deterministic rerun |
| RAI2 (later) | `visa_insurance` topic tag prepares the out-of-scope guardrail |

**Selected sources:** S01–S07 (see `week3_data_inventory.md`). **Pathway:** A only; web pages converted to `.txt` snapshots because the lab pathway reads PDF/TXT. No image/table/time-series pathway is justified.

## Part B comparison (same 7 sources, 800-char chunks)
| Approach | Chunks | Page-traceable | Starts lowercase (mid-sentence proxy) | Avg chars |
|---|---|---|---|---|
| A. Fixed-size (whole doc) | 364 | 0% | 66% | 794 |
| B. Paragraph-aware (lab default, whole doc) | 378 | 0% | 34% | 765 |
| **C. Page-aware (selected)** | **511** | **100%** | 29% | 564 |

**Selected: C, page-aware, 800 chars.** *Benefit:* every chunk carries its page, which FR3/RAI1 need for verifiable citations; fewer mid-sentence starts than fixed. *Accepted limitation:* more, smaller chunks; PDF line wraps still split some sentences (29% start lowercase); the lowercase proxy is crude; no heading/section detection. Chunk size 800 (not the lab's 500) so a deadline and its condition tend to stay together — not tuned beyond this.

## Added step: requirement signal tagging (heuristic v1)
Each chunk stores `signals`: dates, duration phrases ("at least 3 months before"), topics (deadline, credit, eligibility, funding, calendar, visa_insurance) and `max_year`. This supports the project idea of detecting deadlines/durations from both PDFs and web pages and, later, matching them with holiday/term windows from S03. **Limit:** regex only; Malay keywords are partial; it is *not* verified extraction. Accuracy belongs to Topic 4.

## Freshness profile (`source_profile.json`)
`possibly_outdated` = latest year found in date expressions (else the catalogue's declared year) < 2026. Result: S02, S04, S05, S06 flagged; S01, S03, S07 not. Heuristic only.

## ETL vs ELT; batch vs streaming
- **ETL:** sources are small (~300 KB text), cleaned and chunked *before* storage so the stored artefact is already traceable.
- **Batch:** sources change per semester or per announcement, not continuously; rerun on update (NFR4).

## Stage contracts
| Stage | Contract / evidence | Failure response |
|---|---|---|
| Ingest | Explicit path exists; SHA-256 recorded | Stop — missing input |
| Inspect | Readable text per source; pages < 30 chars listed | Flag empty pages; **stop** if a whole source has no text |
| Prepare | Page-aware chunks, none empty | Stop on transformation error |
| Validate | Outputs exist, non-empty, reloadable, hashed; Part C == Part B bytes | Stop — missing/empty/mismatch |
| Record lineage | Each source → output with hashes, version, rights, limitations | Stop/report missing mapping |

## Outputs and evidence
`data/processed/prepared_text.jsonl` (511 chunks), `data/processed/source_profile.json`, `data/week3_manifest.json` (7 records, verified PASS), `data/pipeline_v1/documents/` (outputs + `pipeline_manifest.json`; rerun PASS).

## Caveats
Rights/version not verified for all sources (S01/S04 carry all-rights-reserved notices). Commit prepared text only to a **private** repo; do not commit raw files. Embeddings, vector index and the answer/guardrail layers are out of Week 3 scope. Availability matching against holidays is planned, not implemented.

## Traceability update (extends Week 2 RTM)
| Objective | Requirement | Week 3 evidence |
|---|---|---|
| O1 | FR2, FR3 / RAI1 | Chunks with source id, URL, page; manifest |
| O3 | NFR3, FR4 / RAI3 | Freshness profile, empty-page flags |
| O3 | NFR4 | Hash-verified rerun (Cell 9) |
