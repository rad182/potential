## What is Git/Github?

Git is a Distributed Version Control System, in that each client creates a complete mirror of the
working repository. It records any changes to a set of files that the client makes and allows you to
recall earlier versions.

For a more comprehensive description of what is unique and preferable about Git in regards to other
Version Control Systems, a thorough overview of its commands and a brief summary of its history,
see the [Git Manual Pages](http://git-scm.com/book/en/v2/Getting-Started-About-Version-Control).

Github is an online repository for Git projects which we use to host Artsy's codebases and allow
collaboration between a group of developers working on a single project.

If you are unfamiliar with Git, check out this [short tutorial](https://try.github.io) on basic git commands (such as `git status`, `git commit`, and `git push`). Otherwise, this document will provide a quick refresher on the majority
of git tools and commands you'll need to use, and perhaps introduce you to some useful new ones.

## Useful Tools and Techniques

###Working with Remotes

####Setting up a Remote

To start working on any project at Artsy, you're going to need to 'fork' the repository ( create a
personal copy of the codebase ) and clone it onto your development environment so you can work on it
locally. Finally, you will want to keep your local repository synced with the origin repository by
setting it as the 'upstream' repository. We recommend Github's own guide to [Forking a Repo](https://help.github.com/articles/fork-a-repo/).

####Pull

Now that you have your environment set up locally, you'll want to make sure your code is in sync
with the most current version on Github. To get the most recent updates from the upstream repository and
merge them into your current branch, we can use git's `pull`.

```
git pull [remote-name] [branch-name]
```

For instance, to merge the 'master' branch from the upstream into your current branch:
```
git pull upstream master
```

> Note: The master€ branch in Git is not a special branch. It is exactly like any other branch. The only
> reason nearly every repository has one is that the git init command creates it by default and most
> people don't bother to change it.

###Branching

To begin working on your own features or bug fixes, you'll want to create a 'branch.' A branch creates
a new pointer to the saved state of your current working environment - whatever the state of the
branch you're currently working on will be the state of the branch you create.

> For this reason, when starting a new feature / bug fix it's usually worth syncing your master with
> the current upstream repository and branching from there so you'll be working with the most recent code.

To create a branch, you'll have to give it a name. To create a new branch and 'check it out' at the
same time:
```
git checkout -b [branchname]
```

At this point, if you'd like to see all of the branches you currently have:
```
git branch
```

If you want to work on a different feature, you can ```checkout``` another branch.
```
git checkout [branchname]
```

Finally, if you've succesfully pushed and merged a branch into the main project and want to keep your
working space clean you can delete the branch.
```
git branch -d [filename]
```

### Viewing Commit History

####Logs

If you want to see the history of the commits made to the current branch, you can use `log` and
its various display options to examine different facets of a branches development.


At its most basic, the `log` will display the history of all commits made to the current branch.
```
git log
```

If you'd like to see the actual changes introduced in each commit, use the -p flag.
```
git log -p
```

If you want to easily see a more extensive history of all commits made to a branch, you can tell
git to show you the 'one line' summaries.
```
git log --oneline
```

If you'd like to examine how branches were split and merged into the current branch in a more visual
way, you can view them as branching tree.
```
git log --graph
```

####Status

If you want to see the status of your current working environment, git provides `status`,
which will let you know what is different from the last commit in your current working directory and
which files have already been staged for being committed.
```
git status
```

If at any point while working on a project you want to see the differences between your current
working environment and the last commit made on the branch, you can use git's `diff` tool:

```
git diff
```
To see the difference between the currently staged version of files and those in your working directory:
```
git diff --staged
```

### Staging and Committing Changes

Now that you know how to examine the history of your branch and examine changes you've made to your
working directory, you're going to want to start creating and saving changes to the codebase.

A git environment is essentially three things - your 'working directory', the 'staging area' and the
'commits' made on a branch. You do work in your working environment, when you want to save the state
of a change to the project for the next snapshot of the repository you move it to 'Staging', and
when you're ready to freeze that snapshot of the entire repository for future recall ( or when it
describes a logical change being implemented ) you `commit` it.

To move files you're working on ( or new files you've created that Git is not tracking ) to the
staging area, we use `add`.
```
git add [filename]
```

You can also add all changes, and tell git which new files to start tracking ( and which deleted ) ones
to stop tracking quickly using:
```
git add --all
```

When you feel that the files and changes that you have staged are in a state that you'd like to save
and be able to return to, we can commit all of the files in staging.
```
git commit
```

If you'd like to quickly give your commit a name and don't want to have a text document open:
```
git commit -m "[title for commit]"
```
> Lean towards using the plain `git commit` rather than the command line version, it takes a bit
longer but writing summaries and spending more time on your commit messages will help prevent
ambiguity down the line.

