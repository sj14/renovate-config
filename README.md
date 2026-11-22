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

- Monthly, all minor, patch and digest updates in one branch, merged straight into the default branch when CI passes (no PR).
- Releases are only picked up once they are 7 days old.
- Major updates open a regular PR and are not merged automatically.
- Vulnerability fixes skip the schedule and are merged automatically too.
- Runs `go mod tidy` after Go updates.
