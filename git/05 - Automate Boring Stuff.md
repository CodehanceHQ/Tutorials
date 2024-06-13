## Automating Boring Stuff
##### Git commands can get really boring since we have to use them multiple times
##### in a working day, so here we are going to automate it.

### Step 1: What shell are you using ( terminal )
```
echo $SHELL

# output examples:

/bin/bash for Bash
/bin/zsh for Zsh
/bin/fish for Fish
```

### Step 2: Open with VSCode
```
# Bash Users:
code ~/.bashrc

# Zsh Users:
code ~/.zshrc

# Fish Users:
code ~/.config/fish/config.fish
```

### Step 3: Copy these and add to bottom of file
```
# ===============================================
# Git aliases
# ===============================================

# Status
alias gs='git status'
# Example usage:
# gs

# Check out main
alias gcm='git checkout main'
# Example usage:
# gcm

# Stage changes
alias ga='git add .'
# Example usage:
# ga

# Git commit
alias gc='git commit -m'
# Example usage:
# gc "Your commit message"

# Push changes to the remote repository
alias gp='git push'
# Example usage:
# gp

# Pull changes from the remote repository
alias gpl='git pull'
# Example usage:
# gpl

# Switch to a different branch
alias gco='git checkout'
# Example usage:
# gco branch-name

# Create a new branch and switch to it
alias gcb='git checkout -b'
# Example usage:
# gcb new-branch-name

# Clone a remote repository
alias gcl='git clone'
# Example usage:
# gcl https://github.com/user/repository.git

# Merge a branch into the current branch
alias gm='git merge'
# Example usage:
# gm branch-to-merge

# Delete a remote branch
alias gbdr='git push origin --delete'
# Example usage:
# gbdr branch-name

# Fetch changes from the remote repository
alias gf='git fetch'
# Example usage:
# gf

# List all branches (local and remote)
alias gb='git branch -a'
# Example usage:
# gb

# Add all changes and commit with a message
alias gac='git add . && git commit -m'
# Example usage:
# gac "Your commit message"

# Create a GitHub pull request with the same title and body
alias gpr='gh pr create --base main --title "$1" --body "$1"'
# Example usage:
# gpr "PR title" "PR body"

# Function to switch to a branch and pull the latest changes
alias gcp='function _gcp(){ git checkout $1 && git pull };_gcp'

# Example usage:
# gcp branch-name

# Function to add all changes, commit with a message, and push
alias gacp='function _gacp(){ git add . && git commit -m "$1" && git push };_gacp'

# Example usage:
# gacp "Your commit message"

# Function to create a GitHub pull request with title and body derived from the branch name
alias gpr='function _gpr(){
    current_branch=$(git symbolic-ref --short HEAD);
    body=${1:-$current_branch}
    gh pr create --base main --title "$current_branch" --body "$body";
}; _gpr'

# Example usage:
# gpr "PR what you did" or gpr

# Function to push to origin and set upstream for the current branch
alias gpo='function _gpo(){ current_branch=$(git symbolic-ref --short HEAD); git push origin -u "$current_branch"; }; _gpo'
# Example usage:
# gpo

# Function to add commit and push origin all in one
alias gacpu='function _gacpu(){ current_branch=$(git symbolic-ref --short HEAD); git add . && git commit -m "$1" && git push origin -u "$current_branch"; }; _gacpu'
# Example usage:
# gacpu "Your commit message"

# Function to pull from origin main into feature branch
alias gpm='function _gpm(){ current_branch=$(git symbolic-ref --short HEAD); git checkout main && git pull && git checkout "$current_branch" && git merge main; }; _gpm'
# Example usage:
# gpm

# Function to remove the current branch by checking out to main, then removing the branch and pulling from main
alias gbd='function _gbd(){
    current_branch=$(git symbolic-ref --short HEAD);
    name=${1:-$current_branch}
    git checkout main && git pull && git branch -d "$current_branch";
}; _gbd'
# Example usage:
# gbd branch-name or gbd (this deletes current branch)
```

### Step 4: Reload your (dot) file
```
# For Zsh
source ~/.zshrc

# For Bash
source ~/.bashrc

# For Fish
source ~/.config/fish/config.fish
```