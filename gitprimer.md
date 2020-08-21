# Git Primer

Software Engineering is a team sport. These teams are often globally distributed, and in the open source world, many of the developers that collaborate do not actually know one another personally. How does this collaboration work? After all, in any given software project, individual developers fix individual bugs and implement features in parallel. How does one test to make sure that these changed written in parallel don’t accidentally interfere with each other?

This problem is the domain of Source Configuration Management (SCM) tools, which track the individual work performed by different developers, and provide tools for reviewing changes, resolving conflicts, and merging changes into the main branch (typically called master).

Outside of software, you might have had experience with collaborative cloud-based document editors such as Google Docs, and in some ways this is similar. Multiple people are working together in a shared context, but this shared context is provided by running on centralized infrastructure in the cloud. Everyone’s computer is tethered to the cloud, and collaboration breaks down when Internet connectivity goes out. Over the years, there have been several centralized SCM systems that worked somewhat in this way.

In contrast, `git` works quite a bit different form these sorts of tools because it is a distributed SCM. When you work in `git`, there is typically a shared repository, often published to GitHub, but developers don’t ever actually code directly against the GitHub repository.

Rather, they ‘checkout’ the repository into a local copy on their local filesystem, and then work directly against that local copy. From the point of view of the local git repository, the GitHub URL that we cloned from is the ‘origin’ repository. That is a relationship that allows up to ‘pull’ changes from the origin down to our local copy or ‘push’ changes from our local copy back to the origin repository.

Keep in mind that all this pushing and pulling is manual. If Carlos and Karen are both working on their local copies of the origin repository and they never push these changes back up to the origin server, they have no idea about each other’s changes, and this will likely result in a ‘merge conflict’.

In this course, we will use `git` for SCM. There is of course particular `git` terminology that you will need to familiarize yourself and a number of commands that you will need to become proficient with in order to use `git`. Over the course of the semester, the main challenge you will have is learning how to remotely collaborate with others using a git workflow. This will take a bit of pushing and pulling, along with many other topics. Likely, you will end up with messy merge conflicts, and you may even end up overwriting or losing work. This frustration is an essential part of learning git, the technology that powers the modern open-source world.

This section will list the most fundamental terminology and commands that you will need to understand `git` and be able to collaborate with others.

# Terminology

## Repository

A repository encapsulates a project in a version control framework. A repository may be stored locally on one or more user machines and/or may be stored remotely on a web server. A number of applications and/or users can access the same `git` repository.

If a repository is maintained in a number of locations whether by one user or several users, the repository must be synchronized among all of the different changes. There are a number of possible mistakes that can occur if the repository is not properly synchronized and these mistakes can create a nasty mess to correct. Until you are well versed in using `git` , you should be very deliberate when issuing commands.

A repository can be created locally for existing code or cloned from a web server.

## Remote

A remote is a repository stored on a remote web server. `git` allows for any number of `git` servers to host the same repository; however, it is important to remember that repositories are not guaranteed to have the same state or content. Updating a local repository does not automatically update remote repositories, and it is up to the repository users to synchronize each of the `git` servers by issuing commands.

## Clone

To clone a repository is to copy the repository from a remote web server onto your local machine. When a repository is cloned, the remote `origin` of the repository is set as the URL of the web server. The `origin` references the default remote that the repository will communicate with when `push` or `pull` commands are issued.

_Note: The `origin` is not automatically updated by changes to your local repository. Updates between local and remote repositories must be explicitly transmitted through specific commands._

## Branch

A `git` project is not just a snapshot of the current state of a project’s development; instead, it is a historical graph of all updates published to the repository. The graph is not strictly a tree as branches of the tree can merge back into other branches.

A project may consist of a number of branches. A branch indicates a point or node within the hierarchy of updates set by the user as a reference point where the development diverges into an individual history to be maintained. The `master` branch is the default central branch of a project, but a `git` user can establish critical points within the set of updates where the development diverges into a unique history. Updates can be attached to a specific branch.

Branches allow new developments to be added to a project without affecting the main project. Practically, a branch allows further development of a project while also maintaining the working distribution without changes to one affecting the other. This means that multiple sub-projects based on the main project can proceed with new development without disturbing the main project. Each of these sub-projects can then be tested and then merged back into the main project or into individual sub-projects on their own schedules.

When multiple users are working in the same project on different tasks, it will become critical to properly use branches; however, even a single user can benefit from working with different branches. It is recommended that each student practice working with branches and merging branches on individual projects so that they develop the skills necessary to manage a more complex multi-user project later in the term.

## Add

To add a file to a `git` repository is to stage an update of that file in its current state so that the file can be copied into the local repository. A file must be staged before it can be updated into the repository. Files may be staged one at a time or in bulk; however, a `git` user should take great care in staging files in bulk, as once a file is inserted into the repository, the file is always a part of the repository database.

## Commit

To commit is to copy all files that have been staged in the user’s working directory into the local repository database. Once a file has been committed, a copy of that file is inserted into the repository database.

## Checkout

