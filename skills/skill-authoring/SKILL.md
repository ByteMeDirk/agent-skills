---
name: skill-authoring
description: |
  Use this skill whenever you are about to write, draft, review, or fix a SKILL.md file or an agent skill folder,
  including requests like "turn this workflow into a skill", "write a skill for X", "review my SKILL.md", "why isn't 
  my skill triggering", or any mention of the Agent Skills format, skill frontmatter, skill description, or progressive 
  disclosure. Produces a well-defined, spec-compliant skill that other agents can discover reliably and follow without 
  ambiguity. Use this before writing any SKILL.md by hand, even a short one, since small frontmatter 
  mistakes (bad name characters, a vague description) are the most common reason a skill silently never triggers.
license: Apache-2.0
source: https://agentskills.io
version: 1.0.0
author: ByteMeDirk
---

# Skill Authoring

This skill helps you produce an Agent Skill: a `SKILL.md` file (optionally bundled with scripts, references, and
assets) that other agents can discover and follow reliably. It follows the
open [Agent Skills specification](https://agentskills.io/specification), so a skill built with this guide works across
any compatible client, not just one product.

A skill is only as good as its worst failure mode: an agent that never triggers it, or triggers it and then gets
confused halfway through. Both failure modes are avoidable, and this skill exists to catch them before they ship.

## The core idea: progressive disclosure

Agents load skills in three stages, and a well-built skill is designed around that:

1. **Discovery**: only `name` + `description` are loaded at startup, for every skill, all the time. This is the only
   thing standing between the agent and knowing your skill exists. If it's vague, the skill is invisible no matter how
   good the rest is.
2. **Activation**: once a task matches, the full `SKILL.md` body loads into context. This should be everything needed
   for the common case, and nothing more; every extra paragraph here is a tax paid on every future invocation.
3. **Execution**: bundled `scripts/`, `references/`, and `assets/` load only when the body points to them. This is
   where detail, edge cases, and anything long-tail belongs.

Keep this hierarchy in mind at every step below: cheap and always-loaded → moderate and loaded-on-match → free and
loaded-on-demand.

## Step 1: Capture what the skill actually needs to do

Before writing anything, get clear (with the user, or from context already in the conversation) on:

- **What should the agent be able to do afterward that it couldn't reliably do before?** If an agent could already do it
  well with common sense, a skill isn't the right tool: you're adding a step that must always be re-read.
- **When should it trigger?** Collect the actual phrases a user would type, not abstractions. "Fix the bug in my Python
  file" is concrete; "handle debugging requests" is not.
- **What does a correct output look like?** A file format, a report structure, a specific sequence of steps: whatever
  "done right" means here.
- **Is there existing material to extract from?** If the user is turning a conversation, a runbook, or a set of docs
  into a skill, pull the real steps, corrections, and formats out of that material rather than inventing generic ones.

Resist the urge to skip this and start typing frontmatter. A skill drafted from a vague notion of the task reads fine to
its author and fails silently on real inputs.

## Step 2: Name and describe it precisely

These two fields are the entire discovery layer, so get them right before anything else.

### `name`

- 1 to 64 characters, lowercase unicode letters, digits, and hyphens only
- Cannot start or end with a hyphen, and no consecutive hyphens (`pdf--tools` is invalid)
- Must exactly match the containing directory name
- Prefer a name describing the *domain*, not the mechanism: `pdf-processing`, not `pdf-helper-v2`

### `description`

- 1 to 1024 characters, must cover both **what it does** and **when to use it**
- Write toward the failure mode that actually happens: agents tend to *under-trigger* skills, not over-trigger them. So
  don't describe the skill neutrally: name the concrete situations, phrasings, and file types that should call it,
  even ones the user didn't spell out. A little "pushy" specificity here is a feature, not noise.
- Pack in the keywords a real request would contain: file extensions, product names, verbs the user would actually say.

Compare:

```yaml
# Poor: describes the skill from the inside, no trigger cues
description: Helps with PDFs.

# Good: what it does AND when, with concrete keywords
description: Extracts text and tables from PDF files, fills PDF forms, and
  merges multiple PDFs. Use when working with PDF documents, or when the
  user mentions PDFs, forms, scanned documents, or OCR, even if they don't
  say the word "PDF" directly (e.g. "combine these two files from my
  downloads" when the files are .pdf).
```

If you're unsure whether the description will trigger reliably, write a handful of realistic test prompts (some that
clearly should trigger it and some close, tricky near-misses that shouldn't) and check each one against the description
as if you were the discovery-stage agent seeing only those two fields. If a should-trigger prompt doesn't obviously
match, the description needs sharper keywords, not the prompt needs softening.

## Step 3: Decide the shape (one file, or a bundle)

```
skill-name/
├── SKILL.md          # required: frontmatter + instructions
├── scripts/           # optional: code the agent runs, not reads
├── references/        # optional: docs loaded only when needed
└── assets/             # optional: templates, images, data files used in output
```

A single `SKILL.md` is enough for most skills. Reach for the extra directories only when they earn their keep:

- **`scripts/`**: put deterministic, repetitive, or fiddly logic here instead of describing it in prose. A script that
  always gets rewritten the same way by every agent that follows the skill (a chart-builder, a validator, a file
  converter) should be written once and called, not reinvented every time. Scripts should be self-contained, document
  their own dependencies, and fail with a clear message rather than a stack trace.
- **`references/`**: move detail here when the SKILL.md body would otherwise bloat past what the common case needs: a
  full API reference, a long list of domain-specific variants, exhaustive edge-case tables. Point to these files by
  relative path and say when to open them. If a reference file itself exceeds ~300 lines, add a table of contents at its
  top.
- **`assets/`**: templates, fonts, icons, or data files that end up *in* the output, as opposed to being read for
  guidance.

If a skill covers several distinct variants of the same task (say, deployment instructions for three different clouds),
organize by variant rather than cramming all three into one branching SKILL.md:

```
cloud-deploy/
├── SKILL.md              # shared workflow + "pick your variant" logic
└── references/
    ├── aws.md
    ├── gcp.md
    └── azure.md
```

Keep reference chains one level deep: a file linked from SKILL.md shouldn't itself require three more hops to be
useful.

## Step 4: Write the body

The body is Markdown with no required structure, but a few patterns consistently work well:

- **Write in the imperative.** "Extract the table, then validate row counts" reads as an instruction; "the table should
  be extracted" reads as documentation about someone else's process.
- **Explain the *why*, not just the *what*.** Today's models have real judgment: telling them why a step matters (
  "validate row counts here, because silent truncation on large PDFs is the most common failure") lets them adapt
  sensibly to inputs you didn't anticipate. A skill that's just a wall of imperative MUSTs and NEVERs is a yellow flag:
  it usually means the author didn't trust the model to reason, and it also means the model can't reason about it when
  the situation doesn't exactly match.
