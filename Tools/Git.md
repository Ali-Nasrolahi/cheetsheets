# Git commands and aliases

- [Git commands and aliases](#git-commands-and-aliases)
  - [Git commands explanation](#git-commands-explanation)
    - [Initialize and help](#initialize-and-help)
    - [Status of Working directory](#status-of-working-directory)
    - [Adding](#adding)
    - [Committing & resetting](#committing--resetting)
    - [Log](#log)
  - [Aliases](#aliases)
    - [These are my zsh useful aliases](#these-are-my-zsh-useful-aliases)
    - [Status](#status)
    - [Add](#add)
    - [Commit](#commit)
    - [Logs](#logs)
    - [Show](#show)
    - [Branch](#branch)
    - [Blame](#blame)
    - [Checkout](#checkout)
    - [Rebase](#rebase)
    - [Pull](#pull)
    - [Push](#push)
    - [Fetch](#fetch)
    - [Stash](#stash)

## Git commands explanation

Please checkout [Basic of Git] to familiarize yourself with git concepts and Version Control Systems (VCS)

### Initialize and help

```bash
git init [dir]      # Create an empty Git repository or reinitialize an existing one
git help [COMMAND]  # Display help information about Git
```

### Status of Working directory

```bash
git status              # Shows the working tree status
git status -s, --short  # Give the output in the short-format.
```

### Adding

```bash
git add                 # Add file contents to the index
git add -v, --verbose   # Be verbose
git add -A, --all       # all files in the entire working tree are updated 
git add -u, --update    # Update the index just where it already has an entry matching <pathspec>.

#Examples
git add git-*.sh
git add Documentation/\*.txt
```

checkout `git add` [aliases](#add) as well.

### Committing & resetting

```bash
git commit                            # Record changes to the repository
git commit  -a, --all                 # moves already added files to stage area 
git commit  -m <msg>, --message=<msg> # Use the given <msg> as the commit message.

git reset
```

checkout `git commit` [aliases](#commit) as well.

### Log

You can extremely customize `git log` output checkout `git help log` for more options.

```bash
git log             # Show commit logs
git log --decorate  # Print out ref names of any commits that are shown
git log --oneline   # Shorthand for --pretty=oneline --abbrev-commit
git log --pretty    # Pretty print commit messages        
git log --graph     # Display graphical representation of commit history

```

checkout `git log` [aliases](#logs) as well.

## Aliases

### These are my zsh useful aliases

### Status

```bash
gst     #git status
```

### Add

```bash
ga      # git add
gaa     # git add --all
gav     # git add --verbose
```

### Commit

```bash
gc      #git commit -v
gca     #git commit -v -a
gcam    #git commit -v -a -m
```

### Logs

```bash
glo     #git log --oneline --decorate'
glod    #git log --graph --pretty='\''%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'\
glods   #git log --graph --pretty='\''%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'\'' --date=short
glog    #git log --oneline --decorate --graph
gloga   #git log --oneline --decorate --graph --all
```

### Show

```bash
gsps    #git show --pretty=short --show-signature'
```

### Branch

```bash
gb      #git branch
gba     #git branch -a
gbd     #git branch -d
```

### Blame

```bash
gbl     #git blame -b -w
```

### Checkout

```bash
gco     #git checkout
gcb     #git checkout -b
```

### Rebase

```bash
grb     #git rebase
grba    #git rebase --abort
grbc    #git rebase --continue
grbs    #git rebase --skip
```

### Pull

```bash
gl      #git pull
ggpull  #git pull origin $(git_current_branch)
glum    #git pull upstream $(git_main_branch)
gpr     #git pull --rebase
```

### Push

```bash
gp      #git push
gpv     #git push -v
gpu     #git push upstream
ggpush  #git push origin "$(git_current_branch)"
gpsup   #git push --set-upstream origin $(git_current_branch)
```

### Fetch

```bash
gf      #git fetch
gfo     #git fetch origin
gfa     #git fetch --all --prune --jobs=10
```

### Stash

``` bash
gstaa   #git stash apply
gstc    #git stash clear
gstd    #git stash drop
gstl    #git stash list
gstp    #git stash pop
gsts    #git stash show --text
```

[Basic of Git]: https://www.freecodecamp.org/news/learn-the-basics-of-git-in-under-10-minutes-da548267cc91/