To checkout is to request the local `git` server overwrite the current version of files in the user’s working directory with the specified files from the local repository. A checkout can request one or more files or a branch.

## Merge

To merge is to request the local `git` server combine two branches into a single codebase. The git server will attempt to reconcile the differences between the files; however, a merge can often result in differences that are not automatically reconcilable. If differences cannot be automatically reconciled, a user will have to manually reconcile the differences and then commit the corrected codebase.

## Push

To push is to request the local `git` server transmit the state of the local repository to a remote `git` server’s repository.

## Pull

To pull is to request the local `git` server to request the current state of the repository from a remote `git` server and to have the local `git` server update its local repository from the remote repository.

# Commands

All of the following assumes that you will be working with a command line interface into `git` . While there are applications that can automate these functions for you, it is in your best interest to familiarize yourself with the console interface.

If a command contains a label in angled brackets, _i.e._ `<a_label>`, the label is not to be used literally; instead, an appropriate label should be substituted. Labels may be user defined or may be explicitly defined by the `git` system.

## Getting help with `git`

The following command will list all of the top level `git` commands for reference:

```sh
git –help
```

To examine man page reference of each specific `git` command, follow the command with the `–help` parameter:

```sh
git <command> –help
```

## Repository

To create a new repository in the current directory:

```sh
git init
```

To clone a remote repository to your local machine:

```sh
git clone <url>
```

## Remote

To list both the remote repositories and their associated URL’s:

```sh
git remote -v
```

## Branch

To list the active branch in the current working directory:

```sh
git branch
```

To create a new branch with the name `<branchname>` that is a copy of
the active branch:

```sh
git branch <branchname>
```

## Add

To stage a single file with the name `<filename>` for commit:

```sh
git add <filename>
```

To stage one or more files at the path `<pathspec>` for commit:

```sh
git add <pathspec>
```

_Note: File types can be automatically filtering out using `.gitignore` . It is debatable whether you should use the `*.*` wildcard to add all files in your working directory to a commit as this tends to temporary files that may not be vetted yet. You should not add source files to your source tree as these bloat the size of the repository. Remember that all the repository is the history of all changes, so adding large files that are later deleted from the working directory does not reduce the size of the repository. You should only ever add source files that are explicitly required to be maintained to the project._

## Commit

To commit files to your local repository that have been added using `git add` use the `git commit` command. Each commit requires a commit message that will annotate and accompany the commit as a reference.

To commit the current set of staged files:

```sh
git commit
```

_Note:_ The system will open the associated text editor for the user to add a commit message.

To commit the current set of staged files with the associated commit message `<commitmessage>`:

```sh
git commit -m “ <commitmessage>”
```

## Checkout

To overwrite a single file in the working directory with the name `<filename>` in the local repository:

```sh
git checkout <filename>
```

To overwrite all files from a path in the working directory with all files at the path `<pathspec>` in the local repository:

```sh
git checkout <pathspec>
```

To overwrite all files and folders in the working directory with the state of the branch named `<branch>` in the local repository:

```sh
git checkout <branch>
```

To copy the state of the current branch into a new branch named `<new_branch>` in the local repository:

```sh
git checkout -b <new_branch>
```

## Push

To request the local `git` server transmit the state of the current branch to the remote `origin` server:

```sh
git push
```

To request the local `git` server transmit the state of the branch named `branch` to the remote server named `remote`:

```sh
git push <remote> <branch>
```

## Pull

To request the local `git` server receive the state of the current branch from the remote `origin` server:

```sh
git pull
```

To request the local `git` server receive the state of the branch named `branch` from the remote server named `remote`:

```sh
git pull <remote> <branch>
```

# Example Workflows

## Cloning a new repository

```sh
git clone <url>
```

## Update a file and commit to the current branch of the local repository

```sh
git add <filename>
git commit -m “ <commitmessage>”
```

## Update a file and commit to the current branch of both local and `origin` repositories

```sh
git add <filename>
git commit -m “ <commitmessage>”
git push
```

## Pull (and merge) remote changes with your local changes from the `origin` `master` branch

```sh
git pull origin master
```

## Review

1.  Which `git` command creates a repository on your local machine?

2.  Which `git` command copies a repository from a remote git server onto your local machine and creates a reference to that remote as the `origin`?

3.  If you commit one or more files, what repositories are automatically updated?

4.  What steps are necessary to ensure that a local repository is synchronized with a remote repository?

5.  What steps are necessary to ensure that a remote repository is synchronized with a local repository?

6.  When you checkout a file from a repository, what repository does the file come from?

# Resources

-   [Official git docs](https://git-scm.com/doc)
-   [Udacity Course on git](https://www.udacity.com/course/version-control-with-git--ud123)
-   [git cheat sheet](https://services.github.com/on-demand/downloads/github-git-cheat-sheet.pdf)
-   [git saving changes](https://www.atlassian.com/git/tutorials/saving-changes)
-   [git undoing changes](https://www.atlassian.com/git/tutorials/undoing-changes)
-   [git reference](https://git-scm.com/docs)
