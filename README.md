---
permalink: /about/
title:  "About"
author: "Adam Berg"
---

## Misson Statement

This site aims to create a space for **people of all backgrounds** to **learn more about our digital world**.

## Comments, Feedback, or Corrections

Comments on the site itself are currently disabled.  Any comments can be added as an issue on the [Github repository](https://github.com/adamjberg/adamjberg.github.io/issues).

## Contributing Posts

While contributions are currently limited to a handful of people, this site welcomes contributions from all. The site is hosted as a [Github Page](https://pages.github.com/) public repository [here](https://github.com/adamjberg/adamjberg.github.io).

### Setup

The instructions below should be enough to get going, but this [page](https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll) may cover some other things you need.

1. Create a [Github](https://github.com/join) Account

2. Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

3. [Install Ruby](https://www.ruby-lang.org/en/documentation/installation/)

4. Open terminal

5. Run `gem install bundler`

6. Run `git clone https://github.com/adamjberg/adamjberg.github.io.git xyzdigital.com` (This will create a folder in the directory you are currently in.  If you are new to the command line [this post](https://towardsdatascience.com/a-quick-guide-to-using-command-line-terminal-96815b97b955) gives a brief introduction.)

7. Run `cd xyzdigital.com`

8. Run `bundle install`

9. Run `bundle exec jekyll serve`

10. You should now be able to open the site in your browser by accessing `http://127.0.0.1:4000/`.

### New Post

The site's pages are built using [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).  To create a new post, add a new file in the `_posts` folder with format `YYYY-MM-DD-article-title-with-hyphens.md`.  See existing posts for roughly what format to follow and [Jekyll's step by step tutorial](https://jekyllrb.com/docs/step-by-step/08-blogging/) if you want to know more.