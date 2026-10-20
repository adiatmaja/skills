# skills

My personalised skills for my own Claude Code workflow. Each folder is one skill, and the repo
lives directly in `~/.claude/skills`, so editing a skill and committing it is the whole workflow.

## Skills

| Skill | Use it when | Invoke |
|---|---|---|
| [`finish-session`](finish-session/SKILL.md) | A working session is ending. It stops what the session started, lands its work, updates the records it changed, and reports what is left for you. | `/finish-session`, or say "finish up this session" |

Larger tools live in their own repos:

| Repo | What it is |
|---|---|
| [`harness-review`](https://github.com/adiatmaja/harness-review) | Reviews how you use your AI coding harnesses: KEEP / MODIFY / REMOVE per skill, agent, hook and MCP server, with a token and cost breakdown. Has its own installer, tests and CI. |

## Install

All skills, as your Claude Code skills directory:

```bash
git clone https://github.com/adiatmaja/skills ~/.claude/skills
```

If `~/.claude/skills` already exists, clone elsewhere and copy the folders you want:

```bash
git clone https://github.com/adiatmaja/skills
cp -r skills/finish-session ~/.claude/skills/
```

Other harnesses use other folders (Codex: `~/.agents/skills/`). Claude Code picks up a new skill
in the next session.

## Update

```bash
git -C ~/.claude/skills pull
```

## Layout

```
<skill-name>/
  SKILL.md        # frontmatter (name, description) + the instructions
  ...             # supporting files, only when the skill needs them
```

The `description` field says *when* to use the skill, not what it does; the agent reads it to
decide whether to load the skill. Skills not authored here (third-party or app-synced) are
listed in `.gitignore` so they can sit in the same directory without being committed.

## License

[MIT](LICENSE)
