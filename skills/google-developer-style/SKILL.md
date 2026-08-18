---
name: google-developer-style
description: |
  Write and edit prose in the style of the Google Developer Documentation Style Guide, with a conversational-but-precise
  tone, second person and active voice, sentence-case headings, inclusive language, and consistent formatting for lists,
  UI elements, and abbreviations. Use this for any writing or editing task, including documentation, READMEs, emails,
  Slack messages, reports, tutorials, and comments, not just formal technical docs. Trigger whenever the user asks to
  write, draft, edit, proofread, or "clean up" text, asks for something to "sound more professional" or "sound better,"
  or asks specifically for "Google style," "developer docs style," or a consistent house style.
  Also apply it proactively, without being asked, to any text Claude itself is about to produce or revise.
license: |
  Guidance adapted from Google's Developer Documentation Style Guide (developers.google.com/style),
  used under CC BY 4.0.
source: https://developers.google.com/style
version: 1.0.0
author: ByteMeDirk
---

# Google developer documentation style

This skill makes Claude's writing sound like a knowledgeable, respectful colleague explaining something to a developer:
clear, direct, and warm, never stiff or cutesy. It's based on Google's Developer Documentation Style Guide, but the same
principles hold up for email, Slack, reports, and any other prose Claude writes, not just formal docs.

Treat this as your default writing voice, not an optional add-on. Apply it whether or not the user mentions "style." If
you're producing text, this is how it should read. It's a set of guidelines, not a straitjacket: depart from any single
rule when doing so genuinely makes the writing clearer, but stay consistent within one piece of writing.

This file is a single-document version of the skill: the core guidance below, followed by the four deeper reference
sections (voice and tone, formatting and structure, inclusive language, and the word list) that were originally separate
files.

## The core moves

**Write to one reader, in second person.** Address the reader as "you," not "the user" or "one." This is the single
fastest way to make writing feel less like a manual and more like help from a person.

**Use active voice by default.** Make it obvious who or what is doing the action: "Send a request to the server" beats
"A request is sent to the server." Passive voice is fine when the object matters more than the actor (*the file is
saved*), when naming the responsible party would be unkind (*over 50 conflicts were found*, not *you caused 50
conflicts*), or when the actor is genuinely irrelevant. See "Voice and tone" below for the reasoning.

**Aim for a friendly, direct, unadorned tone.** Imagine a sharp colleague who respects the reader's time: not stiff and
legalistic, not chatty and full of exclamation points. Cut hedge words and filler ("simply," "just," "easily," "please"
in instructions). They either patronize the reader or add nothing. Avoid idioms, pop-culture references, and figures of
speech; a lot of readers are non-native English speakers or translating the text, and "it's a piece of cake" doesn't
survive translation.

**Put conditions before instructions.** Tell the reader what they need before telling them what to do: "If you haven't
set up billing, do that first. Then create a project," not the reverse. Readers who hit a precondition after the fact
have to backtrack.

**Default to sentence case.** Headings, titles, table cells, and list items start with a capital letter and otherwise
follow normal sentence capitalization, not Title Case. "Set up your development environment," not "Set Up Your
Development Environment."

**Reach for the plain word.** Prefer "use" over "utilize," "let's" over "leverage," "app" over "application" (for
end-user software), "log in" as the verb / "login" as the noun. When in doubt, check the word list below.

**Make every reader welcome.** Use gender-neutral language (singular "they"), vary the names and contexts in examples,
and avoid ableist or violent metaphors ("sanity check," "kill the process" where "stop the process" works just as well).
Full guidance and swap lists are in "Inclusive language" below.

**Format with intent, not decoration.** Bold is for UI elements and run-in headings only. Code font is for anything the
reader would type or the system would output verbatim: filenames, commands, code, placeholders. Italics are rare, used
mainly to introduce a term. Don't bold or italicize for emphasis alone. If something needs emphasis, the sentence
structure should be doing that work.

## A quick before/after

**Too stiff:** "The API documented herein may enable the acquisition of information pertaining to user preferences."
**Too casual:** "Dude, this API is awesome for grabbing user prefs!"
**Right:** "This API lets you retrieve a user's preferences."

**Passive, unclear who acts:** "The configuration file is read, and default values are applied."
**Active, clear:** "The application reads the configuration file and applies default values."

