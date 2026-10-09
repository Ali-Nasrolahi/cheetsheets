# `git`

A practical reference for everyday Git commands and useful Zsh aliases. For an introduction to Git concepts and version control, see [Basics of Git](https://www.freecodecamp.org/news/learn-the-basics-of-git-in-under-10-minutes-da548267cc91/).

- [`git`](#git)
  - [Initialize and get help](#initialize-and-get-help)
  - [Working tree and status](#working-tree-and-status)
  - [Inspect changes](#inspect-changes)
  - [Stage changes](#stage-changes)
  - [Commit changes](#commit-changes)
  - [Log and history](#log-and-history)
  - [Branches and switching](#branches-and-switching)
  - [Restore files and reset changes](#restore-files-and-reset-changes)
  - [Fetch, pull, and push](#fetch-pull-and-push)
  - [Rebase and merge](#rebase-and-merge)
  - [Stash](#stash)
  - [Remote repositories](#remote-repositories)
  - [Useful Zsh aliases](#useful-zsh-aliases)
    - [Status](#status)
    - [Add](#add)
    - [Commit](#commit)
    - [Logs](#logs)
    - [Show and blame](#show-and-blame)
    - [Branch and checkout](#branch-and-checkout)
    - [Rebase](#rebase)
    - [Pull](#pull)
    - [Push](#push)
    - [Fetch](#fetch)
    - [Stash](#stash-1)
  - [Quick reminders](#quick-reminders)

## Initialize and get help

```bash
git init [directory]      # Create or reinitialize a repository
git clone <url>           # Clone an existing repository
git help <command>        # Open help for a command
git <command> --help      # Equivalent command-specific help
```

## Working tree and status

```bash
git status                # Show working tree and index status
git status --short        # Compact status output
git status -sb            # Compact output, including branch information
```

Git tracks three important states:

- **Working tree:** files currently in your checkout.
- **Index (staging area):** changes selected for the next commit.
- **Repository:** committed history.

## Inspect changes

```bash
git diff                  # Unstaged changes
git diff --staged         # Staged changes, compared with HEAD
git diff HEAD             # All tracked changes since the last commit
git diff <commit1> <commit2>  # Compare two commits
git show <commit>         # Show a commit and its changes
git blame <file>          # Show the last commit affecting each line
```

`git diff` does not normally show untracked files. Add a new file to the index before reviewing its staged diff.

## Stage changes

```bash
git add <path>            # Stage a file or path
git add -v <path>         # Verbose output
git add -A                # Stage additions, modifications, and deletions
git add -u                # Stage modifications and deletions of tracked files
git add -p                # Interactively stage selected hunks
```

Examples:

```bash
git add src/main.c
git add '*.sh'
git add -p
```

Quote pathspecs containing wildcards when you want Git, rather than the shell, to interpret the pattern.

- `git add -A` includes untracked files and deletions.
- `git add -u` does not stage new untracked files.
- `git add -p` is useful for splitting unrelated changes into separate commits.

## Commit changes

```bash
git commit                        # Commit staged changes
git commit -m "message"           # Commit with a message
git commit -v                     # Include the diff in the editor
git commit -a                     # Stage tracked modifications/deletions and commit
git commit --amend                # Amend the previous commit
git commit --amend --no-edit      # Amend without changing its message
```

`git commit -a` does not include new untracked files. Stage those separately with `git add`.

Amending changes the existing commit. Avoid amending commits that others may already have based work on, unless you coordinate the history rewrite.

## Log and history

```bash
git log                           # Full commit history
git log --oneline                 # One line per commit
git log --oneline --decorate      # Include branch and tag names
git log --graph --oneline         # Graphical history
git log --all --graph --decorate --oneline
git log -p                        # Include patches
git log --stat                    # Summarize changed files
git log -- <path>                 # History for a path
git log --author="name"           # Filter by author
git log --since="2 weeks ago"     # Filter by date
```

For custom output, see `git help log` and `git help pretty-formats`.

## Branches and switching

```bash
git branch                        # List local branches
git branch -a                     # List local and remote-tracking branches
git branch <name>                 # Create a branch
git branch -d <name>              # Delete a merged branch
git branch -D <name>              # Force-delete a local branch

git switch <branch>               # Switch branches
git switch -c <branch>            # Create and switch to a branch
git switch -                      # Switch to the previous branch
```

Use `git switch` for branch operations. The older `git checkout` remains valid and can also switch branches, create branches, or restore files, but those responsibilities are now split between `switch` and `restore`.

## Restore files and reset changes

```bash
git restore <file>                # Discard unstaged changes to a file
git restore --staged <file>       # Unstage a file, preserving its contents

git reset                         # Unstage changes; preserve working-tree files
git reset --soft HEAD~1            # Move HEAD back one commit; keep index and files
git reset --mixed HEAD~1           # Move HEAD back; reset index, preserve files
git reset --hard HEAD~1            # Move HEAD back and discard affected changes
```

Be careful with `git reset --hard`: it can permanently discard uncommitted changes. Resetting commits that have already been pushed can also require rewriting shared history.

To undo a published commit without rewriting history, consider:

```bash
git revert <commit>               # Create a new commit that reverses a commit
```

## Fetch, pull, and push

```bash
git fetch                         # Fetch from the default remote
git fetch origin                  # Fetch from origin
git fetch --all --prune           # Fetch all remotes and remove stale remote-tracking refs

git pull                          # Fetch and integrate from the configured upstream
git pull --rebase                 # Fetch and rebase local commits onto the upstream

git push                          # Push to the configured upstream
git push origin <branch>          # Push a branch to origin
git push -u origin <branch>       # Push and set the upstream
```

- **Fetch** downloads remote updates without integrating them into your current branch.
- **Pull** fetches and integrates updates, usually by merging or rebasing depending on configuration and options.
- **Push** publishes local commits to a remote repository.
- `--prune` removes stale remote-tracking references; it does not delete branches from the remote server.
- `-u` (`--set-upstream`) records the upstream branch for future pull and push operations.

## Rebase and merge

```bash
git rebase <branch>               # Replay current-branch commits onto another base
git rebase --continue             # Continue after resolving conflicts
git rebase --skip                 # Skip the current patch
git rebase --abort                # Abort the rebase

git merge <branch>                # Merge a branch into the current branch
git merge --abort                 # Attempt to abort an in-progress merge
```

Rebase rewrites the commits being replayed; merge integrates histories and may create a merge commit. Avoid rebasing shared commits unless the team expects it.

## Stash

```bash
git stash                         # Save tracked working-tree and index changes
git stash push -m "description"   # Save changes with a message
git stash list                    # List stashes
git stash show                    # Summarize the latest stash
git stash show -p                 # Show its patch
git stash apply                   # Apply the latest stash, keeping it
git stash pop                     # Apply and remove it if successful
git stash drop                    # Remove the latest stash
git stash clear                   # Remove all stashes
```

Stashes are useful when you need to switch context without committing unfinished work. By default, untracked files are not included; use `git stash -u` to include them.

## Remote repositories

```bash
git remote -v                     # List remotes and their URLs
git remote add <name> <url>       # Add a remote
git remote remove <name>          # Remove a remote
git remote show <name>            # Show remote details
git branch -vv                    # Show local branches and upstreams
```

## Useful Zsh aliases

These aliases are commonly provided by the Oh My Zsh Git plugin or similar Zsh configurations. They are **not built into Git**, and exact definitions can vary. Check your configuration if an alias is unavailable.

### Status

```bash
gst       # git status
```

### Add

```bash
ga        # git add
gaa       # git add --all
gav       # git add --verbose
```

### Commit

```bash
gc        # git commit -v
gca       # git commit -v -a
gcam      # git commit -v -a -m <message>
```

Remember that `gca` and `gcam` include tracked modifications and deletions, not new untracked files.

### Logs

```bash
glo       # git log --oneline --decorate
glog      # git log --oneline --decorate --graph
gloga     # git log --oneline --decorate --graph --all
glod      # Graph with commit, refs, subject, date, and author
glods     # Same style as glod, with a short date
```

`glod` and `glods` typically use a custom `--pretty` format with colored commit hashes, decorations, subjects, dates, and authors. Their exact output depends on the alias definition.

### Show and blame

```bash
gsps      # git show --pretty=short --show-signature
gbl       # git blame -b -w
```

`git blame -b -w` ignores whitespace changes and uses boundary markers for missing revisions.

### Branch and checkout

```bash
gb        # git branch
gba       # git branch -a
gbd       # git branch -d
gco       # git checkout
gcb       # git checkout -b
```

### Rebase

```bash
grb       # git rebase
grba      # git rebase --abort
grbc      # git rebase --continue
grbs      # git rebase --skip
```

### Pull

```bash
gl        # git pull
ggpull    # git pull origin <current-branch>
glum      # git pull upstream <main-branch>
gpr       # git pull --rebase
```

`ggpull` and `glum` depend on helper functions that determine the current branch or main branch.

### Push

```bash
gp        # git push
gpv       # git push -v
gpu       # git push upstream
ggpush    # git push origin <current-branch>
gpsup     # git push --set-upstream origin <current-branch>
```

### Fetch

```bash
gf        # git fetch
gfo       # git fetch origin
gfa       # git fetch --all --prune --jobs=10
```

### Stash

```bash
gstaa     # git stash apply
gstc      # git stash clear
gstd      # git stash drop
gstl      # git stash list
gstp      # git stash pop
gsts      # git stash show --text
```

## Quick reminders

| Task | Command |
| --- | --- |
| See what changed | `git status` / `git diff` |
| Stage selected changes | `git add -p` |
| Review staged changes | `git diff --staged` |
| Create a commit | `git commit` |
| Inspect compact history | `git log --oneline --graph --decorate --all` |
| Create and switch branches | `git switch -c <branch>` |
| Temporarily save work | `git stash` |
| Download remote updates | `git fetch` |
| Integrate remote updates | `git pull` |
| Publish commits | `git push` |
| Undo a commit without rewriting history | `git revert <commit>` |
| Unstage a file | `git restore --staged <file>` |

For command-specific details, use `git help <command>` or `git <command> --help`.
