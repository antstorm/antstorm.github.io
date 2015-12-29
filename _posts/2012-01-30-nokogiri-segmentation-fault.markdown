---
layout: post
title:  'Nokogiri Segmentation Fault'
date:   2012-01-30 12:00:00
categories: ruby nokogiri
---

Today I ran into a nasty bug with [nokogiri](http://nokogiri.org/). I was using nokogiri 1.5.0 and libxml 2.7.3. The error message basically stated:

    [BUG] Segmentation fault

The there was a path to a file inside *nokogiri* in the beginning of error message, but the strange thing is that every time I launched my application, the path included a different file.

Anyways what I did was I install the latest version of *libxml*, which currently is 2.7.8. I'm on MacOS X and thanks to [homebrew](http://mxcl.github.com/homebrew/) this is as simple as:

    brew update
    brew install libxml2

After that don't forget to link everything to a new version by executing:

    brew link libxml2

Next we need to reinstall the nokogiri gem to rebuild it's native extensions:

    gem uninstall nokogiri
    gem install nokogiri

The uninstall command will ask you to remove nokogiri binaries and you should agree to that.
To ensure that nokogiri is using the right version of libxml you can execute:

    nokogiri -v

You should see something like this:

    # Nokogiri (1.5.0)
        ---
        warnings: []
        nokogiri: 1.5.0
        ruby:
          version: 1.9.3
          platform: x86_64-darwin10.8.0
          description: ruby 1.9.3p0 (2011-10-30 revision 33570) [x86_64-darwin10.8.0]
          engine: ruby
        libxml:
          binding: extension
          compiled: 2.7.8
          loaded: 2.7.8

The version of libxml at the end should be the one that you have installed with homebrew.

So that's basically it. After that there were no signs of the segmentation fault.
