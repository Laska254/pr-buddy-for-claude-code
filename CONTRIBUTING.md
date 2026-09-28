# Contributing

Issues and pull requests are welcome.

## Layout

- `.claude-plugin/`: the repository is both the marketplace (`marketplace.json`, entry source `.`) and the plugin (`plugin.json`).
- `skills/<name>/SKILL.md`: one file per command, holding the steps.
- `reference/`: material the skills point at: platform APIs, the sending gate, voice, history, guideline mining.

## Ground rules

- **Nothing user-specific.** Voice profiles, guidelines and comment samples live in each user's `~/.claude/pr-buddy/`, never in this repository. The plugin ships no review rules of its own.
- **Posting stays behind the gate** in `reference/sending.md`: the full payload shown, an explicit send word, one batch.
- **Repository state belongs to the user.** Skills run read-only git and hand every state-changing command to the user.
- **`${user_config.*}` is substituted only in skill files**, never in `reference/`, so any line that needs a config value belongs in a `SKILL.md`.

## Testing

1. Run `claude plugin validate .` after every change to the manifests or skills. CI runs it on every push and pull request, together with a check of the relative Markdown links between skills and references.
2. Load your working copy: `claude plugin marketplace add <path to your clone>`, then `claude plugin install pr-buddy@pr-buddy`. A directory marketplace loads the files in place, so `/reload-plugins` picks up edits.
3. Try anything that posts against private state first: a pending GitHub review and GitLab draft notes are visible only to you until submitted. Delete them afterwards.

## Releasing

Users receive a new copy only when `version` in `.claude-plugin/plugin.json` changes; commits pushed without a bump never reach them. Every change meant for users ships with a version bump, following semver. Keep `version` in `plugin.json` only, never in the marketplace entry.

## License

By contributing, you agree that your contributions are licensed under [Apache-2.0](LICENSE).
