# CLAUDE.md

Shared Renovate presets for Budavári Schönherz Stúdió's repositories: `default.json` is the base every repo extends, `weekly.json` and `monthly.json` are the schedule profiles, and `approve-merges.json` merges an approved non-major PR. [README.md](README.md) describes each preset and how a repo extends them.

## Before calling something done

```bash
export pnpm_config_strict_dep_builds=false
pnpm --package=renovate@latest dlx renovate-config-validator --strict --no-global default.json weekly.json monthly.json approve-merges.json
pnpm --package=renovate@latest dlx renovate-config-validator --strict --no-global .github/renovate.json
```

These are the commands `validate.yml` runs with npx; `@latest` validates against the schema the hosted Renovate app runs. Without the export, pnpm refuses the install over Renovate's dependencies' build scripts, which the validator doesn't need.

## Commits and branches

Commit messages and PR titles are plain imperative sentences. PRs are squash-merged: the PR title becomes the commit on `main`, with an empty body.

## Rules

- A preset change reaches every repo on its next Renovate run: repos extend `main`, not a release.
- README's preset table matches the presets: change it together with the preset.
