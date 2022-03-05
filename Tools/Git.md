# Git fun commands and aliases

## Aliases

### These are my zsh useful aliases 

### Status

- > **gst** ===> git status

### Add

- > **ga** ===> git add

### Commit

- > **gc** ===> git commit -v
- > **gca** ===> git commit -v -a
- > **gcam** ===> git commit -v -a -m

### Logs

- > **glo** ===> git log --oneline --decorate'
- > **glod** ===> git log --graph --pretty='\''%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'\
- > **glods** ===> git log --graph --pretty='\''%Cred%h%Creset -%C(auto)%d%Creset %s %Cgreen(%ad) %C(bold blue)<%an>%Creset'\'' --date=short
- > **glog** ===> git log --oneline --decorate --graph
- > **gloga** ===> git log --oneline --decorate --graph --all

### Show

- > **gsps** ===> git show --pretty=short --show-signature'

### Branch

- > **gb** ===> git branch
- > **gba** ===> git branch -a
- > **gbd** ===> git branch -d

### Blame

- > **gbl** ===> git blame -b -w

### Checkout

- > **gco** ===> git checkout
- > **gcb** ===> git checkout -b

### Rebase

- > **grb** ===> git rebase
- > **grba** ===> git rebase --abort
- > **grbc** ===> git rebase --continue
- > **grbs** ===> git rebase --skip

### Pull

- > **gl** ===> git pull
- > **ggpull** ===> git pull origin $(git_current_branch)
- > **glum** ===> git pull upstream $(git_main_branch)
- > **gpr** ===> git pull --rebase

### Push

- > **gp** ===> git push
- > **gpv** ===> git push -v
- > **gpu** ===> git push upstream
- > **ggpush** ===> git push origin "$(git_current_branch)"
- > **gpsup** ===> git push --set-upstream origin $(git_current_branch)

### Fetch

- > **gf** ===> git fetch
- > **gfo** ===> git fetch origin
- > **gfa** ===> git fetch --all --prune --jobs=10
