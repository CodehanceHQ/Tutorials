# Setting up Mongo DB

MongoDB is a NoSQL database designed to handle large volumes of diverse data quickly and efficiently. Unlike traditional relational databases, MongoDB stores data in flexible, JSON-like documents, making it easier to model, manipulate, and query data without needing a fixed schema.

This flexibility is especially valuable for projects that deal with heterogeneous data sources, such as pulling property information from different APIs and enriching it with additional data. In our project, where we need to aggregate, merge, and update property data from various sources, MongoDB's schema-less structure allows us to easily adapt to the different formats and structures of incoming data.

### Pull main
##### I updated the code to use mongo in place of Active Record
```
# checkout into main
git checkout main

# fetch updated main
git pull
```

### Install gems
```
bundle install
```

### Install Mongo DB locally (Mac)
```
# update brew
brew update

# tap and install brew
brew tap mongodb/brew
brew install mongodb-community@7.0

# start brew
brew services start mongodb/brew/mongodb-community

# fix known permission issues
sudo chown -R `id -un` /usr/local/var/mongodb
sudo chown -R `id -un` /usr/local/var/log/mongodb
sudo chown -R `id -un` /usr/local/opt/mongodb-community
sudo chown -R `id -un` /usr/local/var/homebrew/linked/mongodb-community

# check it is running
brew services list

# install compass GUI
brew install --cask mongodb-compass

# open from your App folder or using below command
open -a "MongoDB Compass"

# Upon opening MongoDB Compass, 
# you will be presented with the Connect screen.
# Connect to the locally installed mongodb
mongodb://localhost:27017

```

### Install Mongo DB locally (Win)
```
# download version 7 here:
https://www.mongodb.com/try/download/community

# Run the downloaded installer and follow the setup instructions.

# Choose "Complete" setup type.

# Ensure that "Install MongoDB as a Service" is selected.

# Set the service configuration to start MongoDB automatically.

# Finish the installation process and close the installer.

# download and install GUI (compass)
https://www.mongodb.com/try/download/compass
```

### Start rails server
```
rails server
```

### Check all tests pass
```
rspec
```

### Signup mutations
```
# run signup

mutation {
  signUp(input: {
    fullName: "John Doe",
    email: "example@kernuo.com",
    password: "PAssword@123",
    confirmPassword: "PAssword@123",
    telephone: "1234567890"
  }) {
    data {
      id
      fullName
      email
      telephone
    }
    message,
    errors,
    httpStatus
  }
}
```

### Inspect mongo compass
```
# inspect to  see your record as a document
```