
### Installing Ruby and Rails on macOS
```
# Check if Homebrew is installed
brew -v

# If Homebrew is not installed, follow instructions it prints to add brew to your path
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

# Update Homebrew
brew update

# Install RVM (Ruby Version Manager)
\curl -sSL https://get.rvm.io | bash -s stable

# Install necessary dependencies
brew install openssl readline libyaml gmp

brew install openssl@1.1
brew link --overwrite openssl@1.1

# Add OpenSSL to your PATH
echo 'export PATH="/usr/local/opt/openssl@1.1/bin:$PATH"' >> ~/.zshrc

# Install Ruby 3.3.1 with OpenSSL
rvm install 3.3.1 --with-openssl-dir=$(brew --prefix openssl@1.1)

# Set Ruby 3.3.1 as the default version
rvm use 3.3.1 --default

# Add RVM to your shell profile
echo 'export PATH="$HOME/.rvm/bin:$PATH"' >> ~/.zshrc
echo 'source "$HOME/.rvm/scripts/rvm"' >> ~/.zshrc

# Reload the shell configuration
source ~/.zshrc

# Install Rails
gem install rails -v 7.1.3

# Verify the Rails installation
rails -v
```

# Installing Ruby and Rails on Windows
```
# Download and install RubyInstaller for Windows from https://rubyinstaller.org/downloads/
# Choose the Ruby+Devkit 3.3.1 version and ensure to add Ruby to your PATH during the installation process

# After installation, open a new command prompt and verify Ruby installation
ruby -v

# Install Rails
gem install rails -v 7.1.3

# Verify the Rails installation
rails -v
```