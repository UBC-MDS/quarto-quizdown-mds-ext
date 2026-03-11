# quarto-quizdown-mds-ext

A Quarto extension for interactive quizzes using [quizdown-js](https://github.com/bonartm/quizdown-js), extended with additional question types and review features.

> **Fork of** [parmsam/quarto-quizdown](https://github.com/parmsam/quarto-quizdown) · upstream quizdown-js by [bonartm](https://github.com/bonartm/quizdown-js) · Both MIT licensed.

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

Then write quizzes inside a ` ```quizdown ` code block:

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
| Single choice | Ordered list `1. [x]` |
| Multiple choice | Unordered list `- [x]` |
| Sequence | Ordered list without checkboxes |
| **Matching** *(fork addition)* | `- Left :: Right` |
| **Classification / multi-match** *(fork addition)* | Same syntax — repeated right label auto-detected |

### Traditional matching (1-to-1)

Each right chip maps to exactly one left prompt. Chips disappear from the pool once placed.

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

### Classification / multi-match

When the **same right-side label appears more than once**, it automatically becomes a reusable chip that stays in the pool. Perfect for classify-into-bins tasks.

````markdown
```quizdown
## Classify each ML task as Regression or Classification!

- Predict house price :: Regression
    > The target (price) is a continuous number.
- Detect spam email :: Classification
    > The target is a category: spam or not spam.
- Forecast stock return :: Regression
    > Returns are continuous values, not categories.
- Identify handwritten digit :: Classification
    > Digits 0–9 are discrete categories.
- Estimate delivery time :: Regression
- :: Clustering
    > Clustering is unsupervised — no target label to predict.

> Regression → continuous target. Classification → categorical target.
```
````

**Syntax rules for both modes:**

| Syntax | Meaning |
|--------|---------|
| `- Left :: Right` | Correct pair |
| `- :: Right` | Distractor chip (never correct for any prompt) |
| Indented `> text` under a pair | Per-pair feedback shown after evaluation |
| Indented `> text` under a distractor | Feedback shown when distractor is placed |
| Top-level `> text` | Overall question feedback / hint |

## Print review mode

When a student prints the page (`Cmd/Ctrl+P`), the interactive widget is replaced with a static review sheet showing selected answers, correct answers, per-pair feedback, distractor feedback, and question-level status for all question types. No need to navigate to the results screen first.

## Attributions

- Original Quarto extension: [parmsam/quarto-quizdown](https://github.com/parmsam/quarto-quizdown) (MIT, Sam Parmar 2025)
- quizdown-js library: [bonartm/quizdown-js](https://github.com/bonartm/quizdown-js) (MIT, Malte Bonart 2020-21)
