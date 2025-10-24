# Fixing Mistakes

there are few tools we used to fix mistakes **Checkout, Reset, Revert, Clean**

- [GIT CHECKOUT](#git-checkout)
  - [what happens when you git checkout a branch?](#what-happens-when-you-git-checkout-a-branch)
  - [what happens when you git checkout --file?](#what-happens-when-you-git-checkout--file)
  - [git checkout: overwrite files with staging area copy](#git-checkout-overwrite-files-with-staging-area-copy)
  - [what happens when you git checkout <commit> --file?](#what-happens-when-you-git-checkout-commit--file)
  - [git checkout: from a specific commit](#git-checkout-from-a-specific-commit)
  - [SUMMARY OF CHECKOUT](#summary-of-checkout)
- [GIT CLEAN](#git-clean)
- [GIT RESET](#git-reset)
  - [checkout vs reset](#checkout-vs-reset)
  - [types of git reset](#types-of-git-reset)
  - [git reset <commit> cheat sheet](#git-reset-commit-cheat-sheet)
  - [git reset --<file>](#git-reset-file)
  - [git reset <commit> --<file>](#git-reset-commit---file)
  - [undo a git reset with ORIG_HEAD](#undo-a-git-reset-with-orig_head)
- [GIT REVERT](#git-revert)
- [summary: common ways of undoign changes](#summary-common-ways-of-undoign-changes)
- [COMMAND SUMMARY](#command-summary)
  - [Reset Type Summary](#reset-type-summary)
  - [Checkout vs Reset vs Revert](#checkout-vs-reset-vs-revert)
  - [Quick Tips](#quick-tips)

## GIT CHECKOUT

- restore working tree files or switch branches
  ![image.png](attachment:a1c4284b-8bf7-4df0-a974-c8dbd861b1ee:image.png)

### what happens when you git checkout a branch?

1. change HEAD to point to the new branch
2. copy the commit snapshot to the staging area also called index or cache
3. update the working area with the branch contents

Any changes in your **working directory** (uncommitted edits to files) are kept when you switch branches, **as long as those files exist in both branches** and don’t conflict with files in the branch you’re switching to.

![image.png](attachment:33736e84-7407-479b-88b1-b0491b95f394:image.png)

### what happens when you git checkout --file?

replace the working area copy with the version from the current staging area

- its just → “Forget my local edits to this file bring it back to how it looked in the last committed snapshot.”
  ![image.png](attachment:eae84a4d-a5c6-461c-97c9-daf80277e222:image.png)

### git checkout: overwrite files with staging area copy

- overwrite the working area file with the staging area version from the last commit
  - git checkout - - <file_path>
- The double dash `--` is a **separator** that tells Git (and most Unix commands):
  - “Everything after this point is a file name, not a branch, tag, or option.”

### what happens when you git checkout <commit> --file?

1. update the staging area to match the commit
2. update the working area to match the staging area

0000000000“Take this file’s version from that commit and put it in my working directory.”

![image.png](attachment:86b23ce4-9578-4f54-b394-eb85cd0c3c39:image.png)

### git checkout: from a specific commit

- checkout a file from a specific commit
  - _copies to both working area and stagign area_
  - git checkout <commit> - - <file_path>
- restore a deleted file
  - git checkout <deleteing_commit>^ - - <file_path>
  - “Restore the version of `<file_path>` **from the commit _before_** the one that deleted it.”
    ![image.png](attachment:8c4887fa-3d96-46a5-869c-4acb79f08134:image.png)

### SUMMARY OF CHECKOUT

It’s a command that **switches or restores** things in Git.

**It can do exactly 2 things**

| Action                            | Example                     | Meaning                                                                                 |
| --------------------------------- | --------------------------- | --------------------------------------------------------------------------------------- |
| **1. Switch branches or commits** | `git checkout main`         | Moves you to another branch (or commit) — changes your working directory to match it.   |
| **2. Restore files**              | `git checkout -- hello.txt` | Restores a file from the last commit (HEAD) or staging area — overwrites local changes. |

## GIT CLEAN

- git clean will clear your working area by deleting untracked files
  ![image.png](attachment:bf9fb6bd-311b-4412-946e-3605c1eaae45:image.png)
- use the - -dry-run flag to see what you would be deleted
  - the -f flag to do the deletion
- the -d flag will clean directories

![image.png](attachment:afbd897e-b855-4e4f-8f9e-08deb25770d3:image.png)

**_after doing git clean_**

![image.png](attachment:db6fb1b7-9597-4162-b33f-c8e59fc1e5d2:image.png)

## GIT RESET

- reset is another command that performs different actions depending don the arguments
  - with a path
  - without a path
    - by default, git performs a **git reset -mixed**

understanding reset, is important bcs if we do thsi we might overwrite this specially if there is untracked changes

- for commits:
  - moves the HEAD pointer, optionally modifies file
- for file paths:
  - does not move the HEAD pointer, modifies file

### checkout vs reset

- checkout will move the head but the branch stays
- reset will move and the HEAD the branch reference meaning yoru branch will be modified and differnet from before

### types of git reset

there are three options for reset → soft, mixed, and hard

- soft → just moves the head and A branch is possible a dangling commit, and if we are on B and commit B then A will be gone
- - -soft → “Go back to A’s history, but keep B’s changes as **staged edits (ready to commit)**.”

![image.png](attachment:d8351baf-eb5e-4203-9a43-1fa87c8e1855:image.png)

![image.png](attachment:35a49f24-bccd-4723-a553-c3f37895375a:image.png)

- mixed → it moves the HEAD and copies the file from where the HEAD is and copies it to staging area, MIXED is the default we can also consider this unstage command
- - -mixed → “Go back to A’s history, but keep B’s changes as **uncommitted edits (not staged)**.”

![image.png](attachment:88d2516c-cfc2-47e3-8ec6-c7729cbddb28:image.png)

- hard → it does three operation which adds another one from MIXED which it also copies file from staging area ato working area
- - -hard → “Go back to A’s history, and **remove all B’s changes completely** (nothing left).”

![image.png](attachment:3910d967-c3f8-4905-8c9b-a6f612d2255a:image.png)

### git reset <commit> cheat sheet

1. move HEAD and current branch
2. reset the staging area
3. reset the working area

- -soft = **(1)**

- -mixed = **(1) & (2)** → default

- -hard = **(1) & (2) & (3)**

### git reset - -<file>

![image.png](attachment:ca437dd5-a427-40d8-86c0-8cec64495765:image.png)

- “Unstage this file keep my actual code, just remove it from the staging area.”
  - git add [main.java](http://main.java) helper.java
  - git reset - -helper.java
- Now:
  - `main.java` → still staged
  - `helper.java` → **unstaged**, but your code inside is still there

### git reset <commit> - -<file>

![image.png](attachment:199f8297-e17c-4e5e-95b3-2fb571a2fd2b:image.png)

- “Take this file only and reset it to how it looked in **<commit>,**
  but don’t touch other files or your branch pointer

### undo a git reset with ORIG_HEAD

- in case of accidental git reset -
- git keeps the previous value of HEAD in variable called ORIG_HEAD
- to go back to the way things were:
  - **git reset ORIG_HEAD**
    ![image.png](attachment:6791f098-02ff-48ea-8430-b69da443f6c6:image.png)

## GIT REVERT

### git revert - the “safe” reset

- git revert creates a new commit that introduces the opposite changes from the specified commit
- the original commit stays in the repository
- tip:
  - use revert if you`re undoing a commit that has already been shared or pushed in a remote repo
  - revert does not change history
    ![image.png](attachment:841d4814-a5b3-47a2-9cfa-a35d31f42657:image.png)
- **choose a commit SHA1 to revert**

![image.png](attachment:5abadc16-d3e7-41d2-bd07-90130c318f89:image.png)

### summary: common ways of undoign changes

- checkout
- reset
- revert

## COMMAND SUMMARY

Fixing Mistakes Checkout, Reset, Revert, Clean

| Command / Concept                     | Description                                                                               |
| ------------------------------------- | ----------------------------------------------------------------------------------------- |
| **`git checkout <branch>`**           | Switches to another branch; updates HEAD and your working directory to match that branch. |
| **`git checkout -- <file>`**          | Discards local edits and restores the file from the last committed version (HEAD).        |
| **`git checkout <commit> -- <file>`** | Restores a specific file version from a past commit (without switching branches).         |
| **`git reset -- <file>`**             | Unstages a file (removes from staging) but keeps your actual code edits.                  |
| **`git reset <commit> -- <file>`**    | Resets the file to how it looked in a specific commit — affects only staging, not HEAD.   |
| **`git reset --soft <commit>`**       | Moves HEAD to the target commit; keeps all your changes staged (ready to commit again).   |
| **`git reset --mixed <commit>`**      | Moves HEAD to the target commit; keeps your code but unstages all files (default mode).   |
| **`git reset --hard <commit>`**       | Moves HEAD and branch pointer back; deletes all changes in staging and working directory. |
| **`git revert <commit>`**             | Creates a _new commit_ that reverses the changes of an old commit — safe for pushed code. |
| **`git clean -n`**                    | Dry run — shows which untracked files would be deleted.                                   |
| **`git clean -f`**                    | Deletes untracked files from the working directory (cannot be undone).                    |
| **`git clean -fd`**                   | Deletes both untracked files and folders.                                                 |

---

### Reset Type Summary

| Reset Type            | Moves HEAD | Keeps Staged | Keeps Code | Description                                                       |
| --------------------- | ---------- | ------------ | ---------- | ----------------------------------------------------------------- |
| `--soft`              | ✅         | ✅           | ✅         | “Go back to A’s history, but keep B’s changes staged.”            |
| `--mixed` _(default)_ | ✅         | ❌           | ✅         | “Go back to A’s history, but keep B’s changes as unstaged edits.” |
| `--hard`              | ✅         | ❌           | ❌         | “Go back to A’s history and completely remove B’s changes.”       |

---

### Checkout vs Reset vs Revert

| Command    | Moves HEAD? | Changes Files?      | Rewrites History? | Safe for Shared Repo? | Main Use                                                 |
| ---------- | ----------- | ------------------- | ----------------- | --------------------- | -------------------------------------------------------- |
| `checkout` | ✅          | ✅                  | ❌                | ✅                    | Switch branches or restore files.                        |
| `reset`    | ✅          | ✅                  | ✅                | ⚠️ No                 | Move branch pointer, unstage or delete commits.          |
| `revert`   | ✅          | ✅                  | ❌                | ✅                    | Undo a commit safely by creating a new “reverse” commit. |
| `clean`    | ❌          | ✅ (untracked only) | ❌                | ✅                    | Remove untracked files/folders.                          |

---

### Quick Tips

| Situation                                | Recommended Command        |
| ---------------------------------------- | -------------------------- |
| Undo **unstaged** changes to a file      | `git checkout -- <file>`   |
| Undo **staged** file (keep code)         | `git reset -- <file>`      |
| Undo **last commit** but keep changes    | `git reset --soft HEAD~1`  |
| Undo **last commit** and unstage changes | `git reset --mixed HEAD~1` |
| Undo **last commit completely**          | `git reset --hard HEAD~1`  |
| Undo **a shared/pushed commit**          | `git revert <commit>`      |
| Delete untracked files                   | `git clean -f`             |

---

### revert vs reset vs checkout vs clean

| Command          | Main Use / Effect                                                                    | What it touches                                                             | Safe for shared history?                   |
| ---------------- | ------------------------------------------------------------------------------------ | --------------------------------------------------------------------------- | ------------------------------------------ |
| **git checkout** | Switch branches or restore files to a previous version                               | HEAD (branch pointer), working directory, staging area (if restoring files) | ✅ Safe (doesn’t rewrite history)          |
| **git reset**    | Move HEAD (and branch) to a previous commit; optionally modify staging & working dir | HEAD, branch pointer, staging area, optionally working directory            | ❌ Can rewrite history if branch is shared |
| **git revert**   | Create a new commit that undoes the changes from a previous commit                   | Only creates a new commit, doesn’t touch existing commits                   | ✅ Safe (doesn’t rewrite history)          |
| **git clean**    | Delete untracked files or directories from the working directory                     | Working directory only (untracked files)                                    | ✅ Safe (doesn’t touch commits)            |
