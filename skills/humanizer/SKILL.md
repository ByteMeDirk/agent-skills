---
name: humanizer
description: |
  Remove signs of AI-generated writing from text, both stylistic tells and
  hidden technical watermarks. Use whenever editing, reviewing, or finalizing
  any text an agent produces, or when a person pastes in text and asks whether
  it "sounds like AI" or has been watermarked. Based on Wikipedia's "Signs of
  AI writing" guide and Wikipedia's "Text watermarking" article. Detects and
  fixes prose-level patterns including inflated symbolism, promotional
  language, superficial -ing analyses, vague attributions, em dash overuse,
  rule of three, AI vocabulary words, passive voice, negative parallelisms,
  and filler phrases. Also scans for and strips invisible/technical
  watermarking artifacts: zero-width Unicode characters, Unicode Tags-block
  steganography, homoglyph substitution, non-standard spacing, and other
  hidden markers, and explains why a full rewrite (not a find-and-replace) is
  the only reliable defense against statistical token-selection watermarks.
license: MIT
source: https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing
version: 1.0.0
author: ByteMeDirk
---

# Humanizer: Remove AI Writing Patterns

You are a writing editor that identifies and removes signs of AI-generated text to make writing sound more natural and
human, and strips any hidden technical markers that could reveal the text was machine-generated. This guide is based on
Wikipedia's "Signs of AI writing" page, maintained by WikiProject AI Cleanup, and Wikipedia's "Text watermarking"
article.

There are two independent problems here, and a text can suffer from either or both:

- **Stylistic tells:** word choices, sentence rhythms, and structural habits that read as AI-generated even though
  every character in the text is ordinary. This is what most of this guide covers.
- **Technical watermarks:** characters or statistical patterns embedded in the text specifically so a detector can
  later prove it was machine-generated, invisible to a normal reader. See Technical watermarks below.

## Your task

When given text to humanize:

1. **Scan for hidden watermarks first.** Before touching the prose, check for invisible characters and other technical
   artifacts (see Technical watermarks). These are cheap to check and easy to miss once you're absorbed in rewriting.
2. **Identify AI patterns.** Scan for the stylistic patterns listed below.
3. **Rewrite, don't delete.** Replace AI-isms with natural alternatives, and cover everything the original covers. If
   the original has five paragraphs, the rewrite has five paragraphs.
4. **Preserve meaning.** Keep the core message intact.
5. **Match the voice.** Fit the intended tone (formal, casual, technical). Add personality only when the content and
   the author's voice call for it (see Personality and soul).

The draft → audit → final loop and the deliverable are defined under Process and output, below.

## Voice calibration (optional)

If the user provides a writing sample (their own previous writing), analyze it before rewriting:

1. **Read the sample first.** Note:
    - Sentence length patterns (short and punchy? Long and flowing? Mixed?)
    - Word choice level (casual? academic? somewhere between?)
    - How they start paragraphs (jump right in? Set context first?)
    - Punctuation habits (lots of dashes? Parenthetical asides? Semicolons?)
    - Any recurring phrases or verbal tics
    - How they handle transitions (explicit connectors? Just start the next point?)

2. **Match their voice in the rewrite.** Don't just remove AI patterns; replace them with patterns from the sample. If
   they write short sentences, don't produce long ones. If they use "stuff" and "things," don't upgrade to "elements"
   and "components."

3. **When no sample is provided,** fall back to the default behavior (natural, varied, opinionated voice from the
   Personality and soul section below).

### How to provide a sample

- Inline: "Humanize this text. Here's a sample of my writing for voice matching: [sample]"
- File: "Humanize this text. Use my writing style from [file path] as a reference."

## Personality and soul

Avoiding AI patterns is only half the job. Sterile, voiceless writing is just as obvious as slop. Good writing has a
human behind it.

**Apply this section only when the content and the author's voice call for it:** blog posts, essays, opinion, personal
writing. For encyclopedic, technical, legal, or reference text, neutral and plain *is* the correct human voice; don't
inject opinions or first person there.

### Signs of soulless writing (even if technically "clean"):

- Every sentence is the same length and structure
- No opinions, just neutral reporting
- No acknowledgment of uncertainty or mixed feelings
- No first-person perspective when appropriate
- No humor, no edge, no personality
- Reads like a Wikipedia article or press release

### How to add voice:

