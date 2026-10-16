# Guide for repository leaders

Every Willuhn Group repository has **one leader**. This guide covers what that
role involves. The general rules are in [LAB_CONVENTIONS.md](LAB_CONVENTIONS.md);
how others contribute is in [CONTRIBUTING.md](CONTRIBUTING.md).

> **No need to read everything.** To start, you only need sections 1 to 3:
> create the repo, set up your team, protect `main`. Look up the rest (reviews,
> releases, going public, renaming, handing over) when it comes up.

---

## Your role in one paragraph

You look after `main`. Everything that reaches `main` has been checked by you,
so `main` always works and anyone in the lab can trust it. You review pull
requests, decide when a version is released, keep the README and `CLAUDE.md`
up to date, and hand the repo over properly if you leave. You are **not**
expected to write all the code or to check every line of the science.

---

## 1. Creating a project repo

Project repos are created by their leader. Follow *Starting a new project repo*
in [LAB_CONVENTIONS.md](LAB_CONVENTIONS.md). In short:

1. Org → **New repository**: owner **Willuhn-Group**, visibility **Private**,
   tick **Add a README file**, license **MIT**.
2. Replace the README with the [project README template](PROJECT_README_TEMPLATE.md)
   and fill in the title, description, leader and people.
3. Add `CLAUDE.md` from the [project template](CLAUDE.project-template.md).
4. Create the project team and give it **Write** access (next section).
5. Protect `main` (section 3). Do the same on any general repo you lead.

## 2. Managing your team

- **Create the team:** org → **Teams → New team**, named `team_<project>`.
  Add the project members.
- **Give the team access:** repo → **Settings → Collaborators and teams →
  Add teams** → choose the team → role **Write**.
- **Someone joins or leaves the project:** add them to or remove them from the
  team. Don't change access repo by repo.
- **External collaborators** (not lab members): repo → *Collaborators and teams
  → Add people*, role **Write** or **Read**. Agree on this with the PI first,
  since project repos are private.

Everyone in the lab can already *read* your repo; the team is about who can
*change* it.

## 3. Protecting `main`

The rules are in *Protecting `main`* in [LAB_CONVENTIONS.md](LAB_CONVENTIONS.md).
To set them up (repo admin rights needed, which the leader has):

**CODEOWNERS**
1. In the repo, create the file `.github/CODEOWNERS` (a folder `.github` and,
   inside it, a file named exactly `CODEOWNERS`, **with no extension**: not
   `.md`, not `.txt`). It has one line: `* @your-username` (add a second code
   owner if your team has one). On GitHub: **Add file → Create new file** and
   type `.github/CODEOWNERS` as the name; the `/` creates the folder.
2. Commit it through a PR like any other change.

**Ruleset 1, "main safety"**
1. Repo → **Settings → Rules → Rulesets → New ruleset → New branch ruleset**.
2. Name `main safety`, Enforcement status **Active**.
3. Target branches → **Add target → Include default branch**.
4. Keep **Restrict deletions** and **Block force pushes** ticked. Leave the
   bypass list **empty**.
5. **Create**.

**Ruleset 2, "main review"**
1. New branch ruleset, name `main review`, **Active**, target the default branch.
2. Tick **Require a pull request before merging**: required approvals **1**,
   and tick **Require review from Code Owners**.
3. Bypass list → **Add bypass → Repository admin** → change *Always* to
   **For pull requests only**.
4. **Create**.

**Merging your own PR:** GitHub never lets you approve your own PR. With the
bypass, the merge box shows a checkbox to merge without waiting for the
requirements (bypass rules); tick it and merge. If you are also an org owner,
use the bypass only in the repos you lead.

If GitHub says the feature needs an upgrade, the repo is private and the
organization is still on the free plan: see the plan note in LAB_CONVENTIONS.

If GitHub says *"This ruleset does not target any resources"*, the target
branch is missing: open the ruleset → **Target branches → Add target → Include
default branch** → **Save changes**. Also check that Enforcement status is
**Active**.