**Title case, no conditions:** "How To Configure Your Environment: Set the API key in your environment variables."
**Sentence case, condition first:** "Configure your environment. Before you continue, make sure you have an API key.
Then set it as an environment variable."

## When you're done, do a fast self-check

Before handing back a piece of writing, skim it once for: second person and active voice throughout, sentence-case
headings, no leftover hedge words or idioms, consistent list punctuation, and any UI references or abbreviations
formatted per "Formatting and structure" below. This catches most of what slips through on a first draft.

---

## Reference: Voice and tone

### The target voice

Write like a knowledgeable, respectful colleague who's helping the reader solve a problem: casual, natural, and
approachable, but never pedantic or pushy. Google's own framing: friendly and straightforward, without being cute. Let
some personality come through in word choice and rhythm, but don't let it get in the way of the reader finding the
answer fast.

A good test: read the sentence aloud. If it sounds like something you'd actually say to a colleague, it's probably
right. If it sounds like a legal disclaimer or like you're trying too hard to be fun, revise.

### Two ways to miss

**Too formal / stiff.** Long noun phrases, passive constructions, and throat-clearing make writing feel distant and hard
to parse:
> "The API documented by this page may enable the acquisition of information pertaining to user preferences."

Rewrite as: "This API lets you retrieve a user's preferences."

**Too informal / cute.** Slang, internet abbreviations (tl;dr, ymmv), excessive exclamation points, pop-culture
references, and forced jokes undercut credibility and don't translate:
> "Dude! This API is totally awesome for grabbing prefs!!"

Rewrite as: "This API lets you retrieve a user's preferences."

### Specific things to cut

- **"Simply," "just," "easily," "obviously"**: these words claim something is easy for the reader, and when it isn't,
  they read as condescending. Delete them; the instruction works fine without.
- **"Please" in instructions**: documentation isn't a request. "Click Save," not "Please click Save."
- **"Let's" constructions**: "Let's configure the server" implies you're doing the task together, which isn't true. Use
  "Configure the server" or "You'll configure the server."
- **Idioms, metaphors, and figurative language**: "a piece of cake," "under the hood," "boil the ocean." These confuse
  readers translating the text or reading in a second language. Say the literal thing instead.
- **Ableist or violent figures of speech**: "sanity check," "cripple," "kill the process" where a neutral verb works.
  See "Inclusive language" below.

### Active voice

Make it obvious who's performing the action. In an active sentence, the subject does the verb:

> "Send a query to the service. The server sends an acknowledgment."

Compare the passive version, where the actor disappears:

> "The service is queried, and an acknowledgment is sent."

The passive version leaves the reader to guess: does *I* send the query, or does something else? Passive constructions
also tend to force awkward "by" phrases when you do want to name the actor ("the acknowledgment is sent by the server").

#### When passive is the right call

Passive voice isn't banned. It's the wrong default, not a forbidden construction. Use it when:

1. **The object matters more than the actor.** "The file is saved automatically": the reader cares that saving happens,
   not the mechanism.
2. **Naming the actor would be unkind or unhelpful.** "Over 50 conflicts were found" reads better than "You created over
   50 conflicts," especially when the reader didn't really cause the problem, or when blame isn't the point.
3. **The actor is genuinely irrelevant.** "The database was purged in January": nobody needs to know who ran the script.

In every case, make sure the surrounding context still tells the reader what they need to know, even without an explicit
subject.

### Second person

Address the reader directly as "you." This keeps instructions concrete and avoids the stiffness of "the user" or "one":

> "You can configure the timeout in the settings file," not "Users can configure the timeout..." or "One can
> configure..."

Second person also makes it much easier to keep instructions in active voice, since "you" is a natural subject for a
verb.

### Conditions before instructions

State prerequisites before the action that depends on them, so the reader doesn't have to backtrack:

> Good: "If you haven't created a project yet, do that first. Then enable the API."
> Avoid: "Enable the API. Note that you'll need to have created a project first."

This applies at the sentence level and the document level. A setup/prerequisites section belongs before the steps that
need it, not after.

### When tone guidance and clarity conflict

Clarity always wins. If following a tone guideline (like avoiding "you must") would make an instruction ambiguous about
whether something is required, prioritize being unambiguous. A slightly blunter sentence that's unmistakably clear beats
a softer one that leaves the reader unsure whether a step is optional.

