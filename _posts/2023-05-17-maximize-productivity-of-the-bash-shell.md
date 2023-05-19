---
title: Maximize Productivity Of The Bash Shell
layout: default
comments: false
published: false
---

Maximize Productivity Of The Bash Shell
=======================================

This is a 2023 followup to my 2018 [Bash Shell Tweaks &
Tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html) article.

Since that last article there has been a renaissance in shell-agnostic command
line tooling, often implemented in high-performance
[Rust](https://www.rust-lang.org).

In this article, just to name a few: we will enable a host of useful available
Bash shell options, we will script automatic `pushd` with `Alt-Left` /
`Alt-Right` directory stack navigation, we will integrate an interactive
completion interface and we will use modern tooling to sensibly improve the
interactive Bash experience.

Dispelling Bash Misconceptions
------------------------------

From time to time I still encounter misinformation about Bash limitations,
before moving on let's dispel some misconceptions:

- Bash supports changing directories without the `cd` command via the `autocd`
  shell option
- Bash supports simple filepath *autocorrection* via the the `cdspell` and
  `dirspell` shell options
- `**` globbing is supported via the `globstar` shell option
- Command `history` can be shared between Bash instances
- Bash support *tab-completion* cycling
- Command and context-aware completion is supported via the `bash-completion`
  package

Bash Compared To The Alternatives
---------------------------------

Let's first compare Bash aginst the [Zsh](https://www.zsh.org) &
[Fish](https://fishshell.com) alternative shells. Note, take my opinions with a
large pinch of salt.

From my perspective these are some the of the weak points of the Bash compared
to Zsh and Fish:

- No longer the default shell for macOS since Catalina (2019)
- Primitive *completion* & *auto-correction* subsystems; for example command
  completions will lack descriptions
- Threadbare community plugin ecosystem, unlike Zsh's
  [Oh-My-Zsh](https://ohmyz.sh) or Fish's
  [Fisher](https://github.com/jorgebucaran/fisher)
- GNU Readline line-editor, which Bash uses, lacks certain capabilities such as
  [autosuggestion](https://github.com/zsh-users/zsh-autosuggestions) & [syntax
  highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
- Does not support right-side prompt
- Vi-editing mode vs Vim-editing mode available in Zsh

And now some of the strong points of Bash compared to the alternatives:

- Still the default shell for most Linux distributions and the Windows Subsystem
  for Linux
- Well documented, both online and printed
- Simplicity, especially compared to Zsh which is both [more flexible, yet more
  complex](https://thevaluable.dev/zsh-completion-guide-example)
- Speed & latency compared to fully tricked out Zsh configuration which can
  slow down due to Oh-My-Zsh bloat and other line-editing smarts such as
  *autocorrection*, *autosuggestion* & *syntax highlighting*
- Fully POSIX compliant unlike Fish; the importance of this will vary per user
- Lack of community plugin ecosystem avoids [the paradox of
  choice](https://en.wikipedia.org/wiki/The_Paradox_of_Choice); for example
  which Zsh plugin manager / framework to use:
  [Oh-My-Zsh](https://github.com/ohmyzsh),
  [Prezto](https://github.com/sorin-ionescu/prezto),
  [Zim](https://github.com/zimfw/zimfw),
  [Zinit](https://github.com/zdharma-continuum/zinit), or no plugin manager at
  all?

Be aware, this is a not a exhaustive list.

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
