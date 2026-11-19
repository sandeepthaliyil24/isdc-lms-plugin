# Report templates

Each report starts with the same header:

```markdown
# <Report name> — <Course title>

| | |
|---|---|
| Course URL | <url> |
| Target age group | <age group> |
| Reviewed on | <YYYY-MM-DD> |
| Slides reviewed | <n> of <total> |
| English variant | UK / US |
| Findings | <n> (Critical <n> · High <n> · Medium <n> · Low <n>) |

## Summary
<2–4 sentences: overall state and the most important problems.>
```

Then a findings table, followed by a "Not checked" section.

## 1. Technical bugs — `1-technical-bugs.md`

| ID | Location | Type | Severity | What happens | Evidence | Expected | Steps to reproduce |
|---|---|---|---|---|---|---|---|
| TB-01 | M1-L2-S3 · <url> | Broken link | High | "Download workbook" returns 404 | `https://…/workbook.pdf` → 404 | File downloads | Open slide, click "Download workbook" |

Types: Broken link · Missing image · Media won't play · Console error · Network error ·
Layout/display · Navigation · Progress tracking · Performance · Accessibility (only obvious
items: missing alt text, unreadable contrast).

Severity:
- **Critical**: the learner can't continue or complete the course, or progress or completion is lost.
- **High**: content is missing or broken (dead link, missing media, a blank slide).
- **Medium**: it works but looks wrong or confuses (layout overlap, wrong slide order).
- **Low**: cosmetic.

After the table, add a "Links checked" appendix: total checked, OK, broken, redirected.

## 2. Grammar & spelling — `2-grammar-spelling.md`

| ID | Location | Original (exact) | Issue | Corrected | Severity | Confidence |
|---|---|---|---|---|---|---|
| GR-01 | M2-L1-S4 | "The data are shows that…" | Extra verb | "The data show that…" | Medium | High |

Severity: **High** means the meaning is wrong or unclear · **Medium** means an obvious error a learner would notice · **Low** means minor or inconsistent.
Finish with a "Consistency" section covering terms spelled or capitalised more than one way (for example "e-mail / email").

## 3. Punctuation — `3-punctuation.md`

| ID | Location | Original (exact) | Issue | Corrected | Severity |
|---|---|---|---|---|---|
| PU-01 | M1-L1-S2 | "Its important to start" | Missing apostrophe | "It's important to start" | Medium |

Finish with a "Patterns" section for repeated issues, for example "bullets end with full stops on 12 slides and without them on 9". Give a recommendation for each.

## 4. Age suitability — `4-age-suitability.md`

```markdown
## Verdict
**Suitable / Suitable with changes / Not suitable** for <age group>, because <one sentence>.

## Scorecard
| Criterion | Rating (1–5) | Notes |
|---|---|---|
| Reading level | | estimated grade/age, avg sentence length |
| Vocabulary & jargon | | unexplained terms |
| Concept difficulty | | |
| Tone & engagement | | |
| Examples & relevance | | relatable to this age? |
| Visuals & imagery | | |
| Sensitive content | | |
| Cognitive load per slide | | words per slide, density |
| Length & pacing | | |

## Per-module assessment
| Module | Fit | Main issue |
|---|---|---|

## Findings
| ID | Location | Content (exact) | Concern | Suggested rewrite / change | Severity |
|---|---|---|---|---|---|
| AG-01 | M3-L2-S1 | "Leverage heteroscedasticity-robust estimators…" | Jargon far above 13–17 reading level | "Use methods that still work when the data is spread unevenly…" | High |

## What works well for this audience
<bullets>
```