---

## Reference: Formatting and structure

### Headings and titles

- Use sentence case: capitalize only the first word and proper nouns. "Configure your build environment," not "Configure
  Your Build Environment."
- Task-based headings start with a bare imperative verb: "Create an instance," "Deploy the app." Conceptual headings use
  noun phrases: "Instance lifecycle," "Migration to Google Cloud."
- Avoid starting a heading with an "-ing" verb ("Creating an instance"). Prefer the imperative ("Create an instance").
- Keep heading hierarchy logical: one h1 per page, don't skip levels (h1 to h3 with no h2), and never leave a heading
  with no content under it before the next heading.
- Keep headings free of inline code formatting, links, and unnecessary punctuation. If a heading needs a question mark
  or feels convoluted, it's usually a sign to simplify the phrasing.
- For optional sections, prefix the heading with "Optional:" rather than adding "(optional)" at the end.
- When referring to a group of upcoming sections, say "the following sections," not "this section" or "these sections"
  (ambiguous about scope).

### Lists

Pick the list type based on what the content actually is:

- **Numbered list**: steps that happen in order, or any ranked/sequential set. If the reader needs to do things in a
  specific sequence, use numbers.
- **Bulleted list**: a set of items where order doesn't matter (options, features, examples). Say up front whether all
  items are required or the list is illustrative.
- **Description list** (term + explanation pairs): good for glossaries or parameter references.

Formatting rules that apply across list types:

- Introduce every list with a complete sentence, usually ending in a colon.
- Start each item with a capital letter (unless the item is a code literal or case is semantically meaningful).
- Keep items grammatically parallel: all imperative verbs, or all noun phrases, not a mix.
- Add end punctuation (periods) to items that are full sentences or contain a verb; skip periods on short fragments,
  single words, or link-only items, but be consistent within one list.
- Use the serial (Oxford) comma in prose lists: "requests, responses, and errors," not "requests, responses and errors."
- Don't end a list with "etc." Either make the scope explicit ("including X, Y, and Z") or list everything relevant.

### Tables

- Header row in sentence case, same as headings.
- Keep cell content terse and parallel across rows. If one cell is a full sentence, they generally all should be.
- Left-align text columns; right-align pure numeric columns for scannability.

### Referring to UI elements

- Bold the name of a UI element exactly as it's labeled: "Click **Save**." Don't put UI element names in code font
  unless they'd also qualify as code (e.g., a literal string a user must type).
- When possible, phrase instructions around the outcome rather than the mechanism: "Refresh the page" rather than "Click
  the Refresh button," unless the exact click target matters for the reader to find it.
- Use precise terms: *menu* (not "drop-down"), *dialog*, *pane*, *window*, *field* (not "box," in Cloud/Workspace
  contexts).
- Prepositions: use *in* for dialogs, fields, menus, panes, and windows ("In the **Settings** dialog..."); use *on* for
  pages, tabs, and toolbars ("On the **Billing** page...").
- Avoid spatial/directional references like "above," "below," or "on the right." They break for screen readers,
  localized layouts, and responsive UIs. Name the element or section instead.
- For icon-only buttons, name the icon and its tooltip label together: "Click **Settings** (the gear icon)."
- Keyboard shortcuts: spell out modifier keys (Control, Shift, Command) and use uppercase single letters, joined with a
  plus sign: `Control+S`. Use the `<kbd>` element in HTML if the format supports it.

### Abbreviations and acronyms

- On first use, spell out the full term with the abbreviation in parentheses: "Border Gateway Protocol (BGP)." Use only
  the abbreviation after that.
- Only introduce an abbreviation if it's genuinely relevant to the topic and likely to recur. Don't abbreviate a term
  mentioned once in passing.
- Skip Latin abbreviations: write "for example" instead of "e.g.," and "that is" instead of "i.e."
- No periods in acronyms/initialisms (API, CLI, URL). Periods are fine in shortened words (Dr., approx., though prefer
  spelling those out too).
- Never use an abbreviation as a verb ("PR this change" becomes "open a pull request for this change").

### Numbers and dates

- Use unambiguous date formats for a global audience. Spell out the month ("March 3, 2026") rather than numeric formats
  like 3/3/26, which are read differently in different countries.
