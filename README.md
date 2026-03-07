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

### Matching question

```markdown
## Match each animal to its sound!

- Dog :: Woof
    > Dogs bark to communicate.
- Cat :: Meow
- Cow :: Moo
- :: Neigh
- :: Ribbit

> Think about farm and household animals.
```

- `Left :: Right` — correct pair
- `:: Right` — distractor (no left match)
- Indented `> blockquote` under a pair — per-pair feedback shown after evaluation

## Print review mode

When a student prints the page (`Cmd/Ctrl+P`), the interactive widget is replaced with a static review sheet showing selected answers, correct answers, per-pair feedback, and question-level status for all question types.

## Attributions

- Original Quarto extension: [parmsam/quarto-quizdown](https://github.com/parmsam/quarto-quizdown) (MIT, Sam Parmar 2025)
- quizdown-js library: [bonartm/quizdown-js](https://github.com/bonartm/quizdown-js) (MIT, Malte Bonart 2020-21)