### Checklist: is my repo set up correctly?

Go through this once when the repo is created, and again before the lab or a
paper relies on it. Almost everything is under the repo's **Settings**.

**Basics**
- [ ] The name follows the naming rules (lowercase, underscores, acronyms may be uppercase).
- [ ] Visibility (*Settings → General*, at the bottom): general repo **Public**;
      project repo **Private**, or Public once the paper is submitted.
- [ ] `LICENSE` (MIT), `README.md` and `CLAUDE.md` in the root; README and `CLAUDE.md` name the leader.
- [ ] `.gitignore` covers at least `*.asv`, `localPaths.m` and `*.mat` outside `example_data/`.

**Access** (*Settings → Collaborators and teams*)
- [ ] The leader has **Admin** (org owners have it automatically).
- [ ] Project repo: `team_<project>` has **Write**. General repo: `lab_all` has **Write**.
- [ ] No individuals added one by one who should be in the team (except external
      collaborators agreed with the PI).

**Protection of `main`**
- [ ] `.github/CODEOWNERS` exists, **with no extension**, with `* @leader` (plus any other code owners).
- [ ] Every code owner has Write (through the team).
- [ ] *Settings → Rules → Rulesets* shows two **Active** rulesets targeting the default branch:
  - **main safety**: Restrict deletions + Block force pushes; bypass list **empty**.
  - **main review**: Require a pull request (1 approval) + Require review from
    Code Owners; bypass list **Repository admin → For pull requests only**.
- [ ] Quick test: try to edit a file directly on `main` on GitHub. It should
      make you create a branch and a PR instead.

**Branches**
- [ ] `main` is the only permanent branch (no `developer`, `dev` or old personal branches).
- [ ] No merged branches left undeleted.

**Order and versions**
- [ ] Topics are set (family + technique + language).
- [ ] General repo: at least one release, or the README says there is none yet.
- [ ] Submitted project: tag `v1.0.0` for the submitted version.

**Runs from scratch**
- [ ] A fresh clone, with only the listed dependencies on the path, runs the
      `example_use/` script without edits.

## 4. Reviewing pull requests

Contributors open a PR only when their work is **close to a finished, usable
version** and they have gone through the checklist themselves. Your review
checks that the change is **safe to put on `main`**.

**Try to respond within a week.** If you can't, say so in the PR so the author
isn't left waiting.

### How to review on GitHub

1. Open the PR → **Files changed** tab. Read the diff; click a line to comment on it.
2. **Run it** if the change affects results: in GitHub Desktop, switch to the
   PR's branch and run the example script, or the stage of the pipeline it
   changes, on example data.
3. Click **Review changes** and choose:
   - **Approve**: ready to merge.
   - **Request changes**: explain what needs fixing. The author pushes fixes to
     the same branch and the PR updates automatically.
   - **Comment**: questions without a verdict.

### Review checklist

- [ ] The PR description says what it does and how to test it.
- [ ] The example in `example_use/` (or the changed stage) runs from a **fresh clone**.
- [ ] New or changed functions have the **standard header**.
- [ ] **No absolute paths**; paths built with `fullfile`.
- [ ] **No data files** besides small examples in `example_data/`.
- [ ] Renamed functions are updated **everywhere** they are called.
- [ ] **Breaking changes** are flagged, and you agree with them (see below).

### Breaking changes

A change is *breaking* if anyone's existing script would fail or **give different
results**. In general repos, other projects depend on your code, so:

- Accept breaking changes only when they are worth it, and **release a new version**
  afterwards so projects can choose when to update.
- If results change because of a **bug fix**, tell the people using the repo.

### Merging

- Use **Squash and merge**. The commit message should say what changed and why.
- Click **Delete branch** afterwards.

## 5. Your own changes

Your changes go through a PR too, so every change to `main` leaves a record:

- Work on a branch, as everyone else does, so `main` keeps working while you build.
- Open a PR. You can merge it yourself without an approval (your bypass only
  works through PRs).
