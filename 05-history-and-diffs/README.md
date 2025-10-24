## USEFUL COMMIT MESSAGES

- [USEFUL COMMIT MESSAGES](#useful-commit-messages)
  - [bad commit messages](#bad-commit-messages)
  - [good commits are important](#good-commits-are-important)
  - [an anatomy of a good commit message](#an-anatomy-of-a-good-commit-message)
- [GIT LOG](#git-log)
  - [git log --since](#git-log---since)
  - [git log --follow](#git-log---follow)
  - [git log --grep](#git-log---grep)
  - [git log diff-filter](#git-log-diff-filter)
  - [git log: referencing commits](#git-log-referencing-commits)
  - [referencing commits (^ VS ~)](#referencing-commits-^--vs-~)
- [GIT SHOW AND DIFFS](#git-show-and-diffs)
  - [git show: look at commit](#git-show-look-at-commit)
  - [diff](#diff)
  - [diff commits and branches](#diff-commits-and-branches)
  - [“diff” branches](#diff-branches)
- [COMMAND SUMMARY](#command-summary)
  - [Commit Messages](#commit-messages)
  - [Git Log & Filtering](#git-log--filtering)
  - [Commit References (`~` and `^`)](#commit-references-~-and-^)
  - [Show & Diff Commands](#show--diff-commands)
  - [Branch Merge Info](#branch-merge-info)

### bad commit messages

![image.png](attachment:a3b7f7cc-3604-4cdc-ac63-2d53383fa4d1:image.png)

- this isn`t greate, and when we debug somethign and it says fix bugs
- but what bugs? but if we could have put descriptive words and save alot of time

### good commits are important

- good commits help preserve the history of a codebase
- they help with:
  - debugging & troubleshooting
  - creating release notes
  - code reviews
  - rolling back
  - associating the code with an issue or ticket

### **an anatomy of a good commit message**

![image.png](attachment:25fc8527-0c37-4630-955e-d4770eee8ae0:image.png)

unfortunately, i see a-lot of people doing git commit -m… well that`s fine if our code brief is not doing alot

but sometimes, a one commit message is not enough and what we wanna do is add a commit descriptive body in our line

- description → it should be context and not how we did something like we move this file form that… rather how we did something, its current behaviour and reason why we fixed that code
- good commit message
- encapsulates one logical idea
- doens`t introduce breaking changes
  - i.e tests pass

## GIT LOG

to examine history we can use git log, its basic command that shows history of our repo
![image.png](attachment:4ef21d2c-30b3-4808-b6ae-d3d750349909:image.png)

### git log - -since

- the site is slow, what changed yesterday??
  - git log - -since=”yesterday”
  - git log - -since”2 weeks ago”

### git log - -follow

- log files that have been moved or renamed
  - git log - -name-status - -follow - - <file>

### git log - -grep

- search for commit messages that match a regex::
  - git log -grep <regexp>
  - can be mixed & matched with other git flags
- example:
  ![image.png](attachment:eea6dcce-9a9b-421e-94ec-7c914f1a1004:image.png)

### git log diff-filter

- selective include or exclude files that have been:
- (A)dded, (D)eleted, (M)odified & more…
  ![image.png](attachment:86611e16-cd6c-41e7-a334-70acd97de77b:image.png)

**In git, a “parent” commit - is the commit that came right before the current one**

### git log: referencing commits

- **^ or ^n (Hat/Caret symbol)**
  - no args: = =^1: the first parent commit
  - **n:** the nth parent commit
- **~ or ~n (Tilde Symbol)**
  - no args: ==~1: the first commit back, following 1st parent
  - **n:** the number of commits back, **_following only 1st parent_**

note: ^ and ~ can be combined

### referencing commits (^ VS ~)

- both commit nodes B and C are parent of commit node A. Parent commits are ordered left-to-right
- A is the latest commit
  ![image.png](attachment:d871b93b-4e9d-4c4e-a23b-fe6180b54be9:image.png)
  | Symbol | Moves through | What it follows | Works on |
  | ------ | ------------------------------- | -------------------------------------------- | -------------- |
  | `~` | **Commits** | **The same branch line** (first parent only) | Linear history |
  | `^` | **Commits + merges (branches)** | **Can choose which parent** (1st, 2nd, etc.) | Merge commits |

## GIT SHOW AND DIFFS

**git show** let us see commit and contents like how we can go in browser GITHUB to look but it`s much easier in terminal

### git show: look at commit

- show commit and contents
  - git show <commit>
- show files changed in commit:
  - git show <commit> - -stat
- look at the file from another commit:
  - git show <commit>:<file>

### diff

- diff show you changes:
  - between commits
  - between staging area and the repository
  - what`s in the workign area
- unstaged changes
  - git diff
- staged changes
  - git diff - changes

### diff commits and branches

![image.png](attachment:b8c2493b-619c-49a7-b7a0-a3917cd8b2e5:image.png)

using git diff its gonna show between A and B

its gonna show us the changes on B but not on A

### “diff” branches

- which branches are merged with master, and can be cleaned up?
  - git branch - -merged master
- which branches arent merged with master yet?
  - git branch - -no-merged master

## COMMAND SUMMARY

Commit Messages

| Command / Concept              | Description                                                                                                      |
| ------------------------------ | ---------------------------------------------------------------------------------------------------------------- |
| `git commit -m "message"`      | Create a commit with a short, one-line message                                                                   |
| `git commit`                   | Opens editor to write a full message (title + description)                                                       |
| **Good Commit Message Format** | `type(scope): short summary`_Example:_`fix(auth): handle expired tokens`                                         |
| **Commit Body**                | Explains _why_ the change was made, not just _how_                                                               |
| **Good Commit Practices**      | - One logical change per commit- Clear and specific message- Tests should pass- Avoid “fix bug” or “update code” |

---

Git Log & Filtering

| Command                                    | Description                                   |
| ------------------------------------------ | --------------------------------------------- |
| `git log`                                  | Show detailed commit history                  |
| `git log --oneline`                        | Show compact one-line commit history          |
| `git log --since="yesterday"`              | Show commits made since yesterday             |
| `git log --since="2 weeks ago"`            | Show commits made within last two weeks       |
| `git log --name-status --follow -- <file>` | Show file history even after rename/move      |
| `git log --grep "<pattern>"`               | Search commits by message (regex supported)   |
| `git log --diff-filter=<letter>`           | Filter commits by file status (A, M, D, etc.) |

---

Commit References (`~` and `^`)

| Symbol | Moves Through        | Follows                                | Works On       |
| ------ | -------------------- | -------------------------------------- | -------------- |
| `~`    | **Commits only**     | **First parent (same branch)**         | Linear history |
| `^`    | **Commits + merges** | **Can choose parent** (1st, 2nd, etc.) | Merge commits  |

| Example              | Meaning                           |
| -------------------- | --------------------------------- |
| `HEAD~1` or `HEAD^`  | One commit before current         |
| `HEAD~2` or `HEAD^^` | Two commits before current        |
| `HEAD^2`             | Second parent (merge branch)      |
| `HEAD~3`             | Three commits back in same branch |

---

Show & Diff Commands

| Command                        | Description                                        |
| ------------------------------ | -------------------------------------------------- |
| `git show <commit>`            | Display commit details and diff                    |
| `git show <commit> --stat`     | Show summary of changed files                      |
| `git show <commit>:<file>`     | Show a specific file from a commit                 |
| `git diff`                     | Show unstaged changes (working dir → staging area) |
| `git diff --staged`            | Show staged changes (staging area → last commit)   |
| `git diff <commit1> <commit2>` | Compare two commits                                |
| `git diff <branch1> <branch2>` | Compare two branches                               |

---

Branch Merge Info

| Command                       | Description                                |
| ----------------------------- | ------------------------------------------ |
| `git branch --merged main`    | List branches already merged into main     |
| `git branch --no-merged main` | List branches **not yet merged** into main |
