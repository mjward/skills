# reply-like-me

A skill that writes your messages the way you write them. It studies what you
have written in chat, email and docs, builds a voice profile on your machine,
and uses it whenever you ask your agent to reply to someone.

It writes messages to people: chat replies, DMs, emails and announcements. It
never writes PRs, code, commit messages or technical documents.

## Install

In Claude Code 2.1.275 or later:

```
/plugin install mjward --marketplace mjward/skills
```

In older versions, run `/plugin marketplace add mjward/skills`, then
`/plugin install mjward@mjward-skills`. In Cursor, copy this folder into
`~/.cursor/skills/reply-like-me/`. Start a new session afterwards.

You also need:

- Python 3, for the scripts that count your writing habits.
- At least one connected source. Setup offers only the ones it can reach:

| Kind | Sources |
| --- | --- |
| Chat | Slack, Microsoft Teams, Google Chat, Discord |
| Email | Gmail, Outlook (sent mail only) |
| Docs | Google Docs, Confluence, Notion, Word in OneDrive or SharePoint |
| Files | Slack or Teams exports, text or Markdown files |

Slack and Google Docs have been tested. The others are written but untested,
so expect rough edges.

## Set up your profile

Run this in a local session on your own computer, not a cloud agent:

```
set up reply-like-me
```

It asks which sources to scan, your name, roughly when you started using AI to
help you write (text after that gets stricter checks), and anything to leave
out. Then it:

1. Reads a sample of your messages and docs, about 400 messages per time slice.
2. Drops anything an assistant wrote for you, meeting notes, HR and health
   matters, and other people's private details.
3. Counts your habits: length, punctuation, slang, emoji and reactions.
4. Writes your profile, tests it on three sample drafts, and asks what you
   would change.

Expect 10 to 20 minutes, most of it waiting for the reading to finish.

## What it reads and where it keeps it

Setup reads your own messages through your connectors, so they pass through
your model provider like anything else your agent reads. It only reads, and it
never posts, reacts or edits.

Your profile lives in `~/.config/reply-like-me/` (or `$REPLY_LIKE_ME_DIR`),
outside the skill and outside any repo:

| File | Holds |
| --- | --- |
| `voice.md` | Your voice: rules, a table by situation, and checks |
| `chat-examples.md` | Real messages of yours, with other people's names replaced |
| `docs-examples.md` | How you write at length |
| `stats.md` | The counts behind the rules |
| `corrections.md` | Lessons from edits you made to drafts |
| `drafts/` | Recent drafts, kept until compared with what you sent |
| `raw/` | Collected messages. Delete it once setup is done unless you want faster refreshes |

Never commit the profile or copy it into the skill. Edit any file in it
yourself: it's yours.

## Use it

Ask the way you normally would:

```
reply to Sam in #team-helpline about the failed export
respond to that thread, say we'll fix it tomorrow
draft an email to the vendor asking for the submission date
```

It drafts and shows you the reply. It only sends when you ask it to, and
won't agree to anything you haven't agreed to. When a reply would only say
"seen", "approved" or "thanks", it suggests the reaction you'd use instead.

## It learns from your edits

Each draft is saved in `drafts/`. Next time the skill runs, it finds what you
actually sent, compares the two and records what you changed as a lesson in
`corrections.md`. A lesson seen three times becomes part of `voice.md`. To run
it yourself:

```
learn from my edits
```

## Refresh or remove

- `refresh reply-like-me` reads what you've written since the last run and
  merges it in.
- To remove everything, delete `~/.config/reply-like-me/` and the skill folder.
