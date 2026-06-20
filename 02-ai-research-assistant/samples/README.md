# Demo Samples

Pre-staged files for the live demo. Audience: investigative and data reporters. Prompts for each task are in [../demo-tasks.md](../demo-tasks.md).

| Task | File | What it is | Notes |
|------|------|-----------|-------|
| **1 · Web page → table** | **live URL** + `task1-cou-passenger-load-2002-2025.csv` (backup) | Columbia Regional Airport (COU) monthly passenger load data — clean server-rendered HTML table, **one page per year 2002–2025** | See below — the per-year pagination is the demo's best moment |
| **2 · Messy PDF → spreadsheet** | `task2-tsa-throughput-report.pdf` | Most recent weekly TSA throughput report (~1,000-page PDF, airport/checkpoint/hour) from the [TSA FOIA reading room](https://www.tsa.gov/foia/readingroom) | A table no one would copy-paste by hand — that's the point. Includes ATL |
| **3 · Compare two versions** | `task3-hb268-A-as-introduced.pdf` + `task3-hb268-B-as-passed.pdf` | Georgia HB 268 (2025), the post-Apalachee school-safety bill — **as introduced** (58 pp) vs **as passed** (57 pp) | See "buried changes" below — a real story about a bill drifting from mental health to physical security |
| **4 · Clean messy names/addresses** | `task4-fulton-precincts-messy.csv` | Fulton County May 2026 precinct list (130 rows) | Messy: `precinct_name` ("01B & 01E"), `precinct_address` (inconsistent spacing), `precinct_location_name` (abbreviations) |
| **5 · Build a browse app** | `task5-habersham-ballots.csv` | **9,091** Habersham County ballots: precinct, party, governor vote — the miniature of the Cobb case study | The point is **iterative building** (start simple, keep asking). See below |

## Task 1 — Columbia Regional Airport passenger data

Live page: `https://www.flycou.com/passenger-load-data/?ParamAirline&ParamYear=2025`
Each year is its own URL — swap `ParamYear=` from **2002** to **2025**.

Columns: Month, Enplanements, Deplanements, Monthly Total, Increase/Decrease Previous Year, Lodging Tax. Twelve rows per year, in a real `<th>/<td>` HTML table (works whether the AI browses the URL *or* you paste the page source).

Why it's a strong demo:
- **Reliable** — server-rendered HTML, no login, no JS wall.
- **Paginated by year** — extract 2025, then "now do every year back to 2002." That's exactly the "here's the next page, append those rows" follow-up in [demo-tasks.md](../demo-tasks.md), and the most satisfying scaling moment.
- **Schema drift** — older years (e.g. 2002) drop the Lodging Tax column; the AI has to handle it.
- **Ties to Task 2** — airport passenger counts alongside the TSA throughput PDF.

`task1-cou-passenger-load-2002-2025.csv` is the pre-scraped **expected output** (288 rows, all 24 years) — your backup if wifi/the tool stalls.

## Task 3 — the buried changes (your cheat sheet)

Don't diff blind on stage. Between *as introduced* and *as passed*, HB 268:

- **Added:** a mandatory **mobile panic alert system**; **school mapping data** procurement; GEMA rulemaking authority; **civil-liability immunity**; new code titles (juvenile code Title 15, emergency management Title 38).
- **Dropped/changed:** mental-health **coordinator reimbursement grants**; **Tier 1/Tier 2 behavioral health training**; behavioral/emotional **screening assessments**; references to DBHDD as legal custodian.

The through-line: the bill shifted emphasis from *mental health* toward *physical security/surveillance*. Ask the AI to "group changes by section and flag anything that shifts who benefits or who pays" — then point to the panic-alert/mapping additions.

> Source: both versions downloaded from the Georgia General Assembly document API (legis.ga.gov), 2025–2026 session.

## Task 5 — the goal is iterative problem-solving

The point isn't a polished app — it's showing how you **build by iterating**: ship something tiny, look at it, ask for the next thing. A natural arc:

1. "Make a searchable, sortable table from this CSV in one self-contained HTML file."
2. "Add a filter by precinct and by party, plus a count of matching rows."
3. "Let me click a row to see the full record."
4. "The governor votes have OCR typos — group near-identical names so I can spot them."

Talking points baked into the data: `governor_vote` shows real OCR artifacts — `Michael ”Mike” Thurmond` (curly quotes), `John F- Kennedy` (dash for period), `Brad Raffenspe` (truncated), `[unread]`. And both **Rick Jackson** (2,296) and **Gregg Kirkpatrick** (37) appear — the exact substring-collision (`rick` inside `kirkpatrick`) the [Cobb case study](../case-study-cobb-ballots.md) warns about.

## Through-line for this audience

Tasks 2, 3, and 5 are all **government records turned into data** — the same arc as the [Cobb ballots case study](../case-study-cobb-ballots.md). Task 5 *is* that case study in miniature: the Habersham ballots came from the same pipeline as Cobb's 162,847, so building a browser to explore them mirrors the real project's review tools.

## Backup plan

Live demos break. Save the **expected output** of each task here (e.g. `task2-expected.csv`) so if a tool stalls or wifi dies you can show the finished result and keep moving.