Finally, if you need to remove a file from the index and you want to tell git to stop tracking that
file in your next commit, you can `rm` (remove) the file.
```
git rm [file to remove]
```
> If you use terminal's normal `rm [file to remove]` function you will have to additionally 'stage'
that deletion to git, i.e. `git add [file you removed]`.

### Undoing Changes

#### Re-doing a Commit

If you commit too early ( or mess up your commit message, or forget to add some files ) you can
're-do' a commit with the `--amend` flag.
This will re-open the previous commit, showing your earlier message. The new message will over-write
the previous commits summary and add your newly staged changes to the commit.

```
git commit --amend
```

#### Unstaging a File

If you've added a file to staging area and want to remove it you can use `reset HEAD` to remove
it.

```
git reset HEAD [filename]
```

#### Discarding Changes in Working Directory
If you have a file that you've modified since the last commit and you want to revert it to the last
committed version instead, you can use `checkout`.

This is a dangerous command - all changes will be lost *and cannot be recovered.
```
git checkout -- [filename]
```
> Remember - any changes that have been committed can almost always be recovered, but anything deleted
> in staging can only be recovered up to the last time it was committed.

###Stash

If you're working on a particular part of a feature ( say you start implementing client side code
and then you realize you need to start working on adding a feature to the Backbone model ) and you
want to switch to another 'logical commit', or if you need to switch to another feature branch and
don't feel like making a commit you can save what you're working on in a stash.

```
git stash save [name of stash]
```

You can check current stashes that you have saved:
```
git stash list
```

If you want to return to working on a particular stash, you can apply it to your current environment:
```
git stash apply [stash name] --index
```

Applying a stash to your branch won't delete the original stash. If you want to delete a stash,
you'll need to do so manually:
```
git stash drop [stash name]
```

...or you can 'pop' and that will apply the current stash and drop the stash simultaneously.
```
git stash pop [stash name]
```

###Rebasing

Rather than 'merging' another branch, if you'd like to maintain an easily-readable commit history
you can interactively `rebase` from another branch. This allows you to edit your local history
of commit messages: if you have changes made out of order you can re-arrange them, if you have several
commits that describe a single change you can 'squash' them down to a single, descriptive message.

If the branches you're combining can be merged without conflict and you don't want to modify your
local commit history, enter the branch that you'd like to bring code 'into' and run:
```
# in this case, branchname is the branch you want to rebase 'into' your current branch.
git rebase [branchname]
```

If you'd like to rebase and simulatenously modify your commit history, you can perform an 'interactive
rebase.'
```
git rebase master -i
```

When you interactively rebase a commit, you'll see a window that looks similiar to the following appear
in your text editor:

```
pick 310154e updated README formatting and added blame
pick f7f3f6d changed my name a bit

# Rebase 710f0f8..a5f4a0d onto 710f0f8
#
# Commands:
#  p, pick = use commit
#  r, reword = use commit, but edit the commit message
#  e, edit = use commit, but stop for amending
#  s, squash = use commit, but meld into previous commit
#  f, fixup = like "squash", but discard this commit's log message
#  x, exec = run command (the rest of the line) using shell
#
# These lines can be re-ordered; they are executed from top to bottom.
#
# If you remove a line here THAT COMMIT WILL BE LOST.
#
# However, if you remove everything, the rebase will be aborted.
#
# Note that empty commits are commented out
```

Using the commands available, you might want to clean up your history by 'squashing' the extraneous
commit. Change the text file to say the following, and then save and close the file.

```
pick 310154e updated README formatting and added blame
s f7f3f6d changed my name a bit

# Rebase 710f0f8..a5f4a0d onto 710f0f8
#

...

```

Git will attempt
to combine commit `f7f3f6d changed my name a bit` into `310154e updated README formatting and added blame`.

> Note that the order of the git commits is in the opposite order you would usually see them if you
used `git log` or other ways to examine commit history, the commits at the top are the oldest.

If you want to rebase with earlier commits in the same branch:
```
git rebase -i HEAD~3
```

You can rewrite your history and use `rebase` in many other ways, to see its other uses and see
some more examples of usage, see the Git Manual Pages for [Rewriting History](http://git-scm.com/book/en/v2/Git-Tools-Rewriting-History#Changing-Multiple-Commit-Messages).

### Pushing

When you've made and committed all of the changes you want on your branch, you'll want to push your
changes to an upstream repository:

```
git push [remote name] [branchname]
```

For instance, you might want to push the feature branch you're working on into your local fork of the
project's master branch.

```
git push origin master
```
