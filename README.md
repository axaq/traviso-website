www.travisojs.com
================

1. Install prerequisites:

```bash
# Install rbenv (if using Homebrew)
brew install rbenv ruby-build

# Initialize rbenv
rbenv init

# Install a recent Ruby version
rbenv install 3.2.0
rbenv local 3.2.0

# Now gem install
gem install jekyll
```

2. Start locally watching for changes:

```bash
jekyll serve -w
```

3. Access the home page at `http://localhost:4001/`
