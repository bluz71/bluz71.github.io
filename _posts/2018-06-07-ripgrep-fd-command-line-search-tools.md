---
title: ripgrep & fd - Command-line Search Tools
layout: default
comments: true
published: true
---

ripgrep & fd - Command-line Search Tools
========================================

The [grep](https://en.wikipedia.org/wiki/Grep) and
[find](https://en.wikipedia.org/wiki/Find_(Unix)) utilities are ubiquitous
search work-horses for Unix-like systems. The `grep` utility, as many reading
this post will be aware, is used to search for patterns *within* files whilst
`find` is primarily used to match file names based on supplied expressions.

Both of these legacy tools are still valid today many decades after their
initial release. However, a new wave of tools have recently appeared that make
command-line searching easier, faster and more user-friendly.

This post highlights a couple such actively-developed tools, namely
[ripgrep](https://github.com/BurntSushi/ripgrep) and
[fd](https://github.com/sharkdp/fd).

**ripgrep** (pattern search)
============================

The [ripgrep](https://github.com/BurntSushi/ripgrep) utility is a line-oriented
search tool that recursively searches within the current directory tree for a
supplied pattern.

Written in [Rust](https://www.rust-lang.org), ripgrep has [excellent
performance](https://blog.burntsushi.net/ripgrep) that is as fast, and more
often faster, than other similar pattern search tools.

Programmer-friendliness is provided by the following niceties:

- ripgrep is automatically recursive, unlike `grep`, thus only a search pattern
    need be supplied

- ripgrep will automatically ignore all directories and files specified in
    `.gitignore` files

- ripgrep search hits and file matches are clearly highlighted in distinctive
    color

- ripgrep is invoked by the short two-letter command `rg`, two fewer letters
    than `grep` :tada:

Installation
------------

ripgrep is easily installed using [Homebrew](https://brew.sh) on macOS and
 [Linuxbrew](http://linuxbrew.sh) on Linux.

```sh
brew install ripgrep
```

For other platforms please consult [these installation
details](https://github.com/BurntSushi/ripgrep#installation).

Usage
-----

```sh
rg Foo       # Case sensitive search
rg -i foo    # Case insensitive search
rg -v foo    # Invert search, show all lines that don't match pattern
rg -l foo    # List only the files that match, not content
rg -t md foo # Match by `md` file extension
```

Screenshot
----------

![ripgrep example](http://burntsushi.net/stuff/ripgrep1.png "ripgrep example")

Ignores
-------

As noted already, ripgrep will automatically ignore content specified in
`.gitignore` files.

However, when not in a Git tree, or when wanting to override `.gitignore`,
ripgrep also respects, with higher precedence, ignores specified in `.rgignore`
files.

For instance, the next snippet will create a global ripgrep rule to ignore
content in `tmp` directories.

```sh
echo tmp >> ~/.rgignore
```

Vim integration
---------------

If you are a [Vim](https://www.vim.org) user then I recommend reading
[these vim-grepper](https://bluz71.github.io/2017/05/21/vim-plugins-i-like.html#vim-grepper)
details. This will nicely integrate ripgrep into Vim.

**fd** (file find)
==================

The [fd](https://github.com/sharkdp/fd) utility is a file finding tool. It is a
modern-day simplified alternative to the Unix `find` tool.

Like ripgrep, fd is also written in [Rust](https://www.rust-lang.org) and
likewise has [excellent performance](https://github.com/sharkdp/fd#benchmark).
Note, I am lead to believe that ripgrep and fd do share some code.

Programmer-friendliness is provided by the following niceties:

- fd is automatically recursive, thus only a search pattern need be supplied

- fd will automatically ignore all directories and files specified in
    `.gitignore` files

- fd matches are colorized

- fd is invoked by the short two-letter command `fd`, two fewer letters
    than `find` :tada:

Lastly, it should be noted that fd does **not** provide all the functionality
of `find`, however it sensibly provides functionality for the most common
use-cases.

Installation
------------

fd is easily installed using [Homebrew](https://brew.sh) on macOS and
 [Linuxbrew](http://linuxbrew.sh) on Linux.

```sh
brew install fd
```

For other platforms please consult [these installation
details](https://github.com/sharkdp/fd#installation).

Usage
-----

```sh
fd              # List all non-ignored files
fd foo          # Case insensitive search
fd Foo          # Case sensitive search
fd --type f foo # Match only files
fd --type d foo # Match only directories
fd -e md foo    # Match by `md` file extension
fd '^[0-9]'     # List files starting with a digit
```

Demonstration
-------------

![fd example](https://github.com/sharkdp/fd/raw/master/doc/screencast.svg?sanitize=true "fd example")

Ignores
-------

As noted already, fd will automatically ignore content specified in
`.gitignore` files.

However, when not in a Git tree, or when wanting to override `.gitignore`,
fd also respects, with higher precedence, ignores specified in `.fdignore`
files.

For instance, the next snippet will create a global fd rule to ignore
content in `tmp` directories.

```sh
echo tmp >> ~/.fdignore
```

Vim CtrlP plugin
----------------

If you are a [Vim](https://www.vim.org) user who uses the
[CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin then I recommend [these
CtrlP with
fd](https://bluz71.github.io/2017/05/21/vim-plugins-i-like.html#ctrlp)
settings.

fzf
---

The [fzf](https://github.com/junegunn/fzf) utility is a generic command-line
fuzzy finder. If you are already using fzf and are not using fd as the file
finder than I recommended adding the following settings to your shell's
configuration file (e.g `~/.bashrc`).

```sh
export FZF_DEFAULT_COMMAND='fd --type f --color=never'
export FZF_ALT_C_COMMAND='fd --type d . --color=never'
```

If you are unfamiliar with fzf, stay tuned for an upcoming post in this
channel. :eyes:

Conclusion
==========

As an old-school command-line user I find both ripgrep and fd to now be
indispensable tools. Give them a try yourself, maybe you will like them too.
