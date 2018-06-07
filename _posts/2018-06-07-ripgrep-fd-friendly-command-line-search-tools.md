---
title: ripgrep & fd, Friendly Command-line Search Tools
layout: default
comments: true
published: false
---

ripgrep & fd, Friendly Command-line Search Tools
================================================

The [grep](https://en.wikipedia.org/wiki/Grep) and
[find](https://en.wikipedia.org/wiki/Find_(Unix)) utilities are ubiquitous
work-horses for Unix-like systems. The `grep` utility, as most reading this
will be aware, is use to search for plain-text data within files whilst `find`
is primarily used to match files themselves based on supplied criteria.

Both of these legacy tools are still valid today many decades after their
initial release. However, a new wave of tools have recently appeared that make
command-line search easier, faster and more user-friendly.

This post highlights a couple such tools that I now use every day.

ripgrep
=======

The [ripgrep](https://github.com/BurntSushi/ripgrep) utility is line-oriented
search tool that recursively searches the current directory for a supplied
pattern. Written in [Rust](https://www.rust-lang.org/), ripgrep has [excellent
performance](https://blog.burntsushi.net/ripgrep) that is as fast, and more
often, faster than other search tools.

Programmer-friendliness is provided by the following benefits:

- ripgrep will ignore all directories and files specified in `.gitignore` files

- ripgrep search hits are clearly highlighted in color

- ripgrep is automatically recursive, unlike `grep`, hence it is very easy to
    search the current working tree

- ripgrep is easily invoked by the very short two-letter command `rg`

Installation
------------

Usage
-----
```sh
rg World    # Case-sensitive search
rg -i world # Case insensitive
rg -v world # Invert search, show all lines that don't match pattern
```

Screenshot
----------

![ripgrep example](http://burntsushi.net/stuff/ripgrep1.png "ripgrep example")


fd
==

Demonstration
-------------

![fd example](https://github.com/sharkdp/fd/raw/master/doc/screencast.svg?sanitize=true "fd example")


ripgrep in Vim
--------------

fd in fzf
---------
