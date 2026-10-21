# Git Clone and Branch Switching Guide (Windows)

This document explains how to clone a GitLab repository on Windows, authenticate with a Personal Access Token (PAT) when required, inspect and switch branches, open the project in Visual Studio Code (VS Code), and push a personal documentation repository to GitHub.

> ## Security and company-policy reminders
>
> - Never commit passwords, Personal Access Tokens, SSH private keys, `.env` files, or confidential company information.
> - If a token has appeared in a screenshot, chat, or public repository and is still valid, revoke it in GitLab and create a replacement.
> - A company GitLab repository may contain proprietary assets. Before copying or publishing any project content to a personal GitHub account, confirm that company policy, confidentiality obligations, copyright, and repository permissions explicitly allow it.
> - Repository URLs, local paths, and branch names in this guide are examples. Replace them with values you are authorized to use.

## 1. Prerequisites

1. Install [Git for Windows](https://git-scm.com/download/win).
2. Install [Visual Studio Code](https://code.visualstudio.com/).
3. Confirm that your GitLab account has access to the target repository.
4. Prepare an approved HTTPS authentication method. If you use a Personal Access Token, paste it only when Git prompts for a password. Do not place it in a command, script, URL, or documentation file.

Check whether Git is installed:

```bat
git --version
```

If Windows says that `git` is not recognized, install Git for Windows and reopen CMD.

## 2. Create a local folder and open CMD

1. Create a folder for your project, for example:

   ```text
   D:\SAP工作\s4_BI
   ```

2. Open the folder in File Explorer.
3. Click the address bar, type `cmd`, and press Enter.
4. Command Prompt (CMD) opens with that folder as the current directory.

Alternatively, open CMD manually and navigate to the directory:

```bat
cd /d "D:\SAP工作\s4_BI"
```

The `/d` option allows CMD to switch both the drive and directory. Keep the quotation marks if the path contains spaces.

## 3. Clone a repository from GitLab

On the GitLab project page, find the **Clone** or **Code** button and copy the HTTPS clone URL. Then run:

```bat
git clone <GitLab-HTTPS-repository-URL>
```

Example format only:

```bat
git clone https://gitlab.example.com/group/project.git
```

If your company uses an internal GitLab hostname or a self-signed certificate, follow your IT team's instructions. Do not disable SSL certificate verification just to bypass an error.

After a successful clone, Git normally creates a folder named after the repository. Enter that folder:

```bat
cd project
```

Replace `project` with the actual directory name created by Git.

## 4. Enter your username and password / token

If authentication is required, CMD may display prompts similar to:

```text
Username for 'https://gitlab.example.com': your_username
Password for 'https://your_username@gitlab.example.com':
```

Enter:

- **Username:** usually your GitLab username.
- **Password:** if the GitLab instance requires a Personal Access Token, paste the token here instead of your normal account password.

The terminal may not display any characters or asterisks while you paste the token. This is normal. Press Enter after pasting.

### Create a Personal Access Token in GitLab

The exact menu labels may differ by GitLab version:

1. Sign in to GitLab.
2. Open the avatar menu and go to **Preferences**, **Edit profile**, or **User Settings**.
3. Select **Access Tokens**.
4. Create a token, set a descriptive name and expiration date.
5. Select only the minimum scopes needed for your task.
6. Copy and store the token securely when it is created. GitLab may show the full token only once.

Permission guidance:

- For cloning a private repository over HTTPS, `read_repository` is usually sufficient.
- For pushing over HTTPS, `write_repository` is usually required.
- Select `api`, `read_api`, or other API-related scopes only if your task actually requires API access.
- Available scopes depend on the GitLab version, instance configuration, and organizational policy.

**A token is a credential. Never commit it to a repository or include it in screenshots, README files, or chat messages.**

## 5. Verify the cloned repository

From inside the repository folder, run:

```bat
git status
git branch -a
git remote -v
```

These commands show:

- `git status`: the current branch and working-tree status.
- `git branch -a`: local branches and remote-tracking branches.
- `git remote -v`: the configured remote repository URLs.

For example, remote branches might include:

```text
remotes/origin/20260120
remotes/origin/dev
remotes/origin/feature/guobin/修改
remotes/origin/master
remotes/origin/master_back
```

These names are illustrative. Use the branch names shown by your own repository.

## 6. Switch to the required branch

### Option A: Create a local branch that tracks the remote branch (recommended)

If you want to work on a remote branch such as `origin/feature/guobin/修改`, run:

```bat
git switch -c "feature/guobin/修改" --track "origin/feature/guobin/修改"
```

This creates a local branch and configures it to track the specified remote branch.

If the local branch already exists, switch to it with:

```bat
git switch "feature/guobin/修改"
```

Older Git versions can use `checkout`:

```bat
git checkout -b "feature/guobin/修改" "origin/feature/guobin/修改"
```

### Option B: Inspect a remote branch without creating a local branch

If you run:

```bat
git checkout origin/feature/guobin/修改
```

Git may display a message like:

```text
You are in 'detached HEAD' state.
```

This means you are in a **detached HEAD** state. You are viewing a commit referenced by the remote branch rather than working on a normal local branch. New commits made in this state are not automatically attached to the local branch you may have expected.

If you are in this state and have not created any commits you need to keep, you can create a local branch at the current commit:

```bat
git switch -c "feature/guobin/修改"
```

If you already created an important commit while detached, record its hash first with `git log -1 --oneline`, then create a branch to preserve it. Do not switch away and discard uncommitted work.

### Verify the current branch

```bat
git branch
git status
```

The branch marked with `*` is usually the current local branch. Confirm that `git status` reports the branch you intend to use.

## 7. Open the project in VS Code

From CMD, while inside the project directory, run:

```bat
code .
```

If CMD says that `code` is not recognized:

1. Open VS Code.
2. Select **File → Open Folder...**.
3. Select the cloned project folder.

You can browse project files in the Explorer panel. Use the **Source Control** panel on the left to review changed files, stage changes, and create commits.

## 8. Useful Git commands for daily work

Check the working tree:

```bat
git status
```

View the latest commits:

```bat
git log --oneline -5
```

List local branches:

```bat
git branch
```

Fetch remote updates without merging them into the current branch:

```bat
git fetch
```

Before integrating remote changes, confirm that your working tree is clean and follow your team's workflow for `git pull`, merge, rebase, and code review.

## 9. Push this documentation to a personal GitHub repository

These steps are intended for a repository you own or content you are explicitly authorized to publish. If your current folder is a company GitLab project, do **not** push its source code to personal GitHub without clear authorization. Instead, create a separate folder and a separate GitHub repository containing only this general-purpose guide.

### Step 1: Create an empty GitHub repository

1. Sign in to GitHub.
2. Open [GitHub's new repository page](https://github.com/new).
3. Use a repository name such as `git-clone-guide`.
4. Choose **Public** or **Private**.
5. To avoid an initial-history conflict, leave automatic README, license, and `.gitignore` creation unchecked for this workflow.
6. Click **Create repository** and copy the HTTPS repository URL. It will look similar to:

   ```text
   https://github.com/<your-username>/git-clone-guide.git
   ```

### Step 2: Save `README.md` in a separate folder

Create a personal documentation folder, for example:

```text
D:\Personal\git-clone-guide
```

Save this document as `README.md` in that folder. Make sure the filename is not accidentally `README.md.txt`.

Open CMD in that folder:

```bat
cd /d "D:\Personal\git-clone-guide"
```

### Step 3: Initialize the local repository

If this folder is not already a Git repository, run:

```bat
git init
git branch -M main
git add README.md
git commit -m "docs: add Git clone guide"
```

If Git reports that your author identity is not configured, set your own name and email:

```bat
git config --global user.name "Your GitHub display name"
git config --global user.email "you@example.com"
```

Then run the `git commit` command again. This identity is recorded in commits; it is not your account password.

### Step 4: Add the GitHub remote

Replace the example URL with the HTTPS URL of the GitHub repository you created:

```bat
git remote add origin https://github.com/<your-username>/git-clone-guide.git
```

Check the remote URL:

```bat
git remote -v
```

If Git reports `remote origin already exists`, inspect the current URL first:

```bat
git remote -v
```

Only if this is your personal documentation repository and you intend to change its destination, run:

```bat
git remote set-url origin https://github.com/<your-username>/git-clone-guide.git
```

Do not run this blindly in a company project folder, because it can replace the existing GitLab remote URL.

### Step 5: Push to GitHub

```bat
git push -u origin main
```

For the first push, GitHub may ask you to sign in through a browser, Git Credential Manager, a Personal Access Token, or another approved authentication method. GitHub does not accept your regular account password as the password for HTTPS Git operations. Use a supported authentication method, and never embed a token in the remote URL.

After the push succeeds, refresh the GitHub repository page. You should see `README.md`.

### Step 6: Update the documentation later

After editing the README, run these commands from the documentation repository folder:

```bat
git status
git add README.md
git commit -m "docs: update Git guide"
git push
```

If you changed multiple files, you can use:

```bat
git add .
```

Before committing, review `git status` and make sure you are not including credentials or confidential files.

## 10. Troubleshooting

### `Authentication failed`

- Check that the username is correct.
- Confirm that the token has not expired or been revoked and has the required scopes.
- If Windows has cached an old credential, open **Credential Manager** and review the relevant GitLab or GitHub entry. Remove or update the incorrect credential, then try again.
- Follow any company single sign-on or IT authentication requirements.

### `Repository not found` or permission denied

- Check that the repository URL is complete and correct.
- Confirm that your account has access to the project.
- Make sure you are using the correct account and GitLab/GitHub host.

### `detached HEAD`

Use `git status` to verify the state. If you want to start normal work from the current commit, create a branch:

```bat
git switch -c "my-work-branch"
```

If you have important commits that are not attached to a branch, preserve them before switching elsewhere.

### `src refspec main does not match any`

This usually means that no commit has been created yet or that the current branch has a different name. Check:

```bat
git status
git branch
git log --oneline -3
```

Make sure a commit exists, then push the correct branch name.

### A token was accidentally committed

Deleting the file or editing the README does not guarantee that the credential is gone from Git history. Revoke the token immediately in the relevant platform. If it was pushed to a remote repository, follow the platform's and your organization's security procedures to address the exposed credential and its history.

---

## Quick command reference

### Clone a GitLab repository

```bat
cd /d "D:\SAP工作\s4_BI"
git clone <GitLab-HTTPS-URL>
cd <cloned-directory>
git status
git branch -a
git switch -c "<local-branch-name>" --track "origin/<remote-branch-name>"
code .
```

### Push personal documentation to GitHub

```bat
cd /d "D:\Personal\git-clone-guide"
git init
git branch -M main
git add README.md
git commit -m "docs: add Git clone guide"
git remote add origin https://github.com/<username>/<repository>.git
git push -u origin main
```

> These command groups describe two different workflows. Do not accidentally replace the `origin` remote of a company GitLab project with your personal GitHub URL, and do not publish company code without authorization.
