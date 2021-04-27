# Git and Github

A quick aside: git and GitHub are not the same thing. Git is an open-source, version control tool created in 2005 by developers working on the Linux operating system; GitHub is a company founded in 2008 that makes tools which integrate with git. You do not need GitHub to use git, but you cannot use GitHub without using git. There are many other alternatives to GitHub, such as GitLab, BitBucket, and "host-your-own" solutions such as gogs and gittea. All of these are referred to in git-speak as "remotes", and all are completely optional. You do not need to use a remote to use git, but it will make sharing your code with others easier.

## Jargon

*content addressable storage system, packfiles, deltas, working area/working tree, staging area/cache/index, git rerere*

## Git

> Distributed Version Control System

### How does git store information?

> At its core, git is like a key value store.

- The **Value** = **Data**
- The **Key** = **Hash of the Data** *(SHA1 - 40-digit hexadecimal number)*

> **NB:** You can use the key to retrieve the content

### The value - blob

> git stores the ***compressed*** data in a blob, along with metadata in a header

- the identifier **blob**
- the size of the content
- \0 delimiter
- content

### Tree

a **tree** contains pointers (using SHA1):

- to blobs
- to other trees

and metadata:

- ***type*** of pointer (**blob** or **tree**)
- ***filename*** or directory name
- ***mode*** (executable file, symbolic link, ...)

> **NB:**
>
> - *Trees point to blobs & trees*
> - *Identical content is only stored once!*

### Commit object

a **commit** points to:

- a tree

and contains metadata:

- ***author and committe*r**
- ***date***
- ***message***
- ***parent commit (one or more)***

the SHA1 of the commit is the hash of all this information

> **NB:**
>
> - *Commits point to parent commits and trees*
> - *Tree "Snapshot of  the repository". Points at files  and directories.*
> - *A commit is a  code snapshot*

### Why can't we "change" commits?

- If you change any data about the commit, the commit will have a new SHA1 hash.
- Even if the files don't change, the created date will.

### References - Pointers to commits

- Tags
- Branches
- HEAD - a pointer to the current commit

> **NB:**
>
> - *refs/heads is where branches live*
> - */refs/heads/master contains which commit the branch points to*
> - *HEAD is usually a pointer to the current branch*

### What's a branch?

- A branch is just a pointer to a particular commit.
- The pointer of the current branch changes as new commits are made. 

### What is head?

- **HEAD** is how git knows what branch you're currently on, and what the next parent will be.
- It's a pointer.
  - It usually points at the ***name*** of the current branch.
  - But, it can point at a commit too (detached HEAD).
- It moves when:
  - You make a commit in the currently active branch
  - When you checkout a new branch

### Lightweight tags

- Lightweight tags are just a simple pointer to a commit.
- When you create a tag with no arguments, it captures the value in HEAD

### Annotated tags: git tag -a

> Point to a commit, but store additional information.

- Author, message, date.

### Tags & branches

- Branch
  - The current branch pointer moves with every commit to the repository 
- Tag
  - The commit that a tag points doesn't change.
  - It's a snapshot!

### The working area

- The files in your working area that are also not in the staging are are not handled by git.
- Also called **untracked files**

### The staging area (a.k.a index, cache)

- What files are going to be part of the next commit
- The staging area is how git knows what will change between the current commit and the next commit.

> **NB:**
>
> - *a "clean" staging area isn't empty!*
>- *Consider the baseline staging area as being an exact copy of the latest commit.*

### "unstage" files from the staging area

- Not removing the files.
- You're replacing them with a copy that's currently in the repository.

### Git stash

- Save un-committed work. 
- The stash is **safe** from destructive operations.

### The repository

- The files git knows about!
- Contains all of your commits

### Head-less / detached head

- Sometimes you need to checkout a specific commit (or tag) instead of a branch.
- git moves the HEAD pointer to that commit
- as soon as you checkout a different branch or commit, the value of HEAD will point to the new SHA
- There is no reference pointing to the commits you made in a detached state.

