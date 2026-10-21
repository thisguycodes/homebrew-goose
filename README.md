# homebrew-goose

Homebrew tap for builds of [thisguycodes/goose](https://github.com/thisguycodes/goose),
a fork of [goose](https://github.com/aaif-goose/goose).

## Install

```sh
brew tap thisguycodes/goose
brew install thisguycodes/goose/goose
```

The fully-qualified name is required: homebrew-core already ships an unrelated
`goose` formula (a database migration tool).

Upgrade with `brew upgrade thisguycodes/goose/goose`.

## How it works

`Formula/goose.rb` is **generated** — do not edit it by hand. It points at the
release tarballs (`goose-<target-triple>.tar.bz2`) published on the fork's
GitHub releases, for macOS (arm64/x86_64) and Linux (arm64/x86_64, gnu).

To update the tap after cutting a release on the fork:

```sh
script/update-formula          # uses the fork's latest release
script/update-formula v1.2.3   # or a specific tag
git commit -am "goose vX.Y.Z" && git push
```

A scheduled GitHub Action (`.github/workflows/update-formula.yml`) does this
automatically once a day.

## Cutting a release on the fork

The fork inherits upstream's release workflow. Tag a commit like
`v1.55.0-thisguy.1` (must start with `vX.Y.Z`) and push the tag; the workflow
builds and attaches the `goose-<triple>.tar.bz2` assets the formula expects.
