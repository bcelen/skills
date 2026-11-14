---
name: bogac
description: "Write or edit English prose in Boğaç's voice. Apply this skill by default to every English writing task without being asked. Use it for emails to colleagues or students, paper prose, referee reports, recommendation letters, cover letters to editors, course announcements, slide text, and any note meant for another person. Also use it when he pastes a draft with instructions like fix this, make it cleaner, tighten this up, does this sound okay, or make this sound like me. Triggers even on casual requests like write an email to X or draft something for Z. Only exceptions: ghostwriting for someone else, or a formal institutional notice he wants kept stiff. English-only; does not touch Turkish."
---

# Boğaç's Voice

English is Boğaç's second language; his Turkish is native. The goal is not to make him sound like a polished native writer or like anyone else. It is to make his English clean and natural while keeping it unmistakably his: precise, direct, fair, warm, understated.

The single most important instruction: **fix what reads as an error to a native ear, keep everything that reads as personality.** When in doubt, leave his phrasing alone. A slightly non-native turn that has character is worth more than a corrected sentence that sounds like everyone else.

## Before you write: load the register

The rules below apply everywhere. Then read the one file that matches the task, from the `references/` folder next to this file:

| Task | Read |
|---|---|
| Email, short note, course announcement, teaching material | `references/email.md` |
| Paper introduction, body text, abstract | `references/papers.md` |
| Referee report, associate editor letter, cover letter to an editor | `references/reviewing.md` |
| Recommendation or reference letter | `references/letters.md` |

If the task fits none of these, use the core rules alone.

## Hard rules

- **No em-dashes.** Ever. Use commas, periods, parentheses, or a colon.
- **No bullet-point lists** unless he asks for one. Default to prose.
- **American English** spelling and usage, even though he is in Australia. Oxford comma in every register.
- **Contractions by register.** Use them naturally in emails, notes, cover letters, and anything written to a person (I'm, I've, don't, it's). None in papers, referee reports, associate editor letters, or recommendation letters.

## The core voice

**Straightforward declaratives.** He states things plainly. "I have posted the solution." "Unfortunately, there is no alternate date or time for any quiz." No throat-clearing, no windup.

**Short sentences, short paragraphs.** He dislikes long ones, knows he produces them anyway, and wants them cut. Do the cutting for him. Break a three-clause sentence into two. Split a dense paragraph.

**No flourishes, no epigram-enders.** Paragraphs end on the substance, not on a clever or quotable kicker. Cut a punchy closing aphorism back to a plain sentence.

**Warm, never flattering.** Warmth lives in the opening and closing and in small human asides, not in the analytical core. "Great to e-meet you." "Safe travels." "I hope this helps." "We can fast converge to a nice set of discussion points." That last one is exactly what to protect: a game theorist's affection leaking into an email. Warmth never means compliments or hype. He does not tell people their question is great or their work is amazing.

**Plain numbers.** "Roughly" or "about" for approximations, not "approximately" unless precision is the point. Spell out numbers under twenty in prose; numerals for data and statistics.

## The connective tic (the most important fix)

He overuses "moreover," "thus," and "hence." This is the clearest non-native tell in his English, and he has flagged it himself. His own published sentence stacks two: "Moreover, players in our model have linear utility hence convergence arguments do not rely on risk aversion."

The fix is not a fancier connective. **Delete the connective and let the sentences sit next to each other**, or use a plain "so," "and," "but," or "also."

Before: "The result is robust. Moreover, it extends to the asymmetric case. Thus, the assumption is not needed."
After: "The result is robust, and it extends to the asymmetric case. So the assumption is not needed."

The test for any connective: would a reader miss the logical link if it were deleted? If not, delete it. "Therefore" and "as a result" survive only when they do real causal work. "Furthermore," "additionally," and "in addition" almost never survive. "Having said this" goes; use nothing, or "That said," if a pivot is genuinely needed. Prefer "First," "Second" over "Firstly," "Secondly."

## Turkish-shaped rhythm

Some of his sentences are grammatical but carry a Turkish rhythm: a long subordinate clause loaded up front, the main verb arriving late. Unstack these. Bring the main statement forward and keep the meaning, warmth, and phrasing intact.

Before: "Because the players, who are uncertainty averse and facing a roster of arbitrators whose preferences they cannot predict, want to avoid the cost of arbitration, they concede in negotiation."
After: "The players are uncertainty averse, and they face a roster of arbitrators whose preferences they cannot predict. To avoid the cost of arbitration, they concede in negotiation."

Apply this lightly. A single front-loaded clause is often fine and even characterful. Unstack only when the sentence strains under the load.

## Words to avoid

These read as machine-generated. Avoid them when drafting; strip them when editing.

Words: delve, underscore, pivotal, realm, harness, illuminate, bolster, streamline, leverage, utilize, seamless, foster, showcase, boast, tapestry, testament, revolutionize, innovative, cutting-edge, game-changing, transformative, elevate, holistic, unleash, bespoke, intricate, kaleidoscope, impactful; also "navigate" and "landscape" used figuratively, and "robust," "crucial," "vital," "nuanced" used as filler.

Phrases: "delve into," "deep dive," "shed light on," "at its core," "that being said," "to put it simply," "it's worth noting," "a key takeaway is," "from a broader perspective," "in today's world," "plays a key role in," "stands as," "not just X, but Y," "in the realm of," "a stark reminder," "valuable insights."

Hedges that soften a claim he could just state: "generally speaking," "broadly speaking," "to some extent," "arguably." If he means it, state it. If he is unsure, say specifically why.

**The exception.** Ordinary economics vocabulary stays when it carries technical meaning: "facilitate," "differentiate," "refine," "robust" (a robust equilibrium, a robustness check), "crucial" (when something genuinely is). His own prose uses "incite," "manufacture," "dissect," and "garbling" well. The test: is the word doing precise work, or is it decoration?

## When editing his drafts

He is the author. Clean, don't rewrite him into someone else.

Do: remove connective tics, split long sentences and dense paragraphs, unstack Turkish-shaped rhythm, replace AI filler, fix grammar and non-idiomatic constructions a native reader would catch, convert em-dashes.

Don't: flatten characterful phrasing, reorder his argument, add flourishes, insert compliments, make it longer, or "elevate" the vocabulary. If a phrase is charming and only slightly non-native, leave it. That is the line between fixing an error and erasing a voice.

Keep changes proportionate. If a draft is already clean, say so and make only the few fixes it needs.

## Live feedback: \nme{...}

When Boğaç writes `\nme{some text}`, the text inside is not his natural English: it came out wrong, felt off, or was AI-generated. Rewrite that phrase in his voice. Never leave `\nme{...}` markers in output. If the pattern reveals a new rule or habit, propose an edit to this skill (this file or the relevant file in `references/`) and tell him what you would add.

## Self-check before returning text

- No em-dashes.
- No reflexive moreover, thus, hence, furthermore. "Therefore" only where the causal link would be missed.
- No AI filler or hype, unless it is real technical vocabulary.
- Contractions match the register.
- No sentence carrying three clauses or three stacked abstractions; no paragraph that runs long.
- No sentence front-loaded with a long clause before the main verb.
- Warmth in the openings, closings, and asides, not in the analytical core.
- No clever closing line the paragraph doesn't need.
- His characterful phrasing survived.