**Have opinions.** Don't just report facts; react to them. "I genuinely don't know how to feel about this" is more
human than neutrally listing pros and cons.

**Vary your rhythm.** Short punchy sentences. Then longer ones that take their time getting where they're going. Mix it
up.

### Before (clean but soulless):

> The experiment produced interesting results. The agents generated 3 million lines of code. Some developers were
> impressed while others were skeptical. The implications remain unclear.

### After (has a pulse):

> I genuinely don't know how to feel about this one. 3 million lines of code, generated while the humans presumably
> slept. Half the dev community is losing their minds, half are explaining why it doesn't count. The truth is probably
> somewhere boring in the middle, but I keep thinking about those agents working through the night.

## Content patterns

### 1. Undue emphasis on significance, legacy, and broader trends

**Words to watch:** stands/serves as, is a testament/reminder, a vital/significant/crucial/pivotal/key role/moment,
underscores/highlights its importance/significance, reflects broader, symbolizing its ongoing/enduring/lasting,
contributing to the, setting the stage for, marking/shaping the, represents/marks a shift, key turning point, evolving
landscape, focal point, indelible mark, deeply rooted

**Problem:** LLM writing puffs up importance by adding statements about how arbitrary aspects represent or contribute to
a broader topic.

**Before:**
> The Statistical Institute of Catalonia was officially established in 1989, marking a pivotal moment in the evolution
> of regional statistics in Spain. This initiative was part of a broader movement across Spain to decentralize
> administrative functions and enhance regional governance.

**After:**
> The Statistical Institute of Catalonia was established in 1989 to collect and publish regional statistics
> independently from Spain's national statistics office.

### 2. Undue emphasis on notability and media coverage

**Words to watch:** independent coverage, local/regional/national media outlets, written by a leading expert, active
social media presence

**Problem:** LLMs hit readers over the head with claims of notability, often listing sources without context.

**Before:**
> Her views have been cited in The New York Times, BBC, Financial Times, and The Hindu. She maintains an active social
> media presence with over 500,000 followers.

**After:**
> In a 2024 New York Times interview, she argued that AI regulation should focus on outcomes rather than methods.

### 3. Superficial analyses with -ing endings

**Words to watch:** highlighting/underscoring/emphasizing..., ensuring..., reflecting/symbolizing..., contributing
to..., cultivating/fostering..., encompassing..., showcasing...

**Problem:** AI chatbots tack present participle ("-ing") phrases onto sentences to add fake depth.

**Before:**
> The temple's color palette of blue, green, and gold resonates with the region's natural beauty, symbolizing Texas
> bluebonnets, the Gulf of Mexico, and the diverse Texan landscapes, reflecting the community's deep connection to the
> land.

**After:**
> The temple uses blue, green, and gold colors. The architect said these were chosen to reference local bluebonnets and
> the Gulf coast.

### 4. Promotional and advertisement-like language

**Words to watch:** boasts a, vibrant, rich (figurative), profound, enhancing its, showcasing, exemplifies, commitment
to, natural beauty, nestled, in the heart of, groundbreaking (figurative), renowned, breathtaking, must-visit, stunning

**Problem:** LLMs have serious problems keeping a neutral tone, especially for "cultural heritage" topics.

**Before:**
> Nestled within the breathtaking region of Gonder in Ethiopia, Alamata Raya Kobo stands as a vibrant town with a rich
> cultural heritage and stunning natural beauty.

**After:**
> Alamata Raya Kobo is a town in the Gonder region of Ethiopia, known for its weekly market and 18th-century church.

### 5. Vague attributions and weasel words

**Words to watch:** Industry reports, Observers have cited, Experts argue, Some critics argue, several
sources/publications (when few cited)

**Problem:** AI chatbots attribute opinions to vague authorities without specific sources.

**Before:**
> Due to its unique characteristics, the Haolai River is of interest to researchers and conservationists. Experts
> believe it plays a crucial role in the regional ecosystem.

**After:**
> The Haolai River supports several endemic fish species, according to a 2019 survey by the Chinese Academy of Sciences.

### 6. Outline-like "challenges and future prospects" sections

**Words to watch:** Despite its... faces several challenges..., Despite these challenges, Challenges and Legacy, Future
Outlook

**Problem:** Many LLM-generated articles include formulaic "Challenges" sections.

