# Examples — What Good Output Looks Like

This directory shows annotated examples of what `/build-my-voice` should produce, using fictional patterns. Nothing here is real — the names, messages, and phrases are invented to illustrate the shape of good output without leaking any actual voice data.

---

## Good cross-context invariant vs. bad

Cross-context invariants are rules that hold everywhere, regardless of surface (DM, Slack, email, doc). They describe something true about how this person thinks and communicates — not just how they text.

### Good invariant

> Lead with the point, not with preamble. The first sentence says what the message is about. If you find yourself writing "I wanted to reach out about..." — delete it. Start with the thing.

**Why this works:** It's specific enough to act on. It gives you a concrete anti-pattern to watch for ("I wanted to reach out about...") and a clear corrective (start with the thing). A writer handed this rule knows exactly what to change in a draft. It also applies everywhere — Slack DMs, pitch emails, technical docs — which is the bar for a cross-context invariant.

---

### Bad invariant — too vague

> Be direct and concise.

**Why this fails:** This is advice for everyone. It says nothing about *this* person's version of direct, how short is short enough, what counts as preamble for them, or what the tension is when directness might read as brusque. A writer can't calibrate from this. It's a platitude, not a rule.

---

### Bad invariant — too surface-specific

> Always use extended vowels for enthusiasm (e.g., "Yooooo", "sooooo good").

**Why this fails:** This is real and useful — but it belongs in the iMessage/texting section of the corpus, not in cross-context invariants. Extended vowels would look bizarre in a pitch email or a pull request description. Promoting a surface-specific pattern to a universal rule creates inconsistency and makes the voice feel unnatural in formal contexts.

---

## Good corpus sample vs. bad

Corpus samples are verbatim examples of the person's actual writing (or in this file, fictional stand-ins). They're the raw material a writer studies to absorb tempo, structure, and register.

### Good corpus sample

```
### Reacting to good news

*Distinctive: Leads with an expressive opener, skips pleasantries, matches the energy of the news immediately.*

Yooooo that's incredible. I had no idea that was even in play — when did this happen? We need to celebrate.

**Patterns to notice:**
- Extended vowels ("Yooooo") signal genuine, high excitement — not sarcasm
- No "Congrats!" opener — skips the stock phrase, goes straight to reaction
- Asks a follow-up question immediately; curiosity is the second beat after the reaction
- Ends with a forward-moving suggestion rather than a dead-end statement
```

**Why this works:** It's long enough to show a complete arc — the opener, the body, the close. The "Distinctive" note names the most important thing to imitate. "Patterns to notice" breaks down the mechanics so a writer can replicate them deliberately instead of guessing. Even this one sample teaches you the sequencing: reaction → curiosity → forward motion.

---

### Bad corpus sample

```
### Quick response

ok
```

**Why this fails:** Too short to learn from. A one-word message might be authentic but it doesn't show structure, tempo, or register. You can't tell if "ok" is warm, neutral, or curt — context is everything and there's none here. A sample this short teaches nothing a writer can act on. Minimum useful length is roughly two to four sentences, or an exchange long enough to show how the person enters, develops, and closes a thought.

---

## Good "phrases you actually use" entry vs. bad

The "phrases you actually use" section of SKILL.md is different from corpus samples. It's a curated vocabulary — the specific expressions this person reaches for, with enough context to know when and how.

### Good phrases entry

```
**Enthusiasm / high-positive reaction**
- "Yooooo" — iMessage/Slack DM only; marks a moment of genuine surprise or celebration; would look weird in email
- "That's so good" — all surfaces; said when something exceeded expectations, not just met them; more specific than it sounds
- "I'm obsessed with this" — used for ideas, designs, a piece of writing; signals deep approval, not just liking something
- "This is the one" — signals convergence; use when a decision is being made and this option is clearly right
```

**Why this works:** Each entry has the phrase and a usage note — when to deploy it, what register it lives in, what emotional state it signals. A writer can use this like a dictionary with stage directions. The entries distinguish phrases that look similar ("that's so good" vs. "I'm obsessed with this") by nailing what each one actually means in context.

---

### Bad phrases entry

```
- Cool
- Nice
- Awesome
```

**Why this fails:** These are generic. Everyone says them. This list doesn't tell you anything distinctive about *this* person — it tells you they speak English. Without context for when these words show up, at what intensity, in what medium, they're useless to a writer. If "Nice." (period, lowercase) is actually a dry sign-off this person uses sarcastically, that's interesting — but you'd never know from a bare list.

---

## Common mistakes

1. **Rules too vague to act on.** If the invariant could appear in a writing tips listicle, it's not specific enough. Push until you have something that names what's distinctive about *this* person.

2. **Samples too short.** Single words and sentence fragments don't show enough structure to imitate. Aim for samples long enough to capture at least one complete thought arc.

3. **Confusing surface-specific patterns with universal ones.** Texting vocabulary, emoji habits, and extended vowels belong in the texting section — not in cross-context invariants. Mis-promoting a surface-specific pattern will break the voice in formal contexts.

4. **Not enough contrast.** The corpus and phrases sections should make clear what's *different* about this person, not just what they do. If every phrase could appear in anyone's messages, you haven't found the voice yet. Ask: what would this person *never* say?

5. **Privacy leaks.** Strip phone numbers, last names, and identifying details before adding any sample to the corpus. Reduce third-party names to first name only or a role label ("my manager"). Exclude content about health, relationships, finances, or anything the person would not want stored in a skill file. When in doubt, leave it out.
