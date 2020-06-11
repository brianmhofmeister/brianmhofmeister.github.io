---
layout: post
title: Migrating to WSL 2
date: 2020-06-11 09:45 -0500
---

Development tools and environments keep evolving.
As a curious engineer I often find myself tinkering with different environments, different tools, and different ways of tweaking my workflow.
I am sure a lot of other software developers do the same thing.
We developers are always looking for shortcuts.
Some developers claim to be "lazy" but I think it's really that we want to be efficient in our work.
Some environments make being effective pretty easy, and some that I've worked with make it very difficult.

My personal laptop is a Windows 10 machine.
I would prefer to run Linux full time, but there are some specific reasons that I do not, and those reasons unfortunately will not go away any time soon.
For a while I've just been running a virtual machine with VirtualBox and it works well enough.
My biggest issue with the VirtualBox approach is that it adds a layer that I need to manage, which means spending addtional time just tinkering to make sure not only the linux guest is up to date, but also that I'm up to date on patches for VirtualBox, etc.
It's not a major amount of extra work, but it is extra work.
It reduces my efficiency.

Now that Windows 10 2004 is generally available, I've decided to try out the new Windows Subsystem for Linux (WSL) 2.
I have played with WSL before, but some parts of it just felt clumsy.
I'm happy the new version has a full Linux kernel.
I've just started using the Remote extension for VS Code and it is pretty slick.
The integration feels seamless, and for the majority of my personal use cases this seems like it will be a substantial benefit.
I'm presently running Ubuntu 20.04 as my distribution, and it seems fine for now. I'm used to Ubuntu based distros, but the size is pretty large and I do not have a gigantic SSD on my machine.
I'll have to see whether disk storage becomes an issue or not.

I am excited to try out the new Docker Desktop support for WSL 2.
I'm hopeful that will make some of my learning ventures much more enjoyable by simplifying the dev/test cycle.

I'll probably post more about this in a few weeks.
For now I'm happy with the switch, it is still a compromise to work with two operating systems on a single host, some day I'd like to get a second machine, one generously loaded with storage and memory and a good number of cores. In the meantime, I appreciate how much investment Microsoft is making in opening up their software ecosystem and providing new tools for developers that helps make our lives a little easier.
