---
name: build-my-voice
description: Mine your iMessages and build a personalized writing voice skill. Run this once — it creates a skill that writes in your voice.
---

# Build My Voice

This skill mines your iMessage history, analyzes how you actually write, and generates a Claude Code voice skill that captures your writing patterns. When it finishes, you get:

- A ready-to-use voice skill at `~/.claude/skills/<name>-voice/`
- A curated corpus of real writing samples in `references/corpus.md`

**Requirements:** macOS, an iMessage MCP server connected to Claude Code, and Full Disk Access granted to your terminal app.

---

## Stage 1: Permissions & Prerequisites

1. Try calling `list_chats` with limit 5.
2. If it succeeds and returns chat data, tell the user: "iMessage access is working. Found your chats — moving to mining." Skip to Stage 2.
3. If it fails or the tool does not exist, tell the user they need the iMessage MCP server. Provide ALL of the following:

**Install the server:**
```
git clone https://github.com/jaredmoskowitz/imessage-mcp.git ~/workspace/imessage-mcp
cd ~/workspace/imessage-mcp
python3.12 -m venv .venv
source .venv/bin/activate
pip install -e .
```

**Add to ~/.claude/settings.json** (tell user to replace `<YOUR_HOME>` with their home directory path):
```json
{
  "mcpServers": {
    "imessage": {
      "command": "<YOUR_HOME>/workspace/imessage-mcp/.venv/bin/python",
      "args": ["-m", "imessage_mcp"]
    }
  }
}
```

**Grant Full Disk Access:**
1. Open System Settings → Privacy & Security → Full Disk Access
2. Click the + button
3. Add your terminal app (Terminal, iTerm2, Ghostty, Warp — whichever runs Claude Code)
4. Close and reopen your terminal

Tell the user: "After completing these steps, restart Claude Code and run /build-my-voice again."

4. STOP here. Do not proceed to Stage 2 until iMessage access is confirmed working.

---

## Stage 2: Mining

Do this work silently — do not dump raw messages to the user. Just collect and organize.

1. Call `list_chats` with limit 25. Note which are 1:1 conversations vs group chats (group chats typically have multiple participants or a group name).

2. Calculate the date 3 months before today in ISO format (YYYY-MM-DD). For each of the top 10-15 most active chats, call `get_messages` with limit 100 and `since` set to that date. From the results, keep only messages where sender is "Me". Store these as your raw corpus.

3. Run `search_messages` for each of these signal-rich phrases (limit 30 each):
   - "I think"
   - "I believe"
   - "honestly"
   - "I'm down"
   - "not gonna lie"
   From each set of results, keep only messages where sender is "Me".

4. Bucket all collected messages by length:
   - **Ultra-short (1-5 words):** acknowledgments, reactions, one-word replies
   - **Medium (1-2 sentences):** coordination, quick thoughts, responses
   - **Long (3+ sentences):** stories, assessments, substantive messages

5. For each message, track whether it came from a 1:1 or group chat.

6. Proceed directly to Stage 3 with the collected data.

---

## Stage 3: Analysis

Analyze the collected messages across these dimensions. For each dimension, pull 2-4 real examples from the mined messages.

1. **Default acknowledgment style** — What are the top 5-8 most common acknowledgments? (e.g., "bet", "cool", "sounds good", "gotcha")
2. **Enthusiasm markers** — Extended vowels ("niiice")? Emoji patterns? Caps usage? Specific hype phrases?
3. **Signal-carrying register** — In medium/long messages, what sentence structures dominate? How do they open? Do they lead with context or jump to the point?
4. **Self-assessment style** — How do they talk about their own work, ideas, or performance?
5. **Humor patterns** — Dry? Playful? Absurdist? Rare or frequent? Self-deprecating?
6. **Punctuation and formatting** — Dropped periods? Ellipses? Multi-message bursts vs composed paragraphs? All lowercase?
7. **Group chat vs 1:1 differences** — Do they perform differently for an audience vs in private?

Present findings to the user in this format:

"Here's what I found in your messages. Let me know if this sounds right or if I'm off on anything:

**Your acknowledgment style:** [summary with examples]
**How you show enthusiasm:** [summary with examples]
**Your signal-carrying register:** [summary with examples]
**Self-assessment style:** [summary with examples]
**Humor patterns:** [summary with examples]
**Punctuation and formatting:** [summary with examples]
**Group vs 1:1 behavior:** [summary with examples]"

Ask: "Does this sound like you? Anything I'm getting wrong?"

WAIT for the user's response. Incorporate their feedback before proceeding to Stage 4.

---

## Stage 4: Generation

Generate two files based on the analysis. Before writing any samples, apply a privacy pass: strip phone numbers, replace contact names with first names only or generic labels ("a friend", "coworker"), and exclude clearly private or sensitive content.

