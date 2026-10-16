# CLAUDE.md — <repo-name>

@../.github/LAB_CONVENTIONS.md

> If the import above did not load (the `.github` repo is not cloned next to this
> one), read https://github.com/Willuhn-Group/.github/blob/main/LAB_CONVENTIONS.md
> before changing anything. Those rules apply here.

## What this repo is
- **Type:** project repo (citable code for one study)
- **Study:** <short title, model, brain regions, recording technique>
- **Paper status:** <in progress / submitted (tag vX) / published (DOI)>
- **Leader:** <name> (reviews every PR into main)
- **People:** <names>
- **Visibility:** private (lab members only) until the paper is submitted; public from submission on

## Pipeline
1. <stage> — `matlab/<folder>/` — <one line>
2. ...

## Pinned dependencies
| Dependency | Version | Used for |
|---|---|---|
| behavior_medpc | <vX.Y.Z> | behavior parsing |
| utilities_matlab | <vX.Y.Z> | file listing, plotting |
| <external toolbox> | <version> | <use> |

## Configuration
- `matlab/config/projConfig.m` holds all project parameters.
- Data paths come from `localPaths.m` (git-ignored; copy `localPaths.template.m`).
  Never write a server or drive path anywhere else.

## Rules specific to this repo
- **Do not edit or copy functions from general repos here.** If one needs a fix,
  stop and say so: the fix goes to the general repo first, then the pin is updated.
- Only project-specific helpers go in `matlab/utils/`.
- If a release tag exists for a submitted paper, do not change code on that tag's
  history; work on a branch toward the next version.

## Known issues / in progress
- <list>
