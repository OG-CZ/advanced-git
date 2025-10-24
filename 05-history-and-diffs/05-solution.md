### prerequisite

In this exercise, we'll practice making a good commit, take a look at some of the interesting command line arguments for git log, use git show to get more information about a commit, then take a quick look at git branch.

```bash
# force checkout it will overwrite any changes not recommaneded but if u need to switch having a uncommited work but staged files
1. git checkout -f exercise5
```

### Step 1 - Make a Good Commit

We've all made commits with short, silly, or otherwise unhelpful messages. Let's practice making a solid commit message for use in this example.

```bash
2. cat hello.txt                                                        # concatinate and print hello.txt

3. git mv hello.txt hello.template                                      # were gonna rename it and changing file extension

4. git commit -m "replacing greeting with some tokens for i18n"
    [exercise5 cd72151] replacing greeting with some tokens for i18n
    1 file changed, 2 insertions(+)
    create mode 100644 05-history-and-diffs/hello.template
```

### Step 2 - Git Log

Let's take a look at our new commit using git log. First we'll see how to see all commits in the log since yesterday:

```bash
5. git log --since="yesterday"                                           # see all commits done yesterday
commit f9dda1dcb7f7b056510097be4625408ad0da282a (origin/main, origin/exercise4, origin/HEAD, main, exercise4)
Author: fake <fakez@users.noreply.github.com>
Date:   Thu Oct 23 17:24:31 2025 +0800

    advanced-git

commit ba1be392576aa8c7e0fbd90ddd851c21db5de165
Author: fake <fake@users.noreply.github.com>
Date:   Thu Oct 23 17:23:36 2025 +0800

advanced-git

6. git log --name-status --follow --oneline hello.template               # we are tracking file as it changes
4b2b90e Replacing greeting with tokens for i18n
R073    hello.txt       hello.template
fec9e7b Changing Hello to Hola
M       hello.txt
afa34a6 Changing World to Mundo
M       hello.txt
e348ebc Testing the emergency git-casting system
M       hello.txt
43388fe Initial commit
A       hello.txt
```

grep stands for Global Regular Expression Print.
It searches your commit messages for the word "i18n" (case-sensitive by default).
It’s a command-line search tool used to find text patterns inside files or command outputs.

```bash
# Show me all commits in the last two weeks authored by og-cz where the commit message contains the word i18n
7. git log --grep="i18n" --author=og-cz --since=2.weeks
commit cd72151e9b1f4e16082973725eb10a37e0a249ac (HEAD -> exercise5)
Author: fake <fake@users.noreply.github.com>
Date:   Fri Oct 24 11:46:16 2025 +0800

    replacing greeting with some tokens for i18n
```

without the log grep

```bash
8. git log --author=og-cz --since=2.weeks                                  # shows all commit by the author in the past 2 weeks ago
commit cd72151e9b1f4e16082973725eb10a37e0a249ac (HEAD -> exercise5)
Author: fake <fake@users.noreply.github.com>
Date:   Fri Oct 24 11:46:16 2025 +0800

    replacing greeting with some tokens for i18n

........
.................. so on and so forth

commit 52ae477cd887be5c3a5ea075e33dc4b60ddce907
Merge: 2a46962 e05bcee
Author: fake <fake@users.noreply.github.com>
Date:   Thu Oct 23 17:01:51 2025 +0800

    Merge branch 'mundo' into exercise4

commit 2a469621e975c5ebaae50e330b879566989d9714
Author: fake <fake@users.noreply.github.com>
Date:   Thu Oct 23 17:01:51 2025 +0800

    Merge branch 'mundo' into exercise4
```

We can use --diff-filter to find commits where files have been renamed:

```bash
# This command shows the commit history of renamed files it filters and displays only commits where files were renamed.
9. git log --diff-filter=R --find-renames
commit ad08aabad6c9f47576382814eb5ae2e2d7d0ad7e (tag: my-exercise3-tag, tag: exercise3-annotated-tag)
Author: @users.noreply.github.com
Date:   Tue Oct 21 17:18:28 2025 +0800

    advanced-git

commit 5576059e6b9bbb495818211d23293b0799d51910
Author: @users.noreply.github.com
Date:   Tue Oct 21 16:44:10 2025 +0800

    advanced-git
....
```

Or to find commits where files have been modified:

```bash
# Shows a compact commit history (--oneline) but only for commits that modified existing files (--diff-filter=M).
10. git log --diff-filter=M --oneline
f9dda1d (origin/main, origin/exercise4, origin/HEAD, main, exercise4) advanced-git
ba1be39 advanced-git
8e3b9fe (mundo) txt
2a46962 change hello to hola
964e10a advanced-git
14a5cdf (exercise3) advanced-git
....
```

### Step 3 - Git Show

Now that we've mastered git log, how do we actually see what happened in a commit? Let's use git show to find out.

```bash
11. git log --grep=i18n --oneline                                   # find all commit with commit message of i18n and shows it`s info
90163cf (HEAD -> exercise5) i18n last added new line text for hello.template
a3c91d1 i18n another one
cd72151 replacing greeting with some tokens for i18n

# now let us see full commit and diff for cd72151 like how we doing in GITHUB, i modified hello.template in some time here
12. git show 90163cf
Author fake@users.noreply.github.com
Date:   Fri Oct 24 12:12:25 2025 +0800

    i18n last added new line text for hello.template

diff --git a/05-history-and-diffs/hello.template b/05-history-and-diffs/hello.template
index a8966c6..3ed5e3e 100644
--- a/05-history-and-diffs/hello.template
+++ b/05-history-and-diffs/hello.template                           # show all the changes
@@ -1,4 +1,5 @@
 hello mundo!
 ADDING SOME TEXT FOR PRACTICING GOOD COMMIT MESSAGE <<<
 adding a new one for git show and diff!!
```

### Step 4 - Git Branch

Let's say you're working on a complicated codebase with a master branch and lots of feature branches. Some of your coworkers forget to clean up their branches when they're done (we're all guilty). Which branches have been merged into master and can be cleaned up? Which branches haven't been merged yet? If you've been following along, yours may look different.

```bash
# how do we do "diff branches" or how do we find out which branches are merged in master or weren`t

13. git branch --merged master
  exercise2
  exercise3
  exercise4
  main
  mundo

14. git branch --no-merged master
  dangling-exercise3
* exercise5

```
