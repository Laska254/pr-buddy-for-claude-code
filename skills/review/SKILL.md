---
name: review
description: Review a pull request or merge request someone else wrote, on GitHub or GitLab, and post the review under the user's name once they approve it. Use when the user shares a PR/MR link or number to review.
---

# Review

Review someone else's PR/MR from a local clone, agree every comment with the user, and post it all as one batch. Shared reference lives in the plugin's `reference/` directory, two levels above this skill's base directory.

Platforms the user enabled for pr-buddy: GitHub `${user_config.github}`, GitLab `${user_config.gitlab}`.

1. **Setup.** Follow [setup](../../reference/setup.md). The PR/MR author must be someone else; when it is the user, the task is the `check` skill. Then compare `git rev-parse HEAD` with the PR/MR head SHA. When they differ, hand the user `gh pr checkout {n}` or `glab mr checkout {iid}` (after `git status` shows a clean tree) and wait. Done when: platform known, HEAD equals the head SHA, guidelines and voice loaded.

2. **Gather** metadata, the platform diff and every existing discussion, per the platform reference. When the description links an issue (`#12`, `Closes #4`), read it: it states what the change is meant to do. Done when you know the intent, have the whole diff, and know what has already been said.

3. **Review**, relentless, per [reviewing](../../reference/reviewing.md), with HEAD as the code under review. A point already raised in a discussion comes back only as a follow-up, when it is still unaddressed. Done when the reviewing pass is done.

4. **Draft with the user.** Present the findings numbered. For each: the location (path and diff line, or general), what is wrong and why it matters in your words to the user, then, on the line right under it, the suggested solution (or that it needs the author's decision, naming the options), and the comment exactly as it would be posted. The comment stays in the user's voice; a question that already carries the solution needs no more. The user keeps, edits, drops or discusses each one. Propose a verdict (comment, approve, request changes) with your reason; the user picks it. A comment on a line outside the diff goes into the summary body. Done when every finding has the user's decision and the verdict is chosen.

5. **Send.** Follow [sending](../../reference/sending.md) with the platform's "Post a review" section.
