---
title: Fuzzy Finding in Bash with fzf
layout: default
comments: true
published: false
---

Fuzzy Finding in Bash with fzf
==============================

Fuzzy finding is a search technique that uses approximate pattern matching
rather than exact matching. For some search tasks, fuzzy finding will be highly
effective at generating relevant results for a minimal amount of search effort.

The [fzf](https://github.com/junegunn/fzf) utility is a line-oriented fuzzy
finding tool for the Unix command-line. fzf filters input, in real-time, by way
of a user supplied query text.

Adhering to the *Unix philosophy*, fzf performs one primary task, fuzzy
finding, which takes input from **STDIN** and emits results to **STDOUT**.
Hence, it is easy to build useful [fzf-based search
helpers](https://bluz71.github.io/2018/11/21/fuzzy-finding-in-bash-with-fzf#search-helpers).

Some notable characteristics of fzf:

- high-performance core [written in Go](https://junegunn.kr/2015/02/fzf-in-go)

- simple and pleasing command line interface

- support for ANSI color codes

- optionally preview search results in a split window

Installation
------------

fzf is easily installed using [Homebrew](https://brew.sh) on macOS and
 [Linuxbrew](http://linuxbrew.sh) on Linux.

```sh
brew install fzf
```

For other platforms please consult [these installation
details](https://github.com/junegunn/fzf#installation).

When **STDIN** is not supplied fzf will use the [find
command](https://en.wikipedia.org/wiki/Find_(Unix)) to fetch a list of files
to filter through. I recommend installing and using the
[fd](https://github.com/sharkdp/fd) utility instead.

```sh
brew install fd
```

One of the search helper examples, listed below, uses the
[ripgrep](https://github.com/BurntSushi/ripgrep) utility. If you plan to use
that helper then please also install the ripgrep utility.

```sh
brew install ripgrep
```

Details about the ripgrep and fd search tools are noted in [this
post](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html).

Configuration
-------------

Please add the following configuration settings to your `~/.bashrc` file.

Enable fzf key bindings in the Bash shell.

```sh
. $(brew --prefix)/opt/fzf/shell/key-bindings.bash
```

Use the `fd` command instead of the `find` command in fzf Bash key bindings.

```sh
export FZF_DEFAULT_COMMAND='fd --type f --color=never'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_ALT_C_COMMAND='fd --type d . --color=never'
```

fzf look and behaviour are overridden in the **FZF_DEFAULT_COMMAND**
environment variable. I like a top-down, 40% fzf window that also responds to
page-up/page-up keys.

```sh
export FZF_DEFAULT_OPTS='
  --height 40% --multi --reverse
  --bind ctrl-f:page-down,ctrl-b:page-up
'
```

I recommend you experiment and choose your own **FZF_DEFAULT_OPTS**.

Note, if necessary please refer to my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) file.

Usage
-----

Screenshot
----------

Bash Key Bindings
-----------------

- Alt-c
- Ctrl-r
- Ctrl-t

<a id="search-helpers"></a>Search Helpers
-----------------------------------------

- fzf find-file-and-edit
- fzf find-file-with-text-and-edit
- fzf git add
- fzf git unadd
- fzf git browser
- fzf process kill
