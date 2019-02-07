---
title: Turbocharge the CtrlP Vim plugin
layout: default
comments: true
published: true
---

Turbocharge the CtrlP Vim plugin
================================

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin is a well established
fuzzy finder for the Vim editor. This plugin is most often used to quickly
navigate to a file, within the current development tree, by simply typing
letters that appear in the file name; CtrlP will take care of the fuzzy
matching and display of those matches.

Being a mature plugin now CtrlP is not quite as cool as it used to be primarily
due its poor performance with very large development trees, especially when
compared with modern alternatives such as
[fzf](https://github.com/junegunn/fzf.vim). Transitioning over to fzf however
is not quite seamless for those of us who occasionally dabble with GUI-based
Vim's such as gVim or MacVim; in both cases fzf will pop up a terminal
window which can be distracting. Note, CtrlP does not do this.

This post is all about the middle ground, that being to still use CtrlP for
fuzzy finding, but greatly improving its performance and also improving the
quality of the match results.

**UPDATE (DEC 2018)**: I now use fzf.vim instead of CtrlP as documented
[here](https://bluz71.github.io/2018/12/04/fuzzy-finding-in-vim-with-fzf.html).
The fzf issue noted above no longer applies since Neovim & Vim integrated a
terminal.

Faster file listing using fd
============================

The single biggest enhancement that can be done to improve CtrlP's performance
is to specify a fast external file lister. The fastest such tool I have
encountered is [fd](https://github.com/sharkdp/fd).

It is quite easily installed using `brew` on macOS or Linux (using
[Linuxbrew](https://docs.brew.sh/Linuxbrew)):

```
brew install fd
```

Then in your *vimrc* configure CtrlP to use fd when fuzzy file finding:

```viml
let g:ctrlp_user_command = 'fd --type f --color=never "" %s'
```

Since fd is usually so fast it is no longer necessary for CtrlP to cache
results, actually it is better to turn CtrlP caching off since that will lead
to mismatches as files come and go in your development tree:

```viml
let g:ctrlp_use_caching = 0
```

Better matches using cpsm
=========================

Another enhancement to improve CtrlP performance and quality is to use the
[cpsm](https://github.com/nixprime/cpsm) CtrlP matcher. The cpsm plugin is a
compiled Vim plugin for CtrlP with an emphasis on improving the ranked
results CtrlP displays when fuzzy finding.

An example would be when editing a C file *foobar.c*, then hitting `ctrl-p`,
immediately the first match will be *foobar.h*. When using CtrlP, without cpsm,
the first match would just be the standard development tree order of files. The
cpsm plugin makes switching to like-files extremely quick.

The cpsm plugin, by being a native compiled plugin, also claims to improve
CtrlP [performance](https://github.com/nixprime/cpsm#performance).

However, being a native compiled plugin does complicate installation. Please
make sure these prerequisites are installed on your machine.

On macOS:

```
brew install cmake python boost
```

On Linux (Debian flavoured):

```
sudo apt install cmake python-dev libboost-all-dev
```

Finally install the cpsm plugin using your preferred Vim plugin manager. I like
[vim-plug](https://github.com/junegunn/vim-plug) since it allows compilation of
the cpsm plugin directly **without** needing to `cd` (then compile) in the cpsm
directory after installation. In vimrc:

```viml
Plug 'nixprime/cpsm', { 'do': 'env PY3=OFF ./install.sh' }
let g:ctrlp_match_func = { 'match': 'cpsm#CtrlPMatch' }
```

Don't forget to do `:PlugInstall` when using vim-plug.

Note, the above cpsm `Plug` lines assumes a Python 2 installation, change to
`PY3=ON` for Python 3 installations.

Fuzzy find functions in the current buffer
==========================================

The [ctrlp-funky](https://github.com/tacahiroy/ctrlp-funky) plugin is a handy
CtrlP extension that provides simple fuzzy function finding in the current
buffer. It is a fast way to navigate to function definitions without needing to
call on the services of *ctags*.

In vimrc using vim-plug:

```viml
Plug 'tacahiroy/ctrlp-funky'
let g:ctrlp_funky_syntax_highlight = 1
nnoremap <leader>f :CtrlPFunky<CR>
```

Note, use whatever mapping you desire to launch ctrlp-funky, here I am using
`<leader>f`.

Summary
=======

The CtrlP plugin still has a place in the Vim world even with modern alternatives
such as fzf available. However, it does take some configuration for CtrlP to
shine as best it can.

Happy fuzzy finding!
