# write-like-you-skill

Mine your iMessages and generate a personalized **writing-voice skill**.

The turnkey flow is **[Claude Code](https://claude.ai/code)** (`/build-my-voice`). This repo also ships **`AGENTS.md`** (OpenAI Codex) and **`.cursor/rules/`** (Cursor) so agents stay aligned when editing the skill or extending templates.

Great for when you're kinda faking it anyways — LinkedIn posts, HackerNews comments, and GitHub READMEs.

## What you get

Run `/build-my-voice` once. It reads your iMessage history, figures out how you write, and generates a voice skill that captures your patterns. After that, any time you need to draft something — a text, a LinkedIn post, a Slack message, a README — invoke your voice skill and your assistant writes like you, not like generic AI output.

The iMessage surface is populated automatically. Other surfaces (Slack, email, LinkedIn, tech docs) start as stubs — you add samples over time, and the skill gets sharper.

## How it works

1. **Permissions check** — verifies iMessage MCP access and Full Disk Access
2. **Mining** — pulls your messages from your most active chats over the last 3 months
3. **Analysis** — identifies your acknowledgment style, enthusiasm markers, humor, punctuation habits, sentence structure, and how your register shifts between casual and substantive
4. **Review** — presents findings and asks "does this sound like you?" before generating
5. **Output** — writes a complete voice skill to `~/.claude/skills/` ready to use

## Prerequisites

- **macOS** — iMessage data is local to your Mac
- **Claude Code** — required for the built-in `/build-my-voice` slash-command workflow
- **An iMessage MCP server** — reads your local iMessage database. [imessage-mcp](https://github.com/jaredmoskowitz/imessage-mcp) works, or use any compatible iMessage MCP.
- **Full Disk Access** for your terminal app — the MCP needs to read `~/Library/Messages/chat.db`

## Install

```bash
git clone https://github.com/jaredmoskowitz/write-like-you-skill.git ~/.claude/skills/write-like-you-skill
```

Then in Claude Code:

```
/build-my-voice
```

The skill walks you through everything — iMessage MCP setup, Full Disk Access, mining, and generation.

## After setup

Your voice skill lives at `~/.claude/skills/<your-name>-voice/`. In Claude Code, invoke `/<your-name>-voice` whenever you want drafting in your voice.

### Expanding to other surfaces

The generated skill starts with iMessage data only. To add more surfaces:

1. Paste real writing samples (Slack posts, emails, LinkedIn posts, docs) into `references/corpus.md` under the matching heading
2. Add a "Distinctive:" note and "Patterns to notice:" section for each sample
3. Once you have 3+ samples for a surface, fill in the rules in SKILL.md for that surface
4. When a pattern shows up across 3+ samples, promote it to "Phrases and patterns you actually use"

## Privacy

- Your messages **never leave your machine**. The MCP reads your local iMessage database directly.
- The generated skill files are stored locally in `~/.claude/skills/`.
- Phone numbers are stripped and contact names are reduced automatically during generation.
- Private content (medical, financial, intimate) is excluded from corpus samples.

## Credits

Built by [Jared Moskowitz](https://github.com/jaredmoskowitz). Inspired by building a voice skill the hard way and realizing everyone's iMessages are sitting right there.
