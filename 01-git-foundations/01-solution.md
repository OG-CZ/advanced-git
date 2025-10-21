### Step 1 - Initialize the Repo

Create a new sample project folder. Run git status to see that it is not yet a git repository. Use git init to initialize it as a repository.

1.) mkdir -p ~folder/location/...
2.) cd ~folder/location/...
3.) git status
4.) git init -> create a new Git repository

### Step 2 - First Commit

Create a new document, stage it for a commit, then commit it to your repository.

5.) echo 'hello, world!' > hello.txt -> create a text file with sample content
6.) git add hello.txt -> add the file to the staging area
7.) git commit -m "message here"

### Step 3 - View the .git Folder

4.) tree .git/objects -> list all object folders (Windows CMD)
 or
 ls -R .git/objects -> list all object folders (Git Bash / macOS / Linux)

```json
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

5.) git cat-file -t <hash> -> show the type of a Git object (e.g., blob, tree, commit)

6.) git cat-file -p <hash> -> display the contents of a Git object

for example:

```json

One of the objects should be a tree object. The tree contains the filename hello.txt and a pointer to the blob.

$> git cat-file -t 581caa
tree

$> git cat-file -p 581caa
100644 blob 980a0d5f19a64b4b30a87d4206aade58726b60e3	hello.txt
The blob object, pointed to by the tree, contains the contents of the file hello.txt

$> git cat-file -t 980a0d5
blob

$> git cat-file -p 980a0d5
Hello World!
The commit object contains a pointer to the tree, along with metadata for the commit, such as the author and commit message.

$> git cat-file -t 43388f
commit

$> git cat-file -p 43388f
tree 581caa0fe56cf01dc028cc0b089d364993e046b6
author Nina Zakharenko <nina@nnja.io> 1507168309 -0700
committer Nina Zakharenko <nina@nnja.io> 1507168309 -0700

Initial commit
```

### Step 5 - Look at refs

7.) cat .git/HEAD -> view which branch HEAD is pointing to

8.) cat .git/refs/heads/main -> see the latest commit hash stored in your branch

9.) git --no-pager log --oneline -> display a compact view of commit history

10.) ls -R .git/refs - show all branch, remote, and tag references
