# quarto-quizdown-mds-ext

A Quarto extension for interactive quizzes using [quizdown-js](https://github.com/bonartm/quizdown-js), extended with a matching question type, feedback after evaluation, partial credit, keyboard access and a print review sheet.

> **Fork of** [parmsam/quarto-quizdown](https://github.com/parmsam/quarto-quizdown) · upstream quizdown-js by [bonartm](https://github.com/bonartm/quizdown-js) · Both MIT licensed. Current version: 1.3.2.

## Install

```bash
quarto add UBC-MDS/quarto-quizdown-mds-ext
```

## Usage

Add the filter to your document front matter:

```yaml
---
filters:
  - quizdown
---
```

Then write quizzes inside a ` ```quizdown ` code block. One block is one quiz; each `##` heading is one question.

````markdown
```quizdown
---
shuffle_answers: true
---

## What is 2 + 2?

- [ ] 3
- [x] 4
    > Correct! Basic arithmetic.
- [ ] 5
```
````

## Question types

| Type | Syntax |
|------|--------|
| Single choice | Ordered list with checkboxes, `1. [x]` |
| Multiple choice | Unordered list with checkboxes, `- [x]` |
| Sequence | Ordered list without checkboxes; the authored order is the answer |
| **Matching** *(fork addition)* | `- Left :: Right` |
| **Classification / multi-match** *(fork addition)* | Same syntax; a right label that repeats becomes a reusable chip |

### Feedback and hints

These work the same way for every type:

| Syntax | Meaning |
|--------|---------|
| Indented `> text` under an option, item or pair | Comment for that option, shown after Evaluate |
| Top-level `> text` in a question | Hint, shown before answering when the student presses the lightbulb |
| Paragraph or code block under the heading | Context shown with the question, before answering |

### Matching (1-to-1)

Each right chip belongs to exactly one left prompt. Placing a chip on a second row moves it.

````markdown
```quizdown
## Match each animal to its sound!

- Dog :: Woof
    > Dogs bark to communicate.
- Cat :: Meow
    > Adult cats meow mainly with humans.
- Cow :: Moo
- :: Neigh
    > Horses neigh — a distractor here.
- :: Ribbit

> Think about farm and household animals.
```
````

| Syntax | Meaning |
|--------|---------|
| `- Left :: Right` | Correct pair |
| `- :: Right` | Distractor chip (never correct for any prompt) |
| Indented `> text` under a pair | Feedback for that pair after Evaluate |
| Indented `> text` under a distractor | Feedback shown when the distractor is placed |

The separator is a spaced ` :: `, so `::` inside code or names (`dplyr::filter`, `` `std::vector` ``) stays in the text: `- dplyr::filter :: keeps rows`. A line without a spaced separator splits at the first `::`, as in older quizzes. A line with no `::` at all (a typo such as `- Dog : Woof`) is skipped with a warning in the browser console, as is a chip text listed twice.

### Classification / multi-match

When the **same right-side label appears more than once**, it becomes a reusable chip, and so does every other chip in that question, distractors included. Good for classify-into-bins tasks. Keep at least as many chips as prompts (add distractors) or the count itself tells students that chips repeat.

````markdown
```quizdown
## Classify each ML task as Regression or Classification!

- Predict house price :: Regression
    > The target (price) is a continuous number.
- Detect spam email :: Classification
    > The target is a category: spam or not spam.
- Forecast stock return :: Regression
- Identify handwritten digit :: Classification
- :: Clustering
    > Clustering is unsupervised — no target label to predict.

> Regression → continuous target. Classification → categorical target.
```
````

## What students see

**Evaluate.** The Evaluate button appears on the last question (or once every question has been visited) and opens the results list: a score, one line per question with ✅ / ❌ (🟡 when partially right), and the comments for the options that matter. The line "Click a result line below to see your answers and feedback" is shown when `enable_retry` is on.

**After Evaluate** every question is locked until "One more time" (the reset button): options cannot be changed, sequence items cannot be moved, chips cannot be placed. A reopened question shows how it was answered:

| Type | Marks | Comments shown |
|---|---|---|
| Single / multiple choice | ✓ green for a correct option the student picked, ✗ red for a wrong one, dashed green ✓ for a correct option the student missed | under every option picked or correct, so a missed option explains itself |
| Sequence | ✓ for an item in its place, ✗ and its correct position (`→ 3`) for a misplaced one | under misplaced items only |
| Matching | ✓ / ✗ per row, with the correct chip after a wrong or empty row | per pair, and for a placed distractor |

**Print review.** When a student prints the page (`Cmd/Ctrl+P`), the interactive widget is replaced by a static review sheet: selected and correct answers, a `Missed` badge on a correct option the student did not pick, per-pair feedback, distractor feedback, and a status per question. No need to open the results screen first.

**Keyboard.** Every question type can be answered without a mouse, and a screen reader hears each change:

| Type | Keys |
|---|---|
| Multiple choice | Tab to an option, Space to tick or untick it |
| Single choice | Tab into the options, arrow keys move between them and pick |
| Sequence | Tab to an item; Arrow Up/Down moves it one place, Home/End to the top or bottom. The ▲▼ buttons are also reachable with Tab |
| Matching | Tab to a chip, Enter or Space selects it; Tab (Shift+Tab) to a row's slot, Enter or Space places the chip. On a filled slot with nothing selected, Enter picks the chip up; Delete or Backspace empties it. Escape drops the selection |

Slots read as `Dog: Woof` or `Dog: empty`, sequence items as `2 of 4: Brew coffee`, and each placement, clearing or move is announced. After Evaluate the keys change nothing, like the mouse.

## Customization

### Quiz options

A YAML block at the top of the quizdown block, before the first heading:

| Option | Default | Meaning |
|---|---|---|
| `shuffle_answers` | `true` | Shuffle options, sequence items and matching chips (sequence questions always shuffle) |
| `shuffle_questions` | `false` | Shuffle question order |
| `shuffle_stems` | `false` | Shuffle the prompts of a matching question; independent of `shuffle_answers`, which shuffles the chips |
| `nQuestions` | all | Show only this many (shuffled) questions |
| `primary_color` | `steelblue` | Accents, selected borders, progress bar |
| `secondary_color` | `#eef3f8` | Option, prompt and hover background (a slight blue) |
| `text_color` | `black` | Text on options |
| `locale` | browser | `en`, `de`, `fr` or `es` for the interface labels |
| `enable_retry` | `true` | Reset button, and reopening questions from the results list |
| `passingGrade` | unset | Percentage needed to show `customPassMsg` instead of `customFailMsg` |
| `label` | unset | Block label, see below |
| `partial_credit` | `false` | Fractional scores, see below |
| `reuse_chips` | by question | Whether a placed matching chip can also fill another row, see below |

### Per-question options

An options block under a question heading (before or after the list) applies to that question only and overrides the quiz YAML. Precedence: init config < quiz YAML < question YAML.

````markdown
```quizdown
---
shuffle_answers: true
---

## Match each animal to its sound

---
shuffle_answers: false
reuse_chips: true
---

- Dog :: Woof
- Cat :: Meow

## Classify

- Dog :: Mammal
- Cat :: Mammal
```
````

### Labels

`label` names the block; the prefix appears on the question heading, in the results list and on the printout. Questions are numbered in display order.

| Authoring | 1 question in block | n questions in block |
|---|---|---|
| no `label` | no prefix | `Q1:`, `Q2:` … |
| `label: B1` (quiz YAML) | `B1:` | `B1.1:`, `B1.2:` … |
| `label: false` (quiz YAML) | no prefix | no prefix |
| `label: B1a` (question YAML) | `B1a:` on that question | `B1a:` on that question |

A number works too (`label: 3` → `3.1:`); an empty string counts as unset. A question's own label applies to that question only.

### Partial credit and scoring

By default every question scores 0 or 1, and the quiz total is the number of questions fully right. With `partial_credit: true` (quiz YAML, or a question's own block), matching and multiple choice score a fraction. The rules are the ones PrairieLearn uses, so a student gets the same number here and in a PrairieLearn assessment built from the same item:

| Type | Score with `partial_credit: true` | PrairieLearn equivalent |
|---|---|---|
| Matching | correct pairs / prompts (3 of 4 right → 0.75) | `pl-matching` with `partial-credit="true"`, which is PrairieLearn's default |
| Multiple choice | (correctly selected − wrongly selected) / correct options, never below 0; nothing selected → 0 | `pl-checkbox` with `partial-credit="net-correct"` (formerly `PC`) |
| Single choice | 0 or 1, unchanged | `pl-multiple-choice` default |
| Sequence | 0 or 1, unchanged | `pl-order-blocks` with `partial-credit="none"`, the default |

The one difference in defaults: PrairieLearn gives matching partial credit unless told otherwise, this extension does not. Set `partial_credit: true` on matching blocks that mirror PrairieLearn items. PrairieLearn's other checkbox methods (`coverage`, `each-answer`) and LCS credit for ordering are not available.

The results show a fractional total with one decimal (`1.1/2`), rounded to the nearest tenth but never onto a whole number, so a partial score never reads as full or empty; whole totals keep the `02/02` look. A partly right question gets 🟡 in the results list, and the printout says `Partially correct (3/4)`. `passingGrade` uses the unrounded total.

### Chip placement (`reuse_chips`)

Whether a placed chip can also fill another row is decided per question, never per chip, so placing a chip reveals nothing about the answer key. In a classification question (some chip correct for 2+ rows) every chip can sit on several rows; in a 1:1 question, placing a chip on a new row moves it there. Override with `reuse_chips: true` / `false` (`false` is an error when a chip is correct for several rows). Dragging a chip out of a row always moves it. `shuffle_stems: true` shuffles the prompts as well and changes nothing about placement.

### Hand-written blocks

The scripts are loaded on every HTML page, so a `<div class="quizdown">` written directly in HTML works as well as a code block.

## Attributions

- Original Quarto extension: [parmsam/quarto-quizdown](https://github.com/parmsam/quarto-quizdown) (MIT, Sam Parmar 2025)
- quizdown-js library: [bonartm/quizdown-js](https://github.com/bonartm/quizdown-js) (MIT, Malte Bonart 2020-21)
