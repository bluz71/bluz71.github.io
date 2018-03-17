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
a different shell such as [Zsh](https://www.zsh.org) or
[Fish](https://fishshell.com) for their productivity enhancements.

This post will document a number of simple tweaks and tips, gleamed from the
interwebs, that will greatly improve user productivity when using the Bash
shell that bring Bash into the modern age. Special note should be given to the
[Sensible Bash](https://github.com/mrzool/bash-sensible) project for some of
the recommendations noted below.

All the *tweaks n' tips* noted in this post are baked into my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files.

macOS, upgrade to Bash 4.x
--------------------------

macOS by default ships with a very old version of Bash, version 3.2, due to
licensing reasons. That version is over a decade old with many missing
enhancements.

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

bash-completion
---------------

Out of the box Bash only provides rudimentary filesystem based completions
unlike the Zsh and Fish shells which offer command-aware completions. For
example, `git pu<TAB>` should command-aware complete with the options of
`pull` or `push` as compared to completing with files or directories starting
with `p` (which is what standard Bash will do).

The [bash-completion](https://github.com/scop/bash-completion) packages extends
the Bash shell with intelligent completions for many common commands.

Most Linux distributions enable *bash-completion* these days.

Homebrew based Bash on macOS on the other hand you will require
*bash-completion* installation and invocation. First install *bash-completion*:

```sh
brew install bash-completion
```

Then source *bash-completion* in your *~/.bashrc* file:

```sh
. $(brew --prefix)/etc/bash_completion
```

Confirm that bash-completion is enabled by trying the `git pu<TAB>` example
noted above, the aim is to have the `pull` and `push` options displayed.

If you have never used command-aware completion before it will be revelation
how useful this capability is especially for commands such as `git` and `ssh`.
Basically you want to hit the TAB character early and often.

Readline Library Configuration
------------------------------

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html)
library is used by Bash for line-editing and history management.

The library is configured through by an `inputrc` file in a user's home
directory. A list of recommended `~/.inputrc` settings follows.

- Display completion matches upon the first pressing of the TAB key:

    ```sh
    set show-all-if-ambiguous on
    ```

- Enable colors when completing files and directories:

    ```sh
    set colored-stats on
    ```

    File and directory colors are defined in the `$LS_COLORS` environment
    variable.

- When a completion matches multiple items highlight the common matching prefix
    in color:

    ```sh
    set colored-completion-prefix on
    ```

    The prefix color used will be the `so` option defined in the `$LS_COLORS`
    environment variable.

    Note, this is a new option only available with the latest versions of
    Readline (version 7.x) and Bash (version 4.4).

- Ignore case when completing:

    ```st
    set completion-ignore-case on
    ```

- Treat hypens and underscores as equivalent when completing:

    ```st
    set completion-map-case on
    ```

- Enable incremental history navigation with the UP and DOWN keys. This will
    use the already typed text as a required prefix when navigating through
    history:

    ```st
    "\e[A": history-search-backward
    "\e[B": history-search-forward
    ```

- Disable beeps and don't display control characters:

    ```st
    set bell-style none
    set echo-control-characters off
    ```

Readline Shortcuts
------------------

The Readline library provides a number of useful default shortcuts.

- Go to the beginning of the current line:

    ```st
    Control-a
    ```

- Go to the end of the current line:

    ```st
    Control-e
    ```

- Go back one word on the current line:

    ```st
    Alt-b
    ```

- Go forward one word on the current line:

    ```st
    Alt-f
    ```

- Search back through history:

    ```st
    Control-r
    ```

    This command does a reverse search, from most recent commands to oldest,
    through existing history for commands that contain the chosen text. Hitting
    `Control-r` again cycles back through the matches whilst `Control-Shift-r`
    cycles forward.

    In my case I have mapped `Control-f` (`f` for find) in my *inputrc* to do
    this same searching:

    ```st
    "\C-f": reverse-search-history
    ```

- Append the last argument of the previous command to the end of the current
    command:

    ```st
    Alt-.
    ```

    For example, after entering `mkdir some-long-directory` simply to
    `cd <Alt-.>` to enter the new directory.

Bash Settings
-------------

New Bash features are rarely enabled by default, hence many users may be
missing out. A sensible Bash experience requires enabling these new
features.

The Bash shell is configured through a `.bashrc` file in a user's home
directory. A list of recommended `~/.bashrc` settings follows.

- Typing a directory name just by itself will automatically change into that
    directory:

    ```sh
    shopt -s autocd
    ```

- Automatically fix directory name typos when changing directory:

    ```sh
    shopt -s cdspell
    ```

- Enable the `**` pattern in file and directory expansions:

    ```sh
    shopt -s globstar
    ```

    For example, `ls **/*.txt` will list all text files in the current
    directory hierarchy.

- History settings:

    ```sh
    HISTCONTROL='erasedups:ignoreboth'
    HISTFILESIZE=9999
    HISTSIZE=9999
    PROMPT_COMMAND='history -a'
    shopt -s histappend
    ```

    The Readline incremental and reverse search commands are much more useful
    when more history is kept.

- Automatically shorten deep paths in the prompt (displayed via `/w` in `PS1`):

    ```sh
    PROMPT_DIRTRIM=4
    ```

    Choose a smaller number if you wish to display less path components.

- Enable history expansion with the space key:

    ```sh
    bind Space:magic-space
    ```

    For example, `!!<space>` will immediately expand to the last command.

Bash Aliases
-------------

A Bash alias is a user defined shortcut. Aliases save keystrokes. Do not be
afraid to alias long commands with shorter aliases, over time they will save
you a lot of time.

As a starting point, here are collection of useful aliases from my *bashrc*
file:

- List command:

    ```sh
    alias ll='ls -l'
    alias ll.='ls -la'
    alias ls='ls --color --classify --human-readable'
    alias lss='ls -la --sort=size'
    alias lst='ls -la --sort=time'
    ```

  We want colorization with human-readable file sizes. The last two aliases
  will order by size (largest to smallest) or by time (newest to oldest).

- Easy navigation up a directory path:

    ```sh
    alias ..='cd ..'
    alias ..2='..; ..'
    alias ..3='..2; ..'
    alias ..4='..3; ..'
    alias ..5='..4; ..'
    ```

    No more need to type `cd ../../..`, just do `..3` instead.

- Git command:

    ```sh
    alias g=git
    complete -o default -o nospace -F _git g
    ```

    If you use git, then why not save a couple characters?

- Confirm unsafe file commands:

    ```sh
    alias cp='/bin/cp -i'
    alias mv='/bin/mv -i'
    alias rm='/bin/rm -i'
    ```

    This will prompt you to confirm when copying over, moving over or deleting
    a file.

- List all files larger than a given size:

    ```sh
    alias lsfs='find_by_size(){ find . -type f -size "$1" -exec ls --color --classify --human-readable -l {} \; ; }; find_by_size'
    ```

    For example, `lsfs +10k` will display all files larger than 10 kilobytes.

The z Directory Navigation Utility
----------------------------------

There are a number of ways to navigate a file system: plain `cd`, or
`pushd/popd`, or defining a `$CDPATH` environment variable.

The [z](https://github.com/rupa/z) utility

Programmer Tools f and rg
-------------------------

Conclusion
----------
