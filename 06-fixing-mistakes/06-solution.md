In this exercise, we'll practice reverting a file and cleaning our repo. Then we'll take a deeper look at git reset and git revert to go back in time.

### Prerequisite

```bash
1. git checkout -b exercise6                                        # creates a branch and switch to it
```

### Step 1 - Undo changes in your working area with git checkout -- <file>

```bash
2. echo "bad data" > hello.template                                 # overwrite the content inside

3. cat hello.template                                               # concatinate and print content inside
bad data

4. git checkout -- hello.template                                   # brings back previous commit content sindie

5. cat hello.template
hello mundo!
ADDING SOME TEXT FOR PRACTICING GOOD COMMIT MESSAGE <<<
adding a new one for git show and diff!!
another one called i18n
actually i changed my mind i wanna add another

# how to checkout a file in specific point in time
7. git log --name-status --follow --oneline hello.template
71efd65 (HEAD -> exercise6) adding hello.template                   # shows history of file
C100    05-history-and-diffs/hello.template     06-fixing-mistakes/hello.template
90163cf i18n last added new line text for hello.template
M       05-history-and-diffs/hello.template
a3c91d1 i18n another one
M       05-history-and-diffs/hello.template
1fc2fcf added another text for hello.template to test git show and diffs
M       05-history-and-diffs/hello.template
cd72151 replacing greeting with some tokens for i18n
A       05-history-and-diffs/hello.template

8. git checkout fec9e7b -- hello.txt

9. cat hello.txt                                                     # changed to previous
Hola World!
This is a test of the emergency git-casting system.
```

As expected, git has restored a snapshot of our hello.txt file from the commit fec9e7b Changing Hello to Hola. First, git copied the staging area from that commit. Next it copied the file from the updated staging area into our working area. Because of this, our hello.txt file appears added to the staging area. We don't really want to keep it, so let's reset it to unstage it and clear the staging area.

Then let's go ahead and delete our hello.template file:

```bash
10. git reset HEAD hello.txt

11. git rm hello.template
rm 'hello.template'

12. git status
On branch exercise6
Your branch is up-to-date with 'origin/exercise6'.
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

	deleted:    hello.template

Untracked files:
  (use "git add <file>..." to include in what will be committed)

	hello.txt

13. git commit -m "Deleting hello.template"
[exercise6 713f6a1] Deleting hello.template
 1 file changed, 2 deletions(-)
 delete mode 100644 hello.template
```

Later on, we decide that deleting hello.template was an accident. Let's track down where we deleted it, and bring it back:

```bash
14. git log --diff-filter=D --oneline -- hello.template                 # searching deleted log for hello.template
234b5db (HEAD -> exercise6) deleting hello.template file for the second itme
3ff11da deleting the template file

15. git --no-pager log --diff-filter=D --oneline -- hello.template      # “Show me a compact list of commits that deleted hello.template, and print it directly to the terminal without opening less.”
234b5db (HEAD -> exercise6) deleting hello.template file for the second itme
3ff11da deleting the template file

16. git checkout 3ff11da^ -- hello.template                             # checkout out before <commit> timeline of hello.template file
now its back on 06-fixing-mistakes/06-solution.md                       # it’s like “paste this old version on top of the current one”, ready to commit if you want.

17. git status
On branch exercise6
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        new file:   hello.template

18. cat hello.template
hello mundo!
ADDING SOME TEXT FOR PRACTICING GOOD COMMIT MESSAGE <<<
adding a new one for git show and diff!!
another one called i18n
actually i changed my mind i wanna add another
```

Excellent, we now have our hello.template file back in our staging area and working area.

### Step 2 - Clean your Repo

We should still have this old hello.txt file sitting around, cluttering up our repo. We can use git clean to blow out anything that isn't tracked by git. We'll do a dry run first, just to be safe:

```bash
# first lets you preview what would be deleted, which is safer than blindly deleting manually.
19. git clean --dry-run
Would remove hello.txt

# Deletes all untracked files, including .gitignore-ignored files (like node_modules/, *.log, etc.)
20. git clean -f
Removing hello.txt
```

All clean!

### Step 3 - Git Reset

```bash
21. git reset -- 06-fixing-mistakes/hello.template                      # unstage a file

22. git reset --hard 234b5dbe62d376f143f5266f09c4f135f798e07a
HEAD is now at 234b5db deleting hello.template file for the second itme

23. git log -2 --oneline                                                # That command is a shortcut to view the last two commits in compact form
234b5db (HEAD -> exercise6) deleting hello.template file for the second itme
6ab7cb6 adding template file

24. git reset 6ab7cb6                                                   # mixed reset on specific commit
Unstaged changes after reset:
D       06-fixing-mistakes/hello.template

25. cat .git/HEAD                                                       # see which branch we on
ref: refs/heads/exercise6

26. git reset ORIG_HEAD                                                 # bring back where it was before doing git reset 6ab7cb6

27. git log -2 --oneline                                                # we reverted it back
234b5db (HEAD -> exercise6) deleting hello.template file for the second itme
6ab7cb6 adding template file
```

And there we are, back at the commit where we deleted the file hello.template

### Step 4 - Git Revert

Let's say we want to undo deleting hello.template, but don't want to alter history. Reverts don't look as nice in your history, but are a safer option when working with collaborators.

```bash
28. git --no-pager log --oneline                                        # see all history of commits in one line
34b5db (HEAD -> exercise6) deleting hello.template file for the second itme
6ab7cb6 adding template file
3ff11da deleting the template file
71efd65 adding hello.template
6db51ca (origin/main, origin/HEAD, main) modifed readme
fd0cf93 (origin/exercise5, exercise5) advanced-git
0d605a3 advanced-git
90163cf i18n last added new line text for hello.template
a3c91d1 i18n another one
1fc2.............................. so on.....

# we wanna get back hello.template but want to make a new commit
29. git revert 234b5db
[exercise6 03f7765] Revert "deleting hello.template file for the second itme"
 1 file changed, 1 insertion(+)
 create mode 100644 06-fixing-mistakes/hello.template
# now its hello.template is back

```

There, we've successfully brought hello.template back from the dead, and we have a revert commit to show others exactly what happened. Plus, we didn't have to change history, so this is a good method to use if the changes you want to revert have already been pushed to your origin.
