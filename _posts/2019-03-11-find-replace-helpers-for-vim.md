---
title: Find & Replace Helpers for Vim
layout: default
comments: true
published: true
---

Find & Replace Helpers for Vim
==============================

**UPDATED SEPTEMBER 2021**

The post, [how do you handle these common find / replace use
cases](https://www.reddit.com/r/vim/comments/armt3o/how_do_you_handle_these_common_find_replace_use),
from the [Vim subreddit](https://www.reddit.com/r/vim) was the spark that led
me down a _find & replace_ rabbit-hole.

Programming often entails
[refactoring](https://en.wikipedia.org/wiki/Code_refactoring) code, that may be
as simple as renaming a local variable or as complex as changing a typename
throughout a project's entire codebase.

Vim does not prescribe a way to do such refactors, for example: one may execute
a find followed by `cw` (change-word) and then repeat with `n.n.n.`, or one may
use the `:%substitute` command, to name a couple choices among many.

This post will highlight my new helpers for _finding & replacing_ in Vim for
the following three scenarios:

-   Find & replace nearby instances

-   Find & replace within the current file

-   Find & replace project-wide

In each scenario the text to be replaced will either be the word under the
cursor when in _normal mode_ (similar to the `*` operator), or the highlighted
text when in _visual mode_ (similar to the
[visual-star-search](https://github.com/nelstrom/vim-visual-star-search)
plugin).

Note, I am **not** proclaiming these helpers as best-in-class, rather they
should be viewed as a _resource of possibilities_.

Prerequisites
-------------

A modern version of Vim, at least [Vim
7.4.858](https://www.vim.org/download.php) or [Neovim
0.2.0](https://github.com/neovim/neovim/wiki/Installing-Neovim), is required
since some helpers make use of the modern `gn` and `:cfdo` commands.

The project-wide helper will also make use of the
[ripgrep](https://github.com/BurntSushi/ripgrep) command-line search tool. The
ripgrep tool is documented in [this
article](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html).

Please set ripgrep as the preferred grep tool in your `~/.vimrc`:

```viml
set grepprg=rg\ --vimgrep\ --smart-case
set grepformat=%f:%l:%c:%m,%f:%l:%m
```

The `:substitute`-based helpers assume global replacement has been set in your
`~/.vimrc`

```viml
set gdefault
```

:bomb: I am aware this is a controversial setting since it **may** break
**some** plugins. But, after many years I have yet to experience any
deleterious effects with the plugins I use, whilst I thoroughly detest the
need to enter `/g` with every `:substitute` command prior to that. Your mileage
may vary.

Nearby Find & Replace
---------------------

Editors such as [Visual Studio Code](https://code.visualstudio.com),
[Atom](https://atom.io) and [Sublime](https://www.sublimetext.com) provide a
multi-cursor option which allows a user to mark multiple locations, after a
search, and then batch-edit simultaneously at those locations. That pattern
works well for finding and replacing instances that are nearby.

With Vim, I recommend against the use of any multi-cursor plugins, instead the
modern `gn`
[command](http://vimcasts.org/episodes/operating-on-search-matches-using-gn) is
the natural operator for this scenario.

Helpers to add to `~/.vimrc`

```viml
nnoremap <silent> <Leader>c :let @/='\<'.expand('<cword>').'\>'<CR>cgn
xnoremap <silent> <Leader>c "sy:let @/=@s<CR>cgn
nnoremap <CR> gnzz
xmap <CR> .<Esc>gnzz
xnoremap ! <Esc>ngnzz
autocmd! BufReadPost quickfix nnoremap <buffer> <CR> <CR>
autocmd! CmdwinEnter *        nnoremap <buffer> <CR> <CR>
```

Initiate nearby replacements by executing `<Leader>c` on the word to be
replaced, or for the current visual selection, type the new text and then hit
`Esc` to complete the change. The dot operator will immediately repeat that
change forward for the next match, hit dot again to continue repeating the
change. However, if one wishes to individually accept or reject each change then
the *enter* and *exclamation mark* mappings listed above will prove useful;
`Enter` will accept the change and then move forward to the next match, `!` will
reject the change whilst also moving forward to the next match.

Note, we restore the default `Enter` behaviour, via an `autocmd`, when in the
quickfix list, that being the ability to go to the entry selected.

Why `<Leader>c` as the mapping? The **c** is for change. So the mnemonic behind
`<Leader>c` is find-and-change. Please replace it with whatever key-sequence
works best for you.

Find & Replace in the Current File
----------------------------------

Helpers to add to `~/.vimrc`

```viml
nnoremap <Leader>s :%s/<C-r><C-w>//<Left>
xnoremap <Leader>s "sy:%s/<C-r>s//<Left>
```

As most Vim users will be aware, the `:substitute` command when prefixed with
`%` is all that is required to substitute throughout the current file. The
`<Leader>s` helper will pre-populate the source field of the `:%substitute`
command with either the word under the cursor or the current visual selection,
one is then free to simply enter the replacement text followed by `Enter` to
perform the substitutions.

Note, if confirmation for each replacement is required than append `/c` to the
end of the `:%substitute` command.

Similar to the `<Leader>c` mapping above, the mnemonic behind `<Leader>s` is
find-and-substitute, **s** for substitute. Please replace that mapping with
whatever key-sequence works best for you.

:bulb: [Neovim](https://neovim.io) provides an option, `inccommand`, to live
preview the `:substitute` command. If you use Neovim then I recommend this
turning on this setting.

```viml
if has("nvim")
    set inccommand=nosplit
endif
```

[This Vimcast](http://vimcasts.org/episodes/neovim-eyecandy) by Drew Neil
highlights the live preview feature.

Project-wide Find & Replace
---------------------------

Helpers to add to `~/.vimrc`

```viml
nnoremap <Leader>S
  \ :let @s='\<'.expand('<cword>').'\>'<CR>
  \ :let &grepprg=&grepprg . ' -w'<CR>
  \ :silent grep <C-r><C-w><CR>
  \ :let &grepprg='rg --vimgrep --smart-case'<CR>
  \ :cfdo %s/<C-r>s// \| update
  \ <Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left>
xnoremap <Leader>S
  \ "sy\|
  \ :silent grep <C-r>s<CR>
  \ :cfdo %s/<C-r>s// \| update
  \ <Left><Left><Left><Left><Left><Left><Left><Left><Left><Left><Left>
```

Note, please make sure [ripgrep](https://github.com/BurntSushi/ripgrep) is
available on the host. Again for reference, I have documented both the ripgrep
tool
[here](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html).

The above-listed `<Leader>S` helpers use ripgrep, via the `grep` command, to do
a project-wide search on either the word under the cursor or the current visual
selection, followed by a pre-populated substitution by way of the `:cfdo`
[command](https://bluz71.github.io/2017/05/15/vim-tips-tricks.html#cfdo) on the
grep matched files in the _quickfix_ list. Yes, the mapping and the
explanation are convoluted, but in use it really is quite simple.

Note, if confirmation for each replacement is required than append `/c` to the
end of the `:%substitute` command.

The mnemonic behind `<Leader>S` is find-and-SUBSTITUTE, capital **S** for
substitute across many files. As per usual, please replace that mapping with a
key-sequence that works best for you.

Summary
=======

I am sure there are many other fine Vim-related find & replace solutions beyond
those I have listed here. Hopefully this article stimulates you to improve your
Vim workflow similar to how [this Reddit
post](https://www.reddit.com/r/vim/comments/armt3o/how_do_you_handle_these_common_find_replace_use)
did for me :beer:
