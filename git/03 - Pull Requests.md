## Creating Pull Request

### Preparing Pull Request
```
# Check the status of your working directory
git status

# Verify you are on your feature branch
git branch

# Pull the latest changes from the remote main branch into your feature branch
git pull origin main

# Check the status again to see if there are any merge conflicts or changes
git status

# Only if no conflicts!
git push
```

### Intiate on Github OR Terminal
```
# github process:

# In github click on:
Pull requests > New Pull Request  

# Choose your branch from
compare: main ( drop down )

# click
Create Pull Request 

# ========== OR ========== 

# terminal process:

# install (gh cli)
https://github.com/cli/cli

# login to gh
gh auth login

# create pull request
gh pr create --base main --title "DEAL-20 Feature name here" --body "Adding new feature"
```

### Questions
```
# 1: How did we integrate work from main branch into the feature branch?
# 2: Why is it important to pull main into feature branch?
# 3: Why might conflicts arise from pulling main into feature branch?
# 4: How do you install `gh` command for github?
```