---
title: ".deb file관련  dpkg 사용 방법.."
excerpt_separator: "<!--more-->"
date: 2012-02-01 15:11:17 +0900
categories:
  - etc
tags:
  - develop_env
  - 개발환경

toc : true
toc_sticky : true
---

# 간단하게는 dpkg -i xxx.deb 인데... 아래 설명이 있다..  공부할게 많다는.

# 

# HOWTO: Use dpkg to Install .deb Files.

6 years ago by [Jon](http://everyjoe.com/author/jon/ "Posts by Jon") | [57 Comments](http://everyjoe.com/technology/howto-use-dpkg-to-install-deb-files/#comments) | [Share a Tip](http://everyjoe.com/technology/howto-use-dpkg-to-install-deb-files/#top)

I’ve written about using [*apt-get* to get and install debian packages](http://everyjoe.com/howto-install-deb-rpm-and-source-code-files/). However, a recent comment by a reader brought the fact that I hadn’t written anything on what to do with a .deb file that exists on your [system](http://everyjoe.com/technology/howto-use-dpkg-to-install-deb-files/#) already either by download or other media.

Using the *apt-get* application is the quickest way to find and install debian packages. The installation part is done by an application named *dpkg.* Dpkg doesn’t have to be used by apt-get, you can use it manually as well.

From the *man dpkg* command:

> dpkg – a medium-level package manager for Debian

Whatever that means.

In keeping with GNU/Linux system security, only the superuser can use the dpkg application. Dpkg is a typical GNU/Linux application that is controlled by command-line switches. Possibly the most common use of dpkg is to install a local .deb file.

To install a .deb file, become root and use the command:

> dpkg -i *filename.deb*

Dpkg can also be used to:

- *dpkg –unpack*: unpacks the file but does not install it
- *dpkg –configure*: presents whatever configuration options are available for the package
- *dpkg –remove*: removes a package

Some of the package manipulation commands are actually carried out by an application called *dpkg-deb*. In those cases, *dpkg* just acts as a front end to dpkg-deb and passes the commands to it.

Dpkg-deb can also be used to manipulate .deb files. Some of the more useful commands of dpkg-deb are:

> dpkg-deb –show *filename.deb*

This will display the information for *filename.deb*. Normally, this is boring information like the application version (which is normally evident from the filename), but in some cases more interesting and useful information is displayed.

Consult the dpkg man page for information on the more arcane uses for dpkg.