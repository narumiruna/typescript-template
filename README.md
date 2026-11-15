# TypeScript Template

## Human workflow

Install [prek](https://prek.j178.dev/installation/), then install dependencies and enable the Git hooks. prek manages the Biome and zizmor hook environments:

```sh
npm install
prek install
npm run build
npm start
```

For an existing Husky checkout, run `git config --local --unset core.hooksPath` before `prek install`.

## Automation

```sh
npm run ci
```

The CI command runs Biome, Vitest, and the TypeScript build.

[`prek.toml`](prek.toml) uses the official [Biome hooks](https://github.com/biomejs/pre-commit) to format, organize imports, lint, and apply safe fixes to staged files before each commit. It also uses the official [zizmor hook](https://github.com/zizmorcore/zizmor-pre-commit) with `--pedantic` to audit supported staged YAML files. If a hook changes files, review and stage the changes before retrying the commit.

Run all hooks manually:

```sh
prek run --all-files
```

Keep the Biome hook revision aligned with `@biomejs/biome` in `package.json` and the schema version in `biome.json`. When updating Biome, migrate its configuration with `npm exec -- biome migrate --write`.

GitHub Actions use commit-pinned actions and job-scoped permissions. CI cancels superseded runs. Dependabot waits at least seven days before proposing new dependency versions; security updates are not delayed.

Audit the GitHub Actions and Dependabot configuration:

```sh
prek run zizmor --all-files
```

## Release workflow

In `narumiruna/typescript-template`, the workflow starts, but the release job is skipped. Projects created from this template run the job without changes to the workflow condition. Before publishing, update the package name and repository URL in `package.json`.

Create and commit a changeset with each releasable change:

```sh
npm run changeset
```

When changesets reach `main`, [`.github/workflows/release.yml`](.github/workflows/release.yml) opens or updates a version pull request. Merging that pull request publishes the package, creates its Git tag, and creates a GitHub release.

Release runs are serialized per branch and do not cancel an active release.

Publishing uses npm trusted publishing. Configure the npm package's trusted publisher with this GitHub repository and the `release.yml` workflow. Also allow GitHub Actions to create pull requests in the repository's Actions settings.
