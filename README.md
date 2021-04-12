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


