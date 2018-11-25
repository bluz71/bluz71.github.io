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

- colorful interface

- optional search preview in a split window

Installation
------------

fzf is easily installed using [Homebrew](https://brew.sh) on macOS and
 [Linuxbrew](http://linuxbrew.sh) on Linux.

```sh
brew install fzf
```

For other platforms please consult [these installation
details](https://github.com/junegunn/fzf#installation).

When **STDIN** is not supplied fzf, by default, will use the [find
command](https://en.wikipedia.org/wiki/Find_(Unix)) to fetch a list of files
to filter through. I recommend installing and then using the
[fd](https://github.com/sharkdp/fd) utility instead.

```sh
brew install fd
```

Details about the fd tool are noted in [this
post](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html#fd).

Configuration
-------------

Please add the following configuration settings to your `~/.bashrc` file.

Enable fzf key bindings in the Bash shell.

```sh
. $(brew --prefix)/opt/fzf/shell/key-bindings.bash
```

Note, if not using Brew, please adjust the above listed path appropriately.

Use the `fd` command instead of the `find` command.

```sh
export FZF_DEFAULT_COMMAND='fd --type f --color=never'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_ALT_C_COMMAND='fd --type d . --color=never'
```

The default fzf look and behaviour are overridden with the
**FZF_DEFAULT_COMMAND** environment variable. I like a top-down, 40% fzf window
that enables multi-selection and also responds to page-up/page-up keys.

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
vim $(fzf)                      # Lauch Vim editor on fuzzy found file
history | fzf                   # Fuzzy find a command from history
cat /usr/share/dict/words | fzf # Fuzzy search a dictionary word
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

In the configuration section above the following Bash key bindings were enabled.

- Reverse search through Bash history using fzf as the filter.

    ```sh
    Control-r
    ```

- Append fuzzy found files to the end of the current shell command.

    ```sh
    Control-t
    ```

- Change to a fuzzy found sub-directory.

    ```sh
    Alt-t
    ```

<a id="search-scripts"></a>Search Scripts
-----------------------------------------

Whilst the above bare and key-bound usages of are useful, fzf really is best
utilized when scripting custom search commands.

Note, some of the scripts make use of
[ripgrep](https://github.com/BurntSushi/ripgrep) and
[bat](https://github.com/sharkdp/bat). If using Brew please install those
utilities as follows.

```sh
brew install ripgrep bat
```

Please read [this
post](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html#ripgrep)
for details about ripgrep, and [this
one](https://remysharp.com/2018/08/23/cli-improved#bat--cat) for details about
bat.

### Find File and Edit

```sh
fzf_find_edit() {
    local file=$(
      fzf --no-multi --height 80% \
          --preview 'bat --color=always --line-range :500 {}'
      )
    if [ -n "$file" ]; then
        $EDITOR $file
    fi
}

alias vf='fzf_find_edit'
```

Fuzzy find a file, with colorful preview, then once selected edit it in your
preferred editor.

### Find File with Text and Edit

```sh
fzf_rg_edit(){
    if [ $# == 0 ]; then
        echo 'Error: search term was not provided.'
        return
    fi
    local match=$(
      rg --color=never --line-number "$1" |
        fzf --no-multi --delimiter : --height 80% \
            --preview "bat --color=always --line-range {2}: {1}"
      )
    local file=$(echo "$match" | cut -d':' -f1)
    if [ -n "$file" ]; then
        $EDITOR $file +$(echo "$match" | cut -d':' -f2)
    fi
}

alias vrg='fzf_rg_edit'
```

Fuzzy find a file, with colorful preview, contained the supplied term, then
once selected edit it in your preerred editor. Note, if your `EDITOR` is Vim or
Neovim then you will be automatically scrolled to the selected line.

### Find and Kill Process

### Git Stage Files

### Git Unstage Files

### Git Log Browser
