---
title: Fuzzy Finding in Bash with fzf
layout: default
comments: true
published: false
---

Fuzzy Finding in Bash with fzf
==============================

The [fzf](https://github.com/junegunn/fzf) utility is a line-oriented fuzzy
finding tool for the Unix command-line.

Fuzzy finding is a search technique that uses approximate pattern matching
rather than exact matching. For some search tasks, fuzzy finding will be highly
effective at generating relevant results for a minimal amount of search effort.

Adhering to the [Unix
philosophy](https://en.wikipedia.org/wiki/Unix_philosophy), fzf performs one
primary task, fuzzy finding, whilst also handling and emitting text streams.
Hence, it is quite easy to build useful [search
scripts](https://bluz71.github.io/2018/11/21/fuzzy-finding-in-bash-with-fzf#search-scripts)
based around fzf.

Some notable characteristics of fzf:

- [high-performance core](https://junegunn.kr/2015/02/fzf-in-go)

- comprehensive feature set

- pleasant colorful interface

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
to filter through. I recommend installing and then using the
[fd](https://github.com/sharkdp/fd) utility instead.

```sh
brew install fd
```

Configuration
-------------

Please add the following configuration settings to your `~/.bashrc` file.

Enable fzf key bindings in the Bash shell. If not using Brew, please adjust the
following path appropriately.

```sh
. $(brew --prefix)/opt/fzf/shell/key-bindings.bash
```

Use the `fd` command instead of the `find` command in fzf key bindings.

```sh
export FZF_DEFAULT_COMMAND='fd --type f --color=never'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_ALT_C_COMMAND='fd --type d . --color=never'
```

fzf look and behaviour are overridden in the **FZF_DEFAULT_COMMAND**
environment variable. I like a top-down, 40% fzf window that enables
multi-selection and also responds to page-up/page-up keys.

```sh
export FZF_DEFAULT_OPTS='
  --height 40% --multi --reverse
  --bind ctrl-f:page-down,ctrl-b:page-up
'
```

Please experiment and choose your own **FZF_DEFAULT_OPTS**.

Usage
-----

Examples:

```sh
fzf                             # Fuzzy file lister
fzf --preview="head -$LINES {}" # Fuzzy file lister with file preview
vim $(fzf)                      # Lauch editor on fuzzy found file
history | fzf                   # Fuzzy find a command from history
```

Commands:

- `Up` / `Down`, move cursor line up and down
- `Enter`, select choice
- `Tab`, mark multiple choices
- `Page Down` / `Ctrl-f`, move cursor line down a page
- `Page Up` / `Ctrl-b`, move cursor line up a page

Search syntax:

- `abcd`, fuzzy match
- `'abcd`, exact match
- `^abcd`, exact prefix match
- `abcd$`, exact suffix match


Bash Key Bindings
-----------------

- Alt-c
- Ctrl-r
- Ctrl-t

Screenshot
----------

<a id="search-scripts"></a>Search Scripts
-----------------------------------------

One of the search helper examples, listed below, uses the
[ripgrep](https://github.com/BurntSushi/ripgrep) utility. If you plan to use
that helper then please also install the ripgrep utility.

```sh
brew install ripgrep
```

Details about the ripgrep and fd search tools are noted in [this
post](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html).

- fzf find-file-and-edit
- fzf find-file-with-text-and-edit
- fzf git add
- fzf git unadd
- fzf git browser
- fzf process kill
