---
layout: single
title: Setup Ruby Bundler to Run This Blog Locally
tags:
  - golang
  - upgrade
categories:
  - notes
header:
  overlay_color: "#000"
  overlay_filter: "0.5"
  teaser: /assets/2025/02/ruby.png
  overlay_image: /assets/2025/02/ruby.png
excerpt: Setup Ruby with RVM
---
# Install RVM

Let's install RVM first:

```bash
gpg --keyserver keyserver.ubuntu.com --recv-keys 409B6B1796C275462A1703113804BB82D39DC0E3 7D2BAF1CF37B13E2069D6956105BD0E739499BDB
\curl -sSL https://get.rvm.io | bash -s stable
```

To load RVM without restarting terminal:

```bash
source ${HOME}/.rvm/scripts/rvm
```

# Install Ruby

Based on [Ruby's site](https://www.ruby-lang.org/id/downloads/), the current version is 3.4.2.

```bash
rvm install 3.4.2
rvm use 3.4.2
```

# Install Bundler

``` bash
gem install bundler
gem update --system 3.6.5
```

# Install Jekyll

Let's install Jekyll and `github-pages`.

```bash
gem install jekyll
gem install github-pages
```

Add `csv` and `logger` gem to `Gemfile` for newer Ruby.
NOTE: I also removed one of the multiple lines of `gem jekyll-github-metadata`.

```diff
diff --git a/Gemfile b/Gemfile
index 2a5af32..4aba967 100644
--- a/Gemfile
+++ b/Gemfile
@@ -13,7 +13,6 @@ group :jekyll_plugins do
   gem "jekyll-coffeescript"
   gem "jekyll-commonmark-ghpages"
   gem "jekyll-gist"
-  gem "jekyll-github-metadata"
   gem "jekyll-paginate"
   gem "jekyll-relative-links"
   gem "jekyll-optional-front-matter"
@@ -22,6 +21,10 @@ group :jekyll_plugins do
   gem "jekyll-titles-from-headings"
 end
 
+# Add this for newer Ruby
+gem "csv"
+gem "logger"
+
 # Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
 # and associated library.
 platforms :mingw, :x64_mingw, :mswin, :jruby do
@@ -41,3 +44,4 @@ gem "kramdown-parser-gfm"
 gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]
 
 gem "webrick", "~> 1.8"
 ```

 Let's install the remaining Gems:

 ```bash
 bundler install
 ```

 # Running

 Just run as usual
 
 ```bash
 jekyll serve --incremental
 ```

# References

- [RVM](https://rvm.io/)
- [Bundler](https://rvm.io/integration/bundler)
- [Installing Rubies](https://rvm.io/rubies/installing)
- [Testing your site locally](https://kbroman.org/simple_site/pages/local_test.html)
- [Testing your GitHub Pages site locally with Jekyll](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)