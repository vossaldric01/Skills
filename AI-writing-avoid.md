---
name: avoid-ai-writing-tells
description: Detect and eliminate the AI-writing "tells" catalogued in Wikipedia's Signs of AI Writing guide — overused vocabulary (delve, tapestry, pivotal, underscore, testament...), puffery, tailing clauses, negative parallelisms ("it's not X, it's Y"), rule-of-three lists, false ranges, vague attribution, compulsive summaries, and formatting habits like em-dash overuse, heavy boldface, and rigid section templates. Use this skill whenever drafting, writing, editing, polishing, or reviewing any substantial prose meant to read as genuinely human-written — articles, blog posts, essays, reports, Wikipedia-style content, marketing copy, LinkedIn/social posts, emails, or documentation — even if the user never mentions AI or detection explicitly. Definitely use it when the user asks to "make this sound more human," "remove AI tells," "de-AI this," "sound less like ChatGPT," or check writing for robotic/formulaic patterns.
---

# Avoid AI Writing Tells

Based on Wikipedia's [Signs of AI Writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) — a field guide built by WikiProject AI Cleanup from thousands of real cases of undisclosed AI text on Wikipedia. It documents the patterns LLMs fall into by statistical default: safe, high-probability words and constructions that smooth specific facts into generic, vaguely impressive filler.

## Core principle: patterns, not a ban list

The source guide is explicit that it is **descriptive, not prescriptive** — a single em dash or one use of "crucial" proves nothing; human writers use both. The tell is *clustering*: several of these patterns stacking in the same paragraph. Mechanically stripping every item below produces flat, personality-free prose, which is its own giveaway. The goal is specificity and restraint, not sterilization. Don't self-censor mid-draft — write naturally, then run the pass below before finalizing.

## The pass: five categories to scan for

### 1. Vocabulary
LLM output leans on a small, shifting set of "safe" words far more than human writing does. Worst offenders across recent model eras: **delve, tapestry, pivotal, underscore(s), boast(s), testament, intricate/intricacies, meticulous(ly), vibrant, crucial, foster/fostering, showcase/showcasing, highlight/highlighting, enhance, garner, landscape, realm, robust, seamless, leverage, navigate the complexities of.**

A word appearing once isn't damning, and banning a word doesn't ban its synonyms — context matters (a literal "underground realm" is fine). Full categorized lists by era, plus clichés and hedge words → `references/vocabulary-and-phrases.md`.

### 2. Sentence-level constructions
- **Negative parallelism** — "It's not X, it's Y," "not only X but Y," "no X, no Y, just Z." A legitimate rhetorical move used so often by LLMs it reads as reflexive.
- **Rule of three** — reflexive triplets: "innovative, transformative, and groundbreaking." Fine once; a tell when every list in the piece has exactly three items.
- **Tailing clauses** — a present-participle (-ing) phrase bolted onto the end of a plain sentence to manufacture significance: "...highlighting its growing influence," "...underscoring the shift." Usually adds no actual information.
- **False ranges** — "from X to Y" implying a spectrum that isn't really there: "from intimate gatherings to global movements."
- **Vague attribution** — "critics argue," "studies show," "observers have noted" — with no one named.

### 3. Paragraph and structure level
- **Puffery / inflated significance** — "plays a vital role," "stands as a testament to," "marks a pivotal moment," applied to facts that don't warrant the weight.
- **Editorializing asides** — "it's important to note," "no discussion would be complete without," "it's worth remembering" — the model inserting unearned opinion about what matters.
- **Compulsive summaries** — restating a short passage as "In summary," "Overall," "In conclusion," even when nothing needed restating.
- **Rigid, template-identical structure** — every article/section getting the same skeleton (e.g. always a "Challenges" and "Future Outlook" section) regardless of whether the subject calls for it.
- **Promotional / tourism-brochure tone** — "breathtaking," "nestled within," "rich heritage," "boasts [feature]" — dressing up a neutral fact as marketing copy.

### 4. Formatting
- **Em dash overuse** — especially spaced em dashes ( — like this — ) used where a comma, colon, or parenthesis would read more naturally. The single most notorious tell; also the easiest to over-correct on, so don't purge every dash reflexively.
- **Heavy, reflexive boldface** for emphasis that doesn't need it.
- **Over-listing** — bullet points for things that read better as a sentence or two.
- **Inconsistent Title Case Headers.**
- **Emoji in headers or list items.**
- **Smart/curly quotes and apostrophes** used inconsistently against the surrounding text's convention.
- **Leftover raw Markdown** — unconverted `# ` headers, canned bold-label list items like "**Feature X**: description," in output meant to be plain prose.
- **Unfilled template/placeholder text** left in from a prompt scaffold.

### 5. Tone mismatches
- **Letter-style phrasing in non-letter content** — "I hope this message finds you well," "Thank you for your time and consideration," dropped into a blog post or article.
- **Leftover chat phrases** signaling unedited copy-paste from an AI conversation — "I hope this helps!," "Of course!," "Let me know if you need anything else."
- **Silent Americanization** — defaulting to American spelling/conventions ("color," "-ize," Oxford comma) even when the subject or audience calls for another variety.

## Applying the pass

1. Draft normally first.
2. Re-read once specifically against the five categories above.
3. Flag clusters, not isolated hits — judge the paragraph, not the word.
4. When cutting a vague phrase, replace it with something concrete and specific — not a blander version of the same generic claim. "Plays a vital role in the local economy" → say what it actually does, with a number or specific mechanism if you have one.
5. Keep genuine voice: a real strong claim, an intentional em dash, a triplet that's actually the right length — all fine. The target is specificity, not blandness.

## Reference

`references/vocabulary-and-phrases.md` — full era-by-era overused-word lists, cliché and hedge-word lists, and short before/after rewrite examples.
