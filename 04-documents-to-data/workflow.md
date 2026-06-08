# Five-Step Document-to-Data Workflow

A repeatable process for extracting structured data from documents using AI.

---

## Step 1: Understand your documents

Before writing any prompts, answer these questions:

- What is the document type? (court filing, inspection report, contract, etc.)
- Is it born-digital PDF, scanned, or a mix?
- What fields do you want to extract?
- How consistent is the structure across documents?
- What is the volume? (a handful vs. thousands)

The answers determine whether AI extraction is the right tool and how much validation you'll need.

---

## Step 2: Define your schema

Decide exactly what fields you want before you start prompting. Write them down.

```
Example schema for inspection reports:
- facility_name
- facility_address
- inspection_date
- inspector_name
- violations (list)
- violation_severity (for each: critical / non-critical / other)
- corrective_action_required (yes/no)
- follow_up_date
```

The tighter your schema, the more consistent the output.

---

## Step 3: Write and test your extraction prompt

Test on 5–10 documents before running at scale.

```
Extract the following fields from this document. Return the result as JSON.

Fields to extract:
- [list your fields here]

Rules:
- If a field is not present in the document, return null for that field.
- Do not infer or guess values that are not explicitly stated.
- For dates, use YYYY-MM-DD format.
- For lists (e.g. violations), return an array of strings.

Document:
[paste document text here]
```

After each test, check: are all fields populating? Are any fields being hallucinated?

---

## Step 4: Validate before you scale

Pick 20–30 documents at random. For each:

1. Read the original document
2. Compare it to the extracted JSON
3. Log every discrepancy

Decide on an acceptable error rate for your story. For anything you'll publish, spot-check manually at the end.

Common problems to watch for:
- Dates in wrong format
- Multi-value fields collapsed into one string
- Null fields that should have values (AI skipped them)
- Hallucinated values (AI filled in something not in the document)

---

## Step 5: Export and analyze

Once validated, export to CSV or load directly into your analysis tool.

```
Convert the following JSON array to CSV. Include a header row with field names.

[paste JSON here]
```

Or write a simple Python script to flatten your JSON files into a single CSV for analysis in Excel, Google Sheets, or a database.

---

## When AI extraction is not enough

- Heavily degraded scans: try [Adobe Acrobat OCR](https://acrobat.adobe.com) or [ABBYY FineReader](https://www.abbyy.com) first
- Handwritten documents: AI can help but error rates are high — validate everything
- Volume over ~1,000 documents: move to the API rather than the chat interface
- Legal-standard accuracy: always have a human review a random sample before publishing
