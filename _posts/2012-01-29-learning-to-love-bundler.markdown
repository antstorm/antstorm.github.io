---
layout: post
title:  'Learning to love Bundler'
date:   2012-01-29 12:00:00
categories: ruby bundler
---

I have recently written few ruby applications, that didn't require any web server. In fact these were daemons based on EventMachine, processing stuff on the backend. One of the first questions I've faced was — "How to manage gem dependencies for these apps?"

### Bundler to the rescue

The very first thing that came to my mind was Bundler. We all remember how this nifty little gem made our lives much easier, when it was introduced as a strong dependency with Rails 3. But the most attractive thing about the Bundler is that it can be used in pretty much all of your ruby applications.

What I really like about using the Bundler is that the boilerplate code is very minimal. It's basically few lines of code and your application has gem dependencies handled.

### Getting down to business

So let's say, you're hacking on something new and you've already created your `awesome_app.rb` file inside the `awesome_app` directory. At some point you have realized that you need to use an external library which usually comes in a form of a gem. Here's what you need to do:

#### 1. Installing Bundler

Have a bundler installed in your system. You can do that from the command line by simply executing:

    gem install bundler

I'm using [RVM](http://beginrescueend.com/) to manage my rubies and gemsets and always have the Bundler installed in my global gemset for each ruby. That way when I start coding something new I just create a new gemset and Bundler is already there, ready to power up my application.

#### 2. Creating a Gemfile

Next you need to create a file named `Gemfile` inside your app's directory. Let's say we want the latest version of the EventMachine gem for this:

    source 'http://rubygems.org'

    gem 'eventmachine'

The `source` defines the repository to grab gems from.

#### 3. Engaging the Bundler

Now we need to initiate the Bundler inside your application. To do that you need to require the `rubygems` inside your application and then initialize Bundler:

    require 'rubygems'
    require 'bundler/setup'

    Bundler.require :default

The last command here tells the Bundler to get your `Gemfile` and check whether all the dependencies you've specified are there in your gemset. In case you're missing a dependency I will get an error saying that the gem is missing when executing your application.

#### 4. Installing the dependencies

At this point all you need to do is to run:

    bundle install

or simply

    bundle

from your command line and the Bundler will take care of the rest. It will figure out what gems and what dependencies you need and will have all of those installed.


### Go and bundle all your non-rails applications

So you see how easily with just few lines of code all of your application's dependencies can be satisfied. All thanks to the amazing [Bundler](http://gembundler.com/) gem.

The real advantage of using Bundler in your apps comes when you need to deal with different gem versions(especially ones hosted on github and not packaged into gems) or you are moving your app to another computer(or even another ruby version).
