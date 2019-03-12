---
title: Find & Replace Helpers for Vim
layout: default
comments: true
published: false
---

# Find & Replace Helpers for Vim

The post, [how do you handle these common find / replace use
cases](https://www.reddit.com/r/vim/comments/armt3o/how_do_you_handle_these_common_find_replace_use),
from the [Vim subreddit](https://www.reddit.com/r/vim) was the spark that lead
me down a Vim _find & replace_ rabbit-hole.

Programming often entails
[refactoring](https://en.wikipedia.org/wiki/Code_refactoring) code, that may be
as simple as renaming a local variable or as complex as changing a typename
throughout a project's entire codebase.

Vim does not prescribe a recommended way to do such refactors, for example: one
may change-word with `cw` and then do `n.n.n.`, or one may use the `:subsitute`
command to name two choices among many.

This post will highlight my new approaches to _finding & replacing_ in Vim for
the following three scenarios:

-   Find & replace nearby instances in the current file

-   Find & replace within the current file

-   Find & replace project-wide

In each scenario the text to be replaced will either be the word under the
cursor when in _normal_ mode (similar to the `*` operator), or the highlighted
text when in _visual_ mode (similar to the
[visual-star-search](https://github.com/nelstrom/vim-visual-star-search)
plugin).

Note, I am **not** proclaiming my approaches as best-in-class, rather they
should be viewed as a _resource of possibilities_.

## Prerequisites

A fairly modern version of Vim, at least [Vim 7.4.858](https://www.vim.org) or
[Neovim 0.2.0](https://neovim.io), is required since some of the helpers will
make use of the modern `gn` and `:cfdo` commands.

The project-wide recipe will also make use of the
[ripgrep](https://github.com/BurntSushi/ripgrep) command-line search tool and
the [Grepper](https://github.com/mhinz/vim-grepper) Vim plugin. The ripgrep tool
is documented in [this
article](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html)
whilst Grepper is documented
[here](https://bluz71.github.io/2017/05/21/vim-plugins-i-like.html#vim-grepper).

The `:subsitute`-based helpers will assume global replacement has been set in
your `~/.vimrc`

```viml
set gdefault
```

:bomb: I am aware this is a controversial setting since it **may** break
**some** plugins. Note, after many years I have yet to experience any
deleterious effects with the plugins I use, 40 or so, whilst I thoroughly
detested the need to enter `/g` with every `:substitute` command prior to that.

## Nearby Find & Replace

Editors such as [Visual Studio Code](https://code.visualstudio.com),
[Atom](https://atom.io) and [Sublime](https://www.sublimetext.com) provide
a multi-cursor option which allows a user to mark multiple locations, after
searching for a term, and then batch-edit simultaneously at those locations.
This pattern works well for finding and replacing instances that are nearby.

With Vim, I recommend against the usage of any multi-cursor plugins, instead the
recent [`gn`
command](http://vimcasts.org/episodes/operating-on-search-matches-using-gn) is
the natural operator for this scenerio.

Helpers to add to `~/.vimrc`

```viml
nnoremap <silent> \c :let @/='\<'.expand('<cword>').'\>'<CR>cgn
xnoremap <silent> \c "sy:let @/=@s<CR>cgn
nnoremap <Enter> gnzz
xmap <Enter> .<Esc>gnzz
xnoremap ! <Esc>ngnzz
```

Initiate the nearby replacements by executing `\c` on the word to be replaced or
for the current visual selection, type the new text and then hit `Escape` to
complete the change. The dot operator will immediately repeat that change
forward for the next match, hitting dot again to repeat the change. However, if
one wishes to accept or reject each change individually then the enter and
exclamation mark mappings listed above will prove useful; `Enter` will accept
the change and then move forward to the next match, `!` will reject the change
whilst also moving forward to the next match.

Why `\c` as the mapping? Back-slash is available for user mappings, and it
reminds me of forward-slash which is Vim's search operator. The **c** is for
change. So the mnemonic behind `\c` is find-and-change. Please replace it with
whatever key-sequence works best for you.
