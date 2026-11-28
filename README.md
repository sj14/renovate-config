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

- Monthly, all minor, patch and digest updates in one PR, merged automatically once all checks are green.
- Releases are only picked up once they are 7 days old.
- Major updates open a regular PR and are not merged automatically.
- Vulnerability fixes skip the schedule and are merged automatically too.
- Runs `go mod tidy` after Go updates.

## Automerge waits for CI

Renovate merges the PRs itself (`platformAutomerge: false`) instead of handing them
to GitHub's auto-merge. Renovate only merges when all checks on the PR are green, so
no required status checks are needed in the branch protection. GitHub's auto-merge
would only wait for required checks and merge right away without them. The PR is
merged on the next Renovate run after CI has passed.

## Repositories without CI

Without CI there is nothing to test an update against, so nothing should be merged
automatically. Use the `no-automerge` preset instead; it opens a PR for every update:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>sj14/renovate-config:no-automerge"]
}
```
