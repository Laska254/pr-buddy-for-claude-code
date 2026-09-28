# Mining guidelines

Turn the user's past review comments into candidate rules for their private guidelines at `~/.claude/pr-buddy/guidelines.md`. Only comments with role review count: they show what the user asks of other people's code.

1. **Group** the comments by the point they make, whatever the wording ("missing `final`" and "`@NotNull final`? Check all places" make one point). Scope each group by the languages of its file paths.
2. **Draft a candidate** from every group of three or more comments: the rule, stated as what the code should do; its scope (a language, a stack, or any); the evidence (comment count, repositories, two verbatim quotes). Mark a group drawn from a single repository as possibly that repository's own convention.
3. **Compare** against the existing guidelines: drop candidates they already cover, and flag any that contradict them.
4. **Decide with the user.** They keep, edit, rescope or drop each candidate. Frequency is evidence, not proof, so nothing enters the guidelines without their decision.
5. **Write** the kept rules into the private file under one heading per scope, adding to what is there. Show the resulting change before saving.
