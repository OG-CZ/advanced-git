### Step 1 - Initialize the Repo

Create a new sample project folder. Run git status to see that it is not yet a git repository. Use git init to initialize it as a repository.

```bash
1. mkdir -p ~/projects/git-foundations          # create a folder for the project
2. cd ~/projects/git-foundations                # navigate into the folder

3. git status                                   # check repo status (not yet a git repo)
fatal: not a git repository (or any of the parent directories): .git

4. git init                                     # initialize as a new Git repository
Initialized empty Git repository in /Users/og-cz/projects/git-foundations/.git/

```

### Step 2 - First Commit

Create a new document, stage it for a commit, then commit it to your repository.

```bash
5. echo 'hello, world!' > hello.txt             # create a sample text file

6. git add hello.txt                            # add file to staging area

7. git commit -m "Initial commit"               # commit the staged file
[main (root-commit) 43388fe] Initial commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt

```

### Step 3 - View the .git Folder

```bash
4.)
tree .git/objects   # -> list all object folders (Windows CMD)
# or
ls -R .git/objects  # -> list all object folders (Git Bash / macOS / Linux)

$> tree .git
.git
├── COMMIT_EDITMSG
├── HEAD
├── config
├── description
├── index
├── info
│ └── exclude
├── logs
│ ├── HEAD
│ └── refs
│ └── heads
│ └── master
```

### Step 4 - Inspect the Objects

Inspect what’s inside .git/objects. You’ll find three object types:

- blob – stores file contents
- tree – stores directory structure
- commit – stores metadata + references

```bash

# One of the objects should be a tree object. The tree contains the filename hello.txt and a pointer to the blob.

9. git cat-file -t 581caa                       # check object type
tree

10. git cat-file -p 581caa                      # view tree contents
100644 blob 980a0d5f19a64b4b30a87d4206aade58726b60e3	hello.txt
# shows a single file hello.txt and its blob hash

11. git cat-file -t 980a0d5                     # check blob type
blob

12. git cat-file -p 980a0d5                     # view blob content
hello, world!                                   # actual content of hello.txt

13. git cat-file -t 43388f                      # check commit type
commit

14. git cat-file -p 43388f                      # view commit metadata and message
tree 581caa0fe56cf01dc028cc0b089d364993e046b6
author og-cz <secret@noreply.github.com> 1736500819 +0800
committer og-cz <secret@noreply.github.com> 1736500819 +0800

Initial commit                                  # commit message
```

### Step 5 - Look at refs

Check what branch and commit your HEAD is pointing to.

```bash
15. cat .git/HEAD                               # shows which branch HEAD points to
ref: refs/heads/main

16. cat .git/refs/heads/main                    # shows the latest commit hash for 'main'
43388fee19744e8893467331d7853a6475a227b8

17. git --no-pager log --oneline                # compact commit history view
43388fe (HEAD -> main) Initial commit

18. ls -R .git/refs                             # show all branch, remote, and tag refs
.git/refs:
heads

.git/refs/heads:
main
```
