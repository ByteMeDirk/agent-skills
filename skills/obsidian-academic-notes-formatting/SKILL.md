---
name: obsidian-academic-notes-formatting
description: |
  Formats Obsidian academic vault notes for MSc Computing & AI coursework into a consistent structure: YAML
  frontmatter, heading hierarchy, callouts, wikilinks, tags, and file naming. Use this skill when the user asks to
  format, clean up, or reformat a lecture, reading, discussion, activity, or summary note; when they say things like
  "format this note", "apply the academic notes format", or "tidy up my Obsidian note"; or when they share Obsidian-
  flavoured Markdown from an academic vault that needs consistent structure and Dataview-compatible metadata.
license: MIT
source: none
version: 1.0.0
author: ByteMeDirk
---

## Ground rules

The researcher's notes are the source of truth. The following must be properly adhered to since the researcher uses
official academic information obtained from lectures created by instructors, content provided by academic sources,
and information gathered in their own capacity when writing their notes.

- You are merely a document formatter and are not a trustworthy source of academic information, thus you should never
  change or remove anything from the notes.
- If any material is discovered to be obviously inaccurate based on verifiable evidence, leave a note underneath the
  inaccurate content telling the researcher to look into it more so they may make the necessary corrections.
- You are allowed to correct any spelling or grammar mistakes that are not specific to the researcher's own words,
  discussion or notes that would be shared with other students, academic advisors, or lecturers. By doing this, the
  researcher is guaranteed to uphold their academic integrity.
- Avoid using AI-generated artifacts like emojis, arrows, and en dashes.

# Title

Every markdown document and filename is titled with the week number: `Week N`.

# Note types

Every note section belongs to one of the following types.

| Type         | Prefix in Title   | Description                                 |
|--------------|-------------------|---------------------------------------------|
| `lecture`    | `Micro-lecture N` | Tutor-delivered conceptual content          |
| `reading`    | `Reading N`       | Assigned reading reflections and summaries  |
| `discussion` | `Discussion N`    | Forum discussion posts and peer replies     |
| `activity`   | `Activity N`      | Short tasks, research exercises, or prompts |
| `summary`    | `Week N Summary`  | Optional week-level synthesis note          |

# Frontmatter YAML

Every note **must** begin with a YAML frontmatter block. This enables Dataview queries and consistent metadata across
the vault. If an entry is not applicable (for example of the note is too general) the value can be `NA`.

```yaml
---
module: "Module Name"
week: N
title: "Human-readable title"
date: YYYY-MM-DD
tags:
  - msc
  - week-N
  - <type>
  - <topic-tag>        # e.g. research-methods, blooms-taxonomy, scientific-method
status: draft | complete
---
```

**Rules:**

- If any information is missing, ask for the information, do not generate the content yourself.
- `module` uses the full module name (e.g. `"Research Methods & Professional Practice"`).
- `week` is an integer.
- `tags` always include `msc`, `week-N` (hyphenated), and the type. Add 1-5 topic-specific tags in `kebab-case`.
- `status` starts as `draft`; change to `complete` once the note is finalised.

## File naming convention

```text
Week-N_<ModuleName>.md
```

Examples:

- `Week-1_Critical-Research-For-Postgraduates.md`
  **Rules:**

- Title-case slug, hyphen-separated, max 5 words.
- Never use spaces or underscores within segments, hyphens only within slugs.
- Use `Week-N` not `WeekN`.

# Heading hierarchy

```text
# Week N - <Type> N: <Descriptive Title>       < H1: Note title (one per note)
 
## <Section / Topic Heading>                   < H2: Major topic or concept block
 
### <Sub-topic or Concept>                     < H3: Breakdown within a topic
 
#### <Detail>                                  < H4: Use sparingly, narrow definitions only
```

**Rules:**

- H1 appears **once** at the top.
- H2 is used for named concepts, themes, or questions covered in the lecture or reading.
- H3 for sub-types, classifications, or nested concepts.
- H4 only for detailed definitional breakdowns, avoid nesting deeper.
- Title format: `Week N - <Type> N: <Title>`, using a hyphen.

## Obsidian feature usage

## Callouts

Use callouts consistently for structured emphasis. Never use bare `>` blockquotes for first-class content - reserve
those for direct quotes from sources.

| Callout Type  | Usage                                             |
|---------------|---------------------------------------------------|
| `[!info]`     | Dates, posted timestamps, factual context         |
| `[!abstract]` | Source summaries at the top of reading notes      |
| `[!note]`     | Key insights or tutor emphasis                    |
| `[!question]` | Discussion/reflection prompts                     |
| `[!tip]`      | Personal takeaways, reflections, open questions   |
| `[!warning]`  | Contradictions, contested claims, caveats         |
| `[!quote]`    | Verbatim quotations from sources with attribution |

**Syntax:**

```text
> [!note] Optional Title
> Content here.
```

## Wikilinks

- Link to related notes: `[[Week-1_Postgrad-Expectations]]`
- Link with display text: `[[Week-1_Postgrad-Expectations|Week 1: Postgrad Expectations]]`
- Use wikilinks in the **Related Notes** section at the bottom of each note.

## Image embeds

- Use Obsidian embed syntax for all images: `![[filename.png]]`
- Place image embeds on their own line, between the paragraph they illustrate and the paragraph that follows.
- Add a plain-text caption as a _italic line_ directly beneath: `*Figure: Bloom's Taxonomy pyramid.*`

## Tags

- All tags live in frontmatter - do not use inline `#tags` in note body.
- Topic tags should be descriptive and reusable: `research-methods`, `scientific-method`, `literature-review`,
  `blooms-taxonomy`, `qualitative`, `quantitative`, `epistemology`.

## Related notes block

Every note ends with a **Related Notes** horizontal-rule section if the information is present:

```text
---
 
## Related Notes
 
- [[<wikilink to related note>]]
- [[<wikilink to related note>]]
```

## Prose formatting rules

- **Bold** (`**text**`): Key terms, defined concepts, important labels.
- _Italic_ (`_text_`): Titles of sources, foreign/specialist terms, light emphasis.
- `Inline code` ( `` `text` `` ): Named cognitive levels, technical terms, taxonomy labels.
- Ordered lists (`1.`): Steps, ranked criteria, sequential processes.
- Unordered lists (`-`): Non-ordered enumerations, features, examples.
- Process flows: Use a fenced code block ( ` ``` ` ) with arrow/step text for visual flow diagrams, not inline Unicode
  arrows in prose.
- Avoid bare `>` blockquotes except for direct verbatim source quotations.
- Do not use horizontal rules (`---`) mid-section: reserve them for separating peer replies in discussions and the
  Related Notes footer.

## Reformatting an existing note: checklist

When reformatting an existing note, apply this checklist in order:

1. Add YAML frontmatter with all required fields.
2. Apply correct file naming convention.
3. Fix H1 to use `Week N - Type N: Title`.
4. Restructure headings to correct hierarchy (H2 for topics, H3 for sub-types).
5. Convert bare `>` blockquotes to appropriate callouts (`[!question]`, `[!quote]`, `[!tip]`, etc.).
6. Apply bold/italic/code formatting per rules.
7. Replace Unicode process arrows with fenced code block flow diagrams.
8. Move all tags to frontmatter; remove any inline `#tags`.
9. Add Related Notes section at the bottom.
10. Set `status: draft` and update to `complete` when done.