> Save your work:

- Create a new branch that points to the last commit you made in a detached state.
  - `git branch <new-branch-name> <commit>`
- Why the last commit?
  - Because the other commits point to their parents.

### Dangling commits

> Discard your work:

- If you don't point a new branch at those commits, they will no longer be referenced in git. (dangling commits)
- Eventually, they will be garbage collected.

### Merging

- Under the hood - merge commits are just commits
- Fast forward: fast-forward happens when there are no commits on the base branch that occurred after the feature branch was created.

### git merge --no-ff (no fast forward)

- To retain the history of a merge commit, even if there are no changes to the base branch
- This will *force* a merge commit, even when one isn't necessary.

### Git log: referencing commits
^ or ^**n**
- no args: ==^1: the first parent commit
- **n**: the n^th parent commit
~ **or** ~**n**
- no args: == ~1: the first commit back, **following 1st parent**
- **n**: number of commits back, ***following only 1st parent***
> **NB**: ^ and ~ can be combined

### Diff

- Diff shows you changes:
  - between commits
  - between the staging area and the repository
  - what’s in the working area

### Less

> Tips for using `less` on the command line.

**To navigate:**

 - f = for next page
 - b = for previous page
 
**To search:**

 - /\<_query_>
 - n = next match
 - p = previous match
 
**To quit:**
 - q = to quit


### Git commands

- `git cat-file -t` (*type*) and `-p` (*print the contents*)
- **`git --no-pager log --oneline`**: display the log in the same screen
- **`git --no-pager log --graph --oneline`**
- `git branch a_new_branch`: create a new branch without switching to it
- `git ls-files -s`: show you what's in the staging area
- `git add <file>`
- `git rm <file>`
- `git mv <file>`
- `git add -p`: stage commits in hunks interactively
- `git stash`
- `git stash list`
- `git stash show stash@{0}`
- `git stash apply`
- `git stash apply stash@{0}`
- **`git stash --include-untracked`**
- **`git stash save "WIP: making progress on foo"`**
- `git stash -p`: selectively stash changes
- `git stash branch <optional branch name>`: start a new branch from a stash
- `git checkout <stash name> -- <filename>`: grab a single file from a stash
- `git stash pop`: remove the last stash and applying changes (**NB**: *doesn't remove if there's a merge conflict*)
- `git stash drop`: remove the last stash
- `git stash drop stash@{n}`
- `git stash clear`: remove all stashes
- `git tag`: list all the tags in a repo
- `git show-ref --tags`: list all tags, and what commit they're pointing to
- `git show-ref --heads`: list all heads, and what commit they're pointing to
- `git tag --points-at <commit>`: list all the tags pointing at a commit
- `git show <tag-name>`: looking at the tag, or tagged contents
- `git merge --no-ff`
- **`git checkout -`**: switch to the previous branch that you were just on
- `git log --since="yesterday"`
- `git log --since="2 weeks ago"`
- `git log --name-status --follow -- <file>`: log files that have been moved or renamed
- `git log --grep=Git --author=Cuong --since=2.weeks`: search for commit messages that match a regular expression (*can be mixed & matched with other git flags*)
- `git log --diff-filter=M --stat`: selectively include or exclude files that have been ((A)dded, (D)eleted, (M)odified, (R)enamed & more...)
- `git show <commit>`
- `git show <commit> --stat`: show files changed in commit
- `git show <commit>:<file>`: look at a file from another commit
- `git diff`: unstaged changes
- `git diff --staged`: staged changes
- `git branch --merged master`: which branches are merged with master, and can be cleaned up?
- `git branch --no-merged master`: which branches aren’t merged with master yet?

## References

- <https://github.com/nnja/advanced-git>
- <https://product.hubspot.com/blog/git-and-github-tutorial-for-beginners>