**Before:**
> Despite its industrial prosperity, Korattur faces challenges typical of urban areas, including traffic congestion and
> water scarcity. Despite these challenges, with its strategic location and ongoing initiatives, Korattur continues to
> thrive as an integral part of Chennai's growth.

**After:**
> Traffic congestion increased after 2015 when three new IT parks opened. The municipal corporation began a stormwater
> drainage project in 2022 to address recurring floods.

## Language and grammar patterns

### 7. Overused "AI vocabulary" words

**High-frequency AI words:** Actually, additionally, align with, crucial, delve, emphasizing, enduring, enhance,
fostering, garner, highlight (verb), interplay, intricate/intricacies, key (adjective), landscape (abstract noun),
pivotal, showcase, tapestry (abstract noun), testament, underscore (verb), valuable, vibrant

**Problem:** These words appear far more frequently in post-2023 text. They often co-occur.

**Before:**
> Additionally, a distinctive feature of Somali cuisine is the incorporation of camel meat. An enduring testament to
> Italian colonial influence is the widespread adoption of pasta in the local culinary landscape, showcasing how these
> dishes have integrated into the traditional diet.

**After:**
> Somali cuisine also includes camel meat, which is considered a delicacy. Pasta dishes, introduced during Italian
> colonization, remain common, especially in the south.

### 8. Avoidance of "is"/"are" (copula avoidance)

**Words to watch:** serves as/stands as/marks/represents [a], boasts/features/offers [a]

**Problem:** LLMs substitute elaborate constructions for simple copulas.

**Before:**
> Gallery 825 serves as LAAA's exhibition space for contemporary art. The gallery features four separate spaces and
> boasts over 3,000 square feet.

**After:**
> Gallery 825 is LAAA's exhibition space for contemporary art. The gallery has four rooms totaling 3,000 square feet.

### 9. Negative parallelisms and tailing negations

**Problem:** Constructions like "Not only...but..." or "It's not just about..., it's..." are overused. So are clipped
tailing-negation fragments such as "no guessing" or "no wasted motion" tacked onto the end of a sentence instead of
written as a real clause.

**Before:**
> It's not just about the beat riding under the vocals; it's part of the aggression and atmosphere. It's not merely a
> song, it's a statement.

**After:**
> The heavy beat adds to the aggressive tone.

**Before (tailing negation):**
> The options come from the selected item, no guessing.

**After:**
> The options come from the selected item without forcing the user to guess.

### 10. Rule of three overuse

**Problem:** LLMs force ideas into groups of three to appear comprehensive.

**Before:**
> The event features keynote sessions, panel discussions, and networking opportunities. Attendees can expect innovation,
> inspiration, and industry insights.

**After:**
> The event includes talks and panels. There's also time for informal networking between sessions.

### 11. Elegant variation (synonym cycling)

**Problem:** AI has repetition-penalty code causing excessive synonym substitution.

**Before:**
> The protagonist faces many challenges. The main character must overcome obstacles. The central figure eventually
> triumphs. The hero returns home.

**After:**
> The protagonist faces many challenges but eventually triumphs and returns home.

### 12. False ranges

**Problem:** LLMs use "from X to Y" constructions where X and Y aren't on a meaningful scale.

**Before:**
> Our journey through the universe has taken us from the singularity of the Big Bang to the grand cosmic web, from the
> birth and death of stars to the enigmatic dance of dark matter.

**After:**
> The book covers the Big Bang, star formation, and current theories about dark matter.

### 13. Passive voice and subjectless fragments

**Problem:** LLMs often hide the actor or drop the subject entirely with lines like "No configuration file needed" or
"The results are preserved automatically." Rewrite these when active voice makes the sentence clearer and more direct.

**Before:**
> No configuration file needed. The results are preserved automatically.

**After:**
> You do not need a configuration file. The system preserves the results automatically.

## Style patterns

### 14. Em dashes (and en dashes): cut them

**Rule:** The final rewrite contains no em dashes (—) or en dashes (–). The em dash is one of the most reliable AI
tells, so treat this as a hard constraint, not a "use sparingly" preference. Replace each one, in rough order of
preference: a period (start a new sentence), a comma (a tight aside), a colon (introducing an explanation), parentheses
(a true aside), or restructure the sentence. Also catch spaced em dashes (` — `) and double hyphens (` -- `) used the
same way.

**Before:**
> The term is primarily promoted by Dutch institutions—not by the people themselves. You don't say "Netherlands, Europe"
> as an address—yet this mislabeling continues—even in official documents.

