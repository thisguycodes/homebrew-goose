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

The fork inherits upstream's `release.yml`, which publishes a release when you
push a tag matching `v1.*` (e.g. `v1.55.0-thisguy.1`):

```sh
git -C <fork-checkout> tag v1.55.0-thisguy.1 && git -C <fork-checkout> push origin v1.55.0-thisguy.1
```

Caveats:

- Enable Actions on the fork first (GitHub disables workflows on forks by default).
- Tag builds turn on macOS signing/notarization (`environment: signing`); without
  the upstream signing secrets those jobs will fail. Either add your own signing
  secrets under a `signing` environment, or strip the signing inputs from
  `release.yml`/`bundle-macos.yml` on your fork's default branch before tagging.
- The Linux CLI tarballs (`x86_64`/`aarch64-unknown-linux-gnu`) don't need signing
  and build on any push to `release/*` branches too — handy for a first test.
