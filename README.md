# BlindSpot – Project Gap Analyzer

An intelligent system that analyzes project documents, datasets and workflows to find **missing assumptions, overlooked constraints and important unasked questions**, then ranks them by potential impact with evidence-based recommendations.

**Live demo:** https://claude.ai/artifact/K6XsBoN4swtpQ9FMyqKJny

## Problem
Projects fail less from known risks than from things nobody thought to ask: no contingency budget, no rollback plan, unaddressed privacy law, dirty source data. These blind spots are usually found late, when they are expensive to fix.

## Solution
BlindSpot reads your input and compares it against a library of expert rules. When a topic is present but its safeguard is missing, it raises a finding. Each finding is scored, ranked and backed by the exact text or data that triggered it.

## Features
- **Document analysis:** 20 rules across Financial, Schedule, Data, Compliance, Technical, Operations, People, Planning, External and Inclusion categories.
- **Assumption detector:** flags vague or unvalidated language ("should be", "TBD", "ASAP", "easy", "etc.").
- **Dataset profiling (CSV/TSV/JSON):** missing values, duplicate rows, mixed data types, extreme outliers (3×IQR), constant columns, small samples, personal/sensitive columns.
- **Impact prioritisation:** each finding gets a score out of 10 and a level (Critical, High, Medium, Low).
- **Evidence-based output:** every finding shows highlighted source sentences or data counts, the question you may have missed, a recommendation and why it matters.
- **Readiness score:** a single percentage summarising overall completeness.
- **Filtering and export:** filter by level or category and copy the full report as text.
- **Privacy:** everything runs in the browser; no data is uploaded anywhere.

## How it works
1. **Input:** paste text or upload a `.txt`, `.md`, `.csv`, `.tsv` or `.json` file.
2. **Detection:** the system checks whether the input is tabular data or narrative text.
3. **Rule matching:** each rule has a *trigger* pattern (topic is mentioned) and a *covered* pattern (safeguard is mentioned). A finding fires when the trigger matches and the safeguard does not. Rules without a trigger (scope, risk register) fire whenever the safeguard is absent.
4. **Scoring:** `score = impact × confidence`. Confidence rises with the number of supporting sentences found (0.6 to 1.0); universal rules use 0.7.
5. **Ranking:** findings are sorted by score and labelled: ≥ 7.5 Critical, ≥ 6 High, ≥ 4 Medium, otherwise Low.
6. **Readiness score:** `100 − 1.6 × (sum of all scores)`, floored at 0.

## Usage
1. Open the live demo, or open `gap-analyzer.html` in any modern browser.
2. Paste your content or click **Upload file**.
3. Click **Analyze**.
4. Review findings from most to least critical. Use the chips to filter, and **Copy report** to export.

## Test data included
| File | Purpose |
|---|---|
| `sample_project_document.txt` | Smart Campus App plan with deliberate gaps (no contingency, testing, privacy, rollback, support plan; vague wording; single-person dependency). |
| `sample_student_data.csv` | 64 student records with planted issues: missing emails and phones, a duplicate row, an outlier GPA (98.5), a text value in a numeric column ("seven"), and personal-data columns. |

## Project files
- `gap-analyzer.html`: complete application (HTML, CSS and JavaScript in one file, no dependencies).
- `sample_project_document.txt`, `sample_student_data.csv`: sample inputs.
- `README.md`: this file.

## Extending the rules
Each rule in the `R` array of `gap-analyzer.html` has the form:

```
[category, title, triggerRegex | null, coveredRegex, impact(1-10), question, recommendation, whyItMatters]
```

Add a new line to create a domain-specific rule (for example healthcare, finance or construction).

## Limitations
- Matching is keyword-based, so it cannot understand deep context or detect gaps phrased in unusual wording.
- Findings are prompts for review, not verdicts; a flagged item may already be handled outside the document.
- Dataset profiling is statistical and does not know your business meaning of each column.

## Future scope
- LLM layer for context-aware reasoning and tailored questions.
- PDF and DOCX input.
- Custom rule packs per industry and team.
- Saved analyses and version-to-version comparison.
- Risk matrix and timeline visualisations.

## Tech stack
Plain HTML, CSS and vanilla JavaScript. No backend, no external libraries.
