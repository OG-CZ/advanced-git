# Git Foundations

- [DATA STORAGE](#data-storage)
  - [What is git?](#what-is-git)
  - [how does git store information?](#how-does-git-store-information)
  - [the key - “SHA1”](#the-key---sha1)
- [GIT BLOBS AND TREES](#git-blobs-and-trees)
  - [the value - blob](#the-value---blob)
  - [under the hood - git hash - object](#under-the-hood---git-hash---object)
  - [where does git store its data?](#where-does-git-store-its-data)
  - [where are blobs stored?](#where-are-blobs-stored)
  - [we need something else](#we-need-something-else)
  - [tree](#tree)
  - [Identical content is only stored once!](#identical-content-is-only-stored-once)
  - [other optimizations - packfiles, deltas](#other-optimizations---packfiles-deltas)
- [GIT COMMITS](#git-commits)
  - [commit points to parent commits and trees](#commit-points-to-parent-commits-and-trees)
  - [commits under the hood - make a commit](#commits-under-the-hood---make-a-commit)
  - [commits under the hood - look in .git/objects](#commits-under-the-hood---look-in-gitobjects)
  - [git cat-file -t (type) and -p (print the contents)](#git-cat-file---t-type-and---p-print-the-contents)
  - [why cant we “change” commits?](#why-cant-we-change-commits)
  - [references - pointers to commits](#references---pointers-to-commits)
  - [references - under the hood](#references---under-the-hood)
- [EXERCISE](#exercise)
- [Commands Exercise](#commands-exercise)
  - [Basic Setup](#basic-setup)
  - [Explore Objects](#explore-objects)
  - [Check References](#check-references)
  - [View History](#view-history)
  - [Branches](#branches)

## DATA STORAGE

### What is git?

distributed version control system

### how does git store information?

- at its core, git is like a key value store
- the **value = data**
- the **key = hash of the data**
- you can use the key to retrieve the content

### the key - “SHA1”

- Secure Hash Algorithm - mathematical formulas that turn any data into a fixed-size fingerprint.
- Is a cryptographic hash function
- given a piece of data, it produces a 40-digit hexadecimal number
- this value should always be the same if the given input is the same
    
    They’re used in:
    
    - Git (for commit and file IDs)
    - Password verification
    - Digital signatures
    - File integrity checks
    - Network security (SSL/TLS, etc.)

## GIT BLOBS AND TREES

### the value - blob

binary large object - its simply a content of a fle

- git stores the **compressed** data in a blob, along with the metadata in a header:
    - the identifier **blob**
    - the size of the content
    - \0 delimeter
    - content
    
    ![image.png](/README/01.png)
    

```bash
echo -e 'blob 14\0Hello, World!' | openssl sha1
```

### under the hood - git hash - object

![image.png](/README/02.png)

![image.png](/README/03.png)

for windows:

### where does git store its data?

- it stores it in the **.git** directory
    
    ![image.png](/README/03.png)
    

### where are blobs stored?

-w flags means right

![image.png](/README/04.png)

### we need something else

- blob is missing information!
    - filenames
    - directory structures

if we save a file as a blob, how do we know what that file is aclled and what directory

well, Git stores information in something called as a tree

### tree

![image.png](/README/05.png)

its basically a directory it records which files (blobs), which subfolders(trees) exist and their metadata

a **tree** contains pointers using **SHA1:**

- to blobs
- also pointers to other trees

and metadata:

- type of pointer (blob or tree)
- filename or directory name
- mode (executable file, symbolic link, …)

![image.png](/README/06.png)

### Identical content is only stored once!

![image.png](/README/07.png)

### other optimizations - packfiles, deltas

- git objects are compressed
- as files change, their contents remain mostly similar
- git optimizes for this by compressing these files together, into a Packfile
- the packfile stores the object, and “deltas”, or the differences between one version of the file and next
- Packfiles are generated when:
    - you have to many objects, during gc, or during a push to remote

## GIT COMMITS

commits are just like any type of git objects taht we talked about

a **commit** points to:

- a tree

and contains metadata:

- author and commiter
- date
- message
- parent commit (one or more)

The SHA1 of the commit is hte hash of all this information

![image.png](/README/08.png)

### commit points to parent commits and trees

![image.png](/README/09.png)

A **commit** in Git is a **snapshot of your entire project’s state** (all files and folders) at a specific moment in time

### commits under the hood - make a commit

![image.png](/README/10.png)

### commits under the hood - look in .git/objects

![image.png](/README/11.png)

### git cat-file -t (type) and -p (print the contents)

![image.png](/README/12.png)

![image.png](/README/13.png)

- 581caa → That’s actually a **prefix** (short version) of a **full SHA-1 hash, its fine shorter as long its unique**

if we print the contents of that we get here 

- 100644 → this is mode it says if its executable
- blob → stored in the tree which points to hello.txt

![image.png](/README/15.png)

### why cant we “change” commits?

- if you change any data about the commit, the commit will ahve a new SHA1 hash
- even if the file dont change, the created date will

### references - pointers to commits

- tags
- branches
- HEAD - a pointer to the current commit
    
    ![image.png](/README/16.png)
    

thats why its fast to switch branches is just changing pointer

### references - under the hood

![image.png](/README/17.png)


## EXERCISE

1.) configure your editor

```json
Set which editor git should use.

This is the program that will open during a commit with no -m flag, a merge, a rebase, etc...

Select from any installed editor. Examples:

emacs: emacs
vi: vi or vim
atom: atom --wait
sublime: subl -n -w
vscode: code --wait
Run: $ $ git config --global core.editor <YOUR_EDITOR>
```

2.) complete exercise 

https://github.com/OG-CZ/advanced-git/blob/main/_EXERCISES/Exercise01-SimpleCommit.md

3.) Check out cheat sheet for using less

Tips for using `less` on the command line.

**To navigate:**

- f = for next page
- b = for previous page

**To search:**

- /<*query*>
- n = next match
- p = previous match

**To quit:**

- q = to quit

If you really want to deep dive, check out the man (a.k.a manual) page for [less](https://linux.die.net/man/1/less).

https://linux.die.net/man/1/less


## Commands Exercise

### Basic Setup

- `git init` - start new repo
- `echo 'text' > file.txt` - create file with content
- `git add file.txt` - stage file

### Explore Objects

- `tree .git/objects` or `ls -R .git/objects` - view git objects
- `git cat-file -t <hash>` - check object type
- `git cat-file -p <hash>` - view object contents

### Check References

- `cat .git/HEAD` - see current branch
- `cat .git/refs/heads/main` - see commit hash

### View History

- `git log --oneline` - see commit history

### Branches

- `git branch <name>` - create new branch
- `ls -R .git/refs` - view all branches/refs