# Demo Tasks — AI Research Assistant

Five real investigative tasks demonstrated live. Prompts are copy-paste ready — adapt the bracketed parts to your own work.

> **Reminder:** AI is great at structure and transformation, risky on facts. Spot-check anything it extracts against the source before you publish.

---

## Task 1: Pull data off a web page — no scraper required

Paste the page's HTML or a screenshot, or give the URL to a tool that can browse.

```
Here is a web page listing [what it lists, e.g. restaurant inspection results].

Extract every record into a table with these columns: [list columns].

Rules:
- One row per record.
- If a value is missing on the page, leave the cell blank — do not guess.
- Return the result as CSV so I can paste it into a spreadsheet.
```

Follow-up when the page is paginated or messy:

```
That's only the first 20 records. Here is the next page's content — append those rows in the same format.
```

---

## Task 2: Turn a messy PDF into a structured spreadsheet

Upload the PDF (or paste the text).

```
This PDF is a [type of document, e.g. campaign finance report].

Extract the data into a table with these columns: [list columns].

Rules:
- Do not infer or guess values that aren't in the document.
- For dates use YYYY-MM-DD. For money, use plain numbers with no $ or commas.
- If the document has multiple sections or pages, keep them all.
- Return the result as CSV.
```

Then validate:

```
List any rows where a value looked ambiguous or hard to read, and tell me which page each came from so I can check the original.
```

---

## Task 3: Compare two versions of a bill or contract

Paste both versions (or upload both files).

```
Here are two versions of [a bill / a contract]: VERSION A and VERSION B.

Show me everything that changed from A to B:
- Language that was added
- Language that was removed
- Language that was reworded (show before and after)

Group the changes by section. Ignore formatting-only differences. Flag any change that meaningfully shifts who benefits, who pays, deadlines, or penalties.
```

---

## Task 4: Clean and standardize messy names or addresses

Paste the column of values.

```
Here is a list of [company names / addresses] entered inconsistently by different people.

Create a table with two columns: the original value, and a standardized version.

Standardize: capitalization, abbreviations (St/Street, Inc/Incorporated), and obvious spelling variants of the same entity.

Do NOT merge two values unless you're confident they're the same entity. Flag any you're unsure about in a third column.
```

---

## Task 5: Build a quick app to browse and search your documents

Have a JSON or CSV file of records ready.

```
I have a [JSON/CSV] file of [what it contains].

Write a single self-contained HTML file that loads this data and lets me:
- See all records in a searchable, sortable table
- Filter by [field]
- Click a row to see the full record

No build tools, no server, no external dependencies — it should run by just opening the file in a browser. Put the data loading at the top so I can swap in my own file.
```

Iterate:

```
Add a free-text search box that filters across all fields as I type, and a count of how many records match.
```
