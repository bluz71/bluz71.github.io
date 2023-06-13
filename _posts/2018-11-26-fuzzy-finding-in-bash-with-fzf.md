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
effort. In practice this often means that just a few notable characters need be
entered to quickly fuzzy find the item of interest.

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

Note, feel free to refer to my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) file to view my
latest fzf configuration.

Installation
------------

fzf is easily installed using [Homebrew](https://brew.sh) on macOS and Linux.

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

Note, some key bindings and scripts in this post make use of the:
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
vim $(fzf)                      # Launch Vim editor on fuzzy found file
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
      fzf --query="$1" --no-multi --select-1 --exit-0 \
          --preview 'bat --color=always --line-range :500 {}'
    )
    if [[ -n "$file" ]]; then
        $EDITOR "$file"
    fi
}

alias fe='fzf_find_edit'
```

Fuzzy find a file, with optional initial file name, and then edit:

- If one file matches then edit immediately

- If multiple files match, or no file name is provided, then open fzf with
  colorful preview

- If no files match then exit immediately

### Find Directory and Change

```sh
fzf_change_directory() {
    local directory=$(
      fd --type d | \
        fzf --query="$1" --no-multi --select-1 --exit-0 \
            --preview 'tree -C {} | head -100'
    )
    if [[ -n $directory ]]; then
        cd "$directory"
    fi
}

alias fcd='fzf_change_directory'
```

Fuzzy find a directory, with optional initial directory name, and then change to
it:

- If one directory matches then `cd` immediately

- If multiple directories match, or no directory name is provided, then open fzf
  with tree preview

- If no directories match then exit immediately

### Find and Kill Process

```sh
fzf_kill() {
    if [[ $(uname) == Linux ]]; then
        local pids=$(ps -f -u $USER | sed 1d | fzf | awk '{print $2}')
    elif [[ $(uname) == Darwin ]]; then
        local pids=$(ps -f -u $USER | sed 1d | fzf | awk '{print $3}')
    else
        echo 'Error: unknown platform'
        return
    fi
    if [[ -n "$pids" ]]; then
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
    local selections=$(
      git status --porcelain | \
        fzf --ansi \
            --preview 'if (git ls-files --error-unmatch {2} &>/dev/null); then
                           git diff --color=always {2}
                       else
                           bat --color=always --line-range :500 {2}
                       fi'
    )
    if [[ -n $selections ]]; then
        local additions=$(echo $selections | sed 's/M //g')
        git add --verbose $additions
    fi
}

alias gadd='fzf_git_add'
```

Selectively stage modified and untracked files, with preview, for committing.
Note, modified and untracked files will be listed for staging.

### Git Log Browser

```sh
fzf_git_log() {
    local selection=$(
      git ll --color=always "$@" | \
        fzf --no-multi --ansi --no-sort --no-height \
            --preview "echo {} | grep -o '[a-f0-9]\{7\}' | head -1 |
                       xargs -I@ sh -c 'git show --color=always @'"
    )
    if [[ -n $selection ]]; then
        local commit=$(echo "$selection" | sed 's/^[* |]*//' | awk '{print $1}' | tr -d '\n')
        git show $commit
    fi
}

alias gll='fzf_git_log'
```

The `ll` Git alias used above can be created with the following command:

```sh
git config --global alias.ll 'log --graph --format="%C(yellow)%h%C(red)%d%C(reset) - %C(bold green)(%ar)%C(reset) %s %C(blue)<%an>%C(reset)"'
```

The `gll` Bash alias displays a compact Git log list that can be filtered by
entering a fuzzy term at the prompt. Navigation up and down the commit list will
preview the changes of each commit.

Git Log Browser in action:

<img width="800" alt="git_log_browser" src="https://raw.githubusercontent.com/bluz71/misc-binaries/master/blog/git_log_browser.png">

### Git RefLog Browser

```sh
fzf_git_reflog() {
    local selection=$(
      git reflog --color=always "$@" |
        fzf --no-multi --ansi --no-sort --no-height \
            --preview "git show --color=always {1}"
    )
    if [[ -n $selection ]]; then
        git show $(echo $selection | awk '{print $1}')
    fi
}

alias grl='fzf_git_reflog'
```

The `grl` Bash alias displays a Git
[reflog](https://git-scm.com/docs/git-reflog) list that can be filtered by
entering a fuzzy term at the prompt. Navigation up and down the hash list will
preview the changes of each hash.

### Git Pickaxe Browser

```sh
fzf_git_log_pickaxe() {
     if [[ $# == 0 ]]; then
         echo 'Error: search term was not provided.'
         return
     fi
     local selection=$(
       git log --oneline --color=always -S "$@" |
         fzf --no-multi --ansi --no-sort --no-height \
             --preview "git show --color=always {1}"
     )
     if [[ -n $selection ]]; then
         local commit=$(echo "$selection" | awk '{print $1}' | tr -d '\n')
         git show $commit
     fi
 }

alias glS='fzf_git_log_pickaxe'
```

The `glS` Bash alias displays a Git log list that has been
[pickaxe](http://www.philandstuff.com/2014/02/09/git-pickaxe.html) (`-S`)
filtered by the supplied search term. Navigation up and down the commit list
will preview the changes of each hash.

Conclusion
----------

The fzf tool has become an indispensable tool in my workflow. Please give it
a try yourself, I am confident it will prove useful to you as well.
