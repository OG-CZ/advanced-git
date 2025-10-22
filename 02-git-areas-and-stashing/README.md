# Git Areas and Stashing

- [Git Areas and Stashing](#git-areas-and-stashing)
  - [Working Area, Staging Area, Repository](#working-area-staging-area-repository)
    - [3 AREAS WHERE CODE LIVES](#3-areas-where-code-lives)
    - [working area](#working-area)
    - [the staging area (a.k.a - index, cache)](#the-staging-area-aka---index-cache)
    - [the repository](#the-repository)
  - [CLOSER LOOK: STAGING AREA](#closer-look-staging-area)
    - [staging area](#staging-area)
    - [git - moving files in & out of the staging area](#git---moving-files-in--out-of-the-staging-area)
    - [git add - p](#git-add---p)
    - [what is hunk?](#what-is-hunk)
    - [“unstage” files from the staging area](#unstage-files-from-the-staging-area)
    - [basics: how content moves in git](#basics-how-content-moves-in-git)
  - [Stashing](#stashing)
    - [GIT STASHING](#git-stashing)
    - [git stash - basic use](#git-stash---basic-use)
    - [advanced stashing - keeping files](#advanced-stashing---keeping-files)
    - [advanced stashing - operations](#advanced-stashing---operations)
    - [advanced stashing - cleaning the stash](#advanced-stashing---cleaning-the-stash)
    - [examining stash contents - git show](#examining-stash-contents---git-show)
    - [git stash - what she use?](#git-stash---what-she-use)
- [COMMAND SUMMARY](#command-summary)

# Working Area, Staging Area, Repository

## 3 AREAS WHERE CODE LIVES

1. working area
2. staging area
3. repository

![image.png](attachment:b5982418-90a4-4aaa-a648-3b626738b9e9:image.png)

### working area

- the files in your working area that are also not in staging area not handle by git
- also called **untracked files**

### the staging area (a.k.a - index, cache)

- what files are goign to be part of the next commit
- the staging area is how git knows what will change between the current commit and the next commit

### the repository

this is in our .git directory

- the file git knows about!
- contains all of your commits

## CLOSER LOOK: STAGING AREA

### staging area

- the staging area is how git knows what will change between the current commit and the next commit
- tip: a “clean” staging area isn`t empty!
- consider the baseline staging area as being an exact copy of the latest commit

![image.png](attachment:b8c8e0ad-70b9-44ab-a0bc-d2cf4a2e63c3:image.png)

### git - moving files in & out of the staging area

add a file to the next commit:

- git add <file>

delete a file in the next commit:

- git rm <file>

rename a file in the next commit:

- git mv <file>

### git add - p

- allows you to stage commit in hunks interactively
- its especially useful if you`ve done too much work for one commit

![image.png](attachment:3df1b1c0-dbc1-4a50-966a-970c82fdb84b:image.png)

### what is hunk?

basically, a _chunk_ or _section_ of modified lines that Git groups together when showing diffs or prompting you during an interactive add/commit.

```json
def greet():
-    print("Hello world")
+    print("Hello there!")

def bye():
-    print("Bye")
+    print("Goodbye!")
```

### “unstage” files from the staging area

- not moving the files
- you`re replacing them with a copy that`s currently in the repository
  ![image.png](attachment:80b0f237-a541-4d9b-a050-e3d1faded898:image.png)

### basics: how content moves in git

![image.png](attachment:1deb4f5a-e9a7-4580-b83e-9cc967b79dcf:image.png)

# Stashing

## GIT STASHING

- save un-committed work
- the stash is **safe** from destructive operations

### git stash - basic use

stash changes

- git stash

list changes

- git stash list

show the contents

- git stash show stash@{0}

apply the last stash

- git stash apply

apply a specific stash

- git stash apply stash@{0}

### advanced stashing - keeping files

keep untracked files

- git stash - -include-untracked

keep all files (even ignored ones!)

- git stash - -all

### advanced stashing - operations

name stashes for easy references

- git stash save “WIP: making progress on foo”

start a new branch from a stash

- git stash branch <branch-name> <optional stash name>

grab a single file from a stash

- git checkout <stash name> - - <filename>

### advanced stashing - cleaning the stash

remove the last stash and applying changes:

- git stash pop
- tip: doenst remove if there is a merge conflict

remove the last stash

- git stash drop

remove the n-th stash

- git stash drop @ stash@{n}

remove all stashes

- git stash clear

### examining stash contents - git show

![image.png](attachment:26e7df39-0a49-4d87-b273-7560ca84c649:image.png)

### git stash - what she use?

keep untracked files

- git stash - -includes-untracked

name stashes for easy reference

- git stash save “WIP: making progress on foo”
  lets you **interactively choose which changes (hunks)** to stash
- git stash -p

# COMMAND SUMMARY

### Working Area

- Files you’re currently editing — not yet tracked or staged.

### Staging Area (Index / Cache)

- Where Git tracks what will be in your next commit.

### Repository

- Stored inside `.git/`, contains all commits and history.

### Staging & File Management

- `git add <file>` — stage file for next commit
- `git add -p` — stage patches interactively
- `git rm <file>` — remove file in next commit
- `git mv <old> <new>` — rename tracked file
- `git restore --staged <file>` — unstage file (keep in working directory)

### Basic Stashing

- `git stash` — stash current tracked changes
- `git stash list` — show all stashed items
- `git stash show stash@{0}` — view summary of a stash
- `git stash apply` — apply latest stash
- `git stash apply stash@{n}` — apply specific stash

### Advanced Stashing Options

- `git stash --include-untracked` — stash tracked + untracked files
- `git stash --all` — stash all (even ignored) files
- `git stash save "WIP: message"` — name your stash for easy reference
- `git stash branch <branch-name> [stash@{n}]` — create new branch from stash
- `git checkout stash@{n} -- <filename>` — restore specific file from stash

### Cleaning Up the Stash

- `git stash pop` — apply & remove last stash
- `git stash drop` — delete latest stash only
- `git stash drop stash@{n}` — delete specific stash
- `git stash clear` — remove all stashes

### Examining Stash Contents

- `git show stash@{0}` — show full diff of a stash
- `git stash -p` — interactively choose hunks to stash
