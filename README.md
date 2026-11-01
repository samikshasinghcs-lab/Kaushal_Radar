# Kaushal Radar - final submission code (KSISU Skillathon 2026 finale)

> **How to run:** this repository holds the full structured package as `kaushal-radar-final.zip`. Download it, unzip, then `pip install -r requirements.txt` and `python run_all.py`.

Route decisions, not rankings. A decision-support prototype for Karnataka's labour-market intelligence: where to look, how sure we are, and no capacity decision until independent evidence agrees. **Nothing here is a government deployment and no department link is live.**

## Run everything
```
pip install -r requirements.txt
python run_all.py          # tests + AI evaluation + demos + regenerates reports/   (about 1 minute)
```

## Layout
| Folder | What | Data |
|---|---|---|
| `core/` | DGPI indicators, decision engine + Corroboration Gate, hash-chained ledger, reliability pipeline, RBAC/security, local API, **departmental file ingestion (`ingest.py`)** | **Real**: Yuvanidhi, GDDP/PCI, MSME (DES Karnataka) in `indicators_v2.csv`. Gate evidence cards other than Yuva Nidhi are MOCK and labelled |
| `ai_layer/` | Token-authenticated AI decision-support pipeline: retrieval, deterministic engine, contradiction/stale/denominator checks, AI analysts, source grounding, numeric guard, hallucination guard, decision gate, human review, audit; 141-case evaluation lab | **Synthetic** evidence; deterministic mock provider (not a language model) |
| `dashboard/` | Six-view dashboard prototype (open the HTML file) | Real data; ledger is browser-local |
| `data/` | Indicator table and **synthetic** sample departmental uploads (good, PII, schema drift) | see file names |
| `reports/` | Auto-generated evidence: AI evaluation, security, scalability, traceability, denominators, two-district routing; usability and corroboration are protocols marked NOT RUN | generated |
| `docs/` | OpenAPI v0.1 and data contract (proposed, not deployed) | - |

## Status (what is and is not claimed)
| Claim | Status |
|---|---|
| DGPI on 31 districts, denominator sensitivity | Built, reproducible |
| Corroboration Gate, ledger, rank stability | Built; non-Yuva-Nidhi evidence is mock |
| Departmental upload -> validate -> harmonise -> store -> evidence card -> retrieve | Built and tested on SYNTHETIC sample files |
| AI layer on Raichur question, citations, numeric check, contradiction, abstention, failure handling | Built; run on SYNTHETIC evidence with a mock provider |
| Live LLM evaluation | The Anthropic adapter exists but was never run live here; the 31 real LLM drafts were checked in `core/llm_brief_test.py` |
| Security tests | Automated; core API auth is a **header stub**, not production auth; no independent pen-test |
| AISHE, PLFS, NCS, ITI/NCVT, EPFO, e-Shram | **PROPOSED only. Not ingested.** |
| Usability test with real users | **NOT RUN** (protocol in `reports/USABILITY_REPORT.md`) |
| Independent corroboration of district signals | **NOT DONE** beyond a second denominator |

## Changes since the first submission
See `docs/CHANGES_SINCE_SUBMISSION.md`: brief checker rank check (65/69 -> 69/69, tuned on the same set, NOT held-out), API backlog and caching fix, concurrency test, routing scatter and seat-allocation note in `analysis/`.

## Not yet done (do not claim)
Blind adversarial test set; live-LLM run on all 31 districts; rules-only vs rules+AI comparison; user test with SUS; independent security/DPIA review; any officer conversation or department agreement.

## Known defects (kept visible on purpose)
- Prompt-injection detector is heuristic: 2 of 10 adversarial cases (paraphrase, Hinglish) are not caught.
- `evidence_analyst@1.1.0` regresses 15 evaluation cases; do not promote it.
- Truncating the newest audit entries is not detected without an external anchor.
- `core/analysis_denominators_and_ai_tests.py` and `core/llm_brief_test.py` need the raw source CSVs from the public GitHub repository.
- The `Kaushal-Radar-main` ChatGPT-built code was reviewed and not merged: its modules are small stubs that duplicate stronger ones here.
