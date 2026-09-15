# Grade Check — GED Prep

**Live at https://acherchu.github.io/grade-check/** — GitHub Pages off `main` in
[Acherchu/grade-check](https://github.com/Acherchu/grade-check). Pushing to `main` redeploys it;
a build takes about a minute. Note: something on this PC auto-commits and pushes this repo
("Auto-commit …" messages), so saved changes can go live without an explicit push — don't leave
`index.html` broken on disk.

A single-file GED study site organized around the four GED tests: Mathematical Reasoning,
Reasoning Through Language Arts (RLA), Science, Social Studies.

- **Take a Practice Test** — one 50-question test per subject, sampled across topics by GED weight.
  33 right ≈ an estimated 145 (passing). The 18th miss ends the test and shows every mistake with how
  to fix it and a link to that topic's lesson. Tests save mid-way (Resume card). Scores are rough
  estimates (`estScore`), clearly labeled unofficial.
- **Practice & Learn** — subject → topic (31 total) → lesson → unlimited practice with instant feedback.
  Topics missed on tests show as **weak spots** until practiced off.

Dim theme only; Web Audio sound effects with a mute button.

## How the code is organized (all in `index.html`)

1. **Helpers** — `MQ(q, correct, wrongs, explain)` builds a question; `fromTable`, `fromGroups`,
   `whichGroup`, `both` build questions from fact tables.
2. **Question library** (from the old 7th/8th grade version, still used as source material):
   `BANK[grade][subject]` + `LESSONS[grade][subject]` topics, with more added via `addTo(...)`.
   Their grade labels are no longer shown anywhere.
3. **`GED`** — the real structure. `GED[subject].topics[]` = `{ id, name, w, from, covers, intro, points, ex }`.
   `from` lists library topics (`"grade|subject|topicId"`) whose items are pulled in; each item gets `.ged = topicId`.
   New GED-only questions are added with `gedAdd(subject, topicId, ...items)` in later scripts.
4. **Engine** — `buildTest(subject)`, test screens, practice screens, `Sound`. `qHtml()` renders any
   question containing a blank line as a reading passage box + question.

Progress lives in `localStorage` key `gradecheck.ged.v1` (`passed`, `best`, `seen`, `current`, `topics`).

## Adding content

- New questions: `gedAdd('Science', 'body', generatorOrRow, ...)` in a `<script>` after the GED structure scripts.
- For reading questions, put the passage first, then a blank line (`\n\n`), then the question.
- Sanity check in the console: for every `GED[s].topics[].items`, generate each item many times and assert
  ≥2 unique choices including the answer; build ~10 tests per subject and confirm 50 questions each.

## Run it

Open `C:\Users\arche\grade-check\index.html` in any browser.
