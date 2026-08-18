---
name: obsidian-markdown-formatting
description: |
  Use this skill whenever you are writing, formatting, cleaning up, or converting notes for an Obsidian vault, 
  or any Markdown file the user says will be opened in Obsidian. Covers Obsidian-flavored Markdown: basic formatting, 
  callouts, wikilinks and embeds, block references, properties (YAML frontmatter), tags, tables, and math, and where 
  it differs from plain CommonMark. Trigger on mentions of "Obsidian", ".md notes", "vault", "wikilinks", "[[...]]", 
  "callouts", "frontmatter properties", "backlinks", "second brain", or "PKM" (personal knowledge management), 
  even if the user just pastes rough notes and asks you to "clean this up" or "format this for my vault" without 
  naming Obsidian explicitly. Also use it before writing any new note that the user intends to drop into an existing 
  Obsidian vault, so links, tags, and properties match the conventions their other notes already use.
license: Apache-2.0
source: https://obsidian.md/help/
version: 1.0.0
author: ByteMeDirk
---

# Obsidian Markdown formatting

Obsidian notes are plain Markdown files, but Obsidian layers its own syntax on top for things plain Markdown can't do:
linking notes to each other, folding admonition boxes, embedding content live, and attaching structured metadata.
Getting this right matters more here than in most Markdown contexts, because a vault is a web of notes that reference
each other. A broken `[[link]]` or a tag typo doesn't just look wrong; it silently breaks the connections the whole
system depends on.

The general approach: write plain Markdown for prose, reach for Obsidian's extensions only where they add real value (a
link because the note actually relates to another, a callout because a box actually needs visual separation), and match
whatever conventions already exist in the user's vault rather than inventing new ones per note.

## Before you start: look for existing conventions

