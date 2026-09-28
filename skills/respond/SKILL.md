---
name: respond
description: Work through review comments on the user's own pull request or merge request on GitHub or GitLab, triaging each thread, discussing or applying changes, then posting replies once the user approves. Use when the user asks to handle, answer or address review feedback on their PR/MR.
---

# Respond

Take the unresolved review threads on the user's own PR/MR to a decision each, apply the agreed changes locally, and post the replies as one batch. Shared reference lives in the plugin's `reference/` directory, two levels above this skill's base directory.

Platforms the user enabled for pr-buddy: GitHub `${user_config.github}`, GitLab `${user_config.gitlab}`.

1. **Setup.** Follow [setup](../../reference/setup.md). The PR/MR author must be the user; when it is someone else, the task is the `review` skill. The current branch must be the source branch, with HEAD equal to or ahead of the remote head. When it is behind, hand the user `git pull` and wait. Done when: platform known, branch and HEAD confirmed, guidelines and voice loaded.

2. **Gather** every unresolved thread with its whole conversation, plus general comments posted after the user's last one, per the platform reference. For each inline thread, read the code it points at as it is now at HEAD. Done when every unresolved thread is listed with its current code.

3. **Triage.** For each thread propose one of: **apply** (the reviewer is right and it is in scope), **discuss** (unclear, arguable, or out of scope), **decline** (the reviewer is wrong, with the reason), judged against the code and the guidelines. A thread whose last word is the user's is **waiting** on the reviewer and needs nothing unless the user says so. Present all threads; the user decides each. Done when every thread has the user's decision.

4. **Discuss** each "discuss" thread with the user until it becomes apply or decline. The code stays untouched meanwhile.

5. **Apply.** Edit the files for every "apply" thread; edits only. Then hand over: which change answers which thread. The user commits and pushes; wait until they say it is pushed. Done when the platform's head SHA equals the local HEAD and that HEAD contains the edits.

6. **Draft replies**, one per thread, in the user's voice. Applied: a short reply naming the pushed commit's short SHA, resolved. Declined: the reasoned reply, left open for the reviewer to close. Discussed: whichever of the two it became. The user approves or edits each reply and each resolve.

7. **Send.** Follow [sending](../../reference/sending.md) with the platform's "Reply to threads" section.
