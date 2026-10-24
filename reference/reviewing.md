# Reviewing

The review pass shared by `review` and `check`. The skill names the code under review and adds its own rules on top.

- Read every changed file in full, beyond the hunks.
- For every changed or removed signature, grep its callers across the repository.
- Apply every guideline from [setup](setup.md) to every changed file.
- When the intent is known (the PR/MR description, a linked issue), check the change against it.
- Build or run tests only with the user's permission, and ask when the result would change a finding.

Done when every hunk is read, every guideline applied, and every changed signature's callers checked.
