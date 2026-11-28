# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for my repositories.

Use it in a repository's `renovate.json`:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>sj14/renovate-config"]
}
```

What it does:

- Monthly, all minor, patch and digest updates in one branch, merged straight into the default branch once all checks are green (no PR, so no notifications).
- Releases are only picked up once they are 7 days old.
- Major updates open a regular PR and are not merged automatically.
- Vulnerability fixes skip the schedule and are merged automatically too.
- Runs `go mod tidy` after Go updates.

## Branch protection

Renovate pushes the update branch straight to the default branch. If the branch
protection requires a pull request, allow the Renovate app to bypass that rule:

```sh
gh api -X PATCH repos/OWNER/REPO/branches/main/protection/required_pull_request_reviews \
  --input - <<'JSON'
{"bypass_pull_request_allowances": {"apps": ["renovate"]}}
JSON
```

If Renovate can't merge the branch anyway, it opens a PR instead. Renovate then
merges that PR itself (`platformAutomerge: false`) and waits for all checks on it,
not only for required status checks as GitHub's auto-merge would.

## Repositories without CI

Without CI there is nothing to test an update against, so nothing should be merged
automatically. Use the `no-automerge` preset instead; it opens a PR for every update:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>sj14/renovate-config:no-automerge"]
}
```
