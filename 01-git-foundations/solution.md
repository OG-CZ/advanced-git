# Git Commands Reference

## Initialize a new Git repository
1.) `git init`

## Create a new text file named hello.txt with the content "hello, world!"
2.) `echo 'hello, world!' > hello.txt`

## Stage the file to Git's index (staging area)
3.) `git add hello.txt`

**Output:**
```
[main dcd2d72] advanced-git
1 file changed, 2 insertions(+)
```

## Show all the files and folders inside .git/objects in a tree-like view
4.) `tree .git/objects`   # for Windows CMD  
`ls -R .git/objects`   # for Git Bash / Linux / macOS

## Check what kind of Git object a given hash refers to
5.) `git cat-file -t <hash>`

**Output:**
```
blob
```

## Display the contents (pretty print) of a Git object
6.) `git cat-file -p <hash>`

**Output:**
```
$> git stash apply stash@{0}
On branch exercise2
Your branch is up-to-date with 'origin/exercise2'.
Changes not staged for commit:
(use "git add <file>..." to update what will be committed)
(use "git checkout -- <file>..." to discard changes in working directory)

        modified:   hello.txt

no changes added to commit (use "git add" and/or "git commit -a")

$> git diff hello.txt
diff --git a/hello.txt b/hello.txt
index 980a0d5..b31a35b 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
Hello World!
+This is a test of the emergency git-casting system.
```

## View the contents of the HEAD file to see the current branch reference
7.) `cat .git/HEAD`

**Output:**
```
ref: refs/heads/main
```

## View the commit hash stored in the branch reference
8.) `cat .git/refs/heads/main`

**Output:**
```
dcd2d7238427f488b7a70ceaa6dfb4b73c197580
```

## Show a condensed, one-line-per-commit log of your repository's history
9.) `git --no-pager log --oneline`

**Output:**
```
dcd2d72 (HEAD -> main) advanced-git
47800aa advanced-git
876a61d (origin/main, origin/HEAD) advanced-git
e53c784 advanced-git
9e5120e advanced-git
c258315 advanced-git
f1697d1 (origin/backup-01) Add HEAD^ to git reset --hard in instructions (#11)
2ee3f1d Update README with links to the course video
116c952 Merge pull request #3 from huchenme/patch-1
a1695e4 Merge pull request #5 from tsauerwein/patch-1
1e08419 Merge pull request #6 from anthonyking-xo/patch-1
1b46fb4 Fix typo
9a793fd Fix spelling
dd2179e Update Exercise3-References.md
55ca04d Merge pull request #2 from raerpo/bugfix/wrong-exercise-name
62564ed fix exercise name on exercise 3
65db70d Merge pull request #1 from solonaarmstrong/patch-1
7765d63 Update Exercise2-StagingAndStashing.md
76e0330 Fix Link in Ex2
e9806af Initial commit
```

## Create new branch
10.) `git branch 01-tutorial`

## View all branches and references
11.) `ls -R .git/refs`

**Output:**
```
.git/refs:
heads/  remotes/  tags/

.git/refs/heads:
01-tutorial  main

.git/refs/remotes:
origin/

.git/refs/remotes/origin:
HEAD  main

.git/refs/tags:
```


<!-- "" Initialize a new Git repository
1.) git init

"" Create a new text file named hello.txt with the content “hello, world!”
2.) echo 'hello, world!' > hello.txt

"" Stage the file to Git’s index (staging area)
3.) git add hello.txt
> outputs:
    [main dcd2d72] advanced-git
    1 file changed, 2 insertions(+)

"" Show all the files and folders inside .git/objects in a tree-like view
4.) tree .git/objects   # for Windows CMD
   ls -R .git/objects   # for Git Bash / Linux / macOS

"" Check what kind of Git object a given hash refers to
5.) git cat-file -t <hash?>
> outputs:
    blob

