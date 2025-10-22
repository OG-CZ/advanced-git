### Step 1 - Where's your HEAD?

```bash
1. cat .git/HEAD # show waht head
ref: refs/heads/exercise3

2. git branch # shows what branch
  01-tutorial
  exercise2
* exercise3
  main
```

### Step 2 - Where are your refs?

```bash
3. git show-ref --heads # Use git show-ref to see which commits your HEADs are pointing at
43388fee19744e8893467331d7853a6475a227b8 refs/heads/exercise2
e348ebc1187cb3b4066b1e9432a614b464bf9d07 refs/heads/exercise3
43388fee19744e8893467331d7853a6475a227b8 refs/heads/master
43388fee19744e8893467331d7853a6475a227b8 refs/remotes/origin/exercise2
e348ebc1187cb3b4066b1e9432a614b464bf9d07 refs/remotes/origin/exercise3
43388fee19744e8893467331d7853a6475a227b8 refs/remotes/origin/master

4. git cat-file -p 43388fee19744e8893467331d7853a6475a227b8 # Whereas our exercise3 branch is pointing to our newer commit from Exercise 2:
tree 581caa0fe56cf01dc028cc0b089d364993e046b6
author Nina Zakharenko <nina@nnja.io> 1507168309 -0700
committer Nina Zakharenko <nina@nnja.io> 1507168309 -0700

Initial commit # commit message


5. git cat-file -p e348ebc1187cb3b4066b1e9432a614b464bf9d07 # Whereas our exercise3 branch is pointing to our newer commit from Exercise 2:
tree cbcdf5dda7853d595fe0b1942cb0d1d72eb910f3
parent 43388fee19744e8893467331d7853a6475a227b8
author Nina Zakharenko <nina@nnja.io> 1507168872 -0700
committer Nina Zakharenko <nina@nnja.io> 1507168872 -0700

Testing the emergency git-casting system
```

### Step 3 - Lightweight Tags:

Lightweight tags are simply named pointers to a commit. Make a new tag, then confirm that it points to the correct commit using show-ref:

```bash
6. git tag my-exercise3-tag

7. git show-ref --tags
ad08aabad6c9f47576382814eb5ae2e2d7d0ad7e refs/tags/my-exercise3-tag

# You can also do a reverse lookup using git tag --points-at:
8. git tag --points-at ad08aab
my-exercise3-tag
```

### Step 4 - Annotated Tags:

Annotated tags serve the same function as regular tags, but they also store additional metadata:

```bash
9. git tag -a "exercise3-annotated-tag" -m "this is my annotated tag for exericse3" # creating annotated

10. git show exercise3-annotated-tag # Using git show, we can see all of the pertinent information about our exercise3-annotated-tag.
# results
tag exercise3-annotated-tag
Tagger: og-cz <secret@noreply.github.com>
Date:   Wed Oct 22 15:51:04 2025 +0800

this is my annotated tag for exericse3 # commit message

commit ad08aabad6c9f47576382814eb5ae2e2d7d0ad7e (HEAD -> exercise3, tag: my-exercise3-tag, tag: exercise3-annotated-tag, origin/main, origin/HEAD, main)
Author: og-cz <secret@users.noreply.github.com>
Date:   Tue Oct 21 17:18:28 2025 +0800

    advanced-git # commit message
```

### Step 5 - Detached HEAD

Now we're going to venture into a "detached HEAD" state. Use git checkout to checkout the latest commit directly.

```bash
11. git log --oneline # displays each commit on a single line, showing only the first seven characters of the commit hash and the commit message
ad08aab (HEAD -> exercise3, tag: my-exercise3-tag, tag: exercise3-annotated-tag, origin/main, origin/HEAD, main) advanced-git
a0abfe2 advanced-git
5576059 advanced-git
543e3b3 Merge pull request #2 from OG-CZ/exercise2
7d4208c (origin/exercise2, exercise2) advanced-git
655becb Merge pull request #1 from OG-CZ/exercise2
b9b4594 advanced-git
65ce9e7 advanced-git
08bd3d8 advanced-git
e6bf228 advanced-git
9396e01 advanced-git

12. git checkout 5576059
M       01-git-foundations/README.md
M       02-git-areas-and-stashing/README.md
Note: switching to '5576059'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 5576059 advanced-git

13. cat .git/HEAD
5576059e6b9bbb495818211d23293b0799d51910 # this shows instead of .git/HEADS/branch_name
# but if you commit something right now, Git won’t know which branch to attach it to.
```

### Step 6 - Create a Dangling Commit

Even though our HEAD is now pointing at a specific commit instead of a branch or tag - we can still make commits. Go ahead and make a new commit, then confirm that our HEAD is now pointing at this new commit:

```bash
14. echo "this is a test file" > 03-references-commits-branches/dangle.txt # created a txt file

15. git add dangle.txt

16. git commit -m "this is a dangling commit exercise"
[detached HEAD e34f6b7] this is a dangling commit exercise
 1 file changed, 1 insertion(+)
 create mode 100644 03-references-commits-branches/dangle.txt

17. git log --oneline
e34f6b7 (HEAD) this is a dangling commit exercise # <- this is it
5576059 advanced-git
543e3b3 Merge pull request #2 from OG-CZ/exercise2
7d4208c (origin/exercise2, exercise2) advanced-git
655becb Merge pull request #1 from OG-CZ/exercise2

18. cat .git/HEAD
e34f6b73d4220208132bc83e56c7ccbb3949bdbd
```

But wait. Because our new commit - e34f6b in this case - was made in a detached HEAD state, it doesn't have any references pointing to it. It's not part of a branch, and has no tags. This is called a Dangling Commit. You'll see this warning if you try to switch branches:

```bash
# git checkout exercise3

Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  9bdea9e This is a dangling commit

If you want to keep it by creating a new branch, this may be a good time
to do so with:

# git branch <new-branch-name> 9bdea9e

19. git branch dangling-exercise3 e34f6b73d4220208132bc83e56c7ccbb3949bdbd

20. git branch
* (HEAD detached from 5576059)
  01-tutorial
  dangling-exercise3 # here`s it
  exercise2
  exercise3
  main
```
