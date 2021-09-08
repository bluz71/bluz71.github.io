---
title: A few Vim & tmux mappings
layout: default
comments: true
published: true
---

A few Vim & tmux mappings
=========================

**UPDATED SEPTEMBER 2021**

Power *Vim* usage involves mappings, usually many of them. This post will list
some of *my* most used mappings. Note, this post is **not** intended to be
about convincing anyone to change *their* mappings, rather the intention is to
simply provide yet another resource of possibilities.

Both *Vim* and *tmux* mappings will somewhat be intermingled in this post since
I often use both as a unified whole.

I will let others explain the benefits of *tmux* and *Vim* together:
[vim + tmux: A Perfect Match](https://teamgaslight.com/blog/vim-plus-tmux-a-perfect-match),
and [Benefits of using tmux](https://blog.bugsnag.com/benefits-of-using-tmux)

The mappings discussed in this post are backed into my [vim
](https://github.com/bluz71/dotfiles/blob/master/vim/custom/mappings.vim)
and [tmux](https://github.com/bluz71/dotfiles/blob/master/tmux.conf)
configurations.

*tmux* prefix
-------------

*tmux* operations are usually invoked with a `<prefix>` plus `<command>` key
combination.

The default *tmux* prefix is `Ctrl-b`. That is an awkward combination to type
with one hand, hence many folk change it to `Ctrl-a`. In my case I find
`Ctrl-w` even nicer to type.

In *~/.tmux.conf:*

```sh
unbind-key C-b
set -g prefix C-w
```

*Vim* Leader
------------

The *Vim* leader is a user definable key that is used as a prefix for custom
mappings. Think of it as being similar to `Alt` or `Ctrl`,  but in addition to
those modifiers.

The default *Vim* leader key is backslash which, much like the *tmux* default
prefix, I find to be an awkward key. The two most common replacements are
`Space` and `,`.

I like comma as my leader key, in *~/.vimrc:*

```viml
let mapleader = ","
```

Use *vi* bindings in *tmux*
----------------------------

In *~/.tmux.conf:*

```sh
setw -g mode-keys vi
unbind-key [
bind-key Escape copy-mode
unbind-key p
bind-key p paste-buffer
bind-key -Tcopy-mode-vi v send -X begin-selection
if-shell 'case "`uname`" in *Linux*) true;; *) false;; esac' \
    'bind-key -Tcopy-mode-vi Enter send -X copy-pipe-and-cancel "xclip -selection primary -i -f | xclip -selection clipboard -i"' \
    'bind-key -Tcopy-mode-vi Enter send -X copy-pipe-and-cancel  "reattach-to-user-namespace pbcopy"'
if-shell 'case "`uname`" in *Linux*) true;; *) false;; esac' \
    'bind-key -Tcopy-mode-vi y send -X copy-pipe-and-cancel "xclip -selection primary -i -f | xclip -selection clipboard -i"' \
    'bind-key -Tcopy-mode-vi y send -X copy-pipe-and-cancel  "reattach-to-user-namespace pbcopy"'
```

The above configuration is compatible with *tmux* version 2.4 and above.

When using *Vim* with *tmux*, it makes sense to configure *vi* style bindings in
*tmux*. The above configuration allows for *Vim* style visual selection and
yanking when in *tmux* copy mode.

*tmux* pane splitting
---------------------

In *~/.tmux.conf:*

```sh
unbind-key %
bind-key | split-window -h
unbind-key '"'
bind-key - split-window -v
```

Nice pane creation in *tmux*, use `<prefix>-|` to create a vertical pane and
`<prefix>--` to create a horizontal pane. The direction of the line indicates
the type of pane that will be created.

*Vim* splitting
---------------

In *~/.vimrc:*

```viml
nnoremap <silent> <Leader>s :split<CR>
nnoremap <silent> <Leader>v :vsplit<CR>
nnoremap <silent> <Leader>q :close<CR>
```

Simple split windows in *Vim*: use `<Leader>s` to create a horizontal split,
`<Leader>v` to create a vertical split and `<Leader>q` to close a split.

Seamlessly navigate *(Neo)vim* splits and *tmux* panes
------------------------------------------------------

In *~/.vimrc:*

```viml
nnoremap <A-h> <C-w>h
nnoremap <A-j> <C-w>j
nnoremap <A-k> <C-w>k
nnoremap <A-l> <C-w>l

Plug 'christoomey/vim-tmux-navigator'
if &term == 'screen-256color'
    let g:tmux_navigator_no_mappings = 1
    nnoremap <silent> <A-h> :TmuxNavigateLeft<cr>
    nnoremap <silent> <A-j> :TmuxNavigateDown<cr>
    nnoremap <silent> <A-k> :TmuxNavigateUp<cr>
    nnoremap <silent> <A-l> :TmuxNavigateRight<cr>
endif
```

In *~/.tmux.conf:*

```sh
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
    | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n M-h if-shell "$is_vim" "send-keys C-h"  "select-pane -L"
bind-key -n M-j if-shell "$is_vim" "send-keys C-j"  "select-pane -D"
bind-key -n M-k if-shell "$is_vim" "send-keys C-k"  "select-pane -U"
bind-key -n M-l if-shell "$is_vim" "send-keys C-l"  "select-pane -R"
```

*(Neo)vim* splits divide up a workspace, similarly *tmux* panes divide up a
terminal window. From a usability perspective we want to use the same mappings
to navigate between *splits* and *panes*.

The above configuration provides `Alt-h` (left), `Alt-j` (down), `Alt-k` (up),
and `Alt-l` (right) navigation between *(Neo)vim* splits and *tmux* panes.

Note, **Alt** based mapping may not work with Vim (last time I checked), but
they do work out-of-the-box with Neovim.

The [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
plugin is a key requirement for these mappings to function. Please install with
your preferred *Vim* plugin manager.

Create a *tmux* window
----------------------

In *~/.tmux.conf:*

```sh
bind-key -n M-w new-window
```

`Alt-w` creates a new *tmux* window (aka a new tab).

Create a *Vim* tab page
-----------------------

In *~/.vimrc:*

```viml
nnoremap <silent> <Leader>t :$tabnew<CR>
```

`<Leader>t` creates a new *Vim* tab page.

Navigate between *tmux* windows
-------------------------------

In *~/.tmux.conf:*

```sh
bind-key -n M-1 select-window -t 1
bind-key -n M-2 select-window -t 2
bind-key -n M-3 select-window -t 3
bind-key -n M-4 select-window -t 4
bind-key -n M-5 select-window -t 5
bind-key -n M-6 select-window -t 6
bind-key -n M-7 select-window -t 7
bind-key -n M-8 select-window -t 8
bind-key -n M-9 select-window -t 9
```

*tmux* windows are numbered tabs which are usually navigated to by
`<prefix>-<number>`. However, I find using a `<prefix>` based mapping a bit
cumbersome if I want to quickly flick between windows.

Instead, I prefer to use `Alt-<number>` to quickly get to the window I want.

Navigate between *Vim* tab pages
--------------------------------

In *~/.vimrc:*

```viml
Plug 'gcmt/taboo.vim'
let g:taboo_tab_format = " tab:%N%m "

nnoremap <Leader>1 1gt
nnoremap <Leader>2 2gt
nnoremap <Leader>3 3gt
nnoremap <Leader>4 4gt
nnoremap <Leader>5 5gt
nnoremap <Leader>6 6gt
nnoremap <Leader>7 7gt
nnoremap <Leader>8 8gt
nnoremap <Leader>9 9gt
```

These mapping provide `<Leader>1`/`<Leader>2`/`...` navigation to switch to a
specific numbered tab. I use the [vim-taboo](https://github.com/gcmt/taboo.vim)
plugin to display numbered tabs rather than use *Vim's* default tab naming
convention.

Fold code in Vim
----------------

```viml
set foldmethod=indent
nnoremap <Leader>z za
```

There are multiple choices available when choosing a Vim fold method. I like to
use `indent` since it is simple and performant. `<Leader>z` toggles a
fold based on the indent level of the current cursor line. Think of `z` as being
an accordion that grows and shrinks.

Equalize *Vim* splits
---------------------

```viml
nnoremap <Leader>= <C-w>=
```

The above mapping equalizes the splits of the current *Vim* workspace.

Maximimse a *Vim* split
-----------------------

```viml
nnoremap <silent> <Leader>m :tab split<CR>
```

This handy mapping maximizes a split into its own full tab page. This is
useful when the current workspace has been divided multiple times making edits
to any single split difficult due to a lack of space. In that case simply
*maximize* the split.

Yank and paste helpers
----------------------

```viml
" Paste from the yank register
noremap <Leader>p "0p
noremap <Leader>P "0P
```

The default, unnamed, register is prone to accidentally being overwrittern by
subsequent operations, so when it comes time to do a paste after a yank you may
sometimes find you are not pasting what you want.

However, the `"0` yank register will remain intact (until the next yank
operation).

`<Leader>p` and `<Leader>P` provide quick access to the yank register. If `p`
does not paste what you want, `<Leader>p` and `<Leader>P` are available if you
want to paste from the last yank.

Insert mode completion mappings
-------------------------------

```viml
inoremap <C-]>     <C-x><C-]>
inoremap <C-Space> <C-x><C-o>
inoremap <C-b>     <C-x><C-p>
inoremap <C-d>     <C-x><C-k>
inoremap <C-f>     <C-x><C-f>
inoremap <C-l>     <C-x><C-l>
```

When in insert mode, Vim provides a suite of useful context-aware completions
via the `<Control-x>` prefix. However, I find it difficult and annoying to enter
these completions, the `Control Control` cadence feels too much like Emacs.

Instead, I prefer to use these simpler mappings:

- `Control-]`, complete using the project tags file

- `Control-Space`, language and context aware omni-completion

- `Control-b`, keyword completion from the current **b**uffer; use
  `<Control-n><Control-b>` to extend a completion to multiple consecutive
  keywords

- `Control-d`, **d**ictionary completion, please also set `set
  dictionary=/usr/share/dict/words`

- `Control-f`, **f**ile path completion

- `Control-l`, whole-of-**l**ine completion

Note, these mappings also allow easy switching from one completion kind to
another even when the completion popup is visible.

Readline-style mappings for insert and command modes
----------------------------------------------------

```viml
nnoremap <C-a> 0
nnoremap <C-e> $
inoremap <C-a>  <C-o>0
inoremap <C-e>  <C-o>$
inoremap <A-b>  <C-Left>
inoremap <A-f>  <C-Right>
inoremap <A-BS> <C-w>
inoremap <A-d>  <C-o>dw
cnoremap <C-a>  <Home>
cnoremap <C-e>  <End>
cnoremap <A-b>  <C-Left>
cnoremap <A-f>  <C-Right>
cnoremap <A-BS> <C-w>
cnoremap <A-d>  <C-Right><C-w>
```

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) library
is used by the Bash shell, and certain other utilities, for line-editing and
history management. If one uses the default Readline key bindings, for example
with Bash, then it makes sense to use those same bindings when in certain Vim
modes.

- `Control-a`, go to the beginning of the line.

- `Control-e`, go to the end of the line

- `Alt-b`, go back one word

- `Alt-f`, go forward one word

- `Alt-Backspace`, delete backward one word

- `Alt-d`, delete forward one word

**Note**, the above `Alt`-mappings function correctly with Neovim and GUI-based
versions of Vim (such gVim), but may not work with terminal Vim.
