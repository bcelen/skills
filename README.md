# Skills

These are AI agent skills I use in my academic work. Each one is a folder with a `SKILL.md` file. They don't depend on any one agent, so they work with Claude Code, Claude.ai, Codex, and any other agent that reads `SKILL.md` folders.

## The skills

| Skill | What it does |
|---|---|
| [`yellowdig`](yellowdig/) | Catches an instructor up on a Yellowdig course community. It reads the discussion through the data API, flags the posts that need a reply, and builds an HTML dashboard with a draft reply for each one. Read-only. See its [README](yellowdig/README.md). |
| [`bogac`](bogac/) | Writes and edits English prose in my own voice: emails, paper prose, referee reports, and letters. It is personal by design, but you can use it as a template for a voice skill of your own. |

## Install

Clone the repository:

```bash
git clone https://github.com/bcelen/skills.git
```

Then copy the skill folder you want to where your agent looks for skills.

**Claude Code**

```bash
cp -r skills/yellowdig ~/.claude/skills/
```

**Codex**

```bash
cp -r skills/yellowdig ~/.agents/skills/
```

**Claude.ai**

Zip the folder and upload the zip in the Skills section of your Claude settings.

```bash
cd skills && zip -r yellowdig.zip yellowdig -x '*.DS_Store'
```

**Other agents**

Point the agent at the skill's `SKILL.md`, or paste its contents into the conversation.

To stay up to date, link the folder instead of copying it, and run `git pull` now and then:

```bash
ln -s "$PWD/skills/yellowdig" ~/.claude/skills/yellowdig
```

## License

[MIT](LICENSE) © 2026 Boğaçhan Çelen
