# Grade Check

**Live at https://acherchu.github.io/grade-check/** — GitHub Pages off `main` in
[Acherchu/grade-check](https://github.com/Acherchu/grade-check). Pushing to `main` redeploys it;
a build takes about a minute.

A single-file study site. Home has two modes:

- **Take a Test** — 100 randomized questions (25 each of Math, English, Science, Social Studies).
  80 right certifies the grade and unlocks the next one; the 21st miss ends the test and shows every
  mistake with how to fix it plus a link to that topic's lesson. Tests save mid-way (Resume card).
- **Practice & Learn** — pick a grade → topic (50 total) → short lesson → unlimited practice with
  instant feedback. Topics missed on tests show as **weak spots** until practiced off.

Grades 7 and 8 exist. Dim theme only; Web Audio sound effects with a mute button.

## Files

- `index.html` — the entire site. No build step, no dependencies. Double-click it and it runs.

## How the code is organized (all in `index.html`)

1. **Helpers** — `MQ(q, correct, wrongs, explain)` builds a question; `fromTable`, `fromGroups`,
   `whichGroup` build questions from fact tables.
2. **`BANK[grade][subject]`** — arrays mixing hand-written questions `[q, correct, [wrongs], explain]`
   and generator functions that return `MQ(...)`. Extra generators are `push`ed in later `<script>` blocks.
3. **`LESSONS[grade][subject]`** — topics `{ id, name, items, intro, points, ex }`. `items` are
   **indices into that BANK array**; a loop tags each bank item with `.topic`. If you insert or reorder
   bank items, update these indices — every item must belong to exactly one topic.
4. **Engine** — `buildTest` (avoids repeats using `progress.seen`, keyed by question + answer),
   test screens, practice screens, `Sound`.

Progress lives in `localStorage` key `gradecheck.v1` (`certified`, `best`, `seen`, `current`, `topics`).

## Adding content

- New questions: call `addTo(grade, subject, topicId, ...generatorsOrRows)` in a `<script>` placed after the
  `LESSONS`/`COVERS` scripts. It pushes onto the bank AND registers the index with the topic — no manual
  index bookkeeping. Helpers: `both(rows, fwd, back)` asks a two-column table in either direction; `cap`, `aan`.
- Each topic's one-line "Covers:" text lives in `COVERS[grade][subject][topicId]`.
- Practice flow: Practice home (grade tabs + subject cards) → subject page (topics + "Practice all" mix) → lesson → practice.
- New grade: add `BANK[9]` and `LESSONS[9]` with all four subjects — `GRADES` is derived from `BANK`.
- Sanity check in the browser console: build ~100 tests with `buildTest(g)` and assert every question
  has ≥3 unique choices including the answer, and every bank item has a `.topic`.

## Run it

Open `C:\Users\arche\grade-check\index.html` in any browser.
