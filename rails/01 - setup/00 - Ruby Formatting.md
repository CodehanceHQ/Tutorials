## Check Gemfile for Rufo

```
# In your Gemfile, add Rufo to the development group for Ruby formatting

group :development do
  gem 'rufo', '~> 0.18.0' # Ruby formatter
end
```

## Install

```
# Install the gems specified in the Gemfile
# Run this command in your terminal

bundle install
```

## VSCode settings

```
Open VSCode settings by pressing Cmd + , on macOS or Ctrl + , on Windows/Linux.
```

## Open settings.json

```
Click on the file icon in the top right corner to open the settings.json file.
```

## Add the following settings

```
"ruby.format": "rufo",
"[ruby]": {
    "editor.defaultFormatter": "jnbt.vscode-rufo",
    "editor.formatOnSave": true,
    "editor.formatOnType": true,
    "editor.tabSize": 2,
    "editor.insertSpaces": true,
    "files.trimTrailingWhitespace": true,
    "files.insertFinalNewline": true,
    "files.trimFinalNewlines": true,
    "editor.rulers": [120],
    "editor.semanticHighlighting.enabled": true
  }
```

## Search and Install VSCode Extensions

```
Install Ruby LSP by shopify
Install Rufo - Ruby formatter by jnbt
```
