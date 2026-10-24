# pr-buddy

A Claude Code plugin for code review on GitHub and GitLab (gitlab.com):

- `/pr-buddy:review` reviews someone else's pull/merge request and posts the review under your name.
- `/pr-buddy:respond` works through the review threads on your own pull/merge request: triage, discussion, local edits,
  then replies.
- `/pr-buddy:check` reviews your own branch or pull/merge request and lists the problems in the session and in
  `.review/` in the clone, posting nothing. It reads the working tree, uncommitted changes included; `--committed`
  limits it to HEAD. On the default branch with no PR/MR (`main`), it reviews only the uncommitted changes and
  suggests moving them to a branch of their own. The report holds only open findings, and `check` offers to delete it
  once the PR/MR is merged or closed or the branch is gone; the points you talk it out of go into
  `.review/waivers.md`, shared by every branch, and come back when the code they rely on changes. Git must ignore
  `.review/`; the first run hands you the command, for bash and for PowerShell, best done once for the whole machine.
- `/pr-buddy:learn` learns from your past review comments: your voice (`/pr-buddy:learn-voice`) and the rules you keep
  enforcing (`/pr-buddy:learn-guidelines`). Run it once before the first review.

| Command   | Whose remarks                  | Edits code          | Posts             |
|:----------|:-------------------------------|:--------------------|:------------------|
| `review`  | yours, on someone else's PR/MR | no                  | yes, after `send` |
| `respond` | the reviewers', on your PR/MR  | yes, once you agree | yes, after `send` |
| `check`   | pr-buddy's, on your own code   | no                  | never             |

On your own PR/MR, `respond` handles what the reviewers wrote and `check` catches what they missed.

Nothing is posted until you have seen the full payload and answered with `send` (or `wyślij`). Each post is a single
batch: one review, one notification.

`review`, `respond` and `check` work on a local clone of the repository. They never check out, fetch, pull, commit or
push; they hand you those commands.

## Requirements

- Claude Code 2.1.271 or later (see [Claude Code version](#claude-code-version)).
- At least one platform CLI, logged in: [`gh`](https://cli.github.com/) for GitHub, [
  `glab`](https://gitlab.com/gitlab-org/cli) for GitLab.
- [`jq`](https://jqlang.org/).
- A POSIX shell (Bash; Git Bash on Windows).

## Claude Code version

Older versions of Claude Code refuse to load pr-buddy. Check yours with `claude --version`; `claude doctor` also shows
how it was installed.

To update, close every Claude Code session first, then run the command for your installation in a regular terminal. None
of them needs administrator rights, except `sudo` for the Linux packages.

| Installed with   | Update command                                                |
|:-----------------|:--------------------------------------------------------------|
| Native installer | `claude update`                                               |
| npm              | `npm install -g @anthropic-ai/claude-code@latest` (no `sudo`) |
| Homebrew         | `brew upgrade claude-code` (or `claude-code@latest`)          |
| WinGet           | `winget upgrade Anthropic.ClaudeCode`                         |
| apt              | `sudo apt update && sudo apt upgrade claude-code`             |
| dnf              | `sudo dnf upgrade claude-code`                                |
| apk              | `apk update && apk upgrade claude-code`                       |

Then start Claude Code again: the new version takes effect only in a new session.

## Platforms

When you enable the plugin, Claude Code asks which platforms pr-buddy may use; both are `on` by default. Turn one off to
keep pr-buddy away from it even while its CLI is logged in, for example a work account: it will neither learn from nor
post to that platform.

In the dialog shown right after install, ←/→ may switch tabs instead of changing a value
([a Claude Code bug](https://github.com/anthropics/claude-code/issues/97958)). Confirm it with Enter to keep the
defaults, then change them in `/config` or in `/plugin` → Installed → pr-buddy → Configure options.

## Install

In a Claude Code session:

```
/plugin marketplace add andret2344/claude-skill-pr-buddy
/plugin install pr-buddy@pr-buddy
```

Or from a shell:

```
claude plugin marketplace add andret2344/claude-skill-pr-buddy
claude plugin install pr-buddy@pr-buddy
```

Then run `/pr-buddy:learn` once.

## Updates

Claude Code does not auto-update third-party marketplaces by default. Either turn it on once (`/plugin` → Marketplaces →
pr-buddy → Enable auto-update), or update by hand:

```
/plugin marketplace update pr-buddy
```

or, from a shell, `claude plugin update pr-buddy@pr-buddy`.

## Private files

Kept in `~/.claude/pr-buddy/`, never in this repository:

- `guidelines.md`: your review rules, drafted by `/pr-buddy:learn-guidelines`; the plugin ships none of its own. Reviews
  also follow the rules a repository documents (`CONTRIBUTING.md`, `CLAUDE.md`, `AGENTS.md`, linter configs), and
  where the two disagree, the repository's rule wins.
- `voice.md`: a profile of how you write review comments, built by `/pr-buddy:learn-voice`.

## Contributing

See [CONTRIBUTING.md](CONTRIBUTING.md).

## License

[Apache-2.0](LICENSE) © andret2344. Modified versions must keep the [NOTICE](NOTICE) file and mark the files they
change.
