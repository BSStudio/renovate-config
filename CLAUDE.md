# CLAUDE.md

Shared Renovate presets for Budavári Schönherz Stúdió's repositories: `default.json` is the base every repo extends, `weekly.json` and `monthly.json` are the schedule profiles, and `approve-merges.json` merges an approved non-major PR once the required checks pass (see the README's prerequisites). [README.md](README.md) describes each preset and how a repo extends them.

## Before calling something done

```bash
pnpm_config_strict_dep_builds=false pnpm --package=renovate@$(pnpm view renovate version) dlx renovate-config-validator --strict --no-global default.json weekly.json monthly.json approve-merges.json
pnpm_config_strict_dep_builds=false pnpm --package=renovate@$(pnpm view renovate version) dlx renovate-config-validator --strict --no-global .github/renovate.json
```

These are the commands `validate.yml` runs with npx, against the newest Renovate release, the schema the hosted Renovate app runs. `pnpm view` resolves that release: a plain `@latest` can reuse an older cached install. pnpm 12 can refuse the install over build scripts in Renovate's dependencies, which the validator doesn't need; the variable turns that off.

## Commits and branches

Commit messages and PR titles are plain imperative sentences. PRs are squash-merged: the PR title becomes the commit on `main`, with an empty body.

## Rules

- A preset change reaches every repo on its next Renovate run: repos extend `main`, not a release, and this repo's own `.github/renovate.json` does too. The validator proves the syntax only; a behavior change first shows on each repo's next run.
- README matches the presets: change the preset table, and any section describing the changed behavior, together with the preset.
