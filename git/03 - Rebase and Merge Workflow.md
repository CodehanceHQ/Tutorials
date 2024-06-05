## Rebase and Merge Workflow

### Understanding Git Rebase and Merge Strategies
```
# Questions:
# 1. What does "rebase and merge" mean in the context of Git?
# 2. What are the benefits of using "rebase and merge"?
# 3. How does rebasing affect the commit history?
# 4. When should you prefer "rebase and merge" over other merge strategies?
# 5. What potential drawbacks might there be to using "rebase and merge"?
```

### Step 1: Fetch the Latest Changes from Main Branch
```
# Ensure your local repository is up-to-date with the remote repository:
git fetch origin main

# Questions:
# 1. What is the purpose of `git fetch origin main`?
# 2. How does `git fetch` differ from `git pull`?
# 3. What happens if you skip this step before merging?
```

### Step 2: Checkout the Main Branch
```
# Switch to the main branch:
git checkout main

# Questions:
# 1. Why is it important to be on the main branch before merging?
# 2. What is the difference between `git checkout main` and `git checkout -b main`?
```

### Step 3: Create a Feature Branch (following JIRA naming convention)
```
# Create a new feature branch:
git checkout -b DEAL-123-description 

# Questions:
# 1. Why is it good practice to create a new branch for each feature or bug fix?
# 2. What does the `-b` option do in the `git checkout` command?
```

### Step 4: Work on Your Feature
```
# Make changes and commit them to your feature branch:
git add .
git commit -m "DEAL-39 your message here"

# Questions:
# 1. Why should you commit frequently?
# 2. What makes a good commit message?
# 3. Why is it important for your JIRA ticket number to match your commit message?
```

### Step 5: Fetch the Latest Changes from Main Again
```
# Ensure your feature branch is up-to-date with the main branch:
git fetch origin main

# View the latest commits on the main branch:
git log origin/main

# Compare changes between your branch and the main branch:
git diff origin/main

# Questions:
# 1. Why fetch from main again before merging your changes?
# 2. Why is it important to keep your feature branch up-to-date with the main branch?
# 3. What does `git log` show you?
# 4. What does `git diff` show you?
```

### Step 6: Rebase onto the Main Branch
```
# Rebase your feature branch onto the latest main branch:
git rebase origin/main

# Questions:
# 1. What does the `git rebase` command do?
# 2. How does rebasing differ from merging?
# 3. What are the advantages of rebasing over merging?
```

### Step 7: Resolve Any Conflicts (if any)
```
# If there are conflicts, Git will pause the rebase and allow you to resolve them manually:
# Open the conflicting files in your editor and resolve the conflicts.
# Stage the resolved files:
git add <resolved-file>
# Continue the rebase process:
git rebase --continue

# Questions:
# 1. What steps should you take to resolve a rebase conflict?
# 2. How can you view the conflicted areas in the files?
# 3. What does the `git add` command do in the context of a rebase conflict?
```

### Step 8: Force Push the Rebased Branch
```
# After resolving conflicts and completing the rebase, force push the rebased branch to the remote repository:
git push --force origin DEAL-123-description

# Questions:
# 1. Why is it necessary to force push after rebasing?
# 2. What are the risks associated with force pushing?
# 3. How can you minimize the risks of force pushing?
```

### Step 9: Open a Pull Request
```
# Go to your repository on GitHub.
# Click on the "Pull requests" tab.
# Click "New pull request".
# Select your feature branch and the base branch (main).
# Review the changes, add a title and description for the pull request, and submit it.

# Questions:
# 1. What is the purpose of a pull request in the Git workflow?
# 2. How can you ensure that the pull request is ready to be merged?
# 3. What steps should you take after opening a pull request?
```

### Step 10: Merge the Pull Request on GitHub
```
# Go to your repository on GitHub.
# Navigate to the "Pull requests" tab.
# Find your open pull request and review the changes.
# Click "Merge pull request" to merge the feature branch into the main branch.
# Confirm the merge if prompted.

# Questions:
# 1. What does merging a pull request do in terms of branch history?
# 2. How can you review changes in a pull request before merging?
# 3. What options do you have for merging (e.g., create a merge commit, squash and merge, rebase and merge)?
```

### Step 11: Delete the Feature Branch on GitHub
```
# After merging the pull request, you can delete the feature branch:
# Click the "Delete branch" button in the merged pull request view.

# Questions:
# 1. Why is it a good practice to delete the feature branch after merging?
# 2. Can you recover a deleted branch? How?
```

### Step 12: Pull the Latest Changes Locally
```
# Ensure your local repository is up-to-date with the remote repository after merging:
git checkout main
git pull origin main

# Delete the local feature branch:
git branch -d DEAL-123-description

# Questions:
# 1. Why is it important to pull the latest changes after merging a pull request?
# 2. What does the `git branch -d` command do?
# 3. What happens if you try to delete a branch that has unmerged changes?
```

### Conclusion
```
This guide walks you through the rebase and merge workflow in Git, covering the steps of fetching updates, creating a feature branch, keeping it up-to-date with the main branch, rebasing onto the main branch, resolving conflicts, force pushing, and opening a pull request. 

It also includes merging the pull request on GitHub, deleting the feature branch, and pulling the latest changes locally.

By following these steps, you can maintain a clean and linear commit history and effectively manage your Git workflow.
```
