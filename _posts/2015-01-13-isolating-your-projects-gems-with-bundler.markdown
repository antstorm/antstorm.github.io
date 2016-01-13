---
layout: post
title:  "Isolating your project's gems with Bundler"
date:   2015-01-13 18:00:00
categories: ruby bundler gems tips
---

Here's a simple trick that I've been using for quite some time now. The intention here is to isolate gems for every project and make it in a seamless way possible.

I know that some of you might say that Bundler is already quite good at picking the right gems from your global set and requiring them as needed. While this is absolutely true and Bundler does a fantastic job with it, there are some benefits to my approach that you don't get with global gems.

Basically it all boils down to a very simple shell function which I use instead of `bundle install`. Here it is:

    function bundle!() {
      bundle $* --path ./.gems
    }

This has been a part of my dotfiles for over a year now. What this does is it tells bundler to put all the gems in the directory called `.gems` in the current path. The immediate benefit is of course
a clean `gem list`, which now only consists of default gems and gems I've chosen to be globally accessable (like [awesome_print](https://github.com/michaeldv/awesome_print) or [pry](http://pryrepl.org/)). I can also now safely run `gem cleanup` without loosing any older dependencies.

After updating some of the gems in the project I can safely run `bundle clean`, which will take care of the unused versions, based on my `Gemfile.lock`, leaving only important stuff. And after I'm done with the project I can nuke the whole directory and it will take all the dependent gems away, leaving zero trace in my system.

But the best things about this approach is that I can run `bundle open <GEM>` in order to get access to the source and most importantly — make modifications without affecting any other projects. This helps a lot (and I mean a LOT) when debugging gems used in your projects.

So in case you're interested — give this approach a go and [tell me](https://twitter.com/antstorm) what you think. The only other thing you need to do to make this work is to add this line in you global `.gitignore`:

    ...
    .gems
    ...

That's it!
