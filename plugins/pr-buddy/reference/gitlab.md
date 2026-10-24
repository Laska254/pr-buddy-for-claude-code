# GitLab

Every call goes through `glab api` against gitlab.com. Below, `{p}` is the URL-encoded project path (`group%2Fproject`) and `{iid}` the MR number. Request bodies go in JSON files: `glab api --method POST <path> -H "Content-Type: application/json" --input <file>`.

`glab` has no built-in filter, so pipe its output through `jq`. With `--paginate` it prints one array per page; merge them with `jq -s 'add | …'`.

## Read

- **You:** `glab api user | jq '{username, id}'`.
- **MR of a branch:** here `{q}` is the URL-encoded path of the branch's own remote, from `git remote get-url "$(git config branch.{branch}.remote)"`, and `glab api projects/{q} | jq '{id, parent: .forked_from_project.path_with_namespace}'` gives its id and, for a fork, its parent. Run `glab api "projects/{q}/merge_requests?source_branch={branch}&state=opened" | jq 'map({iid, author: .author.username, source_project_id})'`, with `{branch}` URL-encoded. The source branch name matches in forks too, so keep only the MRs whose `source_project_id` is that id. For a fork, its MRs live in the parent: run the same query with the same filter against the parent too.
- **Metadata:** `glab api projects/{p}/merge_requests/{iid} | jq '{title, description, author: .author.username, state, draft, source_branch, sha, diff_refs}'`. `sha` is the head; `diff_refs` holds `base_sha`, `head_sha`, `start_sha`.
- **Diff:** `glab api "projects/{p}/merge_requests/{iid}/diffs?per_page=100" --paginate | jq -s 'add'`, per file with `old_path`, `new_path`, `new_file`, `deleted_file`, `renamed_file` and the hunk text in `diff`. Line anchors must land in this diff; read the surrounding code from the local clone.
- **Discussions:** threads needing a response (first note resolvable and unresolved), system notes dropped:
  ```
  glab api "projects/{p}/merge_requests/{iid}/discussions?per_page=100" --paginate | jq -s 'add
    | map(select(.notes[0].resolvable and (.notes[0].resolved | not)))
    | map({id, notes: [.notes[] | select(.system | not)
        | {author: .author.username, body, path: .position.new_path, new_line: .position.new_line, old_line: .position.old_line}]})'
  ```
  Drop the first `select` to see every thread, resolved ones included.
- **Draft notes:** `glab api projects/{p}/merge_requests/{iid}/draft_notes | jq 'map({id, note})'`. These are the user's unpublished notes; publishing publishes all of them, so any that exist before you start are leftovers.

## Line anchors

An inline note carries a `position`:

```json
{ "position_type": "text", "base_sha": "…", "head_sha": "…", "start_sha": "…",
  "old_path": "src/Foo.java", "new_path": "src/Foo.java", "new_line": 42 }
```

The three SHAs come from `diff_refs`. Which line fields to set depends on the line: added gets only `new_line`, removed gets only `old_line`, unchanged gets both. A wrong combination or a line outside every hunk is rejected, so that comment goes into the summary note instead, naming the file and line.

## Post a review

1. For each comment, create a draft note: `POST projects/{p}/merge_requests/{iid}/draft_notes` with `{"note": "<comment>", "position": {…}}` (no `position` for a general comment).
2. Publish them all: `POST projects/{p}/merge_requests/{iid}/draft_notes/bulk_publish` with `{"note": "<summary>", "reviewer_state": "<state>"}`.
3. For an approval only, afterwards: `POST projects/{p}/merge_requests/{iid}/approve` with `{"sha": "<head sha>"}`.

The verdict maps as follows: comment is `reviewer_state: reviewed`; request changes is `reviewer_state: requested_changes` (it marks the reviewer state and does not block merging by itself); approve is `reviewed` plus step 3.

Draft notes stay visible only to the user until step 2. When an earlier step fails, list them and, with the user's consent, delete each with `DELETE projects/{p}/merge_requests/{iid}/draft_notes/{id}`.

## Reply to threads

1. For each reply, create a draft note: `{"note": "<reply>", "in_reply_to_discussion_id": "<discussion id>", "resolve_discussion": true|false}`.
2. Publish them all with `bulk_publish` and an empty body `{}`: the author of the MR sets no reviewer state.
