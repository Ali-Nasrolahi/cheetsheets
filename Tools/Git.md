# Git fun commands and aliases

## Aliases

### These are my zsh useful aliases 

### Status

```bash
gst #git status
```

### Add

```bash
ga #git add
```

### Commit

```bash
gc #git commit -v
gca #git commit -v -a
gcam #git commit -v -a -m
```

### Logs

```bash
glo #git log --oneline --decorate'
glod #git log --graph --pretty='\''%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'\
glods #git log --graph --pretty='\''%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'\'' --date=short
glog #git log --oneline --decorate --graph
gloga #git log --oneline --decorate --graph --all
```

### Show

```bash
- > **gsps** ===> git show --pretty=short --show-signature'
```

### Branch

```bash
gb** ===> git branch
gba #git branch -a
gbd #git branch -d
```

### Blame

```bash
gbl #git blame -b -w
```

### Checkout

```bash
gco #git checkout
gcb #git checkout -b
```
### Rebase

```bash
grb #git rebase
grba #git rebase --abort
grbc #git rebase --continue
grbs #git rebase --skip
```

### Pull

```bash
gl #git pull
ggpull #git pull origin $(git_current_branch)
glum #git pull upstream $(git_main_branch)
gpr #git pull --rebase
```

### Push

```bash
gp #git push
gpv #git push -v
gpu #git push upstream
ggpush #git push origin "$(git_current_branch)"
gpsup #git push --set-upstream origin $(git_current_branch)
```

### Fetch

```bash
gf # git fetch
gfo #git fetch origin
gfa # git fetch --all --prune --jobs=10
```

### Stash

``` bash
gstaa #git stash apply
gstc #git stash clear
gstd #git stash drop
gstl #git stash list
gstp #git stash pop
gsts #git stash show --text
```
