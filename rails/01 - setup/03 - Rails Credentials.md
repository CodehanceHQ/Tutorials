# Understanding Credentials

### credentials

When we create rails app it creates credentials
```
# Questions
# 1: Where does it save master.key & credentials.yml.enc?
# 2: What is the credentials.yml.enc?
# 3: How is the master key linked to credentials.yml.enc?
```

### development credentials

Running below will create development credentials, this way developers
can share the same dev keys ( non production ) keys.
```
EDITOR="code --wait" rails credentials:edit

# Questions:
# 1: Why is it important for devs to share dev api keys?
# 2: How does credentials.yml.enc make things easier for collaborations?
# 3: What key is needed to decript credentials.yml.enc?
# 4: Should all devs have access to dev. credentials.yml.enc file?
# 5: Should dev credentials.yml.enc contain sensitive information?
# 6: Why should we commit dev master.key and credentials.yml.enc?
# 7: How do we make sure master key and credentials are not ignored by git?
```

### production credentials

This contain sensitive data so this is the place to enter that information.
```
EDITOR="code --wait" rails credentials:edit --environment production

# Questions:
# 1: What folder and files does this create?
# 2: Why is it important to add the production.key in .gitignore?
# 3: In production how do we add the production.key?
# 4: What env. variable do we need in production for the master key?
```