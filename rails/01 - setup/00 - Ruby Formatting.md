## Check Gemfile for Rufo

```
# In your Gemfile, add if not already there

group :development do
  gem 'rubocop-rails', '~> 2.25'
  gem 'rubocop', '~> 1.64', '>= 1.64.1'
end
```

## Install

```
# Install the gems specified in the Gemfile
# Run this command in your terminal

bundle install
```

## .vscode
```
mkdir .vscode
touch .vscode/settings.json

{
  "ruby.format": "rubocop",
  "ruby.lint": {
    "rubocop": true
  },
  "[ruby]": {
    "editor.defaultFormatter": "misogi.ruby-rubocop",
    "editor.formatOnSave": true,
    "editor.formatOnType": true,
    "editor.tabSize": 2,
    "editor.insertSpaces": true,
    "files.trimTrailingWhitespace": true,
    "files.insertFinalNewline": true,
    "files.trimFinalNewlines": true,
    "editor.rulers": [120],
    "editor.semanticHighlighting.enabled": true,
  }
}
```

## Search and Install VSCode Extensions

```
Install Ruby LSP by shopify
Install ruby-rubocop by misogi
```

## .rubocop.yml file ( root of project )
```
# The AllCops section applies settings globally.
AllCops:
  Exclude:
    - 'db/schema.rb' # Exclude schema file from linting.
    - 'bin/*'
    - 'config/**/*'
    - 'spec/**/*'
    - 'vendor/**/*'
    - 'tmp/**/*'
    - 'log/**/*'
    - 'public/**/*'
    - 'coverage/**/*'
    - 'Gemfile'
    - 'Gemfile.lock'
    
  TargetRubyVersion: 3.3.1 # Set the target Ruby version for your project.
  NewCops: enable

# Style settings for different cops (Style, Layout, etc.)
Layout/LineLength:
  Max: 100 # Set max line length to 100 characters.

Metrics/BlockLength:
  Exclude:
    - 'spec/**/*.rb' # Exclude spec files from block length cop.

# Enable or disable specific cops.
Lint/UnusedMethodArgument:
  Enabled: true

Style/FrozenStringLiteralComment:
  Enabled: true

# Define settings for Rails projects using rubocop-rails.
require: rubocop-rails

Rails/SkipsModelValidations:
  Enabled: true

# Configure new cops
Gemspec/DeprecatedAttributeAssignment:
  Enabled: true

Style/StringLiterals:
  EnforcedStyle: single_quotes
```

## Run rubocop on a particular file
```
# shows you issues to fix
rubocop app/controllers/graphql_controller.rb

# auto fixes issues
rubocop -A app/controllers/graphql_controller.rb
```