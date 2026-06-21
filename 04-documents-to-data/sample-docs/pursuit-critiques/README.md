# Sample Documents — GSP Pursuit Critiques

**Source:** Georgia State Patrol, Troop A — DPS-1114 pursuit critique forms, 2024
**Request:** Open records request to Georgia Department of Public Safety

Forty-six pursuit critiques from one troop in one year. Each is a completed DPS-1114 form — seven pages, consistent structure across all documents. A natural extraction project: known schema, repeating format, real investigative value.

| File | Pages | Notes |
|------|-------|-------|
| [perry-234-critique.pdf](perry-234-critique.pdf) | 7 | TFC Perry #234, Post 38-Rome. PIT involved. Born-digital PDF. |
| [cantrell-136-noeller-577-critique.pdf](cantrell-136-noeller-577-critique.pdf) | 7 | TFC Cantrell #136 + Tpr. Noeller #577, Post 43-Calhoun. Motorcycle pursuit, crash. Noeller received documented coaching for siren failure. Born-digital PDF. |
| [kennemore-113-critique.pdf](kennemore-113-critique.pdf) | 7 | TPR Kennemore #113, Post 29-Paulding. First pursuit. Letter of Instruction issued for pursuing without siren. **Scanned PDF with OCR artifacts** — good for showing the limits of extraction. |

---

## Why this document set works for class

- **Consistent schema**: Every DPS-1114 has the same sections and fields. You define the schema once.
- **Born-digital vs. scanned**: Perry and Cantrell extract cleanly. Kennemore has OCR noise — shows why you validate.
- **Real investigative questions**: Corrective action rates, PIT use, injury patterns, which posts have the most pursuits. The data tells a story.
- **Volume**: 46 documents is realistic. Too many to read by hand, few enough to validate your extraction before publishing.

---

## Extraction schema

These are the fields worth extracting from each DPS-1114 form.

```
incident_report_number     — e.g., "2024-038-0042"
pursuit_date               — YYYY-MM-DD
trooper_name               — last, first
trooper_badge              — numeric badge number
post                       — e.g., "Post 38-Rome"
reviewing_officer          — name of supervisor conducting review
critique_meeting_date      — YYYY-MM-DD

initiation_reason          — why pursuit began (traffic violation, felony, etc.)
pursuit_distance_miles     — numeric, miles
pursuit_duration           — in minutes:seconds if stated
termination_method         — how it ended (suspect stopped, crash, trooper terminated, etc.)

pit_involved               — yes / no
scrt_involved              — yes / no  (SCRT = tire deflation device)
use_of_force_involved      — yes / no

injuries                   — yes / no
injury_description         — who was injured and how; null if none
fatality                   — yes / no
vehicle_damage             — yes / no; brief description if stated

actions_objectively_reasonable   — yes / no (reviewer's finding)
corrective_action_taken    — yes / no
corrective_action_type     — "letter of instruction" / "documented coaching" / "counseling" / etc.; null if none
corrective_action_detail   — what specifically the trooper was corrected on

areas_for_improvement      — free text from reviewer
positives_noted            — free text from reviewer
```

---

## Demo prompts

### Basic extraction — single document

```
This is a Georgia State Patrol DPS-1114 pursuit critique form. Extract the following fields and return as JSON.

Fields:
- incident_report_number
- pursuit_date (YYYY-MM-DD)
- trooper_name
- trooper_badge
- post
- reviewing_officer
- critique_meeting_date (YYYY-MM-DD)
- initiation_reason
- pursuit_distance_miles (number only)
- pursuit_duration
- termination_method
- pit_involved (yes/no)
- scrt_involved (yes/no)
- use_of_force_involved (yes/no)
- injuries (yes/no)
- injury_description (null if none)
- fatality (yes/no)
- vehicle_damage (yes/no)
- actions_objectively_reasonable (yes/no)
- corrective_action_taken (yes/no)
- corrective_action_type (null if none)
- corrective_action_detail (null if none)
- areas_for_improvement
- positives_noted

Rules:
- If a field is not present in the document, return null.
- Do not guess or infer values not explicitly stated.
- For yes/no fields, return "yes" or "no" only.
```

### Triage — what should a reporter look at?

```
Read this pursuit critique form as an investigative journalist.

1. Was any corrective action taken? If so, what was it and why?
2. Were there any injuries or vehicle damage?
3. Was the pursuit objectively reasonable according to the reviewer?
4. What questions would you want to ask the trooper or the reviewing officer?
5. Is there anything in this document that stands out as worth following up?
```

### Cross-document analysis (after extracting all 46)

```
I have extracted data from 46 Georgia State Patrol pursuit critique forms from 2024. Here is the data as a JSON array:

[paste extracted JSON]

Answer the following questions:
1. How many pursuits resulted in corrective action? What types?
2. How many involved a PIT maneuver? How many of those resulted in injuries?
3. Which posts had the most pursuits?
4. What were the most common initiation reasons?
5. Were there any pursuits that ended in a fatality?

Return each answer as a number or percentage, and flag anything that seems like an outlier.
```

### Handling the scanned document (Kennemore)

The Kennemore critique is a scanned PDF. If extraction produces garbled text, try this:

```
This is a scanned document and may contain OCR errors. The document is a Georgia State Patrol DPS-1114 pursuit critique form. Despite any OCR artifacts, extract the fields listed below as best you can.

For any field where the text is unclear or unreadable, return "OCR unclear" rather than guessing.

[same field list as above]
```

---

## What to do with 46 documents

Once you have the extraction prompt working on the three samples, the workflow is:

1. Write a Python script (vibe-coded) to loop through all 46 PDFs and extract text with `pdfplumber`
2. Pass each document's text to the AI with your extraction prompt
3. Save each result as a JSON file
4. Validate 10–15 at random against the originals
5. Flatten to CSV for analysis

See [../../../04-documents-to-data/workflow.md](../../workflow.md) for the full five-step process.