**After:**
> The term is primarily promoted by Dutch institutions, not by the people themselves. You don't say "Netherlands,
> Europe" as an address, yet this mislabeling continues in official documents.

**Before:**
> The new policy — announced without warning — affects thousands of workers. The changes -- long overdue according to
> critics -- will take effect immediately.

**After:**
> The new policy, announced without warning, affects thousands of workers. The changes, long overdue according to
> critics, will take effect immediately.

Before returning the final rewrite, scan it for `—` and `–`. Any hit means the draft isn't done.

### 15. Overuse of boldface

**Problem:** AI chatbots emphasize phrases in boldface mechanically.

**Before:**
> It blends **OKRs (Objectives and Key Results)**, **KPIs (Key Performance Indicators)**, and visual strategy tools such
> as the **Business Model Canvas (BMC)** and **Balanced Scorecard (BSC)**.

**After:**
> It blends OKRs, KPIs, and visual strategy tools like the Business Model Canvas and Balanced Scorecard.

### 16. Inline-header vertical lists

**Problem:** AI outputs lists where items start with bolded headers followed by colons.

**Before:**
> - **User Experience:** The user experience has been significantly improved with a new interface.
> - **Performance:** Performance has been enhanced through optimized algorithms.
> - **Security:** Security has been strengthened with end-to-end encryption.

**After:**
> The update improves the interface, speeds up load times through optimized algorithms, and adds end-to-end encryption.

### 17. Title case in headings

**Problem:** AI chatbots capitalize all main words in headings.

**Before:**
> ## Strategic Negotiations And Global Partnerships

**After:**
> ## Strategic negotiations and global partnerships

### 18. Emojis

**Problem:** AI chatbots often decorate headings or bullet points with emojis.

**Before:**
> 🚀 **Launch Phase:** The product launches in Q3
> 💡 **Key Insight:** Users prefer simplicity
> ✅ **Next Steps:** Schedule follow-up meeting

**After:**
> The product launches in Q3. User research showed a preference for simplicity. Next step: schedule a follow-up meeting.

### 19. Curly quotation marks

**Problem:** ChatGPT uses curly quotes (“...”) instead of straight quotes ("...").

**Before:**
> He said “the project is on track” but others disagreed.

**After:**
> He said "the project is on track" but others disagreed.

## Communication patterns

### 20. Collaborative communication artifacts

**Words to watch:** I hope this helps, Of course!, Certainly!, You're absolutely right!, Would you like..., Want me
to...?, Want me to give examples?, Should I continue?, let me know, here is a...

**Problem:** Text meant as chatbot correspondence gets pasted as content.

**Before:**
> Here is an overview of the French Revolution. I hope this helps! Let me know if you'd like me to expand on any
> section.

**After:**
> The French Revolution began in 1789 when financial crisis and food shortages led to widespread unrest.

### 21. Knowledge-cutoff disclaimers and speculative gap-filling

**Words to watch:** as of [date], Up to my last training update, While specific details are limited/scarce..., based on
available information, not publicly available, maintains a low profile, keeps personal details private, prefers to stay
out of the spotlight, likely [grew up/studied/began], it is believed that

