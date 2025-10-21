### Step 1 - View the Contents of the Staging Area

1.) git ls-files -s -> show all files currently staged in the index

### Step 2 - Interactive Staging

2.) echo "This is a test of the emergency git-casting system." >> hello.txt -> modify the file by adding new text

3.) git add -p -> stage changes interactively (select specific hunks)

4.) y -> confirm adding the change to the staging area

5.) git status -> check which files are staged or modified

6.) git ls-files -s -> verify what’s currently staged

### Step 3 - Unstage your Change

7.) git reset hello.txt -> remove the file from staging without deleting changes

8.) git status -> confirm that the file is unstaged

### Step 4 - Stash your Changes

9.) git stash save "emergency git-casting" -> stash your current work with a message

or git stash push -m "" -> stash changes with a custom message

10.) git status -> confirm working directory is clean after stashing

11.) git stash list -> show all saved stashes

12.) git stash show stash@{0} -> show details of the latest stash

13.) git stash apply -> reapply the most recent stash

14.) git diff hello.txt -> compare current file changes with the last commit
