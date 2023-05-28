---
title: Maximize Productivity Of The Bash Shell
layout: default
comments: false
published: false
---

Maximize Productivity Of The Bash Shell
=======================================

This is a 2023 followup to the [Bash Shell Tweaks &
Tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html) article
from 2018.

Since that last article there has been a renaissance in shell-agnostic command
line tooling, often implemented in high-performance
[Rust](https://www.rust-lang.org), that have elevated the interactive shell
experience.

In this article: we will enable a host of useful available Bash shell options,
we will script automatic `pushd` along with matching directory stack navigation
bindings, we will integrate an interactive completion interface and we will use
modern command line tooling to sensibly improve the interactive experience,
among other tips and suggestions.

Note, my [bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files are
available at GitHub for reference.

Dispelling Bash Misconceptions
------------------------------

Before proceeding, it is worth correcting some Bash misconceptions that still
occasionally persist:

- Bash supports changing directories without entering the `cd` command
- Bash supports simple file and directory path *autocorrection*
- `**` globbing is supported
- Command `history` can be shared between Bash instances
- Bash does support *tab-completion* cycling
- Command and context-aware completion is supported through the Bash Completion
  package
- Interactive completion is supported by way of a 3rd party package

Keep reading for details about all the above.

Installation And Setup For macOS
--------------------------------

Since OS Catalina (2019), [Zsh](https://www.zsh.org) is now the default shell
for macOS. Apple still ships with Bash, but only version 3.2 which dates back to
2006; such a legacy version of Bash should be avoided for interactive use.

Updating to a modern version of Bash is most easily accomplished with the
[Homebrew](https://brew.sh) package manager:

```sh
brew install bash
echo /opt/homebrew/bin/bash | sudo tee -a /etc/shells
chsh -s /opt/homebrew/bin/bash
```

Note, if using an older Intel-based Mac, please replace `/opt/homebrew` with
`/usr/local`.

The Bash Completion package greatly enhances the interactive shell experience,
please install it as follows:

```sh
brew install bash-completion@2
```

And then add the following to your `~/.bash_profile`:

```sh
[[ -r "$HOMEBREW_PREFIX/etc/profile.d/bash_completion.sh" ]] && . "$HOMEBREW_PREFIX/etc/profile.d/bash_completion.sh"
```

Terminal Emulators
------------------

In recent years there has been a resurgence in terminal emulator development,
mirroring the previously mentioned renaissance in shell-agnostic command line
tooling.

Currently, I recommend the use of a terminal emulator that: is cross platform,
is GPU-accelerated and supports a text configuration that can be stored in a Git
repository.

Any of these fine terminal emulators will will meet those expectations:

- [Alacritty](https://alacritty.org)

- [kitty](https://sw.kovidgoyal.net/kitty)

- [Wezterm](https://wezfurlong.org/wezterm)

Readline Configuration
----------------------

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) library
is used by Bash, and certain other utilities, for line-editing and certain
history management.

Many beneficial Readline capabilities are disabled by default; thankfully it is
very easy to enable these features for the betterment of the interactive
experience.

The Readline library is configured through the `~/.inputrc` file. I use and
recommend these settings:

```sh
# TAB cycles forward through and Shift-TAB cycles backward through completion
# choices.
TAB: menu-complete
"\e[Z": menu-complete-backward

# Substring history search using UP and DOWN arrow keys.
"\e[A": history-substring-search-backward
"\e[B": history-substring-search-forward

# Launch $EDITOR on the current command and execute when finished editing.
"\ee": edit-and-execute-command

# Enable completion coloring.
set colored-completion-prefix on
set colored-stats on

# Ignore case when completing.
set completion-ignore-case on

# Treat hypen and underscores as equivalent.
set completion-map-case on

# The number of completions to display without prompt; when exceeded a
# prompt-to-display will appear.
set completion-query-items 200

# Automatically add slash to the end of symlinked directories when completing.
set mark-symlinked-directories on

# Don't automatically match files beginning with dot.
set match-hidden-files off

# Display the common prefix choices on the first completion then cycle the
# available choices on the next completion.
set menu-complete-display-prefix on

# Turn off the completions pager.
set page-completions off

# Immediately display completion matches.
set show-all-if-ambiguous on

# Smartly complete items when the cursor is not at the end of the line.
set skip-completed-text on
```

It is instructive to highlight a few capabilities enabled above:

- The `<TAB>` and `<Shift-Tab>` keys will now cycle completions choices

- Partially typing a command and pressing `<Up>` will engage `history` substring
  matching, not just start of line matching

- The `<Alt-e>` edit-and-execute binding is a reasonable Vim-mode substitute if
  Vim or Neovim is the configured `$EDITOR`; noting that Readline only supports
  a basic Vi-mode if configured

Shell Options
-------------

Somewhat similar to the previous Readline section, Bash provides a number of
useful shell options that are disabled by default.

Shell options should be set in `~/.bashrc`. These are the options I use and
recommend:

```sh
#  - autocd - change directory without entering the 'cd' command
#  - cdspell - automatically fix directory typos when changing directory
#  - direxpand - automatically expand directory globs when completing
#  - dirspell - automatically fix directory typos when completing
#  - globstar - ** recursive glob
#  - histappend - append to history, don't overwrite
#  - histverify - expand, but don't automatically execute, history expansions
#  - nocaseglob - case-insensitive globbing
#  - no_empty_cmd_completion - do not TAB expand empty lines
shopt -s autocd cdspell direxpand dirspell globstar histappend histverify \
    nocaseglob no_empty_cmd_completion
```

Bash Configuration
------------------

- History
  - Refer to: https://metaredux.com/posts/2020/07/07/supercharge-your-bash-history.html
- Completion
- LS_COLORS
- Prompt

Modern Tooling
--------------

- fzf
  - Control-t
  - Control-r
  - vf and cf recipes
  - gll & glS recipes
- Tools
  - starship / seafly
  - exa
  - zoxide
  - bat
  - delta
  - qmv
- fzf-tab-completion

Timesaving Tips
---------------
- Aliases
- recipes & bindings
  - Fish-like autopushd & dirhistory
  - www / web_search
  - Zsh-like copybuffer, Control-o
  (https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/copybuffer)
  - Zsh-like copydir, current_working_directory / cwd
  (https://github.com/mwilc0x/ohmyzsh/tree/master/plugins/copydir)




Refer to:
  - 7 Command-Line Tools That Make Your Life Easier:
    https://levelup.gitconnected.com/7-command-line-tools-that-make-your-life-easier-d69c38850d6c
  - https://dev.to/zaiste/15-command-line-tools-to-make-you-better-at-shell-cli-35n6
  - https://news.ycombinator.com/item?id=30976207
