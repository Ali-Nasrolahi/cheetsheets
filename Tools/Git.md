# Git fun commands and aliases

## Aliases

### These are my zsh useful aliases might not be available at your machine

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
