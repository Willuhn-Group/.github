# Contributing to Willuhn Group code

Thanks for contributing! This guide covers how to get a change into a lab
repository. The coding rules themselves (naming, headers, paths) are in
[LAB_CONVENTIONS.md](LAB_CONVENTIONS.md).

New to GitHub? Start with [ONBOARDING.md](ONBOARDING.md) (30 minutes, including
a practice pull request). If you get stuck, ask the repo's leader; nobody
expects perfection on the first try. Leading a repo? See [LEADER_GUIDE.md](LEADER_GUIDE.md).

**Every repository has a leader** (named in its README and `CLAUDE.md`). The
leader looks after `main` and checks every change before it is merged.

---

## The short version

1. **Branch** from `main`, one branch per task.
2. **Commit** your progress to that branch as often as you like.
3. When the work is **close to a finished, usable version**, open a **pull request**.
4. The **repo leader** reviews it.
5. **Squash and merge** into `main`. Delete the branch.
6. A **release** (a version tag) is made separately, when it's needed.

The leader is the only person who can push to `main` directly. Everyone else,
however small the change, goes through a pull request.

---

## 1. Create a branch

Name the branch after **what it does**, not after you:

| Good | Avoid |
|---|---|
| `fix-read-medpc-name` | `sergio-dev` |
| `add-lick-microstructure` | `new-stuff` |
| `docs-gettrials-example` | `test2` |

```
git checkout main
git pull
git checkout -b add-lick-microstructure
```

A branch should live days or weeks, not months. Long-lived branches drift away
from `main` and become painful to merge. If your work takes longer, pull `main`
into your branch regularly:

```
git checkout add-lick-microstructure
git pull origin main
```

**Access:** all lab members can see every lab repository. People working on a
repo get write access and branch inside it. External collaborators fork the
repo (public repos only) and open the pull request from their fork.

## 2. Commit

Commit often. Each message says what changed and why, in one line:

- ✅ `Fix read_medpc call in getTrials (function was renamed)`
- ❌ `Update getTrials.m`

Never commit raw data, `localPaths.m`, or files with your own drive paths.

## 3. Open a pull request (PR)

Push your branch whenever you like (it backs up your work):

```
git push -u origin add-lick-microstructure
```

**Open the PR only when the work is close to a finished, usable version**: it
runs, it is documented, and you have gone through the checklist below yourself.
The leader reviews finished work, not work in progress.

- Fill in the PR template: what it does, how to test it, and the checklist.
- Small PRs get reviewed fast. If a PR touches many unrelated things, split it.
- Need input before you're done? Ask the leader directly. If it's easier to show
  the code, open a **Draft** PR with a specific question in its description; the
  leader is not expected to review drafts in full.

## 4. Review

The **repo leader** reviews every PR into `main`.

A review checks that the code is **safe to share**, not every line of the science.

### Review checklist (authors: check it yourself before opening the PR)

- [ ] The example in `example_use/` runs from a **fresh clone** (only this repo and its listed dependencies on the MATLAB path).
- [ ] Every new or changed function has the **standard header** (inputs, outputs, example, version).
- [ ] **No absolute paths** (`M:\`, `\\vs03\`, `C:\Users\...`); paths built with `fullfile`.
- [ ] **No breaking changes** to existing functions (inputs, output fields), or, if there are, they are on purpose and flagged in the PR.
- [ ] Names follow the conventions, and renamed functions are updated **everywhere** they are called.
- [ ] No data files besides small examples in `example_data/`.

Comment on what needs changing; the author pushes fixes to the same branch and
the PR updates automatically.

**Breaking changes** (anything that would make someone's existing script fail
or give different results) must be flagged in the PR; the leader decides whether
and when they go in.

## 5. Merge

Use **Squash and merge**: all the branch's commits become one clean commit on
`main`. Then delete the branch (GitHub offers a button).

Once merged, the change is on `main` and others can try it. It is **not yet a
release**.

## 6. Releases (versions)

A release is a permanent, named snapshot of the code (a git tag such as
`v0.2.0`). Project repos rely on releases, never on whatever `main` is today.

### Choosing the version number: `MAJOR.MINOR.PATCH`

Ask: *would anyone's existing script behave differently?*

| Change | Example | Bump |
|---|---|---|
| Bug fix, nothing else changes for users | fix a renamed function call | PATCH: `v0.1.0` → `v0.1.1` |
| New function or option; old scripts still work | new option in `eventHistogram` | MINOR: `v0.1.1` → `v0.2.0` |
| Existing scripts would break or give different results | rename an output field of `getTrials` | MAJOR: `v0.2.0` → `v1.0.0` |

While a repo is at `0.x`, the API is still settling, and breaking changes bump
the MINOR number instead (`v0.2.0` → `v0.3.0`). A repo moves to `v1.0.0` once the
lab relies on it and we commit to not breaking it casually.

### When to release

Not after every merge. Release when:
- a project needs to pin a new feature or fix,
- a useful batch of changes has built up, or
- a bug that **affected results** was fixed: release right away and tell the
  people using older versions.

### How to release on GitHub

1. Repo → **Releases** → **Draft a new release**.
2. **Choose a tag** → type the new version (e.g. `v0.2.0`) → *Create new tag on publish*; target `main`.
3. Title = the version. Notes: a few bullets on what changed, with breaking changes listed first.
4. **Publish release**.

## 7. Pinning versions in project repos

General repos publish versions; **project repos record which version they use.**

**In the README** (always), a *Requirements* table:

| Dependency | Version | Link |
|---|---|---|
| medpc-behavior | v0.2.0 | https://github.com/Willuhn-Group/medpc-behavior/releases/tag/v0.2.0 |

**As a git submodule** (recommended), so the exact version comes with the clone:

```
git submodule add https://github.com/Willuhn-Group/medpc-behavior external/medpc-behavior
cd external/medpc-behavior
git checkout v0.2.0
cd ../..
git commit -am "Pin medpc-behavior v0.2.0"
```

- Clone a project with its dependencies: `git clone --recurse-submodules <url>`
- Upgrade later: `cd external/medpc-behavior`, `git fetch --tags`, `git checkout v0.3.0`, then commit in the project repo.
- GitHub Desktop handles submodules too.

Release zips (and Zenodo archives) do **not** include submodule contents, so
the README table is what keeps the published record complete.

---

Questions, or something here doesn't make sense? Open an issue in the
`.github` repo or ask in [Slack channel].
