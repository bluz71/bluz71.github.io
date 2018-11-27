---
title: Fuzzy Finding in Bash with fzf
layout: default
comments: true
published: true
---

Fuzzy Finding in Bash with fzf
==============================

The [fzf](https://github.com/junegunn/fzf) utility is a line-oriented fuzzy
finding tool for the Unix command-line.

Fuzzy finding is a search technique that uses approximate pattern matching
rather than exact matching. For some search tasks *fuzzy finding* will be
highly effective at generating relevant results for a minimal amount of search
effort.

Adhering to the [Unix
philosophy](https://en.wikipedia.org/wiki/Unix_philosophy), fzf performs one
primary task, fuzzy finding, whilst also handling and emitting text streams.
Hence, it is quite easy to build useful [search
scripts](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf#search-scripts)
built upon fzf.

Some notable characteristics of fzf:

- real-time updates

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

When **STDIN** is not supplied, fzf, by default, will use the [find
command](https://en.wikipedia.org/wiki/Find_(Unix)) to fetch a list of files
to filter through. I recommend installing and then using the
[fd](https://github.com/sharkdp/fd) utility instead.

```sh
brew install fd
```

Note, some of the key bindings and scripts in this post make use of the:
[ripgrep](https://github.com/BurntSushi/ripgrep),
[bat](https://github.com/sharkdp/bat) and
[tree](http://mama.indstate.edu/users/ice/tree) utilities. If using Brew please
install those utilities as follows:

```sh
brew install ripgrep bat tree
```

Details about the fd tool are noted in [this
post](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html#fd),
whilst ripgrep is noted
[here](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html#ripgrep)
and bat is discussed
[here](https://remysharp.com/2018/08/23/cli-improved#bat--cat).

Configuration
-------------

Please consider adding the following optional configuration settings to your
`~/.bashrc` file.

Enable fzf key bindings in the Bash shell.

```sh
. $(brew --prefix)/opt/fzf/shell/key-bindings.bash
```

Note, if not using Brew installed fzf, please adjust the above listed path
appropriately.

Use the `fd` command instead of the `find` command.

```sh
export FZF_DEFAULT_COMMAND='fd --type f --color=never'
export FZF_CTRL_T_COMMAND="$FZF_DEFAULT_COMMAND"
export FZF_ALT_C_COMMAND='fd --type d . --color=never'
```

The default fzf look and behaviour are overridden by the
**FZF_DEFAULT_COMMAND** environment variable. I like a top-down 75% fzf window
that enables multi-selection and also responds to `control-f`/`control-b` keys.

```sh
export FZF_DEFAULT_OPTS='
  --height 75% --multi --reverse
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

    If desired, optionally enable file previews, using
    [bat](https://github.com/sharkdp/bat), for `Control-t`.

    ```sh
    export FZF_CTRL_T_OPTS="--preview 'bat --color=always --line-range :500 {}'"
    ```

- Change to a fuzzy found sub-directory.

    ```sh
    Alt-c
    ```

  If desired, optionally enable directory previews, using
  [tree](http://mama.indstate.edu/users/ice/tree/), for `Alt-c`.

    ```sh
    export FZF_ALT_C_OPTS="--preview 'tree -C {} | head -100'"
    ```

<a id="search-scripts"></a>Search Scripts
-----------------------------------------

Whilst the above bare and key-bound usages of are useful, fzf really is best
utilized when scripting custom search commands.

Here are some that I use.

### Find File and Edit

```sh
fzf_find_edit() {
    local file=$(
      fzf --no-multi --preview 'bat --color=always --line-range :500 {}'
      )
    if [ -n "$file" ]; then
        $EDITOR $file
    fi
}

alias ffe='fzf_find_edit'
```

Fuzzy find a file, with colorful preview, then once selected edit it in your
preferred editor.

### Find File with Term and Edit

```sh
fzf_rg_edit(){
    if [ $# == 0 ]; then
        echo 'Error: search term was not provided.'
        return
    fi
    local match=$(
      rg --color=never --line-number "$1" |
        fzf --no-multi --delimiter : \
            --preview "bat --color=always --line-range {2}: {1}"
      )
    local file=$(echo "$match" | cut -d':' -f1)
    if [ -n "$file" ]; then
        $EDITOR $file +$(echo "$match" | cut -d':' -f2)
    fi
}

alias frge='fzf_rg_edit'
```

Fuzzy find a file, with colorful preview, that contains the supplied term, then
once selected edit it in your preferred editor. Note, if your `EDITOR` is Vim
or Neovim then you will be automatically scrolled to the selected line.

### Find and Kill Process

```sh
fzf_kill() {
    local pids=$(
      ps -f -u $USER | sed 1d | fzf --multi | tr -s [:blank:] | cut -d' ' -f3
      )
    if [ -n "$pids" ]; then
        echo "$pids" | xargs kill -9 "$@"
    fi
}

alias fkill='fzf_kill'
```

Fuzzy find a process or group of processes, then `SIGKILL` them.
Multi-selection is enabled to allow multiple processes to be selected via the
TAB key.

This script negates the need to run `ps` manually and all the related pain
involved to kill a recalcitrant process :tada: :tada:

### Git Stage Files

```sh
fzf_git_add() {
    local files=$(git ls-files --modified | fzf --ansi)
    if [ -n "$files" ]; then
        git add --verbose $files
    fi
}

alias gadd='fzf_git_add'
```

Selectively stage fuzzily found files for committing. Note, only modified files
will be listed for staging.

### Git Log Browser

```sh
fzf_git_log() {
    local commits=$(
      git ll --color=always "$@" |
        fzf --ansi --no-sort --height 100% \
            --preview "git show --color=always {2}"
      )
    if [ -n "$commits" ]; then
        local hashes=$(printf "$commits" | cut -d' ' -f2 | tr '\n' ' ')
        git show $hashes
    fi
}

alias gll='fzf_git_log'
```

The `ll` Git alias used above should be created with the following command.

```sh
git config --global alias.ll 'log --graph --format=format:"%C(yellow)%h%C(red)%d%C(reset) - %C(bold green)(%ar)%C(reset) %s %C(blue)<%an>%C(reset)"'
```

The `gll` Bash alias displays a compact Git log list that can be filtered by
entering in a fuzzy term at the prompt. Navigation up and down the commit list
will preview the changes of each commit.

Git Log Browser in action:

<img width="800" alt="ruby" src="https://raw.githubusercontent.com/bluz71/misc-binaries/master/blog/git_log_browser.png">

Conclusion
----------

The fzf utility has become an indispensable tool in my workflow. Please give it
a try yourself, I am positive it will prove useful to you as well.