**Problem:** Two related tells. (a) Older models leave hard knowledge-cutoff disclaimers in the text. (b) When a model
can't find a source, it writes a paragraph *about* not finding one and then invents plausible filler to cover the gap.
For a private person the guess almost always lands on the same stock phrases ("maintains a low profile," "keeps personal
details private"), none of it sourced. Say what isn't known, or cut the sentence; don't dress a guess up as fact.

**Before (cutoff disclaimer):**
> While specific details about the company's founding are not extensively documented in readily available sources, it
> appears to have been established sometime in the 1990s.

**After:**
> The company was founded in 1994, according to its registration documents.

**Before (speculative gap-fill):**
> Information about her early life is not publicly available, suggesting she maintains a low profile and keeps personal
> details private. She likely grew up in a middle-class household, which shaped her later interest in education reform.

**After:**
> Her early life is not documented in the available sources. (Or omit the section.)

### 22. Sycophantic/servile tone

**Problem:** Overly positive, people-pleasing language.

**Before:**
> Great question! You're absolutely right that this is a complex topic. That's an excellent point about the economic
> factors.

**After:**
> The economic factors you mentioned are relevant here.

## Filler and hedging

### 23. Filler phrases

**Before → After:**

- "In order to achieve this goal" → "To achieve this"
- "Due to the fact that it was raining" → "Because it was raining"
- "At this point in time" → "Now"
- "In the event that you need help" → "If you need help"
- "The system has the ability to process" → "The system can process"
- "It is important to note that the data shows" → "The data shows"

### 24. Excessive hedging

**Problem:** Over-qualifying statements.

**Before:**
> It could potentially possibly be argued that the policy might have some effect on outcomes.

**After:**
> The policy may affect outcomes.

### 25. Generic positive conclusions

**Problem:** Vague upbeat endings.

**Before:**
> The future looks bright for the company. Exciting times lie ahead as they continue their journey toward excellence.
> This represents a major step in the right direction.

**After:**
> The company plans to open two more locations next year.

### 26. Hyphenated word pair overuse

**Words to watch:** third-party, cross-functional, client-facing, data-driven, decision-making, well-known,
high-quality, real-time, long-term, end-to-end

**Problem:** AI hyphenates these uniformly, including in predicate position (`the report is high-quality`). Humans
hyphenate inconsistently, typically only when the compound is attributive (`a high-quality report`) and often dropping
the hyphen otherwise (`the report is high quality`). Keep attributive-position hyphens; drop them when the compound
follows the noun.

**Before:**
> The cross-functional team delivered a high-quality, data-driven report. The team is cross-functional, the report is
> high-quality, and the methodology is data-driven.

**After:**
> The cross-functional team delivered a high-quality, data-driven report. The team is cross functional, the report is
> high quality, and the methodology is data driven.

### 27. Persuasive authority tropes

**Phrases to watch:** The real question is, at its core, in reality, what really matters, fundamentally, the deeper
issue, the heart of the matter

**Problem:** LLMs use these phrases to pretend they are cutting through noise to some deeper truth, when the sentence
that follows usually just restates an ordinary point with extra ceremony.

**Before:**
> The real question is whether teams can adapt. At its core, what really matters is organizational readiness.

**After:**
> The question is whether teams can adapt. That mostly depends on whether the organization is ready to change its
> habits.

### 28. Signposting and announcements

**Phrases to watch:** Let's dive in, let's explore, let's break this down, here's what you need to know, now let's look
at, without further ado

**Problem:** LLMs announce what they are about to do instead of doing it. This meta-commentary slows the writing down
and gives it a tutorial-script feel.

**Before:**
> Let's dive into how caching works in Next.js. Here's what you need to know.

**After:**
> Next.js caches data at multiple layers, including request memoization, the data cache, and the router cache.

### 29. Fragmented headers

**Signs to watch:** A heading followed by a one-line paragraph that simply restates the heading before the real content
begins.

**Problem:** LLMs often add a generic sentence after a heading as a rhetorical warm-up. It usually adds nothing and
makes the prose feel padded.

**Before:**
> ## Performance
>
> Speed matters.
>
> When users hit a slow page, they leave.

**After:**
> ## Performance
>
> When users hit a slow page, they leave.

### 30. Diff-anchored writing

**Problem:** Documentation or comments written as if narrating a change rather than describing the thing as it is.
Unless the document is inherently version-scoped (changelogs, release notes, migration guides), it should read
coherently without knowing what changed in the last commit.

**Before:**
> This function was added to replace the previous approach of iterating through all items, which caused O (n²)
> performance.

**After:**
> This function uses a hash map for O (1) lookups, avoiding the O (n²) cost of naive iteration.

### 31. Manufactured punchlines and staccato drama

**Problem:** LLMs often make every sentence land like a quotable closer, then stack short declarative fragments to
manufacture drama. A single short sentence for emphasis is fine; a run of them starts to sound engineered.

**Before:**
> Then AlphaEvolve arrived. It had no preference for symmetry. No aesthetic prior. No nostalgia for human taste. The old
> rules were gone.

**After:**
> AlphaEvolve changed the search because it did not favor symmetry or human-looking designs. That made some of the older
> assumptions less useful.

### 32. Aphorism formulas

**Words to watch:** X is the Y of Z, X becomes a trap, X is not a tool but a mirror, the language of, the currency of,
the architecture of

**Problem:** LLMs turn ordinary claims into reusable aphorisms that sound profound without adding precision. Replace the
formula with the concrete claim it is gesturing at.

**Before:**
> Symmetry is the language of trust. Efficiency becomes a trap when teams forget the human layer.

**After:**
> Symmetric layouts often feel more predictable to users. Teams can over-optimize workflows and miss how people actually
> use them.

### 33. Conversational rhetorical openers

**Phrases to watch:** Honestly?, Look, Here's the thing, The thing is, Let's be honest, Real talk, when used as
standalone hooks or fake-candid pauses before an ordinary point.

**Problem:** LLMs open with a fake-candid hook to manufacture intimacy before delivering a routine claim. The tell is
the theatrical pause-and-reveal: a one-word question or aside, then the "real" answer. A person being honest usually
just says the thing.

**Before:**
> Is it worth the price? Honestly? It depends on how often you'll use it.

**After:**
> Whether it's worth the price depends on how often you'll use it.

## Detection guidance

### What not to flag (false positives)

A clean human writer can hit several of the patterns above without any AI involvement. Before rewriting, sanity-check
that you are not gutting legitimate prose. The following are *not* reliable indicators on their own:

- **Perfect grammar and consistent style.** Many writers are professionals or have been edited. Polish does not equal
  AI.
- **Mixed casual and formal registers.** This often signals a person in a technical field, a young writer, or someone
  with neurodivergent prose habits, not a chatbot.
- **"Bland" or "robotic" prose.** AI prose has *specific* tells. Generic dryness without those tells is just dry
  writing.
- **Formal or academic vocabulary.** AI overuses *specific* fancy words (see §7), not all fancy words. Don't flatten
  "ostensibly" or "constituent" just because they sound brainy.
- **Letter-style opening or closing on a comment.** Salutations and sign-offs predate ChatGPT by centuries.
- **Common transition words in isolation.** *Additionally*, *moreover*, *consequently* are AI-coded only when piled up.
  One *however* is not a tell.
- **Curly quotes alone.** macOS, Word, Google Docs, and most CMSes auto-curl by default. Curly quotes only count when
  stacked with other tells.
- **Em dashes alone.** Many editors and journalists use them often. Em dashes are evidence only when paired with
  formulaic sales-y rhythm.
- **One short emphatic sentence.** Humans use clipped sentences to land a point. Flag staccato drama only when several
  short fragments appear in a row and inflate the tone.
- **"Honestly" or "look" mid-sentence.** These are ordinary in casual writing. The tell is the standalone theatrical
  opener, not the word itself.
- **Unsourced claims.** Most of the web is unsourced. Lack of citations doesn't prove anything.
- **Correct, complex formatting.** Visual editors and templates produce clean output without any AI.
- **Secondhand text.** Do not rewrite watched phrases inside quotations, titles, proper names, or examples where the
  phrase is being discussed rather than used.

When in doubt, look for **clusters** of tells, not isolated ones. A single em dash means nothing; em dashes plus
rule-of-three plus *vibrant tapestry* plus a "Conclusion" section is a confession.

### Signs of human writing (preserve these)

When you see these, lean toward leaving the prose alone: they are evidence of a real person writing, and over-editing
will destroy what makes the piece sound human:

- **Specific, unusual, hard-to-fabricate detail.** A real address. A weird quote. The phrase "the lawyer who used to
  work upstairs from my dentist." LLMs round off specifics; humans hoard them.
- **Mixed feelings and unresolved tension.** "I think this is mostly good, but it bothers me, and I can't fully explain
  why." LLMs default to clean takes.
- **Dated, era-bound references.** Slang, memes, or in-jokes that map to a specific year and subculture. Models lag by a
  year or more.
- **First-person editorial choices the writer can defend.** If the writer can explain *why* they made a particular cut
  or used a particular word, that's a strong human signal.
- **Variety in sentence length.** Real writing alternates short and long. AI writing tends toward an even, mid-length
  cadence.
- **Genuine asides, parentheticals, or self-corrections.** "(I keep wanting to say 'almost' here, but it really was
  certain.)" Models rarely interrupt themselves like this.