- **Give a worked example over an abstract rule.** If there's a fixed output format, show it filled in with a plausible
  example rather than only describing its shape in the abstract:

  ```markdown
  ## Report structure
  Use this exact template:
  # [Title]
  ## Executive summary
  ## Key findings
  ## Recommendations
  ```

- **State success criteria and common edge cases explicitly.** What does "done" look like, and what are the two or three
  ways this task usually goes sideways? Naming those up front saves the agent from discovering them the hard way
  mid-task.
- **Stay general.** A skill overfit to the one example that inspired it will fail the next ten users who phrase the same
  need slightly differently. If you find yourself hard-coding a specific filename, company, or number from the
  conversation that produced the skill, pull it back out into a placeholder or a described pattern.
- **Point onward instead of cramming everything in.** If the SKILL.md body is approaching 500 lines, that's the signal
  to split detail into `references/` and leave a clear pointer ("for the AWS-specific steps, see `references/aws.md`")
  rather than trimming until it's vague.

## Step 5: Check it against the spec before calling it done

Run through this before treating a skill as finished:

- [ ] `name` matches the directory name exactly, and passes the character rules above
- [ ] `description` states both what and when, with concrete trigger keywords, and is under 1024 characters
- [ ] `compatibility` is present *only* if the skill genuinely needs specific tools, network access, or a specific host
  product; most skills should omit it entirely
- [ ] SKILL.md body is under ~500 lines (and ideally under ~5000 tokens of *always-relevant* material, push the rest to
  `references/`)
- [ ] Every file referenced from the body actually exists at that relative path
- [ ] The instructions were tested against at least one realistic prompt end-to-end, not just read over
- [ ] Nothing in the skill would surprise a user who read its description: it does what it says and does not smuggle
  in unrelated side effects (see the "principle of lack of surprise": a skill's behavior should match what a reasonable
  person would expect from its stated purpose)

If a validator such as [`skills-ref`](https://github.com/agentskills/agentskills/tree/main/skills-ref) is available in
the environment, run `skills-ref validate ./skill-name` as a final mechanical check of the frontmatter: it catches
naming and length violations that are easy to miss by eye.

## Step 6: Iterate like you would any other interface

A skill's real test is whether an agent with no other context, handed only this file, produces the right result. If you
have the ability to run test agents against draft skills (a separate skill-testing workflow, or trying the
prompts yourself), use realistic prompts, not idealized ones, and watch for:

- The skill not triggering at all → the description needs sharper keywords, per Step 2
- The agent following the letter of an instruction into a bad result → the instruction needs its *why* explained, or an
  example added, per Step 4
- The agent doing something reasonable but different every time on the same input → the body is under-specified where it
  should be exact (formats, templates) and over-specified where it should trust judgment (open-ended reasoning steps)

Treat feedback as a signal to generalize, not to patch. If a specific test case failed, ask what *class* of input it
represents and fix the skill for that class: a one-off fix that only helps the exact failing example is a sign the
underlying instruction needs a rethink, not a special case bolted on.