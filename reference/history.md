# History

Collect the user's past review comments: the raw material for learning their voice and their guidelines. Collect once per run, even when both are being learned.

Keep per comment: platform, repository, PR/MR number, **role**, file path (inline comments only), the commented code line where the platform gives it, body, date. The role is **review** when the user commented on someone else's change and **reply** when they answered on their own.

Cover each platform that the skill states as enabled and whose CLI is logged in (`gh auth status`, `glab auth status`); name any platform skipped, and why. Bound the history to the last two years unless the user gives another window.

## GitHub

1. `gh api user --jq .login` gives the login.
2. Pull requests: `gh search prs --reviewed-by=@me --limit 100 --json repository,number,author` (role review; skip those the user authored) and `gh search prs --author=@me --limit 100 --json repository,number` (role reply).
3. The user's comments on each:
   ```
   gh api "repos/{o}/{r}/pulls/{n}/comments?per_page=100" --paginate \
     --jq '.[] | select(.user.login=="<login>") | {path, body, created_at, code: (.diff_hunk | split("\n") | .[-1])}'
   ```

## GitLab

1. `glab api user | jq '{id, username}'` gives the id and username.
2. The user's merge request comments:
   ```
   glab api "users/{id}/events?action=commented&after=<YYYY-MM-DD>&per_page=100" --paginate | jq -s 'add
     | map(select(.note.noteable_type == "MergeRequest")
       | {project_id, iid: .note.noteable_iid, body: .note.body, path: .note.position.new_path,
          line: .note.position.new_line, sha: .note.position.head_sha, created_at})'
   ```
3. Role: for each distinct `project_id` and `iid`, `glab api projects/{project_id}/merge_requests/{iid} | jq .author.username`; the user's own username means reply.
4. GitLab gives no code with a comment. When a comment cannot be understood without it, read the line from `glab api "projects/{project_id}/repository/files/{url-encoded path}/raw?ref={sha}"`.

## Report

Tell the user how many comments were collected per platform and role before learning from them.
