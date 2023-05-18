---
title: Maximize Productivity Of The Bash Shell
layout: default
comments: false
published: false
---

Maximize Productivity Of The Bash Shell
=======================================

Introduction

Talk about Zsh and Fish.

Benefits Of The Bash Compared To The Alternatives
-------------------------------------------------------

Even with macOS switching their default shell to Zsh, Bash is still a highly
ubiquitous shell used by default for most Linux systems along with the Windows
Subsystem for Linux (WSL).

However, these days, one of the prime benefits of Bash is simplicity and
stability.

For example, the Zsh completion system is more capable than the Bash completion
system, but at the same time it's configuration [is far more
complex](https://thevaluable.dev/zsh-completion-guide-example).

Unlike Zsh, Bash does not have a thriving plugin eco-system. That is
viewed as a negative, less functionality, but it is also a positive in that Bash
users tend to have simpler, smaller and more stable shell setups as [noted by
this comment](https://news.ycombinator.com/item?id=30976207). Zsh on the other
hand has a thriving community of plugins, plugin manager and frameworks. For
example here is a partial list of available Zsh frameworks and plugin managers:

- [Oh-My-Zsh](https://github.com/ohmyzsh)
- [Prezto](https://github.com/sorin-ionescu/prezto)
- [Zim](https://github.com/zimfw/zimfw)
- [Zinit](https://github.com/zdharma-continuum/zinit)
- [zplug](https://github.com/zplug/zplug)
- [Antidote](https://getantidote.github.io/)
- [Znap](https://github.com/marlonrichert/zsh-snap)
- [zcomet](https://github.com/agkozak/zcomet)
- [zgenom](https://github.com/jandamm/zgenom)
- [Zap](https://github.com/zap-zsh/zap)

Which to choose? Oh-My-Zsh is the most popular, but at the same time there is
[critism of the quality of the Oh-My-Zsh
codebase](https://archive.zhimingwang.org/blog/2015-05-03-why-oh-my-zsh-is-completely-broken.html).

Zsh, which syntax highlighting engine to use, []
[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)
or [Fast Syntax Highlighting](https://github.com/zdharma-continuum/fast-syntax-highlighting)?

Zsh, sometimes plugins are incompatible with certain versions? LOOK FOR AN
EXAMPLE.

Fish is not POSIX.

With Bash one tends to set and forget; some functionality is traded in for less
terminal tweaking.

Misconceptions Of The Bash
--------------------------

There still exists some misconception about Bash limitations that are no longer
valid, for example:

- [Changing directories](https://kbknapp.dev/shell-setup) without typing `cd`,
'autocd' exists
- [Case-insensitive path](https://kbknapp.dev/shell-setup) auto-correction,
  `cdspell` and `dirspell` exists
- globstar `**`
- [Shared command history]
- [Tab completion cycling](https://www.fosslinux.com/58416/bash-vs-zsh-differences.htm)
- Command and context-aware completion, for example `git` option completion,
    bash-completion exists
- `autopushd` and Fish-like `Alt-Left` / `Alt-Right` directory stack navigation
- Interactive completion, refer to `fzf-tab-completion`

https://www.quora.com/What-is-the-difference-between-bash-and-zsh

Limitations Of Bash
-------------------

- Capabilities Zsh & Fish has over and above Bash
  - Command line syntax highlighting: https://github.com/zdharma-continuum/fast-syntax-highlighting
  - Auto-suggestions: https://github.com/zsh-users/zsh-autosuggestions
  - Interative & descriptive completions
  - Right-side prompt
  - Proper Vim mode as against Vi mode (I prefer Alt-e)
  - Global aliases
  - auto-expanding aliases
  - Recursive path expansion, cd /u/sh expands to /usr/share

Zsh much more capable and flexible.

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
