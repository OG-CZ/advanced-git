### Step 1 - Fast-Forward Merge

```bash
1. git branch exercise4                                                     # create new branch

2. git checkout exercise4                                                   # checkout in the new branch

3. git --no-pager log --oneline                                             # shows your entire commit history in a short, clean list without opening the pager view.

4. git merge exercise3                                                      # Fast forward will happen if
Updating 43388fe..e348ebc
Fast-forward                                                                # Let’s say you started exercise4 from exercise3, but then added more commits on exercise4
 hello.txt | 1 +
 1 file changed, 1 insertion(+)

5. git log --oneline                                                        # We see our new commit, e348ebc, with it's parent, 43388fe, our initial commit. But there's nothing to let us know that e348ebc was merged into exercise4 from the exercise3 branch.
e348ebc Testing the emergency git-casting system
43388fe Initial commit
```

### Step 2 - Non-Fast-Forward Merge

```bash
6. git reset --hard HEAD^                                                   # reset thing by last commit
or
   git reset --hard HEAD~1
HEAD is now at 964e10a advanced-git

7. git merge --no-ff exercise3                                              # Even if a fast-forward is possible, I still want a merge commit to mark that this branch was merged.
Merge made by the 'recursive' strategy.
 hello.txt | 1 +
 1 file changed, 1 insertion(+)

8. git --no-pager log --oneline

9. git --no-pager log --graph                                               # create a oneline version to graph
*   commit 7ea8b01a763b19037ab79b17e6a54a41b60d88e2
|\  Merge: 43388fe e348ebc
| | Author: anonymous
| | Date:   Wed Oct 4 19:21:05 2017 -0700
| |
| |     Merge branch 'exercise3' into exercise4
| |
| * commit e348ebc1187cb3b4066b1e9432a614b464bf9d07
|/  Author: anonymous
|   Date:   Wed Oct 4 19:01:12 2017 -0700
|
|       Testing the emergency git-casting system
|
* commit 43388fee19744e8893467331d7853a6475a227b8
  Author: anonymous
    Date:   Wed Oct 4 18:51:49 2017 -0700
```

### Step 3 - Setting up for a Conflict

```bash
10. git branch

11. git checkout -b mundo                                                   # create new branch and switch to it -> in one command

under mundo branch
12. echo 'hola, mundo' > hello.txt                                          # create a text file with a content hola mundo

13. cat hello.txt                                                           # concatenate and print files
hello, mundo!

14. git status                                                              # check status
On branch mundo
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   04-merging-and-rebase/04-solution.md
        modified:   04-merging-and-rebase/hello.txt

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        hello.txt

15. git add hello.txt                                                       # add to git

16. git commit -m "change world to mundo"                                   # commit
[mundo e05bcee] change world to mundo
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```

Now, go back to our exercise4 branch, where hello.txt should still read "Hello World!" Create a commit that changes this to "Hola World!"

```bash
17. git checkout -                                                          # just says switch back to previous last switched branch
M       04-merging-and-rebase/04-solution.md
M       04-merging-and-rebase/hello.txt
Switched to branch 'exercise4'

18. echo 'hello, world!' > hello.txt                                        # change it back to mundo to world
19. git add hello.txt

20. echo 'hola, world!' > hello.txt
21. git add hello.txt

22. git commit -m "change hello to hola"
[exercise4 2a46962] change hello to hola
 1 file changed, 1 insertion(+), 1 deletion(-)
```

### Step 4 - Enable ReReRe and Merge

Before trying to merge, let's enable git's ReReRe functionality. Then, let's try to merge the mundo branch into exercise4 as normal.

```bash
23. git config rerere.enabled true                                          # enable git rerere

24. git merge mundo
Auto-merging 04-merging-and-rebase/hello.txt
CONFLICT (content): Merge conflict in 04-merging-and-rebase/hello.txt
Recorded preimage for '04-merging-and-rebase/hello.txt'
Automatic merge failed; fix conflicts and then commit the result.
```

You can see, as expected, we have a merge conflict that must be resolved manually. However, you should notice a new line: Recorded preimage for 'hello.txt'

Run git rerere diff to see what's going on:

```bash
25. git rerere diff                                                        # The git rerere diff command displays the differences for the current state of a conflict resolution that git rerere is tracking
--- a/04-merging-and-rebase/hello.txt
+++ b/04-merging-and-rebase/hello.txt
@@ -1,6 +1,6 @@
-<<<<<<<
+<<<<<<< HEAD
+hola, world!
+=======
 fast forward testings
 different
-=======
-hola, world!
->>>>>>>
+>>>>>>> mundo
```

This shows us the current state of the resolution - what we started with to resolve and what we've resolved it to.

Resolve the conflict in hello.txt by changing it to say "Hola Mundo!" and run rerere diff again:

```bash

### in hello.txt we are on exercise4 branch we changed
+<<<<<<< HEAD
+hola, world!
+=======
 fast forward testings
 different
-=======
-hola, world!
->>>>>>>
+>>>>>>> mundo

### to this
+>>>>>>> hola mundo!


26. git rerere diff                                                         # now that we run rerere.diff it shows a solution
git rerere diff
--- a/04-merging-and-rebase/hello.txt
+++ b/04-merging-and-rebase/hello.txt
@@ -1,6 +1 @@
-<<<<<<<
-fast forward testings
-different
-=======
-hola, world!
->>>>>>>
+Hello Mundo!                                                               # recognized solution
\ No newline at end of file

```

Now things should be clearer - when git sees "Hello Mundo!" on one side of a merge, and "Hola World!" on the other, it will resolve it to "Hola Mundo!" Mark it as resolved and commit it:

```bash
27. git add hello.txt

28. git commit -m "merging in mundo branch"                                 # merged succesfully, even previous try errored
Recorded resolution for '04-merging-and-rebase/hello.txt'.
[exercise4 af670f1] merging in mundo branch
```

### Step 5 - Back up and Merge Again

Now let's back up to just before the last commit, and try the merge again - this time with the help of ReReRe:

```bash
29. git reset --hard HEAD^                                                  # reset back to the previous commit
HEAD is now at 52ae477 Merge branch 'mundo' into exercise4

30. git merge mundo
Auto-merging 04-merging-and-rebase/hello.txt
CONFLICT (content): Merge conflict in 04-merging-and-rebase/hello.txt
Resolved '04-merging-and-rebase/hello.txt' using previous resolution.        # git told us even tho it errors it used the RERERE
Automatic merge failed; fix conflicts and then commit the result.


31. cat hello.txt                                                            # the merge content of hello was fast forward testings \n different
Hello Mundo!                                                                 # but here its Hello Mundo!, so it remembered
```

This time, our merge still failed, but the conflict was resolved automatically, per the line that says Resolved 'hello.txt' using previous resolution. There is no need to resolve the conflict manually, as we can see that our hello.txt now correctly says "Hola Mundo!" All we need to do is stage hello.txt and commit it.
