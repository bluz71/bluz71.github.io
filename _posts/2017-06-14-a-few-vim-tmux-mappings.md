---
title: A few Vim & tmux mappings
layout: default
comments: true
published: true
---

A few Vim & tmux mappings
=========================

Power *Vim* usage involves mappings, usually many of them. This post will list
some of *my* most used mappings. Note, this post is **not** intended to be
about convincing anyone to change *their* mappings, rather the intention is to
simply provide yet another resource of possibilities.

Both *Vim* and *tmux* mappings will somewhat be intermingled in this post since
I often use both as a unified whole.

I will let others explain the benefits of *tmux* and *Vim* together:
[vim + tmux: A Perfect Match](https://teamgaslight.com/blog/vim-plus-tmux-a-perfect-match),
and [Benefits of using tmux](https://blog.bugsnag.com/benefits-of-using-tmux)

The mappings discussed in this post are backed into my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc) and
[tmux.conf](https://github.com/bluz71/dotfiles/blob/master/tmux.conf) files.

*tmux* prefix
-------------

*tmux* operations are usually invoked with a `<prefix>` plus `<command>` key
combination.

The default *tmux* prefix is `Ctrl-b`. That is an awkward combination to type
with one hand, hence many folk change it to `Ctrl-a`. In my case I find
`Ctrl-w` even nicer to type with just my left hand.

In *~/.tmux.conf:*

```sh
unbind-key C-b
set -g prefix C-w
```

*Vim* leader
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

*Vim* localleader
-----------------

The *Vim* local leader is intended to be similar to the leader key except
specific to local buffers. However, in reality *localleader* can be used just
like *leader*. I tend to favor *leader* for primary mappings (common), whilst I
use *localleader* for secondary mappings (less common).

I like space as my *localleader* key, in *~/.vimrc:*

```viml
let maplocalleader = " "
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

When using *Vim* with *tmux* it makes sense to configure *vi* style bindings in
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
nnoremap <silent> <leader>s :split<CR>
nnoremap <silent> <leader>v :vsplit<CR>
nnoremap <silent> <leader>q :close<CR>
```

Simple split windows in *Vim*: use `<leader>s` to create a horizontal split,
`<leader>v` to create a vertical split and `<leader>q` to close a split.

Seamlessly navigate *Vim* splits and *tmux* panes
-------------------------------------------------

In *~/.vimrc:*

```viml
nnoremap <C-h> <C-w>h
nnoremap <C-j> <C-w>j
nnoremap <C-k> <C-w>k
nnoremap <C-l> <C-w>l

Plug 'christoomey/vim-tmux-navigator'
if &term == 'screen-256color'
    let g:tmux_navigator_no_mappings = 1
    nnoremap <silent> <C-h> :TmuxNavigateLeft<cr>
    nnoremap <silent> <C-j> :TmuxNavigateDown<cr>
    nnoremap <silent> <C-k> :TmuxNavigateUp<cr>
    nnoremap <silent> <C-l> :TmuxNavigateRight<cr>
endif
```

In *~/.tmux.conf:*

```sh
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
    | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n C-h if-shell "$is_vim" "send-keys C-h"  "select-pane -L"
bind-key -n C-j if-shell "$is_vim" "send-keys C-j"  "select-pane -D"
bind-key -n C-k if-shell "$is_vim" "send-keys C-k"  "select-pane -U"
bind-key -n C-l if-shell "$is_vim" "send-keys C-l"  "select-pane -R"
```

*Vim* splits divide up a workspace, similarly *tmux* panes divide up a terminal
window. From a usability perspective we want to use the same mappings to
navigate between *splits* and *panes*.

The above configuration provides `Ctrl-h` (left), `Ctrl-j` (down), `Ctrl-k`
(up), and `Ctrl-l` (right) navigation between *Vim* splits and *tmux* panes.

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
nnoremap <silent> <leader>t :$tabnew<CR>
```

`<leader>t` creates a new *Vim* tab page.

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

Instead I prefer to use `Alt-<number>` to quickly get to the window I want.

Navigate between *Vim* tab pages
--------------------------------

In *~/.vimrc:*

```viml
Plug 'gcmt/taboo.vim'
let g:taboo_tab_format = " tab:%N%m "

nnoremap <leader>1 1gt
nnoremap <leader>2 2gt
nnoremap <leader>3 3gt
nnoremap <leader>4 4gt
nnoremap <leader>5 5gt
```

These mapping provide `<leader>1`/`<leader>2`/`...` navigation to switch to a
specific numbered tab. I use the [vim-taboo](https://github.com/gcmt/taboo.vim)
plugin to display numbered tabs rather than use *Vim's* default tab naming
convention.

Enter *Vim* command mode
------------------------

Command mode in *Vim* is entered by typing `:`, this necessarily involves
holding the **Shift** key. A simple and handy shortcut is to map `;` to `:` as
follows to avoid the **Shift** key altogether:

```viml
noremap ; :
```

This will mean losing the `;` repeat for the last `f`, `t`, `F` or `T` command.
In my case that is a non-issue since I use the
[clever-f](https://github.com/rhysd/clever-f.vim) plugin which provides `f` and
`F` as repeat operators among other benefits.

Center next *Vim* search matches
--------------------------------

```viml
noremap n nzz
noremap N Nzz
```

Navigating search results is something often done. These mappings simply center
the `n` (next) or `N` (previous) match. Much less of a need to search for the
cursor on the screen; the next match, more often than not, will be in the
center of the screen.

*Y* should behave like *D* and *C*
----------------------------------

```viml
noremap Y y$
```

Vim by default provides `D` to delete till the end of line and `C` to
change till the end of line. For some reason it does not provide yank till the
end of line. Enable this mapping to set `Y` to do that particular form of
yanking.

Fold code in Vim
----------------

```viml
set foldmethod=indent
nnoremap <leader>z za
```

There are multiple choices available when choosing a Vim fold method. I like to
use `indent` since it is simple and performant. `<leader><Space>` toggles a
fold based on the indent level of the current cursor line.

Equalize *Vim* splits
---------------------

```viml
nnoremap <leader>= <C-w>=
```

The above mapping equalizes the splits of the current *Vim* workspace.

Zoom a *Vim* split
------------------

```viml
nnoremap <silent> <leader>Z :tab split<CR>
```

This handy mapping zooms a split into its own full tab page. This is
useful when the current workspace has been divided multiple times making edits
to any single split difficult due to a lack of space. In that case simply
*zoom* the split.

Yank, paste and delete helpers
------------------------------

```viml
noremap <leader>y "yy
noremap <leader>p "yp
noremap <leader>P "yP
noremap <leader>x "_x
noremap <leader>d "_d
```

Yank and paste into and out-of the user `y`-register. Normal yanks and pastes
are prone to being accidentally overwritten by subsequent operations. Use these
commands if you wish to persist a yank operation.

The `<leader>x` and `<leader>d` commands will delete into the black-hole
register, hence will not overwrite the unnamed register.

Insert mode completion mappings
-------------------------------

```viml
inoremap <C-]>     <C-x><C-]>
inoremap <C-Space> <C-x><C-o>
inoremap <C-d>     <C-x><C-k>
inoremap <C-f>     <C-x><C-f>
inoremap <C-l>     <C-x><C-l>
```

When in insert mode Vim provides a suite of useful context-aware completions via
the `<Control-x>` prefix. However, I find it difficult and annoying to enter
these completions, the `Control Control` cadence feels too much like Emacs.

Instead I prefer to use these simpler mappings:

- `Control-]`, complete using the project tags file

- `Control-Space`, context and programming language aware omni-completion

- `Control-d`, dictionary completion, please also set `set
    dictionary=/usr/share/dict/words`

- `Control-f`, file path completion

- `Control-l`, whole-of-line completion