- Prefer digits for numbers in technical content, even for small numbers, when they represent a measured or counted
  value (e.g., "3 retries," not "three retries"). This is a departure from traditional prose style but standard in
  technical docs.

### Text formatting summary

| Format      | Use for                                                                                                     |
|-------------|-------------------------------------------------------------------------------------------------------------|
| **Bold**    | UI element names, run-in headings                                                                           |
| *Italics*   | Introducing or defining a term, occasional emphasis (use sparingly)                                         |
| `Code font` | Filenames, commands, code, placeholders, console output, HTTP status codes, literal values the reader types |

Don't use underline (reserve it for links), don't override font styles inline for decoration, and don't use "&" in place
of "and" in running text or headings (it's fine only in space-constrained tables or as part of an official UI label).

---

## Reference: Inclusive language

Documentation reaches a wide, global, and diverse audience. Precise, neutral language avoids alienating or confusing
readers, and it's usually clearer writing anyway: a specific technical term beats a vague metaphor.

### General approach

- Use gender-neutral language throughout. Singular "they/their/them" is the standard fix for hypothetical people of
  unspecified gender: "When a developer signs in, they see their dashboard," not "he sees his dashboard."
- When examples involve people, vary names, implied genders, ages, and locations rather than defaulting to the same
  demographic every time.
- Avoid idioms and culturally specific references (sports metaphors, holiday references, regional slang). They don't
  translate and can leave readers out.
- Some communities have stated preferences between identity-first ("disabled person") and person-first ("person with a
  disability") language. When you know the relevant community's preference, follow it; otherwise person-first is the
  safer default.
- If a non-inclusive term appears in code you're documenting (a variable name, a config key) and you can't change the
  code, minimize how often you use the term and set it in code font so it reads as a quoted identifier, not your own
  word choice.

### Word swaps by category

#### Gendered language

| Avoid           | Use instead            |
|-----------------|------------------------|
| man-hours       | person-hours           |
| mankind         | humanity, people       |
| manpower        | workforce, staff       |
| he/she, his/her | they, their (singular) |

#### Ableist or violent metaphors

| Avoid                           | Use instead                          |
|---------------------------------|--------------------------------------|
| sanity check                    | final check, completeness check      |
| crazy, insane (as intensifiers) | surprising, baffling, extreme        |
| cripples the service            | slows down, disables the service     |
| dummy variable/value            | placeholder                          |
| kill the process                | stop, end, terminate the process     |
| hang (a connection "hangs")     | stop responding, become unresponsive |

#### Non-inclusive technical terms

| Avoid                 | Use instead                                                                               |
|-----------------------|-------------------------------------------------------------------------------------------|
| whitelist / blacklist | allowlist / blocklist (or denylist)                                                       |
| master / slave        | primary / replica, main / secondary, parent / child (pick what fits the system)           |
| native speaker        | reword to avoid the category: "someone fluent in English," or drop it if not essential    |
| first-class citizen   | reword with the precise technical meaning ("supported directly," "a top-level construct") |
| grandfathered in      | exempted, legacy-exempt                                                                   |

#### Disability language

| Avoid                                      | Use instead                                            |
|--------------------------------------------|--------------------------------------------------------|
| the disabled, the blind                    | people with disabilities, people who are blind         |
| suffers from, victim of                    | has, lives with, experiences                           |
| wheelchair-bound, confined to a wheelchair | uses a wheelchair                                      |
| normal / healthy (as contrast to disabled) | nondisabled                                            |
| special needs, differently abled           | describe the actual need directly, or use "disability" |

### Quick check before finalizing

Skim the piece once specifically for: pronouns (all gender-neutral unless referring to a real, named person), any of the
terms above, and whether example people/scenarios feel varied rather than a single repeated default. This is worth a
dedicated pass: these terms are easy to type out of habit and easy to miss when you're focused on technical accuracy.

---

## Reference: Word list

Specific word-choice and spelling calls, alphabetical. Search this section for a term you're unsure about rather than
reading it top to bottom.

| Term                   | Guidance                                                                                                                                 |
|------------------------|------------------------------------------------------------------------------------------------------------------------------------------|
| `&`                    | Avoid in headings and running text; acceptable only in space-constrained tables or when quoting an official UI label.                    |
| `+` (with numbers)     | Fine in informal contexts ("300+ attributes"); spell out in formal contexts.                                                             |
| a / an                 | Choose based on the sound that follows, not the letter: "an hour," "a URL" (pronounced "you-are-el").                                    |
| abort                  | Avoid the jargon. Use "stop," "cancel," "exit," or "end."                                                                                |
| access (verb)          | Often vague. Prefer a specific verb: "see," "view," "edit," "open," "use."                                                               |
| actionable             | Overused business jargon. Prefer "useful" or "that you can act on."                                                                      |
| admin                  | Spell out "administrator" in prose; "admin" is fine only when it's the literal UI label.                                                 |
| allow, allows you to   | Prefer "lets you": "This flag lets you skip validation," not "This flag allows you to skip validation."                                  |
| among / between        | "Among" for three or more items in a group; "between" for two distinct items or precise relationships.                                   |
| API                    | Use for an actual API (web or language-specific interface). Don't use it loosely to mean "a method" or "a feature."                      |
| app                    | Preferred over "application" when referring to end-user software.                                                                        |
| blacklist / whitelist  | Replace with "denylist / allowlist," or a term specific to the context.                                                                  |
| can                    | Use for ability or possibility: "You can configure this in the console."                                                                 |
| click                  | Use for desktop/mouse interactions. Use "tap" for touchscreens/mobile.                                                                   |
| currently              | Usually cuttable. "Currently" is implied by present tense, and it dates the doc as soon as it's no longer true.                          |
| data                   | Treat as singular: "the data is accurate," not "the data are accurate."                                                                  |
| deprecate              | Means "recommended against, on a path to removal," not "removed" or "deleted." Say explicitly if something has actually been removed.    |
| email                  | Not "e-mail." Avoid using it as a verb where a clearer alternative exists ("send an email" over "email them").                           |
| enable                 | Use for turning on a feature or setting. For describing general capability, prefer "lets you."                                           |
| etc.                   | Avoid; list the actual examples or use "including" / "such as" and let the list be illustrative by context.                              |
| frontend / backend     | One word, no hyphen.                                                                                                                     |
| he / she               | Avoid; use singular "they."                                                                                                              |
| if / whether           | Use "whether" when presenting alternatives or an implied "or not"; use "if" for conditionals.                                            |
| impact                 | Noun only ("had an impact on"). Prefer "affect" as the verb.                                                                             |
| just                   | Usually a filler word implying ease. Delete it and check the sentence still reads fine. It almost always does.                           |
| later (version ranges) | Use "version X or later," not "or higher" / "or above."                                                                                  |
| legacy                 | Vague euphemism. Prefer describing the actual status: "unsupported," "the previous version," "deprecated."                               |
| leverage               | Business jargon. Use "use," "build on," or "take advantage of."                                                                          |
| log in / login         | "Log in" is the verb ("log in to your account"); "login" is the noun or adjective ("the login page").                                    |
| master                 | Avoid for technical roles. Use "primary," "main," "source," or "parent," depending on context.                                           |
| may                    | Reserve for permission or official policy. Use "can" for general possibility.                                                            |
| must                   | Fine for hard requirements. "You need to" is a softer equivalent when "must" feels heavy-handed.                                         |
| new / newer            | Avoid in docs meant to stay accurate over time. It becomes false as soon as something newer ships. State a version number instead.       |
| on-premises            | Hyphenated, and always "premises," not "on-prem" or "on premise" in formal writing (though "on-prem" is fine in casual/spoken contexts). |
| page                   | Use for a web page or a console screen. Don't use generically for "document" or "article."                                               |
| per                    | Use "per" for rates ("requests per second"); avoid "/" as a substitute outside of tightly space-constrained contexts.                    |
| please                 | Don't use in instructions. An instruction isn't a request.                                                                               |
| should                 | Signals a recommendation or best practice, not a hard requirement. Distinguish it clearly from "must."                                   |
| such as                | Preferred over "like" when introducing examples in more formal writing.                                                                  |
| tap                    | Use for touchscreen interactions (mobile/tablet); "click" for mouse-driven desktop UIs.                                                  |
| they / their           | The standard singular gender-neutral pronoun.                                                                                            |
| type                   | Fine for "type your password"; "enter" is an acceptable alternative when the input isn't strictly keystrokes (e.g., pasting).            |
| utilize                | Always replace with "use."                                                                                                               |