If you can see other notes in the user's vault (via an uploaded file, a folder listing, or the user describing their
setup), skim one or two for how they already do tags, properties, and links: kebab-case vs camelCase tags, a fixed set
of frontmatter fields, wikilinks vs Markdown-style links. A new note that doesn't match its neighbors creates exactly
the kind of inconsistency Obsidian's graph and search features are bad at working around. When there's nothing to go on,
the defaults below are reasonable, but note the choice you made (e.g. "I used wikilinks since that's Obsidian's
default") so the user can correct you if their vault does it differently.

## Basic formatting

Standard Markdown works as expected: headings (`#` through `######`), **bold** (`**text**`), *italics* (`*text*`),
~~strikethrough~~ (`~~text~~`), blockquotes (`> text`), ordered/unordered lists, and fenced code blocks. Obsidian adds a
few of its own:

| Feature                    | Syntax                                 |
|----------------------------|----------------------------------------|
| Highlight                  | `==highlighted text==`                 |
| Task list                  | `- [ ] todo` / `- [x] done`            |
| Footnote                   | `Text[^1]` ... `[^1]: note`            |
| Inline footnote            | `Text ^[the footnote itself]`          |
| Comment (hidden on render) | `%%this won't show%%`                  |
| Horizontal rule            | `---`, `***`, or `___` (3+ characters) |

Two trailing spaces before a line break, or a blank line between paragraphs, both work the way they do in standard
Markdown. To show a Markdown character literally rather than have it trigger formatting, escape it with a backslash:
`\*not italic\*`.

**Tables** use the standard pipe syntax, with the header row needing at least two hyphens per column and colons for
alignment (`:--`, `:--:`, `--:`). If a cell needs a literal `|`, for instance inside a link alias or an image resize,
escape it: `[[Note\|alias]]`.

## Linking notes: wikilinks and embeds

This is the feature that makes a vault a *vault* rather than a folder of files, so get it right:

- **Link to a note**: `[[Note Name]]`. Links by the note's filename (without `.md`), not by path, so renaming a note's
  folder doesn't break links to it.
- **Custom display text**: `[[Note Name|shown text]]`
- **Link to a heading**: `[[Note Name#Heading]]`
- **Link to a specific block**: `[[Note Name#^block-id]]`. Useful for linking to one paragraph or list item rather than
  the whole note
- **Embed content inline** (renders live, stays in sync with the source): prefix any of the above with `!`, for example
  `![[Note Name]]`, `![[Note Name#Heading]]`
- **Embed an image**: `![[image.png]]`, with sizing via `![[image.png|400]]` (width) or `![[image.png|400x300]]`
  (width by height)
- **Embed a PDF, audio, or canvas file**: `![[file.pdf]]`, `![[file.pdf#page=3]]`, `![[audio.ogg]]`, `![[board.canvas]]`

Obsidian also accepts standard Markdown links (`[text](Note%20Name.md)`) and can be configured either way. Check the
vault's existing notes if you're unsure which style to use, since mixing both in one vault works but reads as
inconsistent. Default to wikilinks (`[[...]]`) when there's no signal either way, since that's Obsidian's own default
and the more common convention in the ecosystem.

Only add a link where one is actually useful to the reader or to the vault's graph. Turning every capitalized noun into
a `[[link]]` just because a matching note might exist someday creates noise, not structure.

## Callouts

Callouts turn a blockquote into a labeled, colored admonition box, useful for flagging a warning, an aside, or a
summary without breaking the reader's flow:

```markdown
> [!tip] Optional title
> The callout body. Supports **Markdown**, [[wikilinks]], and embeds.
```

Built-in types: `note` (default), `abstract` (aliases `summary`, `tldr`), `info`, `todo`, `tip` (aliases `hint`,
`important`), `success` (aliases `check`, `done`), `question` (aliases `help`, `faq`), `warning` (aliases `caution`,
`attention`), `failure` (aliases `fail`, `missing`), `danger` (alias `error`), `bug`, `example`, `quote` (alias `cite`).

Add `+` or `-` after the type to make it foldable and set its default state: `[!warning]+` starts expanded,
`[!warning]-` starts collapsed. Nest callouts by adding another level of `>`:

```markdown
> [!question] Outer
> > [!example] Nested inside it
```

Pick the callout type for what it *signals* to a reader skimming the note, not just for the color. A `[!warning]` on a
routine reminder undersells actual warnings elsewhere in the vault.

## Properties (frontmatter)

Properties are YAML metadata at the very top of a note, between a pair of `---` lines:

```yaml
---
tags:
  - project
  - active
aliases:
  - Alternate Name
created: 2026-08-18
status: in-progress
reviewed: false
---
```

A few rules that are easy to get wrong:

- **Lists** (like `tags` or `aliases`) go one value per line under the key, each prefixed with a hyphen, not a
  comma-separated inline string.
- **Dates** use `YYYY-MM-DD`; **date & time** uses `YYYY-MM-DDTHH:MM:SS`.
- **Checkboxes** are plain YAML booleans: `true` / `false`.
- Markdown formatting does **not** render inside property values, and a `[[wikilink]]` written as a bare value will be
  read as plain text. Wrap it in quotes to store it as a link-shaped string: `related: "[[Other Note]]"`.
- A bare `#hashtag` typed into a text property does **not** become a real, searchable tag; only the reserved `tags`
  property (or an inline `#tag` in the note body) does that.
- `tags`, `aliases`, and `cssclasses` are reserved property names with special meaning to Obsidian; don't repurpose them
  for something else in a note that needs the real behavior elsewhere.

Only add properties that the note (or the vault's templates) actually uses for something: filtering, a Dataview query,
a template. A frontmatter block cargo-culted from another note but never referenced anywhere is just noise the reader
has to scroll past.

## Tags

Inline tags go directly in the note body: `#project`. Build a hierarchy with `/`: `#project/active`. Searching
`tag:project` matches the parent and every nested variant under it. Tags are case-insensitive (`#Tag` and `#tag` are the
same tag; whichever casing was used first is what displays), can't contain spaces (use `#multi-word` or `#multiWord`
instead), and must contain at least one non-numeric character (`#1984` doesn't count as a tag; `#y1984` does). Tags can
also live in the `tags` property in frontmatter as a list. Use whichever the vault already favors for whole-note
categorization, and inline tags for pointing at a specific passage within the note.

## Math and diagrams

Inline math: `$e^{2i\pi} = 1$`. Block math: a line with just `$$`, the LaTeX, then a closing `$$`. Diagrams use a fenced
code block with the `mermaid` language tag; link a node to a note with `class NodeName internal-link`.

## Putting a note together

A reasonable order for a new note: properties block first, then an `H1` title (often redundant with the filename, but
some vaults prefer it), then body content organized with headings, and inline tags or links placed naturally where
they're relevant rather than collected in a "tags" line at the bottom. Obsidian's graph and search don't care where in
the note a link or tag appears, but a reader skimming does.

When converting rough or plain-Markdown notes into Obsidian format, resist adding structure that wasn't asked for.
Turning a short note into a heavily-tagged, multi-callout production doesn't make it more "Obsidian"; it just makes it
harder to read. Add the vault-specific syntax where it earns its place: a link where two notes genuinely relate, a
callout where something genuinely needs to stand out, properties where the vault's system actually consumes them.
