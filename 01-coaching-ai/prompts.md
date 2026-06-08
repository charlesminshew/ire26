# Reusable Prompt Patterns — Coaching AI for Investigative Reporting

Copy, adapt, and use these in ChatGPT, Claude, or any chat-based AI.

---

## FOIA / public-records requests

```
I am a journalist at [outlet]. I am requesting records about [topic] from [agency].

Write a formal public-records request letter under [applicable law, e.g. Georgia Open Records Act / federal FOIA].

Include:
- Specific records sought (documents, emails, databases, logs)
- Date range: [start] to [end]
- Request for fee waiver on grounds of public interest
- Request for expedited processing if applicable
- My contact information placeholder

Tone: professional, specific, not adversarial.
```

---

## Decoding a data dictionary

```
I have a government dataset with the following fields. Explain what each field likely means in plain English, flag any fields that are ambiguous or that commonly cause problems in analysis, and suggest which fields I should join or cross-reference.

Fields:
[paste field names and any descriptions here]
```

---

## Spreadsheet formula help

```
I am working in [Excel / Google Sheets].

I have a column [describe the column and its data]. I want to [describe what you want to calculate or extract].

Write a formula that does this. Then explain each part of the formula in one sentence each so I understand what it does.
```

---

## Debugging code

```
I am writing [language] to [describe what the script does].

Here is my code:
[paste code]

Here is the error I am getting:
[paste error message]

Do not rewrite the whole script. Tell me:
1. What is causing the error
2. The minimal change needed to fix it
3. Whether there are any other obvious problems I should know about
```

---

## Pushing back on a vague AI answer

```
That answer is too general. I need you to be specific about [the exact thing you need].

Assume I already know the basics. Skip the caveats and disclaimers. Give me the actual [formula / letter / steps / code].
```

---

## Getting AI to cite its reasoning

```
[Your question here]

After your answer, list every assumption you made. Flag anything you are uncertain about. If any part of your answer could be wrong or depends on information I have not given you, say so explicitly.
```
