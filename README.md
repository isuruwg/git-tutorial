# Git tutorial <!-- omit in toc -->
# TOC <!-- omit in toc -->

- [1. Basics](#1-basics)
  - [1.1. Git states](#11-git-states)
  - [1.2. Initializing a repo](#12-initializing-a-repo)
  - [1.3. Committing](#13-committing)
  - [1.4. Viewing log](#14-viewing-log)
  - [1.5. View all files tracked by git:](#15-view-all-files-tracked-by-git)
  - [1.6. Express commit](#16-express-commit)
  - [1.7. Backing out](#17-backing-out)
  - [1.8. History and making new commands with Alias](#18-history-and-making-new-commands-with-alias)
  - [1.9. Rename and delete files](#19-rename-and-delete-files)
  - [1.10. Comparing differences](#110-comparing-differences)
  - [1.11. Special Markers](#111-special-markers)
    - [1.11.1. HEAD](#1111-head)
  - [1.12. Create and switching branches](#112-create-and-switching-branches)
    - [1.12.1. Example workflow](#1121-example-workflow)
  - [1.13. Conflict resolution](#113-conflict-resolution)
  - [1.14. Tagging](#114-tagging)
  - [1.15. Stashing](#115-stashing)

# 1. Basics

## 1.1. Git states

| Local          -> |      ->      |      ->                  | Remote |
|-------------------|--------------|--------------------------|--------|
| Working directory | Staging area | Repository (.git folder) |Remote repo|

## 1.2. Initializing a repo

```bash
git init <repo name>

# Or from an already existing folder:
git init .
```

## 1.3. Committing

```bash
git commit -m "Message"

# Or open up an editor for typing the commit message by doing:
git commit
```

## 1.4. Viewing log

```bash
git log

# or use git show to get more details
git show
```

## 1.5. View all files tracked by git:

```bash
git ls-files
```

## 1.6. Express commit

Commit directly without having to do `git add` and then committing:

```bash
git commit -a

# or with message:
git commit -am "My commit message"
```

## 1.7. Backing out

Following commands can be used to back out of a staged commit:

```bash
git add README.md
git reset HEAD # This unstages the commit but the README.md stays the same

# If you also want to remove all the changes from the file:
git checkout -- README.md
```

## 1.8. History and making new commands with Alias

```bash
git log --oneline --graph --decorate --all
# The above command shows a nice history view
# we can save it as an alias by doing:
git config --global alias.hist "log --oneline --graph --decorate --all"

# You can check if it worked by doing:
git config --global --list

# You can also pass a filename to the git log command to view history of a particular file
# This can also be used along with the alias we setup above
git hist -- README.md
```

## 1.9. Rename and delete files

```bash
# Rename:
git mv <Old file name> <New file name>
# Remove:
git rm <File name>
```

## 1.10. Comparing differences

```bash
# Compare current working directory and the last commit in this branch (HEAD)
git diff
# or use difftool command to view in a difftool (eg: Meld)
git difftool

# Compare a specific commit with the last commit in this branch
git diff 5432231 HEAD
git difftool 5432231 HEAD
```

## 1.11. Special Markers

### 1.11.1. HEAD

- Points to Last Commit of Current Branch
- Can be moved

## 1.12. Create and switching branches

```bash
# Create new branch and switch
git checkout -b new-branch-name
```

### 1.12.1. Example workflow

```bash
git checkout -b newbranch
```

Do some edits and do:

```bash
git add .
git commit -m "edited on new branch"
```
Now when you do `git hist` you'll see that the `HEAD` now points to the new branch

```bash
* a073sda (HEAD -> newbranch) edited on new branch
* 14asdfb (master) before switching to new branch
```

Get back to master branch, merge (This will do a Fast-forward merge since there are no conflicts and the branches can be easily merged) and delete old branch:

```bash
git checkout master
git merge newbranch
# Delete merged branch
git branch -d newbranch
```

## 1.13. Conflict resolution

Let's create a new branch

```bash
git checkout -b very-bad
```

```bash
# view all branches
git branch -a
```

Let's change a line in the new branch and commit and also change the same line to a different text in the master branch and commit

CHANGED LINE IN MASTER BRANCH

```bash
git commit -am "line 180 changed in branch"
git checkout master
# now change line 180 and commit
git commit -am "line 180 changed in master"
```
Now let's try to merge with our `very-bad` branch:

```bash
git merge very-bad
# this will create a conflict. Let's open the mergetool to resolve
git mergetool
```
Once merge is done, let's commit

```bash
git add README.md
git commit -m "Resolving conflict"
```

You might see a `README.md.orig` file after this. You can add `*.orig` to [.gitignore](.gitignore) to prevent adding these files accidentally to the repo. You can delete the `.orig` file by `rm README.md.orig`

## 1.14. Tagging

There are two types of tags

1. Lightweight tags `git tag v1`
2. Annotated tags `git tag -a v1 -m "my new version"` or, `git tag -a v1` would launch an editor to type the tag message.

A tag can be deleted by `git tag -d v1`

List all tags by `git tag --list`

Show tag information `git show v1`

## 1.15. Stashing

Let's say we edited some code but don't want to commit the code. And want to change over to another branch or change context and work on something else for a while, we can do that by using `git stash` command.

Let's check our current status: 

```bash
git status
# Output:
On branch master
Changes not staged for commit:
        modified:   README.md
no changes added to commit (use "git add" and/or "git commit -a")
```

And let's say we want to change to a new branch but don't want to commit the changes we did yet.

So we'll do:

```bash
git stash
# Let's now list the stashes:
git stash list
```

<<<<<<< Updated upstream
Now let's do some change to the [README.md](README.md) file.
=======
Some change
>>>>>>> Stashed changes
