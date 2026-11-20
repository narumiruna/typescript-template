# Repository instructions

## Changelog

- Use `npm run changeset` to add a changelog entry in `.changeset/` for each
  releasable change instead of editing `CHANGELOG.md` by hand.
- Include the affected package, the appropriate version bump (`patch`,
  `minor`, or `major`), and a summary of what changed for users.
- Never use `--empty` or create an empty changeset by hand; omit the changeset
  if the change does not need a release.

For example, to record a bug fix without opening a prompt:

```sh
npm run changeset -- --patch typescript-template -m "Fix incorrect output for zero."
```
