# Skills

Agent skills for Claude Code and Cursor.

| Skill | What it does |
| --- | --- |
| [reply-like-me](plugins/mjward/skills/reply-like-me/README.md) | Learns how you write from your chat, email and docs, then drafts your replies in your voice. |

## Install

In Claude Code 2.1.275 or later:

```
/plugin install mjward --marketplace mjward/skills
```

In older versions, add the marketplace first:

```
/plugin marketplace add mjward/skills
/plugin install mjward@mjward-skills
```

In Cursor, clone this repo and link the skill folder into `~/.cursor/skills/`:

```
git clone https://github.com/mjward/skills.git ~/skills
ln -s ~/skills/plugins/mjward/skills/reply-like-me ~/.cursor/skills/reply-like-me
```

Start a new session after installing.
