---
title: Fuzzy Finding in Vim with fzf
layout: default
comments: true
published: false
---

Fuzzy Finding in Vim with fzf
=============================

Following on from [Fuzzy Finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html),
this post will discuss usage of the [fzf](https://github.com/junegunn/fzf) tool
within the [Vim](https://www.vim.org) and [Neovim](https://neovim.io) editors.

Please do read [Fuzzy Finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
to gain insight about the fzf tool itself, how to install, configure and then
use it within a [Bash](https://www.gnu.org/software/bash) command-line.

Fuzzy finding is a powerful search technique that can be just as useful inside
an editor as it is at the command-line. Some fuzzy finding operations within an
editor could include: opening a project file in a far-away location by fuzzy
name, switching to an open buffer by fuzzy name or opening a file by fuzzy
[tag](http://vim.wikia.com/wiki/Browsing_programs_with_tags).

Requirements
------------

For the best user experience, a modern versions of Vim or Neovim that provides
an inbuilt terminal should be used. For Vim that means using version 8.1 or
later, and for Neovim that means using version 0.2 or later. GUI-based Vim's,
such as gVim and MacVim, especially benefit since prior to the integration of a
terminal fzf would spawn an ugly external terminal window which was highly
distracting.

I recommed installing and updating Vim or Neovim using
[Homebrew](https://brew.sh/) on macOS and [Linuxbrew](http://linuxbrew.sh) on
Linux.

For example, to install Neovim:

```sh
brew install neovim
```

And to update Neovim:

```sh
brew upgrade neovim
```

Plugin
------

[fzf](https://github.com/junegunn/fzf) provides basic [Vim
integration](https://github.com/junegunn/fzf/blob/master/README-VIM.md) out of
the box.

However, I strongly recommend installing and using the
[fzf.vim](https://github.com/junegunn/fzf.vim) plugin instead. Note, both
projects are maintained by [Junegunn Choi](https://github.com/junegunn), hence,
will always be synchronized if installed and updated simultaneously.

If using [vim-plug](https://github.com/junegunn/vim-plug), please add the
following to your `~/.vimrc` file:

```viml
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --no-bash' }
Plug 'junegunn/fzf.vim'
```

Then run `:PlugInstall` to install the fzf command-line tool and Vim plugin.

If using a different plugin manager please adjusts the statement appropriately.

Configuration
-------------

```viml
let g:fzf_layout = { "window": "silent botright 16split enew" }
nnoremap <silent> <localleader><Space> :Files<CR>
nnoremap <silent> <localleader>-       :Files <C-r>=expand("%:h")<CR>/<CR>
nnoremap <silent> <localleader>]       :Tags<CR>
nnoremap <silent> <localleader>'       :Marks<CR>
nnoremap <silent> <localleader>,       :Buffers<CR>
nnoremap <silent> <localleader>c       :Commits<CR>
nnoremap <silent> <localleader>h       :Helptags<CR>
nnoremap <silent> <localleader>b]      :BTags<CR>
nnoremap <silent> <localleader>bc      :BCommits<CR>
nnoremap <localleader>gr               :Rg<Space>

if filereadable('config/routes.rb')
    " This looks like a Rails app.
    noremap <silent> <localleader>ec :Files app/controllers<CR>
    noremap <silent> <localleader>eh :Files app/helpers<CR>
    noremap <silent> <localleader>em :Files app/models<CR>
    noremap <silent> <localleader>es :Files app/assets/stylesheets<CR>
    noremap <silent> <localleader>et :Files spec<CR>
    noremap <silent> <localleader>ev :Files app/views<CR>
elseif filereadable('src/index.js')
    " This looks like a React app.
    noremap <silent> <localleader>ec :Files src/components<CR>
    noremap <silent> <localleader>es :Files src/styles<CR>
    noremap <silent> <localleader>et :Files src/__tests__/components<CR>
endif
```

Usage
-----

Rg/Ag

MRU
---

Why Not Just CtrlP
------------------
Speed.
Asynchronous.
Large source trees
CPSM improves things, but not good in Neovim due to Python RPC
