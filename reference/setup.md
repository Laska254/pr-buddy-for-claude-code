# Setup

The shared start of `review` and `respond`.

## Platform

Pick the platform from the link's host: `github.com` means [GitHub](github.md), `gitlab.com` means [GitLab](gitlab.md). Given only a number, read the host from `git remote get-url origin`. Any other host is unsupported: say so and stop.

Then confirm the platform is usable:

1. **Enabled.** The skill states which platforms the user enabled for pr-buddy. For a disabled one, tell the user it is turned off in `/config` and stop.
2. **Logged in.** `gh auth status` or `glab auth status` succeeds. Otherwise tell the user to install and log in to that CLI, and stop.

## Local checkout

Work always runs against a local clone of the target repository, opened as the current working directory. The code you read, grep and build is that clone; the platform only supplies metadata, the diff and discussions.

The user owns all repository state. You run read-only git (`status`, `log`, `diff`, `show`, `rev-parse`, `remote`, `grep`) freely. Every command that changes state (`checkout`, `switch`, `fetch`, `pull`, `stash`, `commit`, `push`) you hand to the user as text and wait for them to run it.

When the working directory is not a clone of the target repository, tell the user to open the session in one and stop.

## Shell

Run every `gh`, `glab` and `jq` command from the platform references in a POSIX shell (Bash; Git Bash on Windows). Their `jq` filters are full of quotes, pipes and `$`, which PowerShell mangles. On Windows, `jq -r` ends every line with `\r`; pipe it through `tr -d '\r'` before feeding values into further commands.

## Guidelines

Review rules belong to the user and live only in `~/.claude/pr-buddy/guidelines.md`. When the file is missing or empty, judge on general engineering grounds and tell the user the review ran without guidelines, and that `/pr-buddy:learn-guidelines` drafts rules from their past reviews.

## Voice

Every text you post is written in the user's voice, from the profile at `~/.claude/pr-buddy/voice.md`, following the Write section of [voice](voice.md). When the profile is missing, stop and ask the user to run `/pr-buddy:learn` (or `/pr-buddy:learn-voice`) first.

## Language

Talk to the user in the language they use with you. Everything posted to the platform is English.
