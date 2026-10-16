# CLAUDE.md — <repo-name>

@../.github/LAB_CONVENTIONS.md

> If the import above did not load (the `.github` repo is not cloned next to this
> one), read https://github.com/Willuhn-Group/.github/blob/main/LAB_CONVENTIONS.md
> before changing anything. Those rules apply here.

## What this repo is
- **Type:** general-purpose (used by several project repos; keep it stable)
- **Purpose:** <one or two lines>
- **Leader:** <name> (reviews every PR into main)
- **Current version:** <vX.Y.Z>

## Layout
<short tree with one line per folder>

## Dependencies
| Dependency | Version | Why |
|---|---|---|
| <repo or toolbox> | <version> | <what it's used for> |

## Rules specific to this repo
- Project repos depend on this code: **do not change function signatures or output
  fields** without flagging it in the PR and a version bump that marks it as
  breaking (MINOR while the repo is at `0.x`, MAJOR from `v1.0.0` on).
- Every public function has a runnable example in `example_use/` using only
  `example_data/`.
- Before finishing a change, run every script in `example_use/` from a clean MATLAB
  path (only this repo and its listed dependencies added).

## Known issues / in progress
- <list>
