# Contributing to Shadesmar Data

Thank you for helping build the most spoiler-safe Stormlight Archive lore reference on the web.

---

## Table of contents

1. [Before you start](#1-before-you-start)
2. [Content policy](#2-content-policy)
3. [Spoiler threshold guidelines](#3-spoiler-threshold-guidelines) ← Read this first
4. [Entity format](#4-entity-format)
5. [Pull request process](#5-pull-request-process)
6. [Reporting errors](#6-reporting-errors)

---

## 1. Before you start

- **Read the entire series** up to at least the book you're contributing on. You cannot correctly tag `revealed_at` for content you haven't read past.
- **Check the [shadesmar-schema](https://github.com/shadesmar-companion/shadesmar-schema)** repo for the current YAML schema before writing an entity file. The schema is the authoritative source of truth for file format.
- **One entity per file.** Do not combine multiple entities in a single YAML.
- **One PR per entity** (or per closely related group of entities, e.g. all 10 Knights Radiant orders).

---

## 2. Content policy

### What you must do

- Write all descriptions **independently**, from your own reading of the books.
- Verify facts against [Coppermind](https://coppermind.net) if needed — but **rewrite in your own words**. The Coppermind text must never appear verbatim in this dataset.
- Write in **English**, present tense, neutral tone. ("Dalinar is a Highprince" — not "Dalinar was" or "I think Dalinar is")
- Use the **spelling from the books** for names. When in doubt, use the index at the back of the hardcover edition.

### What you must never do

- Copy any text from Coppermind, the 17th Shard wiki, Reddit, or any fan wiki. All descriptions must be independently written.
- Copy any text from the books themselves (quotes, paraphrases of prose).
- Include any information that cannot be verified from the books alone.
- Reveal spoiler information in a section tagged with an earlier `revealed_at`.
- Include images or artwork from Dragonsteel Entertainment or commissioned Stormlight Archive art.

---

## 3. Spoiler threshold guidelines

This is the most critical part of contributing. **Getting `revealed_at` wrong is a spoiler bug.** Our SLA for spoiler bugs is 24 hours.

### The core principle: conservative tagging

**When in doubt, use the later position.**

The app shows content to users who have read _up to and including_ the declared position. If you're uncertain whether a detail is revealed in Book 1 Part 2 or Part 3, tag it as Part 3. An over-protected reader (who sees less than they've read) is frustrated but safe. An under-protected reader gets spoiled.

### What "revealed" means

`revealed_at` is not "first mentioned." It is the position at which a **typical reader, reading linearly, would understand this information**.

| Scenario                                                                              | Correct tagging                                  |
| ------------------------------------------------------------------------------------- | ------------------------------------------------ |
| Character appears in Book 1 Prologue                                                  | `{book: 1, part: 0}`                             |
| Name mentioned in passing in Book 1 Part 1, but character doesn't appear until Book 2 | `{book: 2, part: 0}`                             |
| Fact is hinted across Book 2 but confirmed explicitly in Book 3 Part 2                | `{book: 3, part: 2}`                             |
| Backstory revealed via flashback in Book 3                                            | `{book: 3, part: X}` — the part of the flashback |
| Always-known worldbuilding (cosmere cosmology, basic geography)                       | `{book: 0, part: 0}`                             |

### Book and part reference

| Value       | Meaning                                                 |
| ----------- | ------------------------------------------------------- |
| `book: 0`   | Pre-series lore — always visible, no spoiler            |
| `book: 1`   | _The Way of Kings_                                      |
| `book: 2`   | _Words of Radiance_                                     |
| `book: 2.5` | _Edgedancer_ (novella)                                  |
| `book: 3`   | _Oathbringer_                                           |
| `book: 3.5` | _Dawnshard_ (novella)                                   |
| `book: 4`   | _Rhythm of War_                                         |
| `book: 5`   | _Wind and Truth_                                        |
| `part: 0`   | Beginning of book — before Part 1 (Prologue, epigraphs) |
| `part: 1–5` | The numbered parts of the book                          |

### Novella placement in the reading order

- **Edgedancer (2.5)**: Takes place between WoR and OB. A user at `{book: 3, part: 0}` has reached the point where Edgedancer content is safe. A user at `{book: 2, part: 5}` has not yet read Edgedancer — tag accordingly.
- **Dawnshard (3.5)**: Takes place near the end of OB / start of RoW. Users at `{book: 4, part: 0}` have passed it. Users at `{book: 3, part: 5}` have not.

### Relationships and cross-entity spoilers

Every `relationship` entry has its own `revealed_at`. This is independent of the entity's `first_revealed`.

Example: Dalinar's relationship with Navani is established early, but the nature of their relationship deepens significantly later. The early relationship entry might be `{book: 1, part: 1}` while a deeper entry is `{book: 3, part: 4}`.

The app hides relationship entries whose `revealed_at` exceeds the user's position. The **name** of the related entity in a locked relationship is **never shown** — do not include the name in the relationship description (write "This character's mentor" not "Hoid, this character's mentor").

### The `hasContentAhead` signal

The app automatically shows a "🔒 More about [Name] is available as you read" banner when any section or relationship's `revealed_at` is beyond the user's position. You do not need to add this — it is computed automatically from the data you provide.

---

## 4. Entity format

> **Note**: The complete YAML schema is defined in
> [shadesmar-schema](https://github.com/shadesmar-companion/shadesmar-schema).
> Always validate your entity file against the schema before submitting a PR.
> The CI pipeline will reject invalid files.

A minimal well-formed entity looks like:

```yaml
id: dalinar-kholin
type: character
name: Dalinar Kholin
aliases:
  - The Blackthorn
  - Dalinar
first_revealed:
  book: 1
  part: 0 # Appears in the Prologue of The Way of Kings
short_description: >
  Highprince of Alethkar and uncle to King Elhokar. A legendary warrior
  haunted by strange visions during highstorms.
sections:
  - id: dalinar-early-career
    revealed_at:
      book: 1
      part: 1
    content: >
      [Content visible from Book 1, Part 1 onward — written independently.]
relationships:
  - entity_id: elhokar-kholin
    revealed_at:
      book: 1
      part: 1
    type: family
    description: Nephew. Elhokar is the King of Alethkar.
```

### Naming conventions

- `id`: lowercase, hyphenated, unique. Use the full canonical name. (`dalinar-kholin`, not `dalinar` or `DalinarKholin`)
- File name: `{id}.yaml` (e.g. `dalinar-kholin.yaml`)
- File location: `stormlight/{type}s/{id}.yaml` (e.g. `stormlight/characters/dalinar-kholin.yaml`)

### Short description

- Maximum 2–3 sentences.
- Must be safe at `first_revealed` — no information beyond that position.
- No spoilers, even subtle ones.
- Think: "What would I tell someone who just met this character for the first time, having read only up to their first appearance?"

---

## 5. Pull request process

1. **Fork** the repository.
2. Create a branch: `feat/entity-{id}` or `fix/{id}-{description}`.
3. Add your YAML file in the correct directory.
4. Validate against the schema (instructions in shadesmar-schema README).
5. Open a PR with the following in the description:
   - Entity name and type
   - Books covered (e.g. "WoK + WoR")
   - A brief note confirming: "I have read up to [book] and all `revealed_at`
     values are conservative."
6. A maintainer will review within 7 days (spoiler bugs reviewed within 24h).

### What reviewers check

- Does the content accurately reflect the books?
- Are all `revealed_at` values conservative (erring toward later)?
- Is `short_description` spoiler-safe at `first_revealed`?
- Does the entity validate against the schema?
- Is the description independently written (no Coppermind text)?

---

## 6. Reporting errors

### Spoiler bugs (priority: urgent)

If you find that the app shows spoiler content that should be hidden:

1. Open a GitHub Issue with the label `spoiler`.
2. Title: `SPOILER: [entity name] — visible at [book/part] when it shouldn't be`
3. **Do not include the spoiler content in the issue body** — describe it obliquely or send the details directly to a maintainer via GitHub DM.
4. SLA: resolved within 24 hours.

### Content errors (priority: normal)

For factual errors, outdated information, or missing entities:

1. Open a GitHub Issue with the label `content-error` or `missing-entity`.
2. Describe the error and the correct information with a book/chapter reference.
3. SLA: reviewed within 7 days.

---

_This project is fan-created and non-commercial._
_"The Stormlight Archive" is a trademark of Dragonsteel Entertainment, LLC._
