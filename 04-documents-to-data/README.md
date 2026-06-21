# Session 4 — Turning Documents into Data with AI

**Sunday 10:15 AM · 1 hour · Maryland 1-2**

Charles Minshew, Senior Editor of Data Journalism at The Atlanta Journal-Constitution

Agencies produce mountains of PDFs, scanned records, and semi-structured documents that are useless until you can query them. This session covers four strategies for getting structured data out of documents — from quick AI triage to local Python extraction to running a model entirely on your own machine.

## Four strategies

1. **Read & flag** — use Claude or ChatGPT to read a document and surface what a reporter should dig into further
2. **Extract structured data** — use PDFplumber and natural-pdf to pull tables and fields out of PDFs locally, with nothing sent to a server
3. **Organize at scale** — build a manifest, classify document types, and build review tools for exceptions
4. **Go local** — run an AI model on your own computer for sensitive documents or high-volume work

## What to bring

- Laptop (no tablets)
- Free account at [ChatGPT](https://chat.openai.com) or [Claude](https://claude.ai)
- A PDF you'd like to turn into data (optional)

## Sample documents

**United States v. Fulton County** (federal jail conditions case) — three documents, one case, a natural story arc:

- [Consent Decree](sample-docs/consent-decree.pdf) (71 pages) — what the feds required Fulton County Jail to fix
- [Monitor's First Report](sample-docs/monitor-report-1.pdf) (40 pages) — early compliance assessment
- [Monitor's Second Report](sample-docs/monitor-report-2.pdf) (355 pages) — detailed findings; too long to paste into ChatGPT, which is the lesson

See [sample-docs/README.md](sample-docs/README.md) for tailored demo prompts for each document.

**GSP Pursuit Critiques** — 46 DPS-1114 pursuit critique forms from Georgia State Patrol Troop A, 2024. Consistent schema, real investigative questions, mix of born-digital and scanned PDFs:

- [perry-234-critique.pdf](sample-docs/pursuit-critiques/perry-234-critique.pdf) — born-digital, PIT involved
- [cantrell-136-noeller-577-critique.pdf](sample-docs/pursuit-critiques/cantrell-136-noeller-577-critique.pdf) — born-digital, motorcycle pursuit, crash, documented coaching
- [kennemore-113-critique.pdf](sample-docs/pursuit-critiques/kennemore-113-critique.pdf) — scanned PDF with OCR artifacts

See [sample-docs/pursuit-critiques/](sample-docs/pursuit-critiques/) for the full set of 46 documents, extraction schema, and demo prompts.

## Materials

- [slides/](slides/index.html) — session slide deck (open in any browser, arrow keys to navigate)
- [workflow.md](workflow.md) — the five-step repeatable extraction workflow
- [case-study-cobb-ballots.md](../02-ai-research-assistant/case-study-cobb-ballots.md) — the Cobb County ballot classification project: 162,847 TIFFs, OCR + regex, and browser-based review tools
