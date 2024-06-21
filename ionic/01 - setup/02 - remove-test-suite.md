```I this project we are going to use a test framework called cypress instead of the default test framework jasmine which comes with ionic installation.

cypress is more suitable and more pleasant to work with.```


### Step 1: Remove Test Dependencies
```
npm uninstall jasmine-core jasmine-spec-reporter karma karma-chrome-launcher karma-coverage karma-jasmine karma-jasmine-html-reporter @types/jasmine @types/jasminewd2
```

### Step 2: Remove Test Configuration Files 
```
rm -f karma.conf.js src/test.ts src/tsconfig.spec.json
```

### Step 3: Update angular.json
```
Edit the angular.json file to remove the test configuration.
test: {...}
```

### Step 4: Remove All .spec.ts Files
```
cd into you project
find src -name "*.spec.ts" -type f -delete
```