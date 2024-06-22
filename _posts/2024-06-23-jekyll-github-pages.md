---
layout: single
title: Installing Jekyll Github Pages with Custom Theme
tags:
  - fedora
  - github
  - jekyll
categories:
  - notes
header:
  overlay_filter: "0.5"
  overlay_color: "#000"
  teaser: /assets/the-most-terrifying.png
  overlay_image: /assets/the-most-terrifying.png
excerpt: How to create Github Pages with Jekyll remote theme 
---
I was sick and tired having Github emails me about obsolete packages on my Github Pages blog. I was using Minimal Mistakes for a long time. I was busy and my blog was neglected in years. The GH tutorial on [how to setup Jekyll-based GH Pages][gh-tutor] was confusing to read.

# Preparation

Now I have time to understand how remote theme works. I'm also on a new environment, GNU/Linux Fedora. From a blank state, install `gh`(Github utility) and Ruby's `gem`:

```bash
sudo dnf install gh ruby-devel 
```

# New Repository

```bash
gh repo create jpmrblood.github.io --clone --public
```
Or, go to the GH page, instead, to create the site manually.

# Install Jekyll

Check on [supported library][gh-support] section. In the time of writing, Jekyll is 3.9.5.

```bash
gem install jekyll -v 3.9.5
```

# Create Site

Create site:

```bash
mkdir my_blog && cd my_blog
jekyll new --skip-bundle . --force
```
On `Gemfile`, remove `jekyll`and `minima`:

```ruby
gem "jekyll", "~> 3.9.5"
gem "minima", "~> 2.0"
```

Add:

```ruby
gem "jekyll-include-cache", group: :jekyll_plugins
gem "github-pages", group: :jekyll_plugins
```

Change the `theme` with `remote-theme` 

```patch
--- _config.yml.bak     2024-06-23 02:22:52.469379746 +0700
+++ _config.yml 2024-06-23 02:25:04.042035654 +0700
@@ -26,7 +26,8 @@
 
 # Build settings
 markdown: kramdown
-theme: minima
+remote_theme: "mmistakes/minimal-mistakes@4.26.2"
+minimal_mistakes_skin    : "sunrise"
 plugins:
   - jekyll-feed
 
```

Minimal Mistakes needs these plugins:

```yaml
plugins:
  - jekyll-paginate
  - jekyll-sitemap
  - jekyll-gist
  - jekyll-feed
  - jemoji
  - jekyll-include-cache
```

Run the installer:

```bash
bundle install
```

Next, add `Gemfile.lock` to GIT ignore:

```bash
 echo "Gemfile.lock" >> .gitignore 
 ```

 After this, I removed `index.md` and `about.md`. The posts are in the `_posts` directory.
 The rest is to have the configuration like Minimal Mistakes.


[gh-tutor]: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll
[gh-support]: https://pages.github.com/versions/