# `git` and `github`

There is a list of best practices and rough guidelines for how to use `git` and `github`.

## PR Requests

I'd appreciate PRs to

-   adding specific information about how to do PRs for this classes repo,
-   add anything that I missed here.

## Using `github`

`github` provides a wonderful set of repository management features online.
You can go through the `github` [bootcamp](https://help.github.com/categories/bootcamp/) if you don't know how to use it.

You can use `github` to coordinate between team members in many different ways.
I'd strongly recommend that when working in a team, you use the [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) pattern of interaction.
At the highest-level, this means that each team member will be working on their features on separate branches, and will integrate them into the mainline to coordinate with everyone else.

A few of the ways to interact with `github`:

-   `git pull` and `git push` are you main means of pulling your repo off of `github`, and to update the version on `github`.
    If you're working in a group, these are the only ways that you synchronize your work between team members.
    If you want to _submit_ an assignment, `git push` is the **only means** to do so.
-   Note that you'll often have to resolve merge conflicts when you do a `git pull` when another team member has previously made changes to the repo.
    You have to go through your code looking for any place that `git` has inserted text like "HEAD", ">>>", "<<<", and "---" that indicate where you've made changes that conflict with the `github` repo.
-   Alternatively, you can use "Pull Requests" (PRs) to and the [feature branch workflow](https://www.atlassian.com/git/tutorials/comparing-workflows#feature-branch-workflow) to coordinate with your team.
    This is highly recommended and emulates industry environments.
    A huge benefit of these is that you can do code reviews using `github` on PRs!
    See the [software engineering guide](https://www2.seas.gwu.edu/~gparmer/posts/2017-06-08-sweng-in-research.html) for some information on code reviews and PRs.
    However, it is more advanced, so make sure you understand the simpler method, above, and get some practice using it before moving on to PRs.

## Using `git`

I'm not going to do a deep dive into using `git` here, and expect that you can figure out the main [features](https://rogerdudler.github.io/git-guide/) [online](https://medium.com/@igor_marques/git-workflow-basics-d405746f6205).
Some quick git tips can be found at the bottom of [software engineering guide](https://www2.seas.gwu.edu/~gparmer/posts/2017-06-08-sweng-in-research.html).
An incomplete list of commands that you can use with git include:

-   `git diff`, `git diff --stat`, and `git status` which all let you understand at different granularities what changes have been made.
    Before you commit code, you should likely execute all of these commands.
    You do this for two reasons.
    First, you want to make sure that only code modifications that you want in the commit, are in it.
    Each commit should be relatively focused, so it is important to catch this.
    Second, it is a good way to remind yourself what the commit contains so that you can better summarize it in the commit message.
    It is also useful to know that you can execute `diff` on ranges of commits to see what changed (see `git log` to see the list of commits).
-   `git commit -a`, obviously.
    Do not, _do not_, use the `m` flag.
    You should write a "full form" message, not a single line.
    The focus of your message should be on 1. summarizing the commit in a single-line (which is displayed along with the commit in `github`), and 2. an explanation of the details of the commit.
    The latter should explain the intention, and often includes an itemized list of the main aspects of the commit.
    `@`s should be used to reference any issues it addresses.
-   `git branch <b>` and `git co <b>` to use and manage your branches.
    Get used to using branches.
    Branches are useful for when you're trying to debug a problem, or implement a new feature.
    To see how they're useful, it is important to realize that it is not uncommon to get distracted, and have to redirect your attention to another feature or bug.
    Without branches (and the ability to roll-back to before your work on the feature), you end up with a commit history of half-finished features intertwined with bug fixes and other features.
    This makes your Pull Requests schizophrenic and difficult to review.
    So by default, you should _always_ be developing on a non-mainline branch.
    When you go fix a bug, or work on a new feature, ask yourself if it deserves its own branch.
-   `git stash` is an acknowledgment that we mess up with our branches.
    If you find that you're somewhat accidentally developing a new feature, or fixing a new bug, but have a bunch of unstaged changes on the current branch, you can `stash` them, create a new branch, `stash pop` the changes into the new branch.
    I'm sure there are other ways to do this, but this example at the very least demonstrates how `stash` can be used to delay a current set of changes.
-   `git rebase` enables you to update a branch to an updated master, and to "squash" multiple commits, and clean up your commit history.
    When the master progresses beyond the point where your branch split from it, you generally want to fast-forward merge your changes onto the new master.
    `git rebase master` is your friend here.
    It is generally superior to `git merge` as it maintains a cleaner (linear) history.
    `git rebase -i` allows you to rewrite history by combining different commits.
    This is very useful as it enables you to have a set of commits that tell a story, and don't have a number of "in progress" commits.
    My suggestion is that you label your commits that you'll likely want to get rid of in the future with `TODO` as a header on the title line.
    More information about this can be found [here](https://git-scm.com/book/en/v2/Git-Tools-Rewriting-History) and [here](https://gitready.com/advanced/2009/02/10/squashing-commits-with-rebase.html).

## Before making a commit

Before doing a commit, I always (at least) do the following:

-   `git status` - Did I miss `git add`ing any files? Are any files out of place/odd?
-   `git diff --stat` - Give me a rough view of the aggregate changes; are they as expected? Are there more changes in this commit than I remembered?
-   `git diff` - I go through the changes and make a mental list of what changes were made
-   `git commit -a` - Note that I try not use the `-am` flag non-trivial commits.
    Add a single line that summarizes the commit, then a bullet-list of the changes I noted in the previous step.

## Collaborating in a repository without write permissions

If you don't have write permissions in a github repository, you won't be able to push directly even if it is in a branch different to master. These are the steps to follow:

-   `fork` - Go to the original repo and click the fork button, top right of the screen, this creates a copy of the repo in your github account
-   Make the necessary changes and `push` them to your forked repo.
-   Go to the original repo and go to the pull requests menu. Here click on `create new pull request` and then `compare across forks`, look for your forked repository and click `create pull request`.
