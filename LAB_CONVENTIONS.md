# Willuhn Group — Code Conventions

*Version 0.4 · October 2026*

This document defines how code is organized, written and shared across the
[Willuhn-Group GitHub](https://github.com/Willuhn-Group). It lives in the
`.github` repository and applies to every repo in the organization. Each repo's
`CLAUDE.md` imports it, so AI coding assistants follow the same rules as people.

Anything here is open to change: propose edits through a pull request to `.github`.

> **No need to read everything.** If you are new, read the first table
> (*Repository types*) and section 5 (*Data*), and remember three rules: never commit data,
> no absolute paths, nothing reaches `main` without a pull request. Look up the
> rest when you need it.

---

## 1. Repository types

| Type | Purpose | Examples | Visibility | Who contributes |
|---|---|---|---|---|
| **General** | Reusable tools anyone in the lab can use | `behavior_medpc`, `utilities_matlab` | **Public** | Everyone (via PR to the leader) |
| **Project** | Analysis pipeline for one study/paper; becomes the citable code of the publication | `analysis_ephys_SIP`, `calcium_rats_gonogo` | **Private** (lab members only) until the paper is submitted; **public** from submission on (under review and after publication) | Project members |
| **Hub** (`.github`) | Lab-wide conventions, contributing guide, org landing page | `.github` | Public (required by GitHub) | Everyone (via PR) |

**Every repository has one leader**, named in its `CLAUDE.md` and README. The
leader is responsible for `main`: they check every change before it is merged.

**Naming:** lowercase, words separated by underscores. Established acronyms
may stay uppercase (`SIP`, `FSCV`, `LFP`).
- General repos: `<family>_<what>`, with the family first so related repos
  sort together in the repository list (`behavior_medpc`, `utilities_matlab`).
- Project repos: after the study (`calcium_rats_gonogo`, `analysis_ephys_SIP`).
- Branch names are different: they use hyphens (`fix-read-medpc-name`), see
  [CONTRIBUTING.md](CONTRIBUTING.md).

**Topics.** Every repo gets GitHub topics (repo page → ⚙ next to *About* →
*Topics*) so people can filter the organization by them. Topics allow only
lowercase letters, numbers and hyphens (no underscores). Use these:

| Kind | Topics |
|---|---|
| Family | `behavior`, `preprocessing`, `analysis`, `utilities` |
| Technique | `ephys`, `fscv`, `photometry`, `calcium-imaging` |
| Language | `matlab`, `python` |

Filter on GitHub with, for example, `org:Willuhn-Group topic:preprocessing`.
Propose new topics through a PR to this file, so the list stays short.

> *Idea for later:* a topic per project (e.g. `sip`) on the project repo and on
> every general repo it uses, so filtering by it shows the whole set. Keep in
> mind the limit of 20 topics per repo, and that topics on public repos are
> visible to anyone. The project README's *Requirements* table stays the
> reference for which versions are used.

### Access and teams

**Everyone can see everything; only the people working on a repo can change it.**

| Who | Access | How it is set |
|---|---|---|
| All lab members | **Read** on every repo, private ones included | Org base permission |
| A project's members | **Write** on that project's repo | One GitHub team per project (e.g. `team_sip`) |
| All lab members, on general repos | **Write**, so anyone can branch and open a PR | Team `lab_all` |
| The repo leader | **Admin** on their repo | Automatic for whoever creates the repo; for general repos (created public by an org owner), an owner gives the leader Admin |
| Org owners (2–3 people, e.g. the PI and the code coordinator) | Everything, including making a repo public | Org settings |

When someone joins or leaves a project, change the **team**, not each repo.

**Lab code lives in the organization.**
- Code written for the lab is created in **Willuhn-Group**, not in a personal account.
- If it already lives in your personal account, **transfer** it to the
  organization (steps in [LEADER_GUIDE.md](LEADER_GUIDE.md)). Don't fork it: a
  fork leaves two copies that drift apart and keeps the original in your account.
- Don't fork lab repos into personal accounts. To show a lab repo on your
  profile, **pin** it (profile → *Customize your pins*); your commits in the
  organization already count on your profile once they reach `main`.

**Starting a new project repo.** Project repos are created **by their leader**.
Every project repo starts with three files:

| File | Content |
|---|---|
| `LICENSE` | MIT, unless agreed otherwise with the PI |
| `README.md` | A brief description of the project, from the [project README template](PROJECT_README_TEMPLATE.md) |
| `CLAUDE.md` | Filled in from the [project `CLAUDE.md` template](CLAUDE.project-template.md) ([general-repo version](CLAUDE.general-template.md)) |

Steps:
1. Org → *New repository*. Owner: **Willuhn-Group**, visibility: **Private**.
   On the same page tick **Add a README file** and choose **License: MIT**:
   GitHub creates both files for you.
2. Replace the README text with the project README template and fill in the
   description (at least the title, the study in two or three lines, the leader
   and the people).
3. Add `CLAUDE.md` from the [project template](CLAUDE.project-template.md) and fill it in.
4. Create the project team (org → *Teams → New team*), add its members, and give
   the team **Write** on the repo (repo → *Settings → Collaborators and teams*).
5. Add the `CODEOWNERS` file and the two rulesets that protect `main` (next section).

### Protecting `main`

Every repo (project and general) protects `main` with a `CODEOWNERS` file and
two rulesets. The step-by-step clicks are in [LEADER_GUIDE.md](LEADER_GUIDE.md).

**`CODEOWNERS`** (file `.github/CODEOWNERS` in the repo, a plain text file with
**no extension**: not `.md`, not `.txt`) names who must approve changes to `main`:
```
* @leader-username
```
The leader is always a code owner. Teams with postdocs or PhD students can add
more code owners (`* @leader-username @second-username`); then any of them can
approve. Interns are never code owners. Every code owner needs **Write** access
to the repo (through the project team), or GitHub ignores their approval. Only
the leader, as repo admin, has the bypass for their own PRs.

**Ruleset 1, "main safety"**, on `main`, with **no bypass for anyone**:
- Restrict deletions
- Block force pushes

**Ruleset 2, "main review"**, on `main`:
- Require a pull request before merging, with **1 approval**
- Require review from **Code Owners**
- Bypass list: **Repository admin** (the leader), set to **For pull requests only**

What this means in practice:

| Who | What happens |
|---|---|
| A team member opens a PR | It needs the code owner's approval to merge |
| A teammate approves another teammate's PR | It doesn't count; only a code owner's approval does |
| The leader opens their own PR | They can merge it without an approval, but it is still a PR, with a record |
| Anyone, leader included, pushes straight to `main` | Blocked; every change goes through a PR |
| Anyone tries to delete or force-push `main` | Blocked |

**Org owners and the bypass.** GitHub treats org owners as admins of *every*
repo, so the bypass works for them everywhere. Lab agreement: **owners use the
bypass only in the repos they lead.** In any other repo, they open a PR and
wait for that repo's code owner like everyone else.

**Org settings** (owners only, org → *Settings → Member privileges*):

| Setting | Value | Why |
|---|---|---|
| Base permissions | **Read** | Every lab member can see and clone every repo |
| Repository creation | **Private** only for members | Leaders create their own repos; making one public is a separate, deliberate step |
| Repository forking | **Off** for private repos | Private code stays in the org; people branch inside the repo instead |
| Repository visibility change | **Owners only** | Going public at submission is a deliberate step; no project is made public by accident |
| Repository deletion and transfer | **Owners only** | No repo can be deleted or moved out of the org by accident |
| Team creation | **Allowed for members** | Each leader creates their own project team |

Owners can still create public repos (general repos) and change visibility.

> **Plan note:** on GitHub's free plan, rulesets and `CODEOWNERS` are enforced
> on **public** repos only. On private repos they need the GitHub Team plan
> (free for academic organizations). Until the organization is upgraded, the
> rules above are a lab agreement on private repos: GitHub will not block a push
> to `main`, so don't make one.

## 2. Dependencies between repos

- **General repos never depend on project repos.**
- General repos may depend on other general repos (e.g. `behavior_medpc` → `utilities_matlab`).
- **Project repos pin exact versions** of the general repos they use. The README
  has a *Requirements* table:

  | Dependency | Version | Link |
  |---|---|---|
  | behavior_medpc | v0.3.0 | https://github.com/Willuhn-Group/behavior_medpc/releases/tag/v0.3.0 |

- If a project needs a fix or new feature in a general function, **fix it in the
  general repo** (PR), release a new version, and update the pin. Do not copy and
  modify general functions inside a project repo.
- **While waiting for that release**, a project in development may point to a
  specific commit of the general repo (for example, the branch with the fix).
  **Before the paper is submitted, every pin must be a release tag.**
- Projects never rely on whatever is on a general repo's `main` today. Because
  of that, a merge into a general repo's `main` breaks no one: the care goes into
  *when* to release and flagging breaking changes, not into restricting who can
  contribute. Anyone in the lab can branch and open a PR (team `lab_all`); only
  the code owners merge.
- External toolboxes (FieldTrip, WaveClus, …) are listed with the version tested.

## 3. Folder layout

**Each repo's leader defines its layout.** Code is organized differently for
different data and techniques, so there is no single required tree. Whatever
the layout, it must be documented: a short tree in the README
(*Repository structure*) and in the repo's `CLAUDE.md` (*Layout*), with one
line per folder.

What holds in every repo, whatever its layout:
- Example scripts and their small example data live in the repo, and the
  examples run from a fresh clone (paths built relative to the repo, see
  section 4).
- No experimental data in git (section 5).
- Project-specific settings and local paths stay out of the functions
  (`projConfig.m` and the git-ignored `localPaths.m` in project repos).

## 4. MATLAB style

### Naming
- Functions, files and variables: **camelCase** (`getTrials`, `medFiles`).
  The file name must match the function name exactly.
- Constants: UPPER_CASE (`MEDSAMPLERATE`).
- Loop indices: `i` + what is iterated (`ifile`, `itrial`, `irat`).
- Older snake_case names (`read_medpc`, `avg_err_shade`) are renamed when the
  function is next touched, and **every call site is updated in the same commit**.

### Inputs
- Functions with more than two parameters take a single **`cfg` struct**
  (FieldTrip style). Required fields are checked at the top; optional fields get
  defaults.
- Fail early with clear messages: `error('getTrials:missingField', 'cfg.medFile is required')`.

### Paths (most common cause of "works on my machine")
- **Never hard-code absolute paths** (`M:\...`, `\\vs03\...`) in committed code.
- Build paths with `fullfile`, never with `'\'` or `'/'`, so code runs on Windows, Mac and Linux.
- Example scripts find their data relative to themselves:
  ```matlab
  repoRoot = fileparts(fileparts(mfilename('fullpath')));
  dataFile = fullfile(repoRoot, 'example_data', 'example_rat_multipellet');
  ```
- Project data locations go in `localPaths.m` (git-ignored). The repo ships
  `localPaths.template.m`; each user copies it and fills in their own paths.
  `projConfig.m` reads `localPaths.m`.

### Function header
Every function starts with this help block (shown by `help functionName`):

```matlab
function out = functionName(cfg)
% out = functionName(cfg)
%
% One or two lines on what the function does.
%
% Inputs:
%   cfg: configuration struct with fields                          [struct]
%     fieldA: what it is                                           [char]
%     fieldB: what it is (optional, default: 1)                    [double]
%
% Outputs:
%   out: what it is, and its fields                                [struct]
%
% Example:
%   cfg = []; cfg.fieldA = 'x';
%   out = functionName(cfg);
%
% Dependencies: readMedpc (behavior_medpc)
%
% Version history:
%   v0.1  Aug 2024  First version
%   v0.2  Aug 2026  Switched to cfg input
%
% Author(s): Name Surname
% Neuromodulation & Behavior Laboratory, Netherlands Institute for Neuroscience
```

The usage line and all names in the header must match the current code.

### Scripts vs functions
- `clear; clc` is fine at the top of example scripts, never inside functions.
- Example scripts use cell sections (`%%`) so they can be run step by step in a demo.

## 5. Data

- **No raw or processed experimental data in git.** Data stay on the institutional server.
- `example_data/` holds small, de-identified example files only.
- `.gitignore` covers `*.asv`, `localPaths.m`, `*.mat` outside `example_data/`, and output folders.

## 6. Git workflow

Step by step in [CONTRIBUTING.md](CONTRIBUTING.md).

- `main` **always works**.
- One branch **per task**, named after what it does (`fix-read-medpc-name`).
  Commit to it as often as you like, and delete it once it is merged.
- `main` is the **only permanent branch**: no `developer`, `dev` or personal
  branches that live on next to it.
- Changes reach `main` only through a **pull request**, reviewed by the repo
  leader (the code owner). Nobody pushes to `main` directly, the leader included;
  the leader can merge their own PR without an approval (see *Protecting `main`*).
- **Open a PR only when the work is close to a finished, usable version.** The
  leader reviews finished work, not work in progress. Questions along the way go
  to the leader directly (send the link to your pushed branch if it helps to show the code).
- PRs are merged with **Squash and merge**.
- Commit messages say what changed and why: `Rename read_medpc call in getTrials`,
  not `Update getTrials.m`.
- Versions are tagged as `vMAJOR.MINOR.PATCH`:
  - PATCH: bug fix, no behavior change for users
  - MINOR: new function or option, old code still works
  - MAJOR: something that breaks existing scripts

## 7. Releases and citation (project repos)

1. When the paper is submitted: make the repo **public**, tag a release
   (`v1.0.0`) and archive it on [Zenodo](https://zenodo.org) (GitHub
   integration) to get a DOI. A project repo is public only while its paper is
   under review or after it is published; before submission it stays private.
2. Add `CITATION.cff` with authors, title and DOI.
3. The tagged release is frozen. Changes requested in revision get a new tag
   (`v1.1.0`) and a new DOI version; the paper cites the final one.
4. The README's *Requirements* table must list the exact general-repo versions
   used for the paper.

## 8. Documentation

Every README has: purpose (two to three lines), requirements with versions,
installation (which folders to add to the path), a minimal usage example,
status, citation, license and authors.

## 9. Licensing

MIT by default. Talk to the PI before choosing anything else.

## 10. Python (planned)

Python versions of MATLAB tools will live in `python/` in the same repo,
following PEP 8 (snake_case), which is the normal style for each language.
Function names map one to one: `getTrials` ↔ `get_trials`.

## 11. AI coding assistants

- Every repo has a `CLAUDE.md` that imports this file and adds repo-specific context.
- AI-generated code goes through the same PR and review as any other code.
- The person who opens the PR is responsible for the code, whoever (or whatever) wrote it.
