# Git tutorial <!-- omit in toc -->

# Contents <!-- omit in toc -->

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
  - [1.16. Time travel with Reset and Reflog](#116-time-travel-with-reset-and-reflog)
    - [1.16.1. soft reset](#1161-soft-reset)
    - [1.16.2. mixed reset](#1162-mixed-reset)
    - [1.16.3. hard reset](#1163-hard-reset)
  - [1.17. Removing sensitive files from git repo](#117-removing-sensitive-files-from-git-repo)
- [2. GitHub](#2-github)
  - [2.1. Fetch and Pull](#21-fetch-and-pull)
  - [2.2. Resolving conflicts](#22-resolving-conflicts)
  - [2.3. Merge locally](#23-merge-locally)
  - [2.4. Removing branches](#24-removing-branches)
    - [2.4.1. Deleting on GitHub web interface first](#241-deleting-on-github-web-interface-first)
    - [2.4.2. Deleting on local first](#242-deleting-on-local-first)
  - [2.5. Pull with rebase](#25-pull-with-rebase)
  - [2.6. Tags and releases](#26-tags-and-releases)

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

You can also set the following aliases to get different versions of pretty history: 

```bash
git config --global alias.hist2 "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all"

git config --global alias.hist3 "log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all"
```

Or you can add the following to `~/.gitconfig`
```
[alias]
        hist = log --oneline --graph --decorate --all
        hist2 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold green)(%ar)%C(reset) %C(white)%s%C(reset) %C(dim white)- %an%C(reset)%C(bold yellow)%d%C(reset)' --all
        hist3 = log --graph --abbrev-commit --decorate --format=format:'%C(bold blue)%h%C(reset) - %C(bold cyan)%aD%C(reset) %C(bold green)(%ar)%C(reset)%C(bold yellow)%d%C(reset)%n''          %C(white)%s%C(reset) %C(dim white)- %an%C(reset)' --all
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

Now let's do some change to the [README.md](README.md) file and then commit the changes.

```bash
git commit -am "Changed after stashing"
# Now let's get the changes from the stash:
git stash pop
```

## 1.16. Time travel with Reset and Reflog

There are 3 types of `git reset`:

1. `soft`
2. `mixed` (This is the default)
3. `hard` 

### 1.16.1. soft reset
Let's stage some of the changes we have:

```bash
git add .

git status

# Output
On branch master
Changes to be committed:
        modified:   README.md
```

Let's look at the `git log` (using the `git hist` alias we saved in setup)

Output:
```bash
* 02a4e3f (HEAD -> master) Before git reset
* 12a6c5e Updated README using stash
* 974241c Did a change after stash
* c3adad1 Before stash
* cede774 Before stashing
........
```

Let's do a git reset to commit `c3adad1`

```bash
# let's do some changes to a file and then do a reset
git reset c3adad1 --soft
```

Now if you do `git status`, you would get:

```bash
On branch master
Changes to be committed:
        modified:   README.md

Changes not staged for commit:
        modified:   README.md
```

And if we do `git log --oneline --graph --decorate --all`  ( `git hist` alias ), we get:

```bash
* c3adad1 (HEAD -> master) Before stash
* cede774 Before stashing
```

We can see that the soft reset simply moved the `HEAD` to a different location but kept our staging and all our files (Including the ones that were not staged) intact.

### 1.16.2. mixed reset

Now let's get back to our latest commit and also commit the new changes before we try the other types of `reset`

```bash
git hist
# Outputs:
* 17d6ca0 (HEAD -> master) Before mixed reset
* 02a4e3f Before git reset
* 12a6c5e Updated README using stash
```

```bash
git reset 02a4e3f --soft  
```

Now let's do some change and add it to our staging `git add .` and also do some more changes and do a git reset:

```bash
# --mixed can be omitted below since it's the default
git reset 02a4e3f --mixed
```

Now if we look at `git status` we would get:

```bash
On branch master
Changes not staged for commit:
        modified:   README.md
```

```bash
git hist
# Now outputs:
* 02a4e3f (HEAD -> master) Before git reset
* 12a6c5e Updated README using stash
```

### 1.16.3. hard reset

Now let's commit our changes before doing a hard reset

```bash
git reset 17d6ca0
git commit -am "Before hard reset"
```

```bash
git hist
* e6a5b8b (HEAD -> master) Before hard reset
* 17d6ca0 Before mixed reset
* 02a4e3f Before git reset
```
Now let's do a hard reset:

```bash
git reset 17d6ca0 --hard
```

Now let's do `git reflog`, this shows a log of when tips of branches and other references were updated in the local repository:

```bash
# git reflog output:
17d6ca0 (HEAD -> master) HEAD@{0}: reset: moving to 17d6ca0
e6a5b8b HEAD@{1}: commit: Before hard reset
```

Now let's move back to our previous commit by: `git reset e6a5b8b`. You have to be careful with `git reset --hard` because any uncommited changes would be lost after a hard reset.

## 1.17. Removing sensitive files from git repo

If you've accidentally added a sensitive file to a git repo and you would like to remove it from the git history, you can follow the following steps: (NOTE: Please treat any information you have accidentally commited as compromised even if you remove them from the git repo. eg: if you commited a password, please change it)

[[Reference](https://docs.github.com/en/authentication/keeping-your-account-and-data-secure/removing-sensitive-data-from-a-repository)]

```bash
# Install git-filter-repo
python -m venv venv
. ./venv/bin/activate
pip install git-filter-repo
# I used pip install git-filter-repo , you can also use any of the methods mentioned
# here: https://github.com/newren/git-filter-repo/blob/main/INSTALL.md

# Do a fresh clone
git clone git@github.com:USERNAME/YOUR-REPO-NAME.git

# Remove the file
git filter-repo --invert-paths --path PATH-TO-YOUR-FILE-WITH-SENSITIVE-DATA

# Add file to gitignore
echo "YOUR-FILE-WITH-SENSITIVE-DATA" >> .gitignore

git add .gitignore
git commit -m "Add YOUR-FILE-WITH-SENSITIVE-DATA to .gitignore"
```

**Important Note:** git-filter-repo removes the origin url from your repo. This is done for a reason as discussed [here](https://htmlpreview.github.io/?https://github.com/newren/git-filter-repo/blob/docs/html/git-filter-repo.html#DISCUSSION) and [here](https://github.com/newren/git-filter-repo/issues/46#issuecomment-573733491).

**Please do not do the following steps blindly if you are working on a repo with multiple people working on it(as is the case most of the time). Please read the discussion above and follow the appropriate steps**

```bash
# The recommended method is to create a new repo and push your changes instead of using the old repo as this can avoid issues. But, if you really want to use the same remote repo that you started with, please make sure you communicate with everyone else who is working on your project about these changes.

# git-filter-repo removes the origin url, add a remote for a NEW repo
git remote add origin git@github.com:USERNAME/YOUR-NEW-REPO-NAME.git

#verify
git remote -v


git push origin --force --all

# If you have tagged releases too with sensitive files
git push origin --force --tags
```

# 2. GitHub

Let's create a new empty GitHub repo and do:

```bash
git remote add origin git@github.com:isuruwg/git-tutorial.git
git branch -M main
git push -u origin main --tags
```

Let's do `git remote -v` command to check if our remote was added properly.

## 2.1. Fetch and Pull

`git pull` is the combination of two commands, `git fetch` and `git merge`. Git will first fetch all the changes from the remote repo and then merge them onto our local.

You can do a `git fetch` first instead of doing `git pull` if you are unsure about the status of the remote. Then you can merge by doing a `git pull` command. Then you can do a `git push`

It's advised to do a `git fetch` or `git pull` before doing a `git push` if you are working on a repo with multiple people.

## 2.2. Resolving conflicts

Conflicts occuring because the remote and local having diverged can be resolved by using a mergetool. Use, `git mergetool` command to launch merge tool. After merging is done, do a git commit.

## 2.3. Merge locally

You can switch to the branch you want to merge into (eg: `main`) and then do `git merge <branch-name>` where `<branch-name>` is the name of the branch you want to merge into your current branch.

## 2.4. Removing branches

Let's call the branch we want to delete `branch1`

### 2.4.1. Deleting on GitHub web interface first

You can remove `branch1` on GitHub using the GitHub web interface. Once it's done, on your local shell do:

```bash
git branch -a
```
You would still see `branch1` and `remotes/origin/branch1` on your local.

```bash
# Let's delete the local
git branch -d branch1

# Update stale references by doing git fetch with the prune option
git fetch -p
# This will remove the remote/origin/branch1 reference.
```

### 2.4.2. Deleting on local first

```bash
# Delete local
git branch -d branch1

# Delete on origin
git push origin :branch1
```

## 2.5. Pull with rebase

From a content perspective, rebasing is changing the base of your branch from one commit to another making it appear as if you'd created your branch from a different commit. Internally, Git accomplishes this by creating new commits and applying them to the specified base.[[ref](https://www.atlassian.com/git/tutorials/rewriting-history/git-rebase)]

## 2.6. Tags and releases

```bash
# List all tags
git tag

# Create simple tag
git tag <tag_name>

# Create annotated tag
git tag -a <tag_name> -m "description" <Optional_Commit_ID>

# view tag and commit information
git show <tag_name>

# Push specific tags to remote
git push origin <tag_name>

# Push all tags to remote
git push --tags

# Delete tag on local
git tag -d <tag_name>

# Delete tag on remote. DON'T forget the : in the command below
git push origin :<tag_name>
```


