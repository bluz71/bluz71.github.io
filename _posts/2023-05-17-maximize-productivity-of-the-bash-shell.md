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
[Rust](https://www.rust-lang.org), that have elavated the interactive shell
experience.

In this article: we will enable a host of useful available Bash shell options,
we will script automatic `pushd` along with matching directory stack navigation
bindings, we will integrate an interactive completion interface and we will use
modern command line tooling to sensibly improve the interactive experience,
among other tips and recommendations.

Note, my [bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files are
available on GitHub for reference.

Dispelling Bash Misconceptions
------------------------------

Before proceeding, it is worth correcting some Bash misconceptions that still
persist:

- Bash supports changing directories without the `cd` command
- Bash supports simple file and directory path *autocorrection*
- `**` globbing is supported
- Command `history` can be shared between Bash instances
- Bash does support *tab-completion* cycling
- Command and context-aware completion is supported via the `bash-completion`
  package
- Interactive completion is supported via a 3rd party package

Keep reading for details about all the above.

Recommendations
---------------

- Use a modern True-color terminal: Alacritty, Kitty, Wezterm
  - Cross platform
  - GPU accelerated
  - Text configuration that can be git committed

How To Install On macOS
----------------------

Readline Configuration
----------------------

- Alt-e binding

Bash Configuration
------------------

- shopt configuration
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
