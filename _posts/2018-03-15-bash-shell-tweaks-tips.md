---
title: Bash Shell Tweaks & Tips
layout: default
comments: true
published: false
---

Bash Shell Tweaks & Tips
========================

The [Bash Shell](https://www.gnu.org/software/bash) is the most commonly used
Unix shell in existence. It is highly ubiquitous due to it being the default
user shell for the various flavours of Unix, including Linux, macOS and the
Windows Subsystem for Linux.

Due to this ubiquity, and need to maintain strict backward compatibility, it is
rare that newer useful features of the Bash shell are enabled by default. Some
mistake this *lack of default change* for stagnation and simply move across to
a different shell such as [Zsh](https://www.zsh.org) of
[Fish](https://fishshell.com).

This post will document a number of simple tweaks and tips that will greatly
improve user productivity when using the Bash shell.

All the *tweaks n' tips* noted in this post are baked into my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files.

macOS, upgrade to Bash 4.x
--------------------------

macOS by default ships with a very old version of Bash, version 3.2. That
version is over a decade old with many missing features.

Please upgrade to version 4.x of Bash. This is most easily accomplished by
using the [Homebrew](https://brew.sh) package manager.

Install Homebrew followed by:

```sh
brew install bash

sudo su -
echo /usr/local/bin/bash >> /etc/shells
exit

chsh -s /usr/local/bin/bash
```

Launch a new shell and enter `echo $SHELL`, it should list
`/usr/local/bin/bash`.

Install bash-completion
-----------------------

By default Bash has limited completion capabilities, mainly geared around
filesystem expansion, especially when compared with Zsh and Fish which offer
excellent tab completion for command line utilities. However, the
[bash-completion](https://github.com/scop/bash-completion) provides command
line
