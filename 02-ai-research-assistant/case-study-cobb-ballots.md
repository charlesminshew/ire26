# Case Study: AI-in-the-Loop Document Classification

**Cobb County, Georgia — May 2026 Primary ballot analysis**
162,847 TIFF files · 149 precincts · Governor + 2 Supreme Court races parsed

This is the deeper version of what the live demo shows in miniature: using AI as a coding partner to pull structured data out of documents that exist but haven't been extracted yet — locked inside images, PDFs, or scanned files.

> **Important framing:** No AI/LLM did the actual data extraction here — that was OCR + regex. AI (Claude) was the *coding assistant*: writing scripts, debugging, and iterating on the approach. **AI accelerates the coding, not the journalism.** The judgment calls — what to look for, how to verify it, what it means — stayed with the reporter.

---

## The general pattern

1. **Get the documents** — public records, scraping, bulk download
2. **Find the machine-readable layer** — almost every document has one if you look
3. **Write code to extract at scale** — parallel processing, OCR, regex
4. **Handle the exceptions** — build tools to review what the code can't
5. **Verify against known totals** — catch your errors before publishing

This works any time a government agency, court, or institution produces documents at volume.

---

## What we did

Cobb County publishes scanned ballot images as part of its post-election audit. Each ballot is a 3-page TIFF:

- **Page 1:** the printed ballot face (what the voter marked)
- **Page 2:** back of the ballot
- **Page 3:** a machine-generated **plain-text summary** of every race and choice

**Page 3 is the whole insight.** It's not handwriting — it's clean, computer-printed text, so OCR works reliably. Everything else flows from that.

> **Lesson:** Before writing any code, open a sample document and look at *every* page. The machine-readable layer is often hiding in plain sight.

### Classify, then parse

- **Classify** each file (Democratic / Republican / Non-Partisan / Emergency / Unknown) → `ballot_manifest.csv`, one row per file.
  Python + `pytesseract` + `Pillow`, with `multiprocessing.Pool` across 64 workers — 162K files in ~40 minutes.
- **Parse each race**: open page 3, find the line with the race name, read the next non-blank line, match it to a candidate lookup.

```python
# Match candidate names the safe way — with word boundaries
for key, candidate in lookup.items():
    if re.search(r'\b' + re.escape(key) + r'\b', answer_lower):
        return candidate
```

> **The bug that proves the rule:** the substring `rick` lives inside `kirkpatrick`. A naive `'rick' in text` check handed **313** of Gregg Kirkpatrick's votes to Rick Jackson. Word-boundary regex (`\bkey\b`) fixed it — and checking totals against official results is how we caught it.

---

## Exceptions are a second dataset

Every round of automation left a pile of records the code couldn't handle. Instead of opening files by hand, we built tiny **browser-based review tools** — one self-contained HTML file each:

- Shows the ballot image natively
- One keystroke per record (`D`/`R`/`N`, candidate numbers, etc.)
- Saves results as a TSV to merge back into the data

We built three: unreadable ballots (52 files missing page 3), unmatched Governor OCR (9 ballots), and unclassified Supreme Court votes (2,960 ballots).

**Design lessons learned the hard way:**

- **Write results to disk incrementally** — append mode, flush often. One crash shouldn't cost hours.
- **Build resume support** — on startup, read existing output, skip what's done.
- **Make the back arrow work** — reviewers make mistakes; let them step back one at a time.
- **Add a "load TSV" button** — reload saved progress and pick up exactly where you left off.

---

## The full workflow

```
Raw documents
     ↓
Sample inspection — find the machine-readable layer
     ↓
Build classifier → manifest.csv (one row per file)
     ↓
Exceptions? → build a review tool
     ↓
Parse race 1 → check totals against official results
     ↓
Repeat for each race; merge corrections
     ↓
Analysis (vote splitting, precinct breakdowns, …)
```

---

## Does this fit your story? Ask three questions

**1. Is there a machine-readable layer?** PDFs often have embedded text (try copying before OCR). Court filings, inspection reports, permits often have structured layouts. Scanned docs usually have a clean summary page somewhere.

**2. What's the exception rate?**
- 0–1%: pure automation + spot-check against known totals
- 1–5%: automation + a small review tool
- 5%+: reconsider — the machine-readable layer may not be reliable

**3. What are you counting?** Binary (present/absent) → pattern matching. Categorical (one of N) → keyword lookup with word boundaries. Continuous (dollars, dates) → regex + validation.

### Story types this fits

Court records · inspection reports · police use-of-force · campaign finance PDFs · property deeds · legislative roll calls · medical examiner releases — anywhere documents come at volume.

---

## Common pitfalls

| Problem | Symptom | Fix |
|---|---|---|
| Substring collision | Candidate A gets B's votes | Always use `\b` word boundaries |
| Wrong page crop | Race not found in OCR text | Test crop sizes; crop the bottom for races at the end |
| OCR misreads punctuation | Regex never matches headers | Don't require literal punctuation (`r'Justice.*Warren'`) |
| Memory-only processing | Crash = restart from zero | Write to disk incrementally; build resume support |
| Scale underestimate | 40-min job guessed at 5 | Time one file, multiply, divide by workers |
| No ground-truth check | Errors go undetected | Verify totals against an official source |

---

## Takeaways

1. **Find the machine-readable layer first.** Open your documents before writing any code.
2. **Build a manifest early.** One row per document, one column per thing you know. Never process the same file twice.
3. **Exceptions are not failures — they're a second dataset.** Build tools to review them.
4. **Check your work against known totals.** Catch errors before they're published mistakes.
5. **The browser is an underrated newsroom tool.** One HTML file with keyboard shortcuts and a TSV download replaces hours of spreadsheet work — no server required.
6. **AI accelerates the coding, not the journalism.** The judgment is still yours.
