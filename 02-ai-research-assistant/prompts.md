# Prompts behind the ballot work

The live demo shows AI doing small transformations. The deeper projects (Cobb's 162,847 ballots, last week's Montgomery County runoff) used AI as a **coding partner** — to write scripts, debug them, and build review tools fast. The judgment stayed with the reporter. These are the kinds of prompts that did the work, in the order you'd actually use them.

> One rule throughout: AI is great at *transforming*, risky on *facts*. Every number gets checked against an official source before it goes anywhere.

---

## 1. Look before you write any code

```
I have a folder of scanned ballot images as multi-page TIFFs. Open one and tell me what's on each page. I'm looking for any page that's machine-printed text rather than handwriting or marks.
```

*Why:* the whole project hinges on finding the machine-readable layer. For these ballots it was page 3 — a clean, computer-printed summary of every race and choice.

---

## 2. Extract at scale

```
Write a Python script that, for every TIFF in these precinct folders, extracts page 3, runs OCR on it (tesseract, --psm 6), and pulls the line directly under "Governor - Rep". Output a CSV: filename, precinct, governor_vote. Parallelize it across CPU cores and print a progress count.
```

*Why:* one ballot is a recipe; 985 (or 162,000) is a job for code. Time one file, multiply, then parallelize.

---

## 3. Classify safely — watch for substring collisions

```
Match each OCR'd name to my candidate list. Use word-boundary regex, not substring matching — I do NOT want "rick" inside "kirkpatrick" to match "Rick Jackson". Show me any name that doesn't match anything.
```

*Why:* a naive `'rick' in name` check once handed 313 of Gregg Kirkpatrick's votes to Rick Jackson. `\brick\b` fixes it.

---

## 4. Build a manifest, then find the exceptions

```
From the OCR output, build a manifest with one row per ballot. Then tell me: how many ballots had no Governor vote parsed, and bucket them by why — Democratic ballot, nonpartisan, blank, or no summary page at all.
```

*Why:* exceptions aren't failures, they're a second dataset. In Montgomery, 118 had no GOP Governor vote: 86 were Democratic ballots and 32 were emergency/provisional ballots with no summary page — votes the computer never counted.

---

## 5. Build a review tool for the exceptions

```
Write a single self-contained HTML file to review these ballots by hand. Show the ballot image; let me record the Governor vote with one keystroke (R = Jackson, J = Jones, X = blank). Save results to a TSV. Make it resume where I left off, let me step back, and save progress automatically. Make the buttons big and high-contrast for a projector.
```

*Why:* one HTML file with hotkeys replaces hours of opening files by hand. (That's `ballot-review/index.html` in this folder.)

---

## 6. Reconcile against the official count

```
Official results say 477 Jones, 404 Jackson. My automated pass found 470 Jones, 394 Jackson. That's 7 and 10 short. Could the difference be in the ballots I couldn't auto-classify? Help me check the 32 emergency ballots and reconcile.
```

*Why:* this is how you catch your own errors before publishing. The 7 + 10 missing votes are sitting in those 32 hand-marked emergency ballots — review them and the count reconciles. **Never trust a total you haven't checked against the source.**

---

## 7. Go further — ticket-splitting

```
My manifest has one row per ballot with a column for each contest. Write code to cross-tabulate Governor vote against US Senate vote, and show me how many voters split their ticket — e.g. picked one Trump-endorsed candidate for Governor but a different lane for Senate. Give me the counts and the share of ballots that split.
```

*Why:* once every ballot is a clean row, the reporting questions open up. Cross-tabbing two contests on the same ballots is how you find the split-ticket story — and it's only trustworthy because the counts were reconciled first (step 6).
