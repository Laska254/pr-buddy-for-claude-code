---
name: review
description: Review a pull request or merge request someone else wrote, on GitHub or GitLab, and post the review under the user's name once they approve it. Use when the user shares a PR/MR link or number to review.
---

# Review

Review someone else's PR/MR from a local clone, agree every comment with the user, and post it all as one batch. Shared reference lives in the plugin's `reference/` directory, two levels above this skill's base directory.

Platforms the user enabled for pr-buddy: GitHub `${user_config.github}`, GitLab `${user_config.gitlab}`.

1. **Setup.** Follow [setup](../../reference/setup.md). Then compare `git rev-parse HEAD` with the PR/MR head SHA. When they differ, hand the user `gh pr checkout {n}` or `glab mr checkout {iid}` (after `git status` shows a clean tree) and wait. Done when: platform known, HEAD equals the head SHA, guidelines and voice loaded.

2. **Gather** metadata, the platform diff and every existing discussion, per the platform reference. When the description links an issue (`#12`, `Closes #4`), read it: it states what the change is meant to do. Done when you know the intent, have the whole diff, and know what has already been said.

3. **Review**, relentless. Read every changed file in full at HEAD, beyond the hunks. For every changed or removed signature, grep its callers across the repository. Apply every guideline to every changed file. Check the change against its intent. Build or run tests only with the user's permission, and ask when the result would change a finding. A point already raised in a discussion comes back only as a follow-up, when it is still unaddressed. Done when every hunk is read, every guideline applied, and every changed signature's callers checked.

4. **Draft with the user.** Present the findings numbered. For each: the location (path and diff line, or general), what is wrong and why it matters in your words to the user, and the comment exactly as it would be posted. The user keeps, edits, drops or discusses each one. Propose a verdict (comment, approve, request changes) with your reason; the user picks it. A comment on a line outside the diff goes into the summary body. Done when every finding has the user's decision and the verdict is chosen.

5. **Send.** Follow [sending](../../reference/sending.md) with the platform's "Post a review" section.
