## Misson Statement

xyz digital is a family owned creative engineering company dedicated to changing the world bits at a time.

## Comments, Feedback, or Corrections

Comments on the site itself are currently disabled.  Any comments can be added as an issue on the [Github repository](https://github.com/adamjberg/adamjberg.github.io/issues).

## Contributing Posts

While contributions are currently limited to a handful of people, this site welcomes contributions from all. The site is hosted as a [Github Page](https://pages.github.com/) public repository [here](https://github.com/adamjberg/adamjberg.github.io).  If you are interested please reach out to adam@xyzdigital.com.

### Initial Setup

The instructions below should be enough to get going, but this [page](https://help.github.com/en/github/working-with-github-pages/testing-your-github-pages-site-locally-with-jekyll) may cover some other things you need.

1. Create a [Github](https://github.com/join) Account

2. Install [Git](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git)

3. [Install Ruby](https://www.ruby-lang.org/en/documentation/installation/)

4. Open terminal

5. Run `gem install bundler`

6. Run `git clone https://github.com/adamjberg/adamjberg.github.io.git xyzdigital.com` (This will create a folder in the directory you are currently in.  If you are new to the command line [this post](https://towardsdatascience.com/a-quick-guide-to-using-command-line-terminal-96815b97b955) gives a brief introduction.)

7. Run `cd xyzdigital.com`

8. Run `bundle install`

### Running the Site Locally

1. Run `bundle exec jekyll serve`

2. You should now be able to open the site in your browser by accessing `http://127.0.0.1:4000/`.

### New Post

The site's pages are built using [Markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).  To create a new post, add a new file in the `_posts` folder with format `YYYY-MM-DD-article-title-with-hyphens.md`.  See existing posts for roughly what format to follow and [Jekyll's step by step tutorial](https://jekyllrb.com/docs/step-by-step/08-blogging/) if you want to know more.

### Submitting for Review

Submissions will be managed as Pull Requests on Github.  Below will be simple instructions to cover the most basic case, but the first 3 chapters of the [Git Book](https://git-scm.com/book/en/v2) should help you understand a bit better if you don't know Git.

1. Create a new [branch](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell) by running: `git checkout -b post/new/article-title-with-hyphens`

2. Commit the changes you have made to your post by running: `git commit -am "Add Article Title With Hyphens Post"` (replacing with the actual title of the article).

3. Push the branch to Github: `git push origin post/new/article-title-with-hyphens`

4. Navigate to link shown in terminal: `https://github.com/adamjberg/adamjberg.github.io/pull/new/post/new/article-title-with-hyphens`

5. Click "Create pull request"

6. The pull request will be considered and any feedback will be provided there

### Cleaning up for Next Time

Now that the Pull Request is open, you'll want to get back to the master branch for the next time you would like to submit something.

1. Return to the master branch if you are not already there: `git checkout master`

2. Grab the latest changes: `git pull`
