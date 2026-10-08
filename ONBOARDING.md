# Getting started with the lab GitHub

Welcome! This takes about **30 minutes** and gets you set up to use and
contribute to the [Willuhn Group](https://github.com/Willuhn-Group) code.
No programming experience with Git is needed.

Questions at any point: ask Sergio or post in [Slack channel].

---

## Part 1: Your account (10 min)

1. **Create a GitHub account** at [github.com/signup](https://github.com/signup),
   or use the one you already have.
   - Pick a username you're happy to show professionally (e.g. `firstname-lastname`).
   - Add your **real name** and a **photo** to your profile, so lab members
     recognize you in reviews.
2. **Turn on two-factor authentication:** profile picture → **Settings →
   Password and authentication → Enable two-factor authentication**.
   An authenticator app on your phone works best.
3. **Add your work email:** **Settings → Emails** → add your NIN email and verify it.
   Your commits are linked to your account through this email; without it,
   your work won't be credited to you.
4. **Send your GitHub username** (not your email) to Sergio.

## Part 2: Join the lab organization (2 min)

5. You'll receive an invitation by email. Accept it, or go to
   [github.com/Willuhn-Group](https://github.com/Willuhn-Group) and accept the
   banner at the top.
6. Check that you can see the lab repositories, including the private ones.

## Part 3: Install GitHub Desktop (5 min)

7. Download **[GitHub Desktop](https://desktop.github.com)** and install it.
8. Open it → **Sign in to GitHub.com** with your account.
9. In **Options → Git**, check that your name and **work email** are filled in.

> Prefer the command line or MATLAB's built-in Git (*Source Control* in the
> MATLAB toolstrip)? Both work. GitHub Desktop is just the easiest start.

## Part 4: Practice: your first pull request (15 min)

The `github-practice` repo is a sandbox: nothing there matters, so try things freely.

10. **Clone it:** GitHub Desktop → **File → Clone repository** →
    `Willuhn-Group/github-practice` → choose a folder on your computer → **Clone**.
11. **Create a branch:** **Current branch → New branch**, name it
    `add-<yourname>` → **Create branch**.
12. **Make a change:** open `members.md` in any text editor (or MATLAB) and add
    a line with your name and one analysis you'd like to learn. Save.
13. **Commit:** back in GitHub Desktop, the change appears on the left. Write a
    short summary (`Add <your name> to members`) → **Commit to add-<yourname>**.
14. **Push:** click **Publish branch**.
15. **Open a pull request:** click **Create Pull Request**. GitHub opens in your
    browser with the PR form; fill in the template and click **Create pull request**.
16. **Wait for the review.** The repo leader will approve and merge it, or ask
    for a small change; if so, edit, commit and push again on the same branch.

That's the whole workflow you'll use on real repos: **branch → commit → push →
pull request → review → merge.**

## Part 5: Read the rules (10 min, any time this week)

- **[CONTRIBUTING.md](CONTRIBUTING.md):** how to get a change into a lab repo.
- **[LAB_CONVENTIONS.md](LAB_CONVENTIONS.md):** naming, function headers,
  paths, repo types and access.
- Leading a repo? Also **[LEADER_GUIDE.md](LEADER_GUIDE.md)**.

**The three rules that matter most:**
1. **Never commit data**, only code (and small example files).
2. **No absolute paths** (`M:\...`, `\\vs03\...`); use `fullfile`.
3. **Nothing goes to `main` without a pull request** (except by the repo leader).

---

**Leaving the lab later?** Push all your code and tell the leaders of the repos
you work on before your last day. See *Handing over* in the
[LEADER_GUIDE.md](LEADER_GUIDE.md).
