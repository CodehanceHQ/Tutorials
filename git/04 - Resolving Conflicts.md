## Resolving Conflict

### Steps
```
# start off in your feature branch
git checkout <feature-branch>

# pull main into feature ( most conflicts at this point )
git pull origin main

# inspect terminal to see `CONFLICTS`
git status

# open each conflict files and look for:
lines marked with `<<<<<<<`, `=======`, and `>>>>>>>`.

# resolve conflicts
`communicate with your collaborators`

# stage changes
git add .

# commit changes with or without message
`git commit` or `git commit -m "Resolved merge conflicts"`

# push up changes
git push
```

### Questions
```
# 1: What are merge conflicts?
```

```
# 2: What stage are you most likely to see a merge conflict?
```

```
# 3: When do you remove: `<<<<<<<`, `=======`, and `>>>>>>>`?
```

```
# 4: What are `<<<<<<<`, `=======`, and `>>>>>>>`?
```

```
# 5: Why speak to your team whilst fixing conflicts?
```