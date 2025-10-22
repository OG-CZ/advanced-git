# References, Commits, Branches

- [References, Commits, Branches](#references-commits-branches)
  - [References](#references)
    - [three types of git references](#three-types-of-git-references)
    - [what is a branch?](#what-is-a-branch)
    - [what is a HEAD?](#what-is-a-head)
    - [cat .git/HEAD](#cat-githead)
    - [SAMPLE REPO FLOW](#sample-repo-flow)
    - [SUMMARY OF REFERENCES](#summary-of-references)
  - [TAGS AND ANNOTATED TAGS](#tags-and-annotated-tags)
    - [lightweight tags](#lightweight-tags)
    - [annotated tags: git tag -a](#annotated-tags-git-tag--a)
    - [working with tags](#working-with-tags)
    - [tags and branches](#tags-and-branches)
  - [DETACHED HEAD & DANGLING COMMITS](#detached-head--dangling-commits)
    - [head-less / detached head](#head-less--detached-head)
    - [dangling commits](#dangling-commits)
- [COMMAND SUMMARY](#command-summary)

## References

references are pointers to commits

### three types of git references

- tags and annotated tags
- branches
- HEAD

### what is a branch?

- a branch is just a pointer to a particular commit
- the pointer of the current branch changes as new commits are made
  ![image.png](attachment:e6a89273-9243-4457-90c9-3bc53414b361:image.png)

### what is a HEAD?

- head is how git knows what branch you`re currently on, and what the next parent the commit will be
- its a pointer
  - it points at the current name of the branch
  - but, it can point at a commit too (detached HEAD)
- it moves when:
  - you make a commit in the currently active branch
  - when you checkout a new branch
    ![image.png](attachment:85cbdebb-f550-4a40-86b7-13f245e360c5:image.png)

### cat .git/HEAD

**cat → concatenate and print file content**

**.git/head → tells which branch or commit our repo is currently pointing to**

![image.png](attachment:9f5c09c9-1157-4a03-9f7e-c0f18c75de7e:image.png)

- when we checkout on master, the HEAD pointers to refs/heads/master
- when we checkout on feature, the HEAD points to refs/heads/features

### SAMPLE REPO FLOW

![image.png](attachment:4ec66ba7-6010-41d0-8a66-ec8fa2506a48:image.png)

- we just create a folder posts then we created a txt file inside post named welcome.txt with a content of ‘This is……entry.’
- then added a index.txt file ‘welcome.txt Welcome to my blog’ which is outside the posts

**THEN AFTER COMMITING**

- the initial commit was cd0b5

![image.png](attachment:5f02ba03-1aff-4bf1-b8d8-886758740d78:image.png)

**THEN AFTER SWITCHING**

![image.png](attachment:d1a23ae8-fd3f-4f58-a4ff-43040b61fd66:image.png)

- after checking out a new branch, we was on the master branch and now its pointing on the same commit that master was pointing at
- and HEAD is now going to point tech_posts

**COMMITING AT CURRENT REPO STATE**

![image.png](attachment:871a1a9e-87e2-467c-b5e4-b63825697f9e:image.png)

- now we added a commit about python file and we made a commit and that commit 2b0b3 has a parent of cd0b5
  ![image.png](attachment:0d2996de-c148-45b0-8eb5-b2c1708e9865:image.png)
- and now tech \_post have moved, because we are in the tech_post branch it will move along with it

**SUMMARY OF REFERENCES**

**In Git, each branch is just a moving pointer (reference) to a commit in a timeline of snapshots when you create a new branch it starts from the current commit, and as you make new commits, that branch’s pointer moves forward while the old one stays where it was.**

## TAGS AND ANNOTATED TAGS

A Git tag is a permanent pointer to a commit lightweight tags are simple names, while annotated tags are full objects with extra metadata used for official releases.

2 kinds of tags lightweight and annotated

### lightweight tags

- lightweight tags are just simple pointers to a commit
- A **tag** is just like a **branch**, but it’s a **permanent pointer (stays on one specific commit forever)**
  ![image.png](attachment:44744b42-94ec-44b9-ab34-6b9981b64d16:image.png)

**USAGE: quick bookmarks or temporary tags**

### annotated tags: git tag -a

- points to commit, but store additional information
  - author, message, date
    ![image.png](attachment:21e68f78-096f-484b-b08b-17ac66b214a0:image.png)

**USAGE: Official release (e.g. v1.0.0)**

### working with tags

list all tags in a repo

- git tag

list all tags, and waht commit theyre poitning to

- git show-ref - -tags

list all the tags pointing at a commit

- git tag - -points-at <commit>

looking at the tag, or tagged contents:

- git show <tag-name>

### tags and branches

- branch
  - the current branch poitner moves with every commit to the repository
- tag
  - tags are pointer to a commit that a tag points doesn`t change
  - its a snapshot!

## DETACHED HEAD & DANGLING COMMITS

A **detached HEAD** happens when **HEAD points directly to a commit**, not to a branch.

### head-less / detached head

- sometimes you need to checkout a specific commit (or tag) instead of a branch
- git moves the HEAD pointer to that commit
- as soon as you checkout a different branch or commit, the value of HEAD will point to the new SHA
- **there is no reference pointing to the commits you made in a detached state**

![image.png](attachment:53654429-1788-42f9-ba42-0dbba68d647e:image.png)

- git tells im in a detached state

![image.png](attachment:4ea19162-fe2f-4d91-aceb-5a2ee00cda4b:image.png)

what if we wanna save our work? and we are on a deteached head?

save your work:

- create a new branch that points to the last commit you made in a detachted state
  - git branch <new-branch-name> <commit>
- why the last commit?
  - because other commits point to their parents

### dangling commits

A **dangling commit** is a commit that **isn’t pointed to by any branch, tag, or reference** it’s still _in your repo’s database_ (`.git/objects`), but Git has **lost the “name” (reference)** that leads to it.

discard your work:

- if you dont point a new branch at those commits they will no longer be referenced in git. (dangling commits)
- eventually, they will be garbaged collected
- we can still recover this by reflog if not garbaged collected

## COMMAND SUMMARY

Checking References and Commits
| Command | Description |
| ------------------------ | -------------------------------------------------------------- |
| `git log --oneline` | Show commits in compact one-line format (short hash + message) |
| `git show <commit>` | Display details of a specific commit |
| `git cat-file -p <hash>` | Print the content of a Git object (commit, blob, tag, etc.) |
| `cat .git/HEAD` | Shows what branch or commit HEAD is currently pointing to |

Branch Operations
| Command | Description |
| ------------------------------------------------ | ----------------------------------------- |
| `git branch` | List all branches |
| `git branch <new-branch>` | Create a new branch |
| `git switch <branch>` or `git checkout <branch>` | Switch to another branch |
| `git switch -c <new-branch>` | Create **and** switch to a new branch |
| `git branch -d <branch>` | Delete a branch |
| `git merge <branch>` | Merge another branch into the current one |

Tag Operations
| Command | Description |
| ------------------------------------ | ----------------------------------------- |
| `git tag` | List all tags |
| `git tag <tag-name>` | Create a lightweight tag |
| `git tag -a <tag-name> -m "message"` | Create an annotated tag |
| `git show <tag-name>` | View tag details and tagged commit |
| `git show-ref --tags` | List all tags and their commit hashes |
| `git tag --points-at <commit>` | Show tags that point to a specific commit |
| `git push origin <tag-name>` | Push a tag to remote |
| `git push origin --tags` | Push all tags to remote |

Detached HEAD & Recovery
| Command | Description |
| ----------------------------- | ------------------------------------------------------------- |
| `git checkout <commit-hash>` | Move HEAD to a specific commit (detached state) |
| `git switch -c <branch-name>` | Save your detached HEAD work to a new branch |
| `git reflog` | Show recent HEAD history (recover dangling commits) |
| `git branch <name> <commit>` | Create a branch pointing to a specific commit |
| `git gc` | Run garbage collection (removes unreachable/dangling objects) |

Inspecting Repository Internals
| Command | Description |
| ------------------------ | ------------------------------------------ |
| `ls .git/refs/heads/` | List local branch references |
| `ls .git/refs/tags/` | List tag references |
| `tree .git/objects` | Show Git’s internal object storage |
| `git cat-file -t <hash>` | Show type of object (commit/blob/tree/tag) |
| `git cat-file -p <hash>` | Pretty-print the object contents |

Quick Reference: HEAD Behavior
| Situation | `.git/HEAD` content | Meaning |
| ------------------------ | -------------------------- | ----------------------------------- |
| On a branch | `ref: refs/heads/main` | HEAD points to a branch |
| Detached HEAD | `<commit-hash>` | HEAD points directly to a commit |
| After switching branches | `ref: refs/heads/<branch>` | HEAD now follows new branch pointer |
