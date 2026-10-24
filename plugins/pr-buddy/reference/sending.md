# Sending

Everything posted appears under the user's name and notifies people the moment it lands; deleting it later does not recall the notification. So every send is one batch behind one explicit approval.

1. **Re-check the head.** Fetch the current head SHA from the platform. When it differs from the SHA you drafted against, re-map every line anchor against the new diff, tell the user what moved, and restart this sequence.
2. **Check for leftovers.** A pending review (GitHub) or unpublished draft notes (GitLab) the user already has on this PR/MR would be swept into your batch or block it. When any exist, show them and let the user decide before continuing.
3. **Write the payload.** Put the exact request bodies in files in the session scratchpad (or the system temp directory when there is none). What you show is what you send.
4. **Show everything.** Present to the user: every body verbatim with its location, the verdict, which threads get resolved, and the commands you will run. Full text, never a summary.
5. **Wait for the send word.** Proceed only when the user's reply is `wyślij` or `send`. Treat every other reply ("ok", "git", "looks good", a question, an edit) as not yet approved: apply any edits, then show the payload again from step 4.
6. **Send** with the platform's calls for this batch, in the order the platform reference gives.
7. **Report.** Give the link to what was posted. When a call fails, stop at once: quote the error, state exactly what was posted and what was not, and clean up only with the user's consent.
