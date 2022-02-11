# Git and GitHub

## Prerequisites

- You have WSL2 installed on your Windows 10 or Windows 11 workstation. (PowerShell Admin: `wsl --install`).
  - You have generated an SSH public/private key pair. (WSL2 Ubuntu: `ssh-keygen`).
- You have a GitHub account linked to the Centiq organisation. (To link, contact Scott Tipper)
  - The email address associated with this account does not need to be a Centiq address.
  - You should ensure your actual name is set in your profile so it's clear to whom the account belongs.
  - Your WSL2 public key should be added to your GitHub account. (GitHub -> profile -> settings -> SSH & GPG keys -> add key -> copy/paste the contents of your WSL2 file `.ssh/id_rsa.pub`).  For a more detailed guide see [SSH Keys and Github](ssh-notes/ssh-keys-and-github.md).
  - If you have a password on your SSH key then you may want to use ssh-agent.  For details see [Using SSH Keys with Passwords](ssh-notes/using-ssh-keys-with-passwords.md).
- Your Windows network settings should be checked/corrected. Follow [SSH Connection Problems](ssh-notes/ssh-connection-problems.md).
- Before using `git` for the first time, ensure your name and email are configured:

  ```text
  git config --global user.name "first_name last_name"
  git config --global user.email "my_name@example.com"
  ```

  and confirm you can connect to GitHub over SSH with your credentials:

  ```text
  ssh -T git@github.com
  ```

  should give:

  ```text
  Hi <yourusername>! You've successfully authenticated, but GitHub does not provide shell access.
  ```

## Git Terminology

`git`: A source code versioning tool, installed on your workstation. Available for Windows and Linux.

### Repository (Repo)

- A collection of files under a common directory usually making up a complete, functional part of a project.
- When we refer to a repo, we are usually talking about a hosted copy of the files. In our case, hosted by GitHub.
- Repo hosts also provide services around the repo to assist with collaboration and project tracking.

---

### Clone

- You will **clone** a repo from GitHub when you are allowed to write directly to that Repo.
  - `git clone some-repo-url`
  - This is the normal action within Centiq.
- After **cloning** a repo, a new subdirectory, `.git/`, will be created at the repo root. This subdirectory contains configuration data indicating where the GitHub repo is located, together with branch, commit, attribution data, etc.

---

### Branch

- A **branch** is an isolated space in which changes to the main code may be made without affecting the main code.
- A **branch** stores the *differences* in the files, so is small in comparison to the main repo.
- **Branch** names are arbitrary, but the main branch is normally `main` or `master`.

---

### Checkout

- You **checkout** a branch in order to work on it.
  - `git checkout some-existing-branch`
- A new branch can be created and **checked out** in a single operation.
  - `git checkout -b some-new-branch`

---

### Staging

- Modified files need to be *staged* prior to *committing* them.
  - `git add some-list-of-files`

---

### Commit

- A **commit** records a series of changes to a branch.
- You provide a meaningful *commit message* whenever you make a commit.
- Git assigns a *commit id* automatically.
  - `git commit -m "some-meaningful-message"`

---

### Push

- After making a commit (or series of commits) you **push** the commits to the repo (GitHub).
  - `git push`

---

### Pull

- If multiple collaborators are working on a branch, you may **pull** their pushed commits down to your copy to keep yours up-to-date.
- You should ensure you start with an up-to-date branch by **pulling** from GitHub.
- **Pulling** *merges* any changes made in GitHub's copy of the branch.
  - `git pull`

---

### Merge Conflict

- When branches are merged (for example when your work is merged into main) **merge conflicts** can occasionally arise.
- **Merge conflicts** occur when two or more collaborators have made changes to the same area in the same file.
- **Merge conflicts** must be resolved before moving on.

---

## GitHub Terminology

GitHub<sup>1</sup>: A hosted provider of git repositories and collaborative tooling. GitHub is owned by Microsoft.

<sup>1</sup> By GitHub I really mean any hosted git repo provider: GitHub, GitLab, Bitbucket, Azure DevOps, etc.

### Fork

- You will **fork** a repo from GitHub when you want your own copy of the repo.
  - Your copy can be independent of the *upstream* repo, or it can be used to develop updates for it when you aren't able to write directly.
  - This is the most common way open source developers contribute to public repos.
  - You will probably not need this in Centiq.

---

### Issue

- A collaboration document used to record such things as requirements, bugs, etc.
- Everything should start with an issue:
  - New repo needed - issue on the project backlog.
  - Bugs and enhancements to existing repo - issue on the repo itself.

---

### Pull Request (PR)

- A collaboration document used to inform the team that your work is ready to be *reviewed* and hopefully merged.
- **PRs** are linked to one or more issues, which the changes address.
- **PRs** should contain instructions for reviewers to help them understand how to review your changes.

---

### Pull Request Review (PR Review)

- A collaborative *process* to ensure others get an opportunity to examine and test changes before merging.
- Reviewers' responsibilities:
  - Are the changes *understandable* and do they *make sense*?
  - Does the style of the changes comply with corporate guidelines and standards?
  - If *testing* is needed: do the tests!
  - Provide feedback!
    - Best Practice⭐

---

## ~~Best~~Good Practices

- Applies to everyone working with git and GitHub.
- As always, there are times when these cannot be followed, accidents occur, etc.
- If in doubt, ask.

---

### Never Push to Main

- Pushing changes to the main branch should never be done.
  - Best Practice⭐
- However, the first push to a new repo will always be to main🙃.

---

### Short-lived Branches

- With the exception of the main branch, branches should be short-lived.
  - Or the greater the chance for hard-to-resolve merge conflicts.

---

### Lightweight Branches

- The number of changes in a branch should be kept small.
  - Don't indulge in unrelated work (I'll just fix this while I'm here).
  - Heavyweight branches make the accompanying PR all but impossible to review properly.

---

### Documentation

- Documentation lives with the code.
  - Best Practice⭐
- Traditionally written in *Markdown*.
- Every repo should have at least a README file at its root level.

---

## Development Workflow

---

### Kick-off

- This is a "one-time" activity.
  - Clone the required repo.
  - Checkout (possibly create) the required branch.
  - Pull.

---

### Develop

- This is usually a repeated activity.
  - Make changes and save file(s).
  - Stage changes.
  - Commit.
  - Push.
- When the first changes have been pushed:
  - Create draft PR.
  - A one-time activity.

---

### Finish

- This is a "one-time" activity.
  - Test.
  - Open PR for review and feedback.

---

### Merge

- When the PR is approved:
  - Merge with *squashed commits*.
  - Delete the branch.
  - This is a "one-time" activity.
