# Grade Check — GED Prep

**Live at https://acherchu.github.io/grade-check/** — GitHub Pages off `main` in
[Acherchu/grade-check](https://github.com/Acherchu/grade-check). Pushing to `main` redeploys it;
a build takes about a minute. Note: something on this PC auto-commits and pushes this repo
("Auto-commit …" messages), so saved changes can go live without an explicit push — don't leave
`index.html` broken on disk.

A single-file study site with a level ladder: **7th Grade → 8th Grade → GED**. Each level unlocks the next.

- **Grade tests (7th, 8th)** — 100 random questions, 25 each of Math, English, Science, Social Studies.
  80 right certifies the grade; the 21st miss ends the test.
- **GED level** (unlocked by passing 8th) — the four GED tests: Mathematical Reasoning, Reasoning Through
  Language Arts (RLA), Science, Social Studies. One 50-question test each, sampled by GED topic weight;
  33 right ≈ an estimated 145 (passing); the 18th miss ends it. Scores are rough estimates (`estScore`),
  labeled unofficial. The GED level counts as done when all four are passed.
- Every failed test shows each mistake, how to fix it, and a link to that topic's lesson. Tests save mid-way.
- **Practice test** (button next to every test) — the same test (`start(level, subject, true)`), but each
  answer is marked right/wrong with the explanation before moving on, it never ends early, and it
  doesn't certify a grade or pass a GED subject. Results say whether the score *would* pass.
- **Practice & Learn** — level tabs (open on the highest unlocked level; practice is never locked) →
  subject → topic → lesson → unlimited practice. Topics missed on tests show as **weak spots**.

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
4. **Engine** — `LEVELS` builds the ladder: grade levels wrap `LESSONS`/`BANK` topics, the GED level wraps `GED`.
   `buildTest(level, subject)`, `sample()`, test screens, `gedTestsPage`, practice screens, `Sound`.
   `qHtml()` renders any question containing a blank line as a reading passage box + question.

Progress lives in `localStorage` key `gradecheck.v2` (`certified`, `best`, `gedPassed`, `gedBest`, `seen`,
`current`, `topics` keyed `level|subject|topicId`). On first load it imports results from the older
`gradecheck.v1` (grade) and `gradecheck.ged.v1` (GED) keys.

## Adding content

- New questions: `gedAdd('Science', 'body', generatorOrRow, ...)` in a `<script>` after the GED structure scripts.
- For reading questions, put the passage first, then a blank line (`\n\n`), then the question.
- Sanity check in the console: for every `GED[s].topics[].items`, generate each item many times and assert
  ≥2 unique choices including the answer; build ~10 tests per subject and confirm 50 questions each.

## Run it

Open `C:\Users\arche\grade-check\index.html` in any browser.

The file is now too big for the Browser pane's `file://` snapshot, so preview it over HTTP: there's a
`grade-check` entry in `C:\Users\arche\.claude\launch.json` (port 8083). Note `localhost` has its own
localStorage, separate from the live site.
