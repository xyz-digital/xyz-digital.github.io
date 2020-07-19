---
layout: post
title:  "Jekyll on Windows"
author: "Adam Berg"
date:   2020-06-06 10:00:00 -0700
categories: software
---

![Dr. Jekyll and Mr. Hyde]({{"/assets/images/jekyll-hyde.jpg" | relative_url}})

Or should I say Jek "Hell" on Windows?

<!--more-->
    
    gem install bundler jekyll
    jekyll new my-awesome-site
    cd my-awesome-site
    bundle exec jekyll serve

"Quick-start Instructions" from https://jekyllrb.com/

I had already gone through this process on MacOS and Ubuntu, so I figured can't be _that_ bad.  Boy was I wrong.

# Problem 1: Installing Ruby

> gem : The term 'gem' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.

## Ok Google, How to Make Gems on Windows

The above is what you get when trying to run the first installation step. `gem` is a program that is bundled with a Ruby installation.  If you didn't know this, you're immediately lost. Maybe I'm crazy, but you shouldn't need to have any background knowledge to install something like Jekyll.

You decide to [google](https://www.google.com/search?q=gem+windows) it.  Helpful information nowhere to be found, but I did manage to order some Armor windows at https://gemwindows.com/.

That makes sense though, those are some generic words - I bet if I past the error message I'll get something more helpful.  I proceed to Google "gem : The term 'gem' is not recognized as the name of a cmdlet".  The first results I get are:

1. [Code Academy Forum](https://discuss.codecademy.com/t/command-line-says-gem-is-not-recognized-as-an-internal-or-external-command/41618)
    - Not Helpful
2. [Stack Overflow](https://stackoverflow.com/questions/27239491/cannot-install-gem-make-is-not-recognized-as-an-internal-or-external-command-o)
    - Mostly not helpful, though a response with 0 votes at the bottom of the page points to https://rubyinstaller.org/downloads
3. [Medium](https://medium.com/@andreahanna/i-got-a-gem-is-not-recognized-error-when-i-ran-the-command-in-cmd-a4d9d1bcf0c2)
    - I had high hopes for this one, but this is the wisdom provided "When you install Ruby using the Windows installer you need to check the 'Add Ruby executables to your PATH' box"
4. [Ruby Forum](https://www.ruby-forum.com/t/gem-is-not-recongnized/111891)
    - Useless
5. [Github rubygems Issue #1525](https://github.com/rubygems/rubygems/issues/1525)
    - Completely unrelated question, but some reference to "I'm assuming that you are are installing via http://rubyinstaller.org/"

The above is not my actual thought process, but I believe would be a very reasonable one for a beginner. I had a mentor once shoot me a link to [let me google that for you](https://lmgtfy.com/?q=how+to+use+google).  It was meant in a light-hearted way and was an effective way of saying that I would be better off finding my answer on Google because that's all he would do if he tried to help.  

The problem is that I think developers make this harder than it needs to be.  Why doesn't Ruby have an FAQ page with that **exact** error message above?  This is why user testing is important.  Watching someone use a piece of software for the first time is incredibly informative.  I can't count the number of times I've tried a software, bumped in to something that I was stuck on and then quit - never to use that software again.

## Choclatey

I ended up just installing via [Chocolatey](https://chocolatey.org/).  I'm hardly a Windows person, but Package Management is great and this seems to be _the_ option on Windows.  I thought this was going to be easy and then I saw their installation page:

![Choco Installation]({{"/assets/images/choco-installation.png" | relative_url}})

I almost rebooted right there to switch over to Ubuntu, but instead decided this would be a decent ~~rant~~ post.

I follow the above, and I think it worked.  Not sure what Set-ExecutionPolicy has done, but the software is named after chocolate so they're probably good guys, right?

Almost there, I think to myself.

> choco install ruby

After that does it's magic I run `gem` and the output confirms it is installed. I check back with the Jekyll instructions:

> gem install bundler jekyll

    Successfully installed bundler-2.1.4
    Parsing documentation for bundler-2.1.4
    Done installing documentation for bundler after 1 seconds
    Temporarily enhancing PATH for MSYS/MINGW...
    Building native extensions. This could take a while...
    ERROR:  Error installing jekyll:
            ERROR: Failed to build gem native extension.

        current directory: C:/tools/ruby27/lib/ruby/gems/2.7.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
    C:/tools/ruby27/bin/ruby.exe -I C:/tools/ruby27/lib/ruby/2.7.0 -r ./siteconf20200606-7024-1b8pmlc.rb extconf.rb
    creating Makefile

    current directory: C:/tools/ruby27/lib/ruby/gems/2.7.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
    make "DESTDIR=" clean
    current directory: C:/tools/ruby27/lib/ruby/gems/2.7.0/gems/http_parser.rb-0.6.0/ext/ruby_http_parser
    make "DESTDIR="
    make failedNo such file or directory - make "DESTDIR="

    Gem files will remain installed in C:/tools/ruby27/lib/ruby/gems/2.7.0/gems/http_parser.rb-0.6.0 for inspection.
    Results logged to C:/tools/ruby27/lib/ruby/gems/2.7.0/extensions/x64-mingw32/2.7.0/http_parser.rb-0.6.0/gem_make.out
    1 gem installed

# Problem 2: Building Gem Native Extensions

Turns out the choco installation doesn't include the dev kit, and the [ruby2.devkit](https://chocolatey.org/packages/ruby2.devkit) package doesn't look all that maintained.

`choco uninstall ruby`.  I accept defeat, and switch to the [installer](https://rubyinstaller.org/downloads/) on their website.

As I go through the installer I note some important default options checked.  "Adding to system PATH" and "Run 'ridk install' to setup MSYS2 and development toolchain. MSYS2 is required to install gems with C extensions."  

![Ruby Installer]({{"/assets/images/ruby-installer.png" | relative_url}})

Cool installer, bro. I guess option 1 sounds good?

    gem install bundler jekyll

Takes a minute or two before this successfully completes.  I look over at my other monitor to see this:

![Jekyll Homepage]({{"/assets/images/jekyll-home.png" | relative_url}})

"Simple", "Get up and running in _seconds_", that cute little octopus mocking me as I am rounding the corner on **45 minutes**!

I smile, I'm almost there.  I even get to skip two steps because I already ran `jekyll new`.  I run `bundle exec jekyll serve`...

# Problem 3: Run `bundle install` to install missing gems

Thankfully this time I get a pleasant notice in the terminal letting me know I need to run one more command.  I do so, and finally I'm greeted with my blog on `http://127.0.0.1:4000/`.

# Closing Notes

I came here to write an entirely different post, but I feel like this whole process has been a great example of what a day in the life can be like.  If you ever wonder "why does it take so long to just make a small change on this website", this is just the tip of that iceberg.