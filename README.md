# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for Budavári Schönherz Stúdió repositories.

## Usage

Every repo extends the base and exactly one schedule profile in `.github/renovate.json`, then adds only what is specific to it:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>BSStudio/renovate-config",
    "github>BSStudio/renovate-config:weekly"
  ]
}
```

Repos follow `main`. A change here reaches every repo on its next Renovate run, so CI validates each preset first.

## Presets

| Preset | What it does |
| --- | --- |
| `default` | Labels, Europe/Budapest timezone, security updates at any time, 3 day minimum release age on registries with release timestamps, SHA-pinned actions, Node.js runtime on majors only, monthly lock file maintenance. Regular updates are not merged automatically; non-major GitHub Actions updates and lock file maintenance merge unattended once checks pass. |
| `weekly` | PRs open Friday 18:00 to Saturday 08:00, then nothing changes until the next window. |
| `monthly` | PRs open on the 1st and 2nd of the month, then nothing changes until the next window. |
| `approve-merges` | Approving a non-major PR merges it once the required checks pass. |

Outside the window, open PRs are neither updated nor rebased. Tick the rebase checkbox in a PR, or a checkbox on the Dependency Dashboard, to act on one anyway.

## `approve-merges` prerequisites

GitHub auto-merge waits only for required status checks. Without them it merges on approval even with failing CI. Extend `approve-merges` only when the repo's `main` ruleset (see [`BSStudio/.github`](https://github.com/BSStudio/.github)):

1. requires code owner review,
2. requires the status checks that matter (path-filtered workflows cannot be required),
3. does not dismiss approvals on push,
4. and "Allow auto-merge" is enabled in the repo settings.

In those repos `CODEOWNERS` leaves workflows and lock files without an owner, so the unattended updates do not wait for approval:

```text
# No owner: Renovate merges these once required checks pass.
.github/workflows/
pnpm-lock.yaml
```
