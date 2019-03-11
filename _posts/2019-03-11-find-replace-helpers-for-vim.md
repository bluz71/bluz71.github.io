---
title: Find & Replace Helpers for Vim
layout: default
comments: true
published: false
---

# Find & Replace Helpers for Vim

The post, [how do you handle these common find / replace use cases in an
efficient
way?](https://www.reddit.com/r/vim/comments/armt3o/how_do_you_handle_these_common_find_replace_use),
from the [Vim subreddit](https://www.reddit.com/r/vim) was the spark that lead
me down a Vim _find & replace_ rabbit-hole.

Programming often entails
[refactoring](https://en.wikipedia.org/wiki/Code_refactoring) code, that may be
as simple as renaming a local variable or as complex as changing a typename
throughout a project's entire codebase.

Vim does not prescribe a recommended way to do such refactors, for example: one
may change-word with `cw` and then do `n.n.n.`, or one may use the `:subsitute`
command to name two choices among many.

This post will highlight my new approaches to _find & replace_ within Vim for
the following three common scenarios:

-   Find & replace a few nearby instances in the current file

-   Find & replace entirely in the current file

-   Find & replace project-wide

In each scenario the text to be replaced will either be the word under the
cursor when in _normal_ mode (similar to the `*` operator), or the highlighted
term when in _visual_ mode (similar to the
[visual-star-search](https://github.com/nelstrom/vim-visual-star-search)
plugin).

Note, I am **not** proclaiming my approaches as best-in-class, rather they
should be viewed as a _resource of possibilities_.

## Prerequisites

A modern version of Vim is required, either [Vim
8.1](https://www.vim.org/download.php) or [Neovim 0.3.4](https://neovim.io) at
the time of this posting. That is due to the usage of the somewhat recent `cgn`
and `:cfdo` commands.

The project-wide recipe will make also use of the
[ripgrep](https://github.com/BurntSushi/ripgrep) command-line search tool and
the [Grepper](https://github.com/mhinz/vim-grepper) Vim plugin. The ripgrep tool
is documented in [this
article](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html)
whilst Grepper is documented
[here](https://bluz71.github.io/2017/05/21/vim-plugins-i-like.html#vim-grepper).
