### Step 1: Add to dev dependencies in Package.json
```
...
"devDependencies": {
  ...
  "cypress": "^13.12.0",
  "@cypress/angular": "^2.0.4"
},

# Question
# 1: What is cypress testing in javascript?
# 2: Why is it important to write tests?
# 3: What types of tests can we write with cypress?
```

### Step 2: Install
```
npm install
```

### Step 3: Update package.json
```
# Add Cypress scripts to your package.json to 
# make it easier to run Cypress,
# this means we can run cypress as `npm run cypress:open`
# and also `npm run cypress:run`

"scripts": {
  "cypress:open": "cypress open",
  "cypress:run": "cypress run"
}

# Questions:
# 1: What does this help achieve?
# 2: What two ways can we now run cypress?
```

### Step 4: Open Cypress
```
# This will also create the necessary folder 
# structure and example files, click on e2e so it gets
# configured.

npm run cypress:open
```

### Add base url to (cypress.config.ts)
##### It is in the root of your app
```
import { defineConfig } from "cypress";

export default defineConfig({
  e2e: {
    setupNodeEvents(on, config) {
      // implement node event listeners here
    },
    baseUrl: "http://localhost:8100", # <<<<<<< Add this!
  },

  component: {
    devServer: {
      framework: "angular",
      bundler: "webpack",
    },
    specPattern: "**/*.cy.ts",
  },
});

# Questions:
# 1: Why did we add the baseUrl?
# 2: What will runs in that url?
# 3: Why must it match your ionic app url?
# 4: Why must you update this to match your ionic app url?
```

### Step 5: Create end to end (e2e) folder
```
mkdir -p cypress/e2e

# Question:
# 1: What is end 2 end testing?
# 2: What is the alternative to end 2 end testing?
```

### Conclusion
```
Congratulations! you have setup your project to use cypress, once we have learnt a little more about building ionic mobile apps, then you can go ahead and test it with cypress!

Happy testing to come :)
```