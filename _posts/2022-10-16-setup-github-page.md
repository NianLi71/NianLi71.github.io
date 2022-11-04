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


## [Testing site locally](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll)
```bash
bundle install

bundle exec jekyll serve # this command will be loading changes automatically
```


## [How to add page and posts](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/adding-content-to-your-github-pages-site-using-jekyll)
* https://jekyllrb.com/docs/posts/

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