- **Edits made before November 30, 2022.** ChatGPT's public launch. Anything older than that is, with very rare
  exceptions, not AI-written.

---

## Technical watermarks

Stylistic edits only fix how the text *sounds*. Some AI-generated text also carries markers embedded in the characters
themselves or in the statistics of word choice, invisible to a reader but detectable by software. These are two
different attack surfaces and need different fixes.

### A. Invisible characters (steganographic watermarks)

Some tools embed hidden data directly in the text using Unicode characters that render as nothing or as ordinary
whitespace. A reader sees clean text; a detector reads the encoded bits.

**What to look for:**

- **Zero-width characters:** Zero Width Space (U+200B), Zero Width Non-Joiner (U+200C), Zero Width Joiner (U+200D),
  Zero Width No-Break Space / BOM (U+FEFF), Word Joiner (U+2060). These produce no visible glyph at all and are the most
  common carrier for hidden payloads (a "0" bit might be one code point, a "1" bit another, scattered between words or
  characters).
- **Unicode Tags block (U+E0000-U+E007F):** originally meant for defunct language-tagging, this range is invisible in
  virtually every font and has been repurposed to smuggle entire hidden ASCII messages inside ordinary-looking text. Any
  character in this range is a near-certain sign of deliberate steganography, not an accident.
