# Willuhn Group — Code Conventions

*Version 0.2 · October 2026 · updated after the lab meeting of 7 Oct 2026*

This document defines how code is organized, written and shared across the
[Willuhn-Group GitHub](https://github.com/Willuhn-Group). It lives in the
`.github` repository and applies to every repo in the organization. Each repo's
`CLAUDE.md` imports it, so AI coding assistants follow the same rules as people.

Anything here is open to change: propose edits through a pull request to `.github`.

---

## 1. Repository types

| Type | Purpose | Examples | Visibility | Who contributes |
|---|---|---|---|---|
| **General** | Reusable tools anyone in the lab can use | `medpc-behavior`, `matlab-utilities` | **Public** | Everyone (via PR to the leader) |
| **Project** | Analysis pipeline for one study/paper; becomes the citable code of the publication | `ephys-induced-polydipsia`, `calcium_rats_gonogo` | **Private** (lab members only) until publication | Project members |
| **Hub** (`.github`) | Lab-wide conventions, contributing guide, org landing page | `.github` | Public (required by GitHub) | Everyone (via PR) |

**Every repository has one leader**, named in its `CLAUDE.md` and README. The
leader is responsible for `main`: they check every change before it is merged.

**Naming:** lowercase, words separated by hyphens (`lfp-tools`, `sip-ephys`).
General repos are named after what they do; project repos after the study.

### Access and teams

**Everyone can see everything; only the people working on a repo can change it.**

| Who | Access | How it is set |
|---|---|---|
| All lab members | **Read** on every repo, private ones included | Org base permission |
| A project's members | **Write** on that project's repo | One GitHub team per project (e.g. `team-polydipsia`) |
| All lab members, on general repos | **Write**, so anyone can branch and open a PR | Team `lab-all` |
| The repo leader | **Admin** on their repo | Automatic for whoever creates the repo |
| Org owners (2–3 people, e.g. the PI and the code coordinator) | Everything, including making a repo public | Org settings |

When someone joins or leaves a project, change the **team**, not each repo.

**Starting a new project repo.** Project repos are created **by their leader**.
Every project repo starts with three files:

| File | Content |
|---|---|
| `LICENSE` | MIT, unless agreed otherwise with the PI |
| `README.md` | A brief description of the project, from the [project README template](PROJECT_README_TEMPLATE.md) |
| `CLAUDE.md` | Filled in from the project `CLAUDE.md` template |

Steps:
1. Org → *New repository*. Owner: **Willuhn-Group**, visibility: **Private**.
   On the same page tick **Add a README file** and choose **License: MIT**:
   GitHub creates both files for you.
2. Replace the README text with the project README template and fill in the
   description (at least the title, the study in two or three lines, the leader
   and the people).
3. Add `CLAUDE.md` from the project template and fill it in.
4. Create the project team (org → *Teams → New team*), add its members, and give
   the team **Write** on the repo (repo → *Settings → Collaborators and teams*).

**Org settings** (owners only, org → *Settings → Member privileges*):
- Base permissions: **Read**
- Repository creation: **allowed for members**
- Changing repository visibility: **owners only**, so going public at
  publication is a deliberate step and no project is made public by accident

> On GitHub's free plan, branch rules are not enforced on private repos: anyone
> with Write can technically push to `main`. "PRs for everyone but the leader"
> is a lab agreement there until the organization plan is upgraded.

## 2. Dependencies between repos

- **General repos never depend on project repos.**
- General repos may depend on other general repos (e.g. `medpc-behavior` → `matlab-utilities`).
- **Project repos pin exact versions** of the general repos they use. The README
  has a *Requirements* table:

  | Dependency | Version | Link |
  |---|---|---|
  | medpc-behavior | v0.3.0 | https://github.com/Willuhn-Group/medpc-behavior/releases/tag/v0.3.0 |

- If a project needs a fix or new feature in a general function, **fix it in the
  general repo** (PR), release a new version, and update the pin. Do not copy and
  modify general functions inside a project repo.
- External toolboxes (FieldTrip, WaveClus, …) are listed with the version tested.

## 3. Folder layout

**General repo**
```
matlab/          % functions (subfolders by topic if needed)
python/          % only once Python versions exist
example_use/     % one runnable example script per main function family
example_data/    % small example files (a few MB max)
README.md  LICENSE  CLAUDE.md  .gitignore
```

**Project repo**
```
matlab/
  config/        % projConfig.m + localPaths.template.m
  <stage>/       % one folder per pipeline stage (behavior/, lfp/, spikes/, …)
  utils/         % project-specific helpers only
example_use/
example_data/
docs/            % pipeline diagrams, notes
README.md  LICENSE  CITATION.cff  CLAUDE.md  .gitignore
```

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
% Dependencies: readMedpc (medpc-behavior)
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
  Commit to it as often as you like.
- Changes reach `main` through a **pull request reviewed by the repo leader**.
  The leader is the only person who may push to `main` directly.
- **Open a PR only when the work is close to a finished, usable version.** The
  leader reviews finished work, not work in progress. Questions along the way go
  to the leader directly (or in a clearly marked Draft PR with a specific question).
- PRs are merged with **Squash and merge**.
- Commit messages say what changed and why: `Rename read_medpc call in getTrials`,
  not `Update getTrials.m`.
- Versions are tagged as `vMAJOR.MINOR.PATCH`:
  - PATCH: bug fix, no behavior change for users
  - MINOR: new function or option, old code still works
  - MAJOR: something that breaks existing scripts

## 7. Releases and citation (project repos)

1. At publication (or at submission, if the journal requires code access; agree
   with the PI): make the repo **public**, tag a release (`v1.0.0`) and archive
   it on [Zenodo](https://zenodo.org) (GitHub integration) to get a DOI.
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
