# Git Cheatsheet

**Git** is the open source distributed version control system that facilitates GitHub activities on your laptop or desktop. This cheat sheet summarizes commonly used Git command line instructions for quick reference.

## Configure Tooling
Configure user information for all local repositories.

```bash
# Sets the name you want attached to your commit transactions.
$ git config --global user.name "[name]"
```

```bash
# Sets the email you want attached to your commit transactions.
$ git config --global user.email "[email address]"
```

```bash
# Enables helpful colorization of command line output.
$ git config --global color.ui auto
```

## Create Repositories
Start a new repository or obtain one from an existing URL.

```bash
# Creates a new local repository with the specified name.
$ git init [project-name]
```

```bash
# Downloads a project and its entire version history.
$ git clone [url]
```

## Make Changes
Review edits and craft a commit transaction.

```bash
# Lists all new or modified files to be committed.
$ git status
```

```bash
# Shows file differences not yet staged.
$ git diff
```

```bash
# Snapshots the file in preparation for versioning.
$ git add [file]
```

```bash
# Shows file differences between staging and the last file version.
$ git diff --staged
```

```bash
# Unstages the file, but preserves its contents.
$ git reset [file]
```

```bash
# Records file snapshots permanently in version history.
$ git commit -m "[descriptive message]"
```

## Group Changes
Name a series of commits and combine completed efforts.

```bash
# Lists all local branches in the current repository.
$ git branch
```

```bash
# Creates a new branch.
$ git branch [branch-name]
```

```bash
# Switches to the specified branch and updates the working directory.
$ git checkout [branch-name]
```

```bash
# Combines the specified branch's history into the current branch.
$ git merge [branch]
```

```bash
# Deletes the specified branch.
$ git branch -d [branch-name]
```

## Refactor Filenames
Relocate and remove versioned files.

```bash
# Deletes the file from the working directory and stages the deletion.
$ git rm [file]
```

```bash
# Removes the file from version control but preserves the file locally.
$ git rm --cached [file]
```

```bash
# Changes the file name and prepares it for commit.
$ git mv [file-original] [file-renamed]
```

## Suppress Tracking
Exclude temporary files and paths.

```bash
# A text file named `.gitignore` suppresses accidental versioning of files and paths matching the specified patterns.
*.log
build/
temp-*
```

```bash
# Lists all ignored files in this project.
$ git ls-files --other --ignored --exclude-standard
```

## Save Fragments
Shelve and restore incomplete changes.

```bash
# Temporarily stores all modified tracked files.
$ git stash
```

```bash
# Restores the most recently stashed files.
$ git stash pop
```

```bash
# Lists all stashed changesets.
$ git stash list
```

```bash
# Discards the most recently stashed changeset.
$ git stash drop
```

## Review History
Browse and inspect the evolution of project files.

```bash
# Lists version history for the current branch.
$ git log
```

```bash
# Lists version history for a file, including renames.
$ git log --follow [file]
```

```bash
# Shows content differences between two branches.
$ git diff [first-branch]...[second-branch]
```

```bash
# Outputs metadata and content changes of the specified commit.
$ git show [commit]
```

## Redo Commits
Erase mistakes and craft replacement history.

```bash
# Undoes all commits after [commit], preserving changes locally.
$ git reset [commit]
```

```bash
# Discards all history and changes back to the specified commit.
$ git reset --hard [commit]
```

## Synchronize Changes
Register a repository bookmark and exchange version history.

```bash
# Downloads all history from the repository bookmark.
$ git fetch [bookmark]
```

```bash
# Combines bookmark's branch into current local branch.
$ git merge [bookmark]/[branch]
```

```bash
# Uploads all local branch commits to GitHub
$ git push [alias] [branch]
```

```bash
# Downloads bookmark history and incorporates changes.
$ git pull
```