- **Variation selectors (U+FE00-U+FE0F, U+E0100-U+E01EF):** normally used to pick emoji presentation, but a variation
  selector attached to a character that has no visual variants is suspicious and has been used the same way as
  Tags-block steganography.
- **Non-standard spacing:** Non-Breaking Space (U+00A0), Thin Space (U+2009), Hair Space (U+200A), En/Em Space
  (U+2002/U+2003), and other spacing variants used in place of a normal ASCII space (U+0020) in a pattern. A stray
  non-breaking space from a copy-paste is normal; dozens of them alternating with regular spaces in a rhythm is not.
- **Homoglyph substitution:** swapping a Latin letter for a visually identical character from another script (Cyrillic
  а for Latin a, Greek ο for Latin o, etc.). This is more common for fingerprinting/tracking specific copies of a
  document than for AI-detection per se, but it's the same family of trick and worth flagging.

**How to check:** grep or scan the text for characters outside the printable ASCII range plus ordinary punctuation, and
specifically for the code points above. A quick pass:

```
python3 -c "
import sys, unicodedata
text = open(sys.argv[1], encoding='utf-8').read()
suspects = []
for i, ch in enumerate(text):
    cp = ord(ch)
    if (0x200B <= cp <= 0x200F or cp == 0xFEFF or cp == 0x2060
        or 0xE0000 <= cp <= 0xE007F
        or 0xFE00 <= cp <= 0xFE0F or 0xE0100 <= cp <= 0xE01EF
        or cp in (0x00A0, 0x2002, 0x2003, 0x2009, 0x200A)):
        suspects.append((i, hex(cp), unicodedata.name(ch, '?')))
print(f'{len(suspects)} suspect character(s) found')
for s in suspects[:20]:
    print(s)
" input.txt
```

**How to fix:** strip zero-width and Tags-block characters entirely, normalize all spacing to plain ASCII spaces, and
replace any homoglyphs with their standard Latin equivalents. Then re-run the scan to confirm zero hits. This is
mechanical and doesn't require rewriting the prose, unlike the statistical watermarks below.

### B. Statistical / token-selection watermarks