### File 1: SKILL.md

Generate a complete voice skill with this structure:

**Frontmatter:**
```yaml
---
name: <name>-voice
description: Write in <Name>'s voice across any surface — texts, emails, Slack, docs, and more.
---
```

**Sections to include:**

1. **"# <Name>'s Voice"** — One-line description of what this skill does.

2. **"## How to use this skill"** — A 5-step process:
   - Identify the surface (text, email, Slack, etc.)
   - Pull the invariants (cross-context rules that always apply)
   - Layer the context-specific rules for that surface
   - Run the before-sending checklist
   - Consult the corpus for tone calibration

3. **"## Cross-context invariants"** — 5-8 rules derived from the analysis. Each rule must be specific and actionable, not vague. Back each with a real example. Example format: "Never open with 'Hey!' — default opener is jumping straight to the point. Example: 'yo did you see the thing about...'"

4. **"## Context-specific voice"** — Start with a fully populated section:
   - **"### Text messages (iMessage)"** — 8-12 detailed rules drawn directly from the mining. Cover: acknowledgments, enthusiasm, question-asking style, how they say no, how they make plans, how they share news, punctuation patterns, message length patterns.

   Then add stub sections for each of these surfaces:
   - Slack DMs
   - Slack public posts
   - Short emails
   - Long-form emails
   - LinkedIn posts
   - Technical docs
   - PR descriptions

   Each stub says: "Not yet populated. Add samples to references/corpus.md under the [Surface] heading, identify patterns, then fill in rules here."

5. **"## Phrases and patterns you actually use"** — Organized by function:
   - Acknowledgments
   - Enthusiasm
   - Staking a position
   - Admitting limits
   - Plus any other categories that emerged from analysis

   Each entry includes the phrase AND when/how it's used.

6. **"## Phrases and patterns to avoid"** — Standard anti-patterns:
   - Corporate filler ("per my last email", "circle back", "synergize")
   - Preamble padding ("I hope this message finds you well")
   - Apology pile-ups ("Sorry, but I just wanted to...")
   - Overly hedged claims ("I could be wrong, but maybe possibly...")
   - Plus anything the user specifically flagged

7. **"## Before sending checklist"** — 8 items. Example items: "Does this open the way I actually open messages?", "Would I actually use this emoji?", "Is this the right length for how I'd handle this topic?", etc. Derive from the real patterns found.

8. **"## Corpus and updating"** — Point to `references/corpus.md` with instructions: "To expand this skill to new surfaces, add 8-12 real samples to the corpus under the appropriate heading, analyze the patterns, then add rules to the Context-specific voice section above."

### File 2: references/corpus.md

Generate a corpus file with this structure:

1. **"# <Name>'s Writing Corpus"** — Brief explanation of what this file is for.

2. **"## iMessage / Texting"** — 8-12 curated samples from the mining. Each sample formatted as:
   ```
   ### [Descriptive Title]
   *Distinctive:* [one line explaining why this sample matters]

   ```
   [verbatim message(s) in a code block]
   ```

   **Patterns to notice:**
   - [2-4 specific pattern bullets]
   ```

3. **Stub sections** for: Slack, Email, LinkedIn, Technical Writing, PR Descriptions — each empty with the instruction: "Add 8-12 representative samples following the format above."

4. **"## Cross-corpus observations"** — Seed with 3-5 observations from the analysis about patterns that span contexts.

5. **"## How to add to this corpus"** — A 6-step process:
   1. Collect 15-20 raw samples from the surface
   2. Filter to 8-12 that show range (short/medium/long, casual/serious)
   3. For each, write a one-line "Distinctive" note
   4. Add 2-4 "Patterns to notice" bullets
   5. Update Cross-corpus observations if new patterns emerge
   6. Add or update rules in SKILL.md's Context-specific voice section

---

## Stage 5: Output

1. Ask the user: "What do you want your voice skill named? Default is `<firstname>-voice`." Wait for their answer.

2. Create the directories:
   ```
   mkdir -p ~/.claude/skills/<name>-voice/references
   ```

3. Write `~/.claude/skills/<name>-voice/SKILL.md` with the generated content from Stage 4.

4. Write `~/.claude/skills/<name>-voice/references/corpus.md` with the generated corpus from Stage 4.

5. Tell the user:
   - "Your voice skill is installed at `~/.claude/skills/<name>-voice/`"
   - "Invoke it by telling Claude to use `/<name>-voice` when writing something for you."
   - Suggest a test: "Try asking Claude to draft a text to a friend about weekend plans using `/<name>-voice`."

6. Mention: "Right now this skill knows your texting voice. To expand to email, Slack, or other surfaces, add samples to `~/.claude/skills/<name>-voice/references/corpus.md` under the appropriate heading, then ask Claude to analyze them and update the skill."
