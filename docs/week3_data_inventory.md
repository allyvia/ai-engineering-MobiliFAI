# Week 3 — Data Inventory (MobiliFAI)
**Student:** Sang Yen Ling | A24AI0126 | **Pathway:** A (PDF / TXT) | **Real project data used — not the dummy pack.**

## A1 — Requirement → data map
| Requirement | Needed information | Source(s) | Why usable / constraint |
|---|---|---|---|
| FR1 / FR2 | Procedures, eligibility, deadlines, pre-departure steps | S02, S05, S06, S07 | Official; portal listings stale (2022), JASSO "as of Jan 2024" |
| FR1 / FR4 | Credit, registration, academic rules; term dates and public holidays (availability check) | S01, S03, S04 | S03 is the only 2026/27 source; S04 is Bahasa Malaysia (2022 ed.) |
| FR3 / RAI1 | Title, URL, **page number** for citations | All | Page kept per chunk (page-aware chunking) |
| RAI3 / FR4 | Version / validity / staleness | All | `source_profile.json` flags `possibly_outdated` |
| NFR4 | Update one source without redesign | All | Catalogue + SHA-256 manifest; single-source rerun |

## A4 — Inventory
| Source ID | File / URL | Type and purpose | Version / date / owner | Access / rights | Inspection evidence |
|---|---|---|---|---|---|
| S01 | `pdf/FAI_Academic_Handbook_2025_2026.pdf` · drive.google.com/file/d/1coXIUom2QVI_YlUtQ0CjFeNkZU2xOPy3/view | PDF; programme rules, credit load, credit transfer/exemption, calendar | 2025/2026 · FAI, UTM · sha256 `44a14636…` | Not verified; "all rights reserved" notice → keep repo private | 63 pages, 170,996 chars, 245 chunks. **PDF pp.53–56 (printed 48–51, calendar 2025/26) have no text layer.** Printed page = PDF page − 5 |
| S02 | `pdf/UTM_International_Outbound_Pre-Departure_Briefing.pdf` · international.utm.my/wp-content/uploads/2023/05/… | PDF slides; approval steps, timelines, insurance, report | Undated (uploaded 2023/05) · UTM International · `43ae6143…` | Not verified | 11 pages, 5,447 chars, 13 chunks. Slide 4 yields "at least 3 months before the departure date" |
| S03 | `pdf/UTM_Academic_Calendar_20262027.pdf` | PDF; semester dates, breaks, public holidays (availability) | 2026/2027 · UTM · `991821af…` | Not verified; source URL not recorded | 2 pages, 8,362 chars, 11 chunks; 11/11 contain dates; English + Malay tables |
| S04 | `pdf/UTM_Academic_Rules_and_Regulations.pdf` | PDF; academic rules (credit transfer etc.) | Edisi Keenam Belas, 2022 · UTM DVC (Academic & Intl) · `89eb1fb6…` | Not verified; copyright notice | 84 pages, 106,047 chars, 226 chunks; **Bahasa Malaysia**; edition may be superseded |
| S05 | `web/utm_mobility_portal.txt` · international.utm.my/utm-mobility-portal-2/ | Web snapshot; how to apply, programme list | Snapshot 2026-11-16 · UTM International · `b65ed36a…` | Public page; snapshot for coursework | 5,619 chars, 8 chunks. **Every listed programme/deadline is 2022** → flagged outdated |
| S06 | `web/jasso_student_exchange_support.txt` · jasso.go.jp/en/ryugaku/scholarship_j/ukeire.html | Web snapshot; scholarship amount, eligibility | Page "as of 2024-01-31" · JASSO · `aecdc6d9…` | Public page | 2,271 chars, 3 chunks; 80,000 JPY/month; applications only via Japanese school |
| S07 | `web/shisu_china_link_2026.txt` · oisa.shisu.edu.cn/…/id/663.html | Web snapshot; China Link scholarship guide | Page dated 2026-01-28 · SISU · `b2b2c996…` | Public page | 3,137 chars, 5 chunks; duration 1–12 months; apply ≥3 months before start |

**Checkpoint A:** FR1/FR3 need S01–S07 text with page + source identity; the loaded samples above (S02 slide 4, S05 snapshot) are in the notebook output.

## Known limitations found during inspection
- Web pages were captured as text snapshots (main content only); they will drift from the live pages.
- S01 calendar pages are images → need OCR (future); S03 (2026/27) covers current dates instead.
- S05 and S02 contain no usable current deadlines — the assistant must say so, not guess (FR4/RAI3).
- Rights are **not verified** for any source; do not publish the raw files or make the repo public.
