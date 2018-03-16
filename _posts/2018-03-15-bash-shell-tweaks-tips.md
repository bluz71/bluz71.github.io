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
[Fish](https://fishshell.com).

This post will document a number of simple tweaks and tips, gleamed from the
interwebs, that will greatly improve user productivity when using the Bash
shell. Special note should be given to the
[Sensible Bash](https://github.com/mrzool/bash-sensible) project for some of
the recommendations noted below.

All the *tweaks n' tips* noted in this post are baked into my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files.

macOS, upgrade to Bash 4.x
--------------------------

macOS by default ships with a very old version of Bash, version 3.2 due to
licensing reasons. That version is over a decade old with many missing
features.

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

```
brew install bash-completion
```

Then source *bash-completion* in your *~/.bashrc* file:

```
. $(brew --prefix)/etc/bash_completion
```

Confirm that bash-completion is enabled by trying the `git pu<TAB>` example
noted above, the aim is to have the `pull` and `push` options displayed.

If you have never used command-aware completion before it will be revelation
how useful this capability is especially for commands such as `git` and `shh`.

GNU Readline Library Settings
-----------------------------

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html)
library is used by Bash for line-editing and history management.

The library is configured through by an `inputrc` file in the users home
directory. A minimal set of recommended `~/.inputrc` settings follows.

- Enable `HOME` and `END` keys to move cursor to the start or end of a command
    augmenting the Readline defaults of `Control-a` and `Control-e`:

    ```
    "\e[1~": beginning-of-line
    ```

- Display completion matches upon first pressing of the TAB key:

    ```
    set show-all-if-ambiguous on
    ```

- Enable `$LS_COLORS`-based colors when completing files or directory:

    ```
    set colored-stats on
    ```

- Ignore case when completing:

    ```
    set completion-ignore-case on
    ```

- Treat hypens and underscores as equivalent when completing:

    ```
    set completion-map-case on
    ```

- When completion matches multiple items, replace the common prefix with "...":

    ```
    set completion-prefix-display-length 2
    ```

- Enable incremental history searching with the UP and DOWN keys. This will use
    the already typed text as prefix when navigating through history:

    ```
    "\e[A": history-search-backward
    "\e[B": history-search-forward
    ```

- Disable beeps and don't display control characters:

    ```
    set bell-style none
    set echo-control-characters off
    ```

Bash Settings
-------------

Bash Tips
---------

- `Alt-.` append

- `Ctrl-r` search history

The z Directory Navigation Utility
----------------------------------
