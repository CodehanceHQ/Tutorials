## Default Merge Workflow

### Understanding Git Merge Strategies
```
# Questions:
# 1. What is a merge commit, and how does it differ from a rebase?
# 2. What are the advantages and disadvantages of using a merge commit?
# 3. When might you choose to use a merge commit over a rebase or squash?
# 4. How can merge conflicts be resolved in Git?
# 5. What is a fast-forward merge, and when is it used?
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
# 3. Why is it import for your JIRA ticket number to match your commit message?
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

### Step 6: Merge the Main Branch into Your Feature Branch
```
# Merge the latest changes from the main branch into your feature branch:
git merge origin/main

# Resolve any conflicts if necessary:
# Open the conflicting files in your editor and resolve the conflicts.
# Stage the resolved files:
git add <resolved-file>
# Continue the merge process:
git commit

# Questions:
# 1. Why might you need to merge the main branch into your feature branch before merging your feature branch back into the main branch?
# 2. What are merge conflicts?
# 2. How do you resolve merge conflicts?
```

### Step 7: Push Your Feature Branch
```
# Push your feature branch to the remote repository:
git push origin DEAL-123-description

# Questions:
# 1. Why is it important to push your branch to the remote repository?
# 2. What happens if you try to push without first fetching and merging changes from the main branch?
```

### Step 8: Open a Pull Request
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

### Step 9: Merge the Pull Request on GitHub
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

### Step 10: Delete the Feature Branch on GitHub
```
# After merging the pull request, you can delete the feature branch:
# Click the "Delete branch" button in the merged pull request view.

# Questions:
# 1. Why is it a good practice to delete the feature branch after merging?
# 2. Can you recover a deleted branch? How?
```

### Step 11: Pull the Latest Changes Locally
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
This guide walks you through the default merge workflow in Git, covering the steps of fetching updates, creating a feature branch, keeping it up-to-date with the main branch, merging feature branches, resolving conflicts, pushing changes, and opening a pull request. It also includes merging the pull request on GitHub, deleting the feature branch, and pulling the latest changes locally.

By following these steps, you can effectively manage your Git workflow and ensure smooth integration of feature branches into the main branch.
```