- When a second code owner or a colleague can look, ask them to review. Nobody
  checks your work otherwise, and a second pair of eyes catches what you can't see.

## 6. Releases

A release is a named snapshot (`v0.2.0`) that project repos can rely on.
Release when a project needs a new feature or fix, when useful changes have
built up, or **immediately** after fixing a bug that affected results.

Choosing the number and publishing a release are described in
[CONTRIBUTING.md, section 6](CONTRIBUTING.md#6-releases-versions).

## 7. Going public at submission (project repos)

A project repo stays private until its paper is submitted. When the paper is
submitted:

1. **Clean up:** README complete (pipeline, requirements with exact versions,
   how to run), no absolute paths, no data, `CLAUDE.md` up to date.
2. Add **`CITATION.cff`** with the authors and paper.
3. Ask an **org owner** to change the repo to **Public** (only owners can).
4. Connect the repo to **[Zenodo](https://zenodo.org)** (log in with GitHub,
   switch the repo on), then publish a release `v1.0.0`. Zenodo archives it and
   gives you a **DOI**.
5. Add the DOI to the README, `CITATION.cff` and the paper.
   Revisions get a new release (`v1.1.0`) and a new DOI version; the final
   paper cites the last one.
6. Add the repo to the *Project repositories* table on the lab's GitHub landing page.

## 8. Keeping things up to date

- **README:** status, requirements and versions.
- **`CLAUDE.md`:** remove items from *Known issues* once they are fixed; update
  the pinned dependencies when you upgrade.
- **Open PRs and branches:** every few months, close abandoned PRs and delete
  merged or dead branches.

### Moving a repo from your account into the organization

1. If the organization already has a fork of it, check the fork first: anything
   that exists only there (commits, branches) must be pushed to your repo before
   you delete the fork. Then delete the fork (*Settings → Danger Zone*).
2. Your repo → **Settings → Danger Zone → Transfer ownership** → choose
   **Willuhn-Group**. Commits, branches, tags, issues and PRs move with it, and
   GitHub redirects the old address.
3. Rename it if needed to follow the naming rules (see below), add the team,
   `CODEOWNERS` and the rulesets, and update your local remote.

### Renaming a repo

Rename early, before the repo has releases and other projects use it. Then:

1. Repo → **Settings → General → Repository name** → new name (following the
   naming rules in LAB_CONVENTIONS).
2. Everyone with a clone updates it:
   `git remote set-url origin https://github.com/Willuhn-Group/<new_name>.git`
   (GitHub Desktop: *Repository → Repository settings → Remote*). Renaming the
   local folder is optional; if you do, update the MATLAB path.
3. Projects that use the repo as a submodule update it once, through a PR:
   ```
   git submodule set-url external/<old_name> https://github.com/Willuhn-Group/<new_name>
   git mv external/<old_name> external/<new_name>     # optional
   ```
   and fix the `addpath` in `projConfig.m` if the folder changed. Other members
   then run `git submodule sync` and `git submodule update --init`.
4. Update the name and links in the README, `CLAUDE.md`, the org landing page
   and the `.github` docs.
5. **Never create a new repo with the old name.** GitHub redirects the old name
   to the new one; a new repo with the old name breaks that redirect.

## 9. Handing over or leaving the lab

Before you leave, or if you step down as leader:

1. Agree on a **new leader** with the PI and update the README, `CLAUDE.md`
   and `.github/CODEOWNERS`.
2. Make sure **all your code is pushed** and nothing important lives only on a
   branch or on your computer.
3. Merge or close your open PRs.
4. An org owner gives the new leader **Admin** on the repo, and later removes
   you from the org when you leave.

Once someone is removed from the org, they lose access to the private lab
repos, and the lab keeps only what was pushed to them, so step 2 matters.

---

> **Note (free GitHub plan):** rulesets and `CODEOWNERS` are enforced only on
> public repos until the organization is upgraded to GitHub Team. On private
> repos, "everything reaches `main` through a PR" is a lab agreement until then.
