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
  - [Create and switching branches](#create-and-switching-branches)
    - [Example workflow](#example-workflow)

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

## Create and switching branches

```bash
# Create new branch and switch
git checkout -b new-branch-name
```

### Example workflow

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
git branch -d newbranch
```

