---
title: Jekyll
---

# Start

Create a new Jekyll site:

    jekyll new SITE-NAME
    cd SITE-NAME

Create a Jekyll site for an existing folder:

    cd EXISTING-FOLDER
    jekyll new --skip-bundle . --force


# GitHub Pages

If publishing on GitHub Pages, on `Gemfiles`, comment this:

    # gem "jekyll", "~> X.X.X"

Uncomment that:

    gem "github-pages", group: :jekyll_plugins


# Install dependencies

    bundle install
    
# Local preview

    bundle exec jekyll serve
    
Open: <http://localhost:4000>

# Live reload

    bundle exec jekyll serve --live-reload

# Configuration

Create/edit `./_config.yml`:

```yml
title: Awesome Jekyll Site
author: Your Name
description: Some description...
baseurl: /PROJECT-NAME/
url: https://USERNAME.github.io
github_username:  USERNAME
theme: minima
```

Temas compatíveis: <https://pages.github.com/themes/>

# "Event Machine" Error

If you get an error with `eventmachine` on Windows, try:

    gem uninstall eventmachine
    gem install eventmachine --platform=ruby

