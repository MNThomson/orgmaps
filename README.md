# OrGMaps

[Organic Maps](https://github.com/organicmaps/organicmaps) with Google routing, Google search, and a Garmin watch companion app. Android only. <img src="https://img.shields.io/badge/Claude%20Code-100%25-white.svg?style=for-the-badge&labelColor=%23D97757&logo=claudecode&logoColor=white" alt="Claude Code 100%" height="14">

## Patches

Patches (`patches/`) applies onto the upstream release tag pinned in `upstream.tag`.

**0001** `[routing]`: Google Routes API router, with traffic colouring and alternative routes. The returned polyline is re-derived from the OSM road graph for rich street names and lane guidance.

**0002** `[search]`: Google Places autocomplete behind a search bar toggle, details rendered into the place page.

**0003** `[garmin]`: Garmin watch app mirroring turn-by-turn routing.

## Setup

```console
$ task init
```

Also needs Android Studio (its bundled JBR is `JAVA_HOME`) and the SDK + NDK at `~/Library/Android/sdk`.

For the watch app: `brew install --cask connectiq connectiq-sdk-manager`, then install the Forerunner 245 device definitions through the GUI SdkManager (the cask ships none and `monkeyc` fails without them).

## Develop

Code lives in `.work/src`, which is gitignored and disposable (reset and re-patched on every bump). Edit and commit there, then `task save`. Anything not saved is lost.

```console
$ task edit                    # open .work/src in $EDITOR
$ task patch                   # reset .work/src to the pinned tag, replay patches/
$ task save                    # commits in .work/src -> patches/
```

Android tasks are grouped under `build`, `test`, `install` (connected phone) and `emulator`; `task --list` for all of them. Each group's debug variant is aliased to the bare verb, so `task build` is `task build:debug`.

```console
$ task build:debug             # -> dist/organicmaps.apk   (also: :beta, :release)
$ task test:unit               # app + Garmin JVM tests
$ task install:debug           # phone: build + install + launch    (also: install:beta)
$ task emulator:debug          # AVD:   boot + build + install      (also: emulator:beta)
$ task emulator:start          # just boot the AVD                  (also: emulator:stop)
```

Squash into topical commits before saving, `[subsystem] Summary` per Organic Maps convention. Fewer patches means fewer conflicts to resolve on a bump.

Organic Maps' own code style and architecture notes are in `.work/src/CLAUDE.md`, once the checkout exists.

## Update upstream

```console
$ task upstream:status                     # pinned vs latest
$ task rebase                              # bump to latest + replay
$ task rebase TAG=2026.07.23-6-android     # or pin one
```

Conflicts stop in `.work/src` (fix: `git am --continue`, `task save`)

## Licence

Apache 2.0 (matching upstream). The patches contain portions of Organic Maps source, copyright the [Organic Maps Project](https://organicmaps.app) and its contributors. This is an unofficial personal fork, not affiliated with or endorsed by the [Organic Maps Project](https://organicmaps.app).
