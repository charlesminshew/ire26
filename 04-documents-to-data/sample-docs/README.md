# Sample Documents — United States v. Fulton County

**Case:** United States v. Fulton County, No. 1:23-cv-04764 (N.D. Ga.)
**Source:** [CourtListener](https://www.courtlistener.com/docket/69519152/united-states-v-fulton-county/)

The United States sued Fulton County over conditions at the Fulton County Jail. These three documents trace the arc from what the government required to whether the county is actually complying.

| File | Pages | What it is |
|------|-------|------------|
| [consent-decree.pdf](consent-decree.pdf) | 71 | The agreement — what Fulton County must fix and by when |
| [monitor-report-1.pdf](monitor-report-1.pdf) | 40 | First independent monitor's assessment of compliance |
| [monitor-report-2.pdf](monitor-report-2.pdf) | 355 | Second monitor's report — much more detailed, covers more areas |

---

## Why these documents are useful for this class

**The Consent Decree** is structured legal language with specific, numbered requirements. Good for:
- Extracting a list of all requirements with deadlines
- Building a compliance tracker schema

**Monitor's First Report** is a narrative assessment — findings, conclusions, recommendations. Good for:
- AI triage: what are the most significant findings?
- Comparison: what changed between Report 1 and Report 2?

**Monitor's Second Report** at 355 pages is the real-world problem: *you can't paste this into ChatGPT*. Good for:
- Demonstrating context limits and chunking strategies
- Local extraction with PDFplumber
- Building a browse tool to navigate findings by section

---

## Demo prompts for each document

### Consent Decree — extract requirements as structured data

```
Read this consent decree. Extract every requirement Fulton County must comply with as a table with these columns:
- section_number
- requirement (one sentence summary)
- deadline (if stated, in YYYY-MM-DD; otherwise "ongoing")
- responsible_party

One row per requirement. If a field isn't stated, leave it blank — don't guess.
Return as CSV.
```

### Monitor's First Report — triage

```
Read this monitor's report as an investigative journalist. 

List the five most significant findings about jail conditions. For each one:
- What page is it on?
- What exactly did the monitor find?
- Is Fulton County in compliance on this point?
- What should a reporter follow up on?

Flag anything the monitor describes as urgent or deteriorating.
```

### Monitor's Second Report — chunking strategy

The 355-page report is too long to paste in full. Use this approach:

```
This is pages [X–Y] of the second monitor's report on Fulton County Jail. 

Summarize the key findings in this section. For each finding note:
- The topic area (staffing / medical / physical plant / etc.)
- Whether the county is compliant, partially compliant, or non-compliant
- Any specific numbers or dates mentioned
- Anything flagged as urgent

I will send you additional sections. Keep your summaries consistent so I can combine them.
```

### Compare Report 1 vs. Report 2

```
I have two monitor's reports on Fulton County Jail's compliance with a federal consent decree.

Compare them on these areas: [list specific areas from the decree].

For each area:
- What did Report 1 find?
- What did Report 2 find?
- Did conditions improve, worsen, or stay the same?
- What is still unresolved?
```
