---
layout: post
title:  "Setup Github Page"
date:   2022-10-16 02:25:00 +0800
categories: github
---

## Setup Github Page

## [Create site with jekyllrb][Create-site-doc]

**General Steps**
1. Create a repository
2. Create site with jekyll
```bash
gem install jekyll

gem install bundler

# if needed
bundle update --bundler
```

## [Testing site locally](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
```bash
bundle install

bundle exec jekyll serve # this command will be loading changes automatically

bundle exec jekyll serve --drafts
```


## How to add page and posts
* https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-content-to-your-github-pages-site-using-jekyll

#### Pages
* https://jekyllrb.com/docs/pages/

#### Posts
* https://jekyllrb.com/docs/posts/

#### Work with drafts
* How to add drafts https://jekyllrb.com/docs/posts/#drafts
* Local test with drafts
    ```sh
    bundle exec jekyll serve --drafts
    ```

## How to start with Liquid template
https://jekyllrb.com/docs/step-by-step/02-liquid/
Don't forget to add front matter to the top of the page:
```
---
# front matter tells Jekyll to process Liquid
---
```

## [Adding a theme](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-a-theme-to-your-github-pages-site-using-jekyll)



[Create-site-doc]: https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll



## Some trouble shooting
#### Liquid Exception: undefined method 'untaint' 
error:
```
Liquid Exception: undefined method 'untaint' for "Welcome to Jekyll!":String in /_layouts/post.html`
```
solution:
https://github.com/github/pages-gem/issues/870
https://github.com/github/pages-gem/pull/867
in Gemfile, upgrade:
```
# from
-gem "github-pages", "~> 227", group: :jekyll_plugins
# to
+gem "github-pages", "~> 228", group: :jekyll_plugins
```

#### bundler: failed to load command: jekyll 
error:
```
bundler: failed to load command: jekyll (/opt/homebrew/lib/ruby/gems/3.2.0/bin/jekyll)
<internal:/opt/homebrew/Cellar/ruby/3.2.2_1/lib/ruby/3.2.0/rubygems/core_ext/kernel_require.rb>:37:in `require': cannot load such file -- webrick (LoadError)
```
solution:
https://stackoverflow.com/questions/69890412/bundler-failed-to-load-command-jekyll
```
bundle add webrick
bundle exec jekyll serve
```