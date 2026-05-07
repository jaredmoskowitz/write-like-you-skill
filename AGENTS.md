# AGENTS.md

Guidance for AI coding agents (Codex, Cursor, Claude Code, etc.) working in this repo.

## What this is

A single Claude Code skill, [SKILL.md](SKILL.md), that mines a user's iMessages and generates a personalized voice skill for Claude Code. There is no runtime code — the skill itself is markdown executed by Claude Code, supported by templates the skill writes into the user's `~/.claude/skills/` directory.

## Repo layout

```
SKILL.md                    # The skill itself — entry point: /build-my-voice
templates/
  SKILL.md.template         # Template for the generated voice skill
  corpus.md.template        # Template for the generated corpus
examples/
  README.md                 # (currently just a README)
README.md                   # User-facing setup + usage
LICENSE not present (project is MIT-style; consult Jared)
```

## Hard rules

- **macOS only.** The skill reads iMessage data from `~/Library/Messages/chat.db` via the iMessage MCP server. Don't add cross-platform branches.
- **Privacy is the top constraint.** When generating output:
  - Strip phone numbers from any sample that survives into output.
  - Reduce contact names to first names or generic labels (`a friend`, `coworker`).
  - Exclude clearly private content categories: medical, financial, intimate, anything the user flags.
  - Never write raw mined data to disk inside this repo. Mining results go to `~/.claude/skills/<name>-voice/` only.
- **No data leaves the machine.** This skill must not introduce network calls, telemetry, or remote logging anywhere — including in the generated voice skill.
- **The generated skill lives outside this repo.** `~/.claude/skills/<name>-voice/` is the install destination. Don't commit generated voice skills, mined corpora, or example output that contains real messages.

## The five stages

[SKILL.md](SKILL.md) defines a strict sequence. Don't reorder it casually:

1. **Permissions check** — verify iMessage MCP is connected and Full Disk Access granted. STOP if not.
2. **Mining** — pull from top 10–15 active chats over the last 3 months, plus signal-rich phrase searches. Filter to messages where sender is "Me".
3. **Analysis** — bucket by length, identify patterns across 7 dimensions (acknowledgments, enthusiasm, register, self-assessment, humor, punctuation, group-vs-1:1).
4. **Review** — present findings, ask "does this sound like you?", incorporate feedback BEFORE generating.
5. **Generation + Output** — apply privacy pass, write `SKILL.md` and `references/corpus.md` to `~/.claude/skills/<name>-voice/`.

## When you change something

- Editing [SKILL.md](SKILL.md) (the build skill itself)? Make sure the stage numbers and dependencies stay coherent. The skill is a state machine — don't break it.
- Editing [templates/SKILL.md.template](templates/SKILL.md.template) or [templates/corpus.md.template](templates/corpus.md.template)? The structure must match what `SKILL.md` Stage 4 says it generates (frontmatter, section headings, stubs for non-iMessage surfaces). If you change the template structure, update Stage 4 in `SKILL.md` to match.
- Adding a new surface (new communication context)?
  - Add a stub heading to `templates/corpus.md.template` and `templates/SKILL.md.template`.
  - Update the surface list in `SKILL.md` Stage 4 ("Then add stub sections for each of these surfaces").
  - Update the README's "Expanding to other surfaces" section if the workflow changes.

## Style of generated skills

The generated voice skill must produce writing that sounds like a person, not Claude. Anti-patterns to keep out of templates:

- Corporate filler ("per my last email", "circle back", "synergize")
- Preamble padding ("I hope this message finds you well")
- Apology pile-ups ("Sorry, but I just wanted to...")
- Overly hedged claims ("I could be wrong, but maybe possibly...")
- AI tells: em dashes used as a tic, "It's worth noting", "I'd be happy to..."

## Testing

There's no automated test suite. To validate changes:

1. Run `/build-my-voice` end-to-end on real iMessage data.
2. Inspect the generated `~/.claude/skills/<name>-voice/SKILL.md` and `references/corpus.md` for the privacy pass (no phone numbers, no full names of private contacts).
3. Generate a sample (e.g. "draft a text to a friend") with the new voice skill and check it doesn't sound like default Claude output.