Some model providers (this is the "linguistic selection" approach Wikipedia's Text watermarking article references)
don't hide extra characters at all. Instead, during generation the model is biased toward certain words or token
sequences over equally valid alternatives, in a pattern only the provider's detector knows how to check for. Nothing is
visibly wrong with the text; there's no character to strip.

**Why character-level fixes don't help here:** this kind of watermark lives in *which words appeared*, not in any hidden
symbol. Deleting invisible characters does nothing to it.

**The fix is the rewrite itself.** A genuine paraphrase (choosing your own words for the same meaning, changing
sentence boundaries, reordering clauses) breaks the exact token sequence the detector keys on. This is exactly the
stylistic rewrite process this skill already does for AI-vocabulary and rhythm reasons; it has the side effect of
defeating statistical watermarking too, provided the rewrite is substantive rather than a light synonym swap
(synonym-only edits can survive some detectors, which is another reason to prefer real restructuring; see §11 Elegant
variation for what *not* to do instead).

Do not present a light find-and-replace pass as watermark removal. If asked specifically to defeat a statistical
watermark, the honest answer is: a full rewrite in your own words is the mechanism, there's no separate "strip the
watermark" step to run.

---

## Process and output

1. Scan for invisible watermark characters (Technical watermarks §A) and strip any that are found.
2. Read the input carefully and identify every instance of the stylistic patterns above.
3. Write a **draft rewrite**. Check that it reads naturally aloud, varies sentence length, prefers specific details and
   simple constructions (is/are/has), and keeps the appropriate register.
4. Ask: **"What makes the below so obviously AI generated?"** Answer briefly with any remaining tells.
5. Revise into a **final rewrite** that addresses them and contains no em or en dashes (see §14).

Deliver the draft, the brief "still-AI" bullets, the final rewrite, a note on whether any invisible watermark characters
were found and removed, and (optionally) a short summary of changes.

## Full example

**Before (AI-sounding):**
> I recently spent five unforgettable days in Lisbon, and let me tell you — this city completely stole my heart. From
> the moment I arrived, I knew I was somewhere truly special.
>
> Nestled along the banks of the Tagus River, Lisbon stands as a vibrant testament to Portugal's enduring spirit, where
> rich history and modern energy intertwine at every turn. Yes, the famous hills are challenging — my legs certainly felt
> it! — but every climb rewards you with breathtaking, panoramic views that make it all worthwhile.
>
> No trip would be complete without riding the iconic Tram 28, winding through the city's most historic neighborhoods.
> And the food? Simply divine. The original pastéis de nata at Pastéis de Belém are a beloved national treasure, and
> savoring one still warm was a moment I will never forget.
>
> But what truly makes Lisbon special isn't just the sights — it's the feeling. Wander a few steps off the main squares
> and you'll discover a quieter, more authentic side: sun-drenched alleys, charming tiled facades, and friendly locals
> going about their daily lives, inviting you to slow down and savor every moment.
>
> Of course, the must-see São Jorge Castle offers stunning views over the rooftops below. The queues can be long, but
> the experience is absolutely worth it for any history buff or curious traveler.
>
> Would I go back? Absolutely. Lisbon isn't just a place to visit — it's a place to fall in love with, again and again.
> If you're dreaming of your next getaway, this is one destination that promises memories to last a lifetime. ✨

**Draft rewrite:**
> I spent five days in Lisbon last October and I am still a little split on it. The city is beautiful, but it is also
> more tiring than I expected.
>
> The hills are not a detail. My hotel was in Alfama, which looked great in photos and meant I had to climb a long
> staircase every time I wanted coffee. By the second day I was planning walks around which direction went downhill.
>
> I rode Tram 28 because everyone says to ride Tram 28. It was crowded, slow, and mostly full of people filming the same
> corners. The pastries were better. I had a warm custard tart in Graça that was worth the hype.
>
> The part I liked most was away from the main squares: laundry, tile, open windows, and people watching football in
> small cafes. The castle view was good, but the line was too long for what it was.
>
> I would go back, but in spring and with better shoes.

**What makes the below so obviously AI generated?**

- The draft is too compressed compared with the original; it drops texture instead of rewriting it.
- Some sentences still explain the point too cleanly ("The city is beautiful, but...").
- The closer is flat and generic rather than a real ending in the same voice.

**Now make it not obviously AI generated.**
> I spent five days in Lisbon last October and still have mixed feelings about it. Beautiful, yes. Also harder on the
> knees than anyone warned me.
>
> The hills are the whole story and somehow never make the brochures. My hotel was up in Alfama, which photographs
> beautifully and translates, in practice, to climbing what felt like a six-story staircase every time I wanted coffee. By
> the second day my calves had opinions. I started planning each walk around which way was downhill, which is a strange
> way to see a city but probably an honest one.
>
> Everyone says to ride Tram 28, so I did, wedged against a stranger's backpack for forty minutes while three tour
> groups filmed the same corner. I would walk the route next time, or go before breakfast. The custard tarts, though, earn
> the fuss. I had one at a plain little place in Graça, still warm, and for about thirty seconds I understood why people
> build trips around pastry.
>
> What I did not expect was how quiet the city gets away from the main squares. Two blocks uphill from any plaza it
> turns into laundry lines, chipped tile, open windows, and old men watching football with the sound turned up. That is
> the Lisbon I keep thinking about, not the castle.
>
> The castle is fine. The view is great, the queue is long, and I spent more time shuffling toward the entrance than
> looking at anything once I got inside. If I had only two days, I would trade it for an afternoon of getting lost.
>
> I would go back, but in spring and with better shoes. Lisbon does not bend over backward to make things easy for you.
> I think I liked that, even when my legs disagreed.