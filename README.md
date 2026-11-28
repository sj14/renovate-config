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

- Monthly, all minor, patch and digest updates in one PR, merged automatically by GitHub's auto-merge.
- Releases are only picked up once they are 7 days old.
- Major updates open a regular PR and are not merged automatically.
- Vulnerability fixes skip the schedule and are merged automatically too.
- Runs `go mod tidy` after Go updates.

## Required status checks

GitHub's auto-merge only waits for the checks that the default branch's protection
lists as required. Without required checks, an update PR is merged right away,
before CI has finished. Mark the CI jobs as required in every repository that uses
this preset, e.g.:

```sh
gh api -X PATCH repos/OWNER/REPO/branches/main/protection/required_status_checks \
  -F strict=false -f 'contexts[]=JOB_NAME'
```

Only require checks that run on every push to a PR. A check that runs only when the
PR is opened or edited is missing after Renovate rebases the PR, which blocks the
merge for good.

## Repositories without CI

Without CI there is nothing to test an update against, so nothing should be merged
automatically. Use the `no-automerge` preset instead; it opens a PR for every update:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["local>sj14/renovate-config:no-automerge"]
}
```
