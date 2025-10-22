### Step 1 - View the Contents of the Staging Area

```bash
1. git ls-files -s
# shows all files currently staged in the index
# (example output)
100644 d670460b4b4aece5915caf5c68d12f560a9fe3e4 0	hello.txt
```

### Step 2 - Interactive Staging

```bash
2. echo "This is a test of the emergency git-casting system." >> hello.txt
# modifies the file by adding new text

3. git add -p
# stage changes interactively (choose hunks)
diff --git a/hello.txt b/hello.txt
index 557db03..9e8f1f8 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 hello, world!
+This is a test of the emergency git-casting system.
Stage this hunk [y,n,q,a,d,s,e,?]? y

4. y
# confirm adding the change to the staging area

5. git status
# shows which files are staged or modified
On branch main
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
	modified:   hello.txt

6. git ls-files -s
# verify what’s currently staged
100644 9e8f1f8f69e5a9f8f0562d294f26af723cb17aa1 0	hello.txt
```

### Step 3 - Unstage your Change

```bash
7. git reset hello.txt
# remove file from staging area, keep changes in working directory

8. git status
# confirm that file is unstaged
On branch main
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
	modified:   hello.txt
```

### Step 4 - Stash your Changes

```bash
9. git stash save "emergency git-casting"
# stash current work with message
Saved working directory and index state "emergency git-casting"
HEAD is now at ad08aab advanced-git

10. git status
# confirm working directory is clean after stashing
On branch main
nothing to commit, working tree clean

11. git stash list
# show all saved stashes
stash@{0}: On main: emergency git-casting

12. git stash show stash@{0}
# show details of the latest stash
 hello.txt | 1 +
 1 file changed, 1 insertion(+)

13. git stash apply
# reapply the most recent stash
On branch main
Changes not staged for commit:
	modified:   hello.txt

14. git diff hello.txt
# compare current file changes with the last commit
diff --git a/hello.txt b/hello.txt
index 557db03..9e8f1f8 100644
--- a/hello.txt
+++ b/hello.txt
@@ -1 +1,2 @@
 hello, world!
+This is a test of the emergency git-casting system.
```
