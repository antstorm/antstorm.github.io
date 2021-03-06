---
layout: post
title:  'Y U NO USE screen'
date:   2013-04-16 12:00:00
categories: unix shell screen
---

Imagine a fairly common situation — you are `ssh`'ed to a remote server, doing stuff and then all of a sudden your internet connection drops and the console has frozen. And even after the connection is back, console won't respond, because the pipe between your computer and remote one has broken.

We've all been there, it sucks. And it sucks even more if there was a long running process executed from that console. At this point you would probably open another connection, find that process and kill it. Or maybe hope for it to finish, because it was doing something important, like running `rsync` or dumping your database. I've seen this quite a lot and surprisingly, people keep on repeating the same mistake over and over again.

There is a very old and simple tool, that can help you to prevent this from happening. It is called — *GNU Screen*. It allows you to create a virtual session inside your `ssh` session(or any console session), that isn't bound to your connection. Meaning that if your connection drops, you will be able to connect to this virtual session afterwards. Hopefully, you already see the benefits you can get from using it, so let's get to how you actually use it.


#### 1. Installing `screen`

Usually `screen` won't be installed your system, so you would:

    apt-get install screen

or

    yum install screen

or

    brew install screen

or do whatever you do to install stuff on your machine.


#### 2. Starting a new `screen` session

To start and new virtual session simply type `screen` into your console. Normally it would greet you with a copyright notice. Press `return` to skip it and you're set — now you are inside the virtual console. Feels pretty much the same, right? Except now you can go ahead and disconnect from the internet and you will be able to connect to the session once you are back online.


#### 3. Communicating with `screen`

Inside a virtual session you can initiate communication with the `screen` tool by pressing `Ctrl + A`, followed by a single letter command:

- `D` — 'detach'. It detaches you from current session, leaving it running in background.
- `K` — 'kill'. This would terminate the `screen` session. It would probably ask you to confirm this, so press `y` if you are sure about it. Logging out from a virtual console also terminates it.

These are two commands that you would need for basic understanding of `screen`, but keep in mind, that `screen`, as most of the UNIX tools, has much more power underneath the bonet.


#### 4. Connecting to an existings virtual session

In case of a lost connection or to resume a previously started session, you need to type

    screen -r

This command will attach you to a `screen` session. In case there are more then one active session, you'll see a list of them. To connect to a particular screen, simply add it's identifier as an argument:

    screen -r 16873.pts-0.yourserver

That way you can have multiple sessions running, allowing you to switch between them and not having to keep multiple `ssh` connections alive. To see a list of active screens, just type in:

    screen -ls

#### 5. Naming your screens

You can also give your screen a name, that you can use later on as a connection identifier:

    screen -S something

and to restore that session:

    screen -r something

------------------

And that's basically it. With these few simple commands in mind you won't be afraid of a frozen consoles and "Broken pipe" error messages, because your
session is safe and sound with `screen`. So go ahead try it and get used to it, because it will help you a lot.


#### Other alternatives

Of course there are other tools allowing you to gain the same benefits. One of the most popular is `tmux`, standing for Terminal Multiplexer. It is very powerfull and allows you to split the current session in virtual "windows", all within a single session. Lots of people use it locally, which allows them to store sessions in memory and restore them later.

But for a simple task of preventing yourself from loosing an `ssh` connection, `screen` is perfect solution. It is super simple and does it's job very well.


#### Next steps

In the next blog post I will show you more of `screen`'s features and how you can use it to share a session between two people, working inside of it simultaneously.
