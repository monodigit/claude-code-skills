# Claude Code Skills

A collection of custom skills for [Claude Code](https://claude.ai/code) — Anthropic's official CLI for Claude.

## Skills

### `/session-handoff`

Manage session transitions when approaching context limits, after completion of a feature or at the end of the day. Documents your progress, updates persistent memory, and generates a ready-to-paste prompt for the next session — so you never lose context when Claude's window resets.

**Features:**
- Auto-detects working directory and suggests smart save paths
- Creates timestamped session summaries
- Updates `MEMORY.md` for persistent context across sessions
- Generates a lean handoff prompt for fast next-session startup
- Optional structured task list generation
- Organizes sessions by month automatically

**When to use:** At ~150K tokens (75% of context limit) or at the end of a significant work session.

## Installation

Clone this repo and copy skills to your Claude Code skills directory:

```bash
git clone https://github.com/Monodigit/claude-code-skills.git
cp -r claude-code-skills/skills/session-handoff ~/.claude/skills/
```

Then invoke in Claude Code:

```
/session-handoff
```

## Usage

```
/session-handoff
```

Or naturally:
- "Document this session for handoff"
- "Prepare for new session"
- "We're approaching context limits, prepare session transition"

## Contributing

Feel free to open issues or PRs. If you've built a useful Claude Code skill, contributions are welcome.

## License

MIT
