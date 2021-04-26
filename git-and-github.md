# Git and Github

A quick aside: git and GitHub are not the same thing. Git is an open-source, version control tool created in 2005 by developers working on the Linux operating system; GitHub is a company founded in 2008 that makes tools which integrate with git. You do not need GitHub to use git, but you cannot use GitHub without using git. There are many other alternatives to GitHub, such as GitLab, BitBucket, and “host-your-own” solutions such as gogs and gittea. All of these are referred to in git-speak as “remotes”, and all are completely optional. You do not need to use a remote to use git, but it will make sharing your code with others easier.

## Jargon

*content addressable storage system, packfiles, deltas*

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

a commit points to:

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
> - *Tree “Snapshot of  the repository”. Points at files  and directories.*
> - *A commit is a  code snapshot*

### Why can’t we “change” commits?

- If you change any data about the commit, the commit will have a new SHA1 hash.
- Even if the files don’t change, the created date will.

### References - Pointers to commits

- Tags
- Branches
- HEAD - a pointer to the current commit

> **NB:**
>
> - *refs/heads is where branches live*
> - */refs/heads/master contains which commit the branch points to*
> - *HEAD is usually a pointer to the current branch*

### Git commands

- `git cat-file -t` (type) and `-p` (print the contents)

## References

- <https://github.com/nnja/advanced-git>