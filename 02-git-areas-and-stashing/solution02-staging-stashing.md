### Step 1 - View the Contents of the Staging Area
1.) git ls-files -s

### Step 2 - Interactive Staging
making change to hello.txt file, then use git add -p to stage your change interactively
2.) echo "This is a test of the emergency git-casting system." >> hello.txt
3.) y
4.) git status
5.) git ls-files -s

### Step 3 - Unstage your Change
6.) git stash save "emergency git-casting" or git stash push -m ""
7.) git stash list
8.) git stash show stash@{0}
9.) or git stash apply will automatically apply the last stashed change

<!-- 
""" create hello.txt with a content hello, world!
1.) echo 'hello, world!' > hello.txt

2.) git add -p

3.) y

4.) git ls-files -s
> output:
    (1/1) Stage this hunk [y,n,q,a,d,e,p,?]? git ls-files -s 
    No other hunks to goto

5.) git stash 
> output: 
    Saved working directory and index state WIP on main: 65ce9e7 advanced-git

6.) git stash list
> output:
    stash@{0}: WIP on main: 65ce9e7 advanced-git

8.) git stash show stash@{0}
> output:
     01-git-foundations/solution.md | 291 -----------------------------------------
    README.md                      |   1 +
    2 files changed, 1 insertion(+), 291 deletions(-)

9.) git stash apply stash@{0}
> output:
    On branch exercise2
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        deleted:    ../01-git-foundations/solution.md
        modified:   ../README.md

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        ../01-git-foundations/simple-git-commit-solution.md
        ./ -->