"" Display the contents (pretty print) of a Git object
6.) git cat-file -p <hash?>
> outputs: 
    $> git stash apply stash@{0}
    On branch exercise2
    Your branch is up-to-date with 'origin/exercise2'.
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   hello.txt

    no changes added to commit (use "git add" and/or "git commit -a")

    $> git diff hello.txt
    diff --git a/hello.txt b/hello.txt
    index 980a0d5..b31a35b 100644
    --- a/hello.txt
    +++ b/hello.txt
    @@ -1 +1,2 @@
    Hello World!
    +This is a test of the emergency git-casting system.
    ```

    Now the change we made earlier to `hello.txt` is back in the working area, but not yet staged for commit.

    Tip: `git stash apply` will automatically apply the last stashed change, so no need to use `stash@{0}` if you want to apply something you just stashed.

    #### End of Exercise Two

"" View the contents of the HEAD file to see the current branch reference
7.) cat .git/HEAD
> outputs:
    ref: refs/heads/main

"" View the commit hash stored in the branch reference
8.) cat .git/refs/heads/main
> outputs:
    dcd2d7238427f488b7a70ceaa6dfb4b73c197580

"" Show a condensed, one-line-per-commit log of your repository’s history
9.) git --no-pager log --oneline 
> outputs:
    dcd2d72 (HEAD -> main) advanced-git
    47800aa advanced-git
    876a61d (origin/main, origin/HEAD) advanced-git
    e53c784 advanced-git
    9e5120e advanced-git
    c258315 advanced-git
    f1697d1 (origin/backup-01) Add HEAD^ to git reset --hard in instructions (#11)
    2ee3f1d Update README with links to the course video
    116c952 Merge pull request #3 from huchenme/patch-1
    a1695e4 Merge pull request #5 from tsauerwein/patch-1
    1e08419 Merge pull request #6 from anthonyking-xo/patch-1
    1b46fb4 Fix typo
    9a793fd Fix spelling
    dd2179e Update Exercise3-References.md
    55ca04d Merge pull request #2 from raerpo/bugfix/wrong-exercise-name
    62564ed fix exercise name on exercise 3
    65db70d Merge pull request #1 from solonaarmstrong/patch-1
    7765d63 Update Exercise2-StagingAndStashing.md
    76e0330 Fix Link in Ex2
    e9806af Initial commit
"" Initialize a new Git repository
1.) git init

"" Create a new text file named hello.txt with the content “hello, world!”
2.) echo 'hello, world!' > hello.txt

"" Stage the file to Git’s index (staging area)
3.) git add hello.txt
> outputs:
    [main dcd2d72] advanced-git
    1 file changed, 2 insertions(+)

"" Show all the files and folders inside .git/objects in a tree-like view
4.) tree .git/objects   # for Windows CMD
   ls -R .git/objects   # for Git Bash / Linux / macOS

"" Check what kind of Git object a given hash refers to
5.) git cat-file -t <hash?>
> outputs:
    blob

"" Display the contents (pretty print) of a Git object
6.) git cat-file -p <hash?>
> outputs: 
    $> git stash apply stash@{0}
    On branch exercise2
    Your branch is up-to-date with 'origin/exercise2'.
    Changes not staged for commit:
    (use "git add <file>..." to update what will be committed)
    (use "git checkout -- <file>..." to discard changes in working directory)

            modified:   hello.txt

    no changes added to commit (use "git add" and/or "git commit -a")

    $> git diff hello.txt
    diff --git a/hello.txt b/hello.txt
    index 980a0d5..b31a35b 100644
    --- a/hello.txt
    +++ b/hello.txt
    @@ -1 +1,2 @@
    Hello World!
    +This is a test of the emergency git-casting system.
    ```

    Now the change we made earlier to `hello.txt` is back in the working area, but not yet staged for commit.

    Tip: `git stash apply` will automatically apply the last stashed change, so no need to use `stash@{0}` if you want to apply something you just stashed.

    #### End of Exercise Two

"" View the contents of the HEAD file to see the current branch reference
7.) cat .git/HEAD
> outputs:
    ref: refs/heads/main

"" View the commit hash stored in the branch reference
8.) cat .git/refs/heads/main
> outputs:
    dcd2d7238427f488b7a70ceaa6dfb4b73c197580

"" Show a condensed, one-line-per-commit log of your repository’s history
9.) git --no-pager log --oneline 
> outputs:
    dcd2d72 (HEAD -> main) advanced-git
    47800aa advanced-git
    876a61d (origin/main, origin/HEAD) advanced-git
    e53c784 advanced-git
    9e5120e advanced-git
    c258315 advanced-git
    f1697d1 (origin/backup-01) Add HEAD^ to git reset --hard in instructions (#11)
    2ee3f1d Update README with links to the course video
    116c952 Merge pull request #3 from huchenme/patch-1
    a1695e4 Merge pull request #5 from tsauerwein/patch-1
    1e08419 Merge pull request #6 from anthonyking-xo/patch-1
    1b46fb4 Fix typo
    9a793fd Fix spelling
    dd2179e Update Exercise3-References.md
    55ca04d Merge pull request #2 from raerpo/bugfix/wrong-exercise-name
    62564ed fix exercise name on exercise 3
    65db70d Merge pull request #1 from solonaarmstrong/patch-1
    7765d63 Update Exercise2-StagingAndStashing.md
    76e0330 Fix Link in Ex2
    e9806af Initial commit
 -->