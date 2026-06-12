# claude-skills

A small pack of practical [Claude](https://claude.com/claude-code) skills I use to run real work. Each one teaches Claude a specific job so you stop re-prompting and start shipping.

A skill is a folder of instructions Claude loads on demand. Drop these into your project and Claude picks them up automatically.

## Skills

| Skill | What it does |
|---|---|
| [`linkedin-post`](skills/linkedin-post/SKILL.md) | Draft or sharpen a LinkedIn post. Combines your own voice with a fresh, high-performing structural mechanic. |
| [`linkedin-lead-magnet`](skills/linkedin-lead-magnet/SKILL.md) | Draft or sharpen a "comment KEYWORD to receive ASSET" lead-magnet post, in your voice. |
| [`build-automation`](skills/build-automation/SKILL.md) | Pick the right runtime for a new automation (inline service vs durable workflow vs heavy orchestrator) and scaffold a known-good starting point. |
| [`design-os`](skills/design-os/SKILL.md) | Build clean, production-grade UI with shadcn/ui and Tailwind, using design-system thinking to avoid generic AI-slop layouts. |
| [`vaughn-wiki`](skills/vaughn-wiki/SKILL.md) | Maintain a structured local markdown knowledge base so decisions and context persist and recall across sessions. |

## Install

Clone the skills you want into your project's `.claude/skills/` directory:

```bash
git clone https://github.com/shreyvaughn/claude-skills.git
cp -r claude-skills/skills/linkedin-post .claude/skills/
```

Or copy any single `SKILL.md` into `.claude/skills/<name>/SKILL.md`. Claude Code loads them automatically the next time it needs them.

## License

MIT. Use them, fork them, make them yours.
