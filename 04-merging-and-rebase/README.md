# Merging and Rebasing

- [Merging and Rebasing](#merging-and-rebasing)
  - [MERGING AND FAST FORWARD](#merging-and-fast-forward)
    - [under the hood - merge commits are just commits](#under-the-hood---merge-commits-are-just-commits)
    - [fast forward](#fast-forward)
    - [git merge --no-ff (no fast forward)](#git-merge---no-ff-no-fast-forward)
  - [MERGE CONFLICTS](#merge-conflicts)
    - [merge conflicts](#merge-conflicts-1)
    - [git RERERE - REuse, REcord, REsolution](#git-rerere---reuse-record-resolution)
    - [git RERERE - how to use?](#git-rerere---how-to-use)
- [COMMAND SUMMARY](#command-summary)
  - [Merge Operations](#merge-operations)
  - [Fast-Forward Merge](#fast-forward-merge)
  - [Merge Conflicts](#merge-conflicts-2)
  - [RERERE (Reuse Recorded Resolution)](#rerere-reuse-recorded-resolution)

## MERGING AND FAST FORWARD

### under the hood - merge commits are just commits

they happened to be a commito have more one parent

- so we can have multiple parents and gettign merge into one commit
- it maintains all commits we have created on a feature branch
- merge commit is just a marker of when a new feature is merge into main/master
  ![image.png](attachment:a0d71578-092a-47cb-b2e4-571f779b06c5:image.png)

### fast forward

A fast-forward happens when the branch you’re merging into (usually main)
has no new commits since the feature branch was created.

![image.png](attachment:a3ec1347-3e5f-4195-84d5-f4e760afa3ee:image.png)

in that case the master pointer just move from the feature and there is no merged happening

![image.png](attachment:79425018-e565-4269-b55c-df23fe7c9d34:image.png)

during this fast-forward commit you **can lose the branch name** (its separate pointer),

### git merge - -no-ff (no fast forward)

- to retain the history of a merge commit, even if there are no changes to the base branch:
  - use git merge - -no-ff
- this will force a merge commit, even when one isn`t necessary
  ![image.png](attachment:a897f996-c96a-48f8-9a7d-6fb5b66cb45d:image.png)
- it tells git even it can do fast forward merge but we do is force to not fast forward merge commit and after it looks like this
  ![image.png](attachment:09e7da8e-c2b2-496d-afab-2f98ea63c566:image.png)

## MERGE CONFLICTS

### merge conflicts

- attempt to merge, but files have diverged
- git stops until the conflicts are resolved
  ![image.png](attachment:df669d96-cef6-40f6-aa94-6cd14a84fad9:image.png)
- during a merge git is goign to try to merge our changes in various strategy and that content cant be automatically merge, we will get a merge conflict error
- we might encounter this if we are working on a repo with alot of contributor
- git will create a new file and contain those conflict and commit and continue

### git RERERE - REuse, REcord, REsolution

- git saves how you resolved a conflict
- next conflict: reuse the same resolution
- **useful for:**
  - long lived feature branch (like refactor)
  - rebasing

### git RERERE - how to use?

turn it on:

- git config rerere.enabled true/1
- use - -global flag to enable all projects # to be used in all projects
  ![image.png](attachment:04a81389-4db9-4eac-ab1c-4efbd64f966c:image.png)
- if the same conflict happens it will matched , in that record resolution

## COMMAND SUMMARY

### Merge Operations

| Command                           | Description                                             | Example Output / Notes                                    |
| --------------------------------- | ------------------------------------------------------- | --------------------------------------------------------- |
| `git merge <branch>`              | Merge the specified branch into the current branch      | Creates a new merge commit if both branches have diverged |
| `git merge --no-ff <branch>`      | Force a merge commit even if a fast-forward is possible | Useful for keeping feature branch history visible         |
| `git merge --abort`               | Cancel a merge that’s in progress                       | Used if merge conflicts occur and you want to stop        |
| `git log --graph --oneline --all` | Visualize branch merges and history                     | Shows commit tree with branches and merges                |

---

### Fast-Forward Merge

| Command                      | Description                                                | Example Output / Notes                               |
| ---------------------------- | ---------------------------------------------------------- | ---------------------------------------------------- |
| `git merge <branch>`         | Fast-forwards main pointer if no new commits exist on main | Moves main → feature without creating a merge commit |
| `git merge --no-ff <branch>` | Prevents fast-forwarding; forces a merge commit            | Keeps history of merged branch (shows a merge point) |
| `git branch -d <branch>`     | Delete a merged branch after fast-forward                  | Only removes the name (pointer), not the commits     |

**fast-forward behavior:**

If `main` has not diverged, Git simply moves `main` to the latest commit of `feature`.

No commits are lost — only the separate feature branch label is removed.

---

### Merge Conflicts

| Command              | Description                         | Example Output / Notes                           |
| -------------------- | ----------------------------------- | ------------------------------------------------ |
| `git merge <branch>` | Merge another branch                | If same file lines are changed → conflict occurs |
| `git status`         | Check merge conflict files          | Shows “Unmerged paths” for conflicted files      |
| `git diff`           | View conflicting sections           | Shows `<<<<<<<`, `=======`, `>>>>>>>` markers    |
| `git add <file>`     | Mark conflict as resolved           | Stage resolved file after fixing manually        |
| `git commit`         | Complete the merge after resolution | Finalizes merge commit after resolving conflicts |
| `git merge --abort`  | Abort and revert merge state        | Returns to pre-merge condition if unresolved     |

Example conflict marker:

```
<<<<<<< HEAD
console.log("Main branch");
=======
console.log("Feature branch");
>>>>>>> feature

```

---

### RERERE (Reuse Recorded Resolution)

| Command                                   | Description                           | Example Output / Notes                            |
| ----------------------------------------- | ------------------------------------- | ------------------------------------------------- |
| `git config rerere.enabled true`          | Enable RERERE for current repo        | Records how you resolve conflicts                 |
| `git config --global rerere.enabled true` | Enable RERERE globally                | Works for all repositories                        |
| `git status`                              | Check if conflict recorded            | Git will reuse known resolutions                  |
| `git rerere status`                       | Show recorded resolutions             | Displays previously saved conflict resolutions    |
| `git rerere diff`                         | View resolution difference            | Compare recorded vs. current conflict resolutions |
| `git rerere forget <file>`                | Remove recorded resolution for a file | If you don’t want Git to auto-resolve next time   |

**Why use RERERE?**

- Helps with **long-lived feature branches** or repeated rebases
- Saves time by automatically reusing previous conflict fixes
