# Guide for repository leaders

Every Willuhn Group repository has **one leader**. This guide covers what that
role involves. The general rules are in [LAB_CONVENTIONS.md](LAB_CONVENTIONS.md);
how others contribute is in [CONTRIBUTING.md](CONTRIBUTING.md).

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
3. Add `CLAUDE.md` from the project template.
4. Create the project team and give it **Write** access (next section).

## 2. Managing your team

- **Create the team:** org → **Teams → New team**, named `team-<project>`.
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

## 3. Reviewing pull requests

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

## 4. Your own changes

You may push directly to `main`; this keeps small fixes quick. For anything
bigger than a small fix:

- Work on a branch, as everyone else does, so `main` keeps working while you build.
- Consider opening a PR and asking a colleague to look. Nobody checks your work
  otherwise, and a second pair of eyes catches what you can't see.

## 5. Releases

A release is a named snapshot (`v0.2.0`) that project repos can rely on.
Release when a project needs a new feature or fix, when useful changes have
built up, or **immediately** after fixing a bug that affected results.

Choosing the number and publishing a release are described in
[CONTRIBUTING.md, section 6](CONTRIBUTING.md#6-releases-versions).

## 6. Going public at publication (project repos)

When the paper is accepted (or at submission, if the journal requires code
access; agree with the PI):

1. **Clean up:** README complete (pipeline, requirements with exact versions,
   how to run), no absolute paths, no data, `CLAUDE.md` up to date.
2. Add **`CITATION.cff`** with the authors and paper.
3. Ask an **org owner** to change the repo to **Public** (only owners can).
4. Connect the repo to **[Zenodo](https://zenodo.org)** (log in with GitHub,
   switch the repo on), then publish a release `v1.0.0`. Zenodo archives it and
   gives you a **DOI**.
5. Add the DOI to the README, `CITATION.cff` and the paper.
6. Add the repo to the *Project repositories* table on the lab's GitHub landing page.

## 7. Keeping things up to date

- **README:** status, requirements and versions.
- **`CLAUDE.md`:** remove items from *Known issues* once they are fixed; update
  the pinned dependencies when you upgrade.
- **Open PRs and branches:** every few months, close abandoned PRs and delete
  merged or dead branches.

## 8. Handing over or leaving the lab

Before you leave, or if you step down as leader:

1. Agree on a **new leader** with the PI and update the README and `CLAUDE.md`.
2. Make sure **all your code is pushed** and nothing important lives only on a
   branch or on your computer.
3. Merge or close your open PRs.
4. An org owner gives the new leader **Admin** on the repo, and later removes
   you from the org when you leave.

Once someone is removed from the org, their copies (forks) of private lab repos
are deleted automatically, so step 2 matters.

---

> **Note (free GitHub plan):** branch rules are not enforced on private repos,
> so on project repos "only the leader pushes to `main`" is a lab agreement,
> not something GitHub blocks.
