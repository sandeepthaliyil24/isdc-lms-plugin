---
name: lms-qa-tester
description: >-
  LMS QA Tester. Walks through every slide of an online course in a live LMS (custom PHP
  LMSs such as IoA and WDC, or any LMS) in the browser, then writes four separate
  reports: technical bugs, grammar & spelling, punctuation, and age-group suitability.
  Use when the user says "QA this course", "test this course", "check the course content",
  "review the slides", "LMS QA", "/lms-qa-tester", or gives a course URL and asks for
  bugs, grammar, punctuation or age-appropriateness checks. Asks for the course URL and
  target age group if they are not given.
---

# LMS QA Tester

Review one course end to end, slide by slide, and produce four separate reports.
The LMS is usually **live production**, so everything here is read-only by default.

## Step 1 — Collect the inputs (ask only for what is missing)

Required before starting:

1. **Course URL**: the course's start page or outline page.
2. **Target age group**: who the course is for.

If either is missing from the user's message, ask for it with `AskUserQuestion` in one
call (both questions together when both are missing). For the age group offer:
`Children (8–12)`, `Teens (13–17)`, `Young adults (18–24)`, `Adults / professionals (25+)`.
The user can type another range via "Other". For the URL, ask them to type it in "Other".

Optional, use if given, never block on them: course name, the content's intended English
variant (UK/US), a learning-objectives list, and anything to skip.

Make a report folder in the current working directory:
`lms-qa-reports/<course-slug>-<YYYY-MM-DD>/`. Make the slug from the course title or the URL path.

## Step 2 — Open the course and get the user signed in

- Use the **built-in browser** (`mcp__Claude_Browser__*`) when present. If it isn't, use
  **Claude in Chrome** (`mcp__claude-in-chrome__*`). Before the first browser call, read the
  matching browser skill (`built-in-browser` or `chrome-browser`).
- Open the course URL. If a login page appears, **ask the user to sign in themselves** and
  wait for them to say it's done. Never type passwords, never sign out, and never change account settings.
- Recommend a dedicated test learner account, because viewing slides may record progress on a live site.

## Step 3 — Map the course

Before reading slides, build the outline: modules → lessons → slides/pages, with URLs or
navigation steps for each. Use `read_page` / `get_page_text` / `javascript_exec` (to list
links and menu items). Write it to `outline.md` in the report folder and tell the user the
size ("6 modules, 41 slides"). If there are many slides, keep going. Don't stop to ask.

## Step 4 — Walk through every slide

For **each** slide, in order:

1. **Capture the text exactly.** Use `get_page_text`. If the slide content is in an iframe,
   canvas, SVG or image (common for slide decks and SCORM/H5P players), take a
   `screenshot` and read the text from it, using `zoom` on small text. Note "text read from image".
2. **Look at it.** Take a screenshot when the layout matters: overlapping or cut-off text,
   unreadable contrast, missing images, broken layout, wrong slide order.
3. **Check technically**, with a `javascript_exec` snippet per page:
   - Images not loaded: `[...document.images].filter(i => i.complete && i.naturalWidth === 0)`
   - All links and embeds (`a[href]`, `iframe[src]`, `video`, `audio`, `source`). Then check
     each unique URL once. Same-origin: `fetch(url, {method:'HEAD', credentials:'include'})`,
     falling back to GET if HEAD is refused. External: a shell `curl -sIL -o /dev/null -w "%{http_code}" --max-time 15`.
     Wait about 1 second between requests. Never crawl in parallel against a live site.
   - `read_console_messages` with `onlyErrors: true` and `read_network_requests`: note
     4xx/5xx responses and failed media/scripts.
   - Media: does the video/audio load and start? Do not watch it all the way through.
   - Navigation: do Next/Previous/menu links go to the right slide? Are any slides missing,
     duplicated, out of order or blank? Does progress/completion update as you go?
4. **Append to `slides.md` immediately**, one section per slide:
   `## M<m>-L<l>-S<s> — <slide title>`, then URL, verbatim text, a visual note, and the
   technical issues found. Write as you go so a long course survives context limits. Never
   rely on memory for quotes.

**Live-site rules (do not break them):**
- Do not submit quizzes, assessments, forms, feedback or comments, do not enrol or pay, and
  do not do anything that emails people **unless the user says yes in chat for that action**.
  You may read quiz questions and their options. If a slide can't be passed without
  submitting, ask the user.
- Only click navigation that a learner would click: Next, Previous, menu items.
- If a slide is locked or behind something you can't get past, log it as a finding and carry on.

## Step 5 — Write four separate reports

Once all slides are captured, write each report from `slides.md` using the templates in
[references/report-templates.md](references/report-templates.md):

| File | Covers |
|---|---|
| `1-technical-bugs.md` | Broken links, missing images/media, console/network errors, layout and display bugs, navigation and progress-tracking problems, slow or failed loads |
| `2-grammar-spelling.md` | Spelling, typos, grammar (agreement, tense, articles, word order, wrong word e.g. its/it's, their/there), unclear or broken sentences, inconsistent terminology |
| `3-punctuation.md` | Full stops, commas, apostrophes, quotation marks, hyphens vs dashes, colons/semicolons, spacing around punctuation, capitalisation after punctuation, inconsistent bullet punctuation |
| `4-age-suitability.md` | Whether the content fits the target age group, judged with [references/age-guidelines.md](references/age-guidelines.md) |

Also write `README.md` in the folder: course, URL, age group, date, slide count, a one-line
count per report, and the five most important fixes.

**Accuracy rules for all reports:**
- Every finding has a location (`M2-L1-S4` + URL), the **exact quoted text**, the problem,
  and a concrete fix (the corrected sentence, not "rephrase this").
- Work out the English variant (UK or US) from the content or the user. Do not flag
  consistent UK spellings (organisation, colour) as errors, but flag mixed usage.
- Don't report style preferences as errors. If you're unsure, mark the finding
  `Confidence: low` instead of leaving it out or overstating it.
- One finding goes in one report only. A typo is grammar and spelling. A missing apostrophe is punctuation.
- If a report has no findings, say so and state what was checked.

## Step 6 — Deliver

Tell the user where the folder is (a markdown link to each of the four reports), give the
counts by severity, and list the top fixes. Also list anything you could not check: locked
slides, quizzes not submitted, media not played to the end, or text that couldn't be read
from images.
