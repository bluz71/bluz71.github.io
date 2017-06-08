---
title: A few Vim & tmux mappings
layout: default
comments: true
published: false
---

A few Vim & tmux mappings
=========================
If one is a power Vim user then one will indeed have many custom mappings
configured. Mappings are usually crafted over a long time and often become
ingrained in finger memory. With that in mind, this post is not written with
the intent of trying to convince anyone to change their mappings, but rather to
provide yet another resouce for possible mappings.

Note, both *Vim* and *tmux* mappings will be intermingled in this post so I use
both as a unified whole. As for why tmux? I will let others explain the
benefits:
[vim + tmux: A Perfect Match](https://teamgaslight.com/blog/vim-plus-tmux-a-perfect-match),
and [Benefits of using tmux](https://blog.bugsnag.com/benefits-of-using-tmux)

Note, the following mappings are back in my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc) and
[tmux.conf](https://github.com/bluz71/dotfiles/blob/master/tmux.conf) files.

*tmux* prefix
-------------
*tmux* operations are usually invoked using a `<prefix>` plus `<command>` key
combination.

The default *tmux* prefix is `<Ctrl-b>`. That is an awkward combination to type
with one hand, hence many folks change it to `<Ctrl-a>`. In my case I find
`<Ctrl-w>` even nicer to type with my left hand.

In *~/.tmux.conf*:
```conf
unbind-key C-b
set -g prefix C-w
```

*Vim* leader
------------
The *Vim* leader is a user definable prefix key that can be used a prefix for
custom mappings. Think of it as being similar to `<Alt>` or `<Ctrl>` based
mappings.

The default Vim leader key is `\``, much like the *tmux* default prefix
key, is an awkward key. The two most common choices replacements are `<Space>`
and `,`.

I like comma, in *~/.vimrc*:
```viml
let mapleader = ","
```

Use *Vim* bindings in *tmux*
----------------------------
```conf
setw -g mode-keys vi

unbind-key [
bind-key Escape copy-mode
unbind-key p
bind-key p paste-buffer
bind-key -Tcopy-mode-vi v send -X begin-selection
if-shell 'case "$OS" in *Linux*) true;; *) false;; esac' \
    'bind-key -Tcopy-mode-vi Enter send -X copy-pipe-and-cancel "xclip -selection primary -i -f | xclip -selection clipboard -i"' \
    'bind-key -Tcopy-mode-vi Enter send -X copy-pipe-and-cancel  "reattach-to-user-namespace pbcopy"'
if-shell 'case "$OS" in *Linux*) true;; *) false;; esac' \
    'bind-key -Tcopy-mode-vi y send -X copy-pipe-and-cancel "xclip -selection primary -i -f | xclip -selection clipboard -i"' \
    'bind-key -Tcopy-mode-vi y send -X copy-pipe-and-cancel  "reattach-to-user-namespace pbcopy"'
```

Nicer *tmux* pane splitting
---------------------------
```conf
unbind-key %
bind-key | split-window -h
unbind-key '"'
bind-key - split-window -v
```

Seamlessly navigate *Vim* splits and *tmux* panes
-------------------------------------------------
```viml
noremap <C-h> <C-w>h
noremap <C-j> <C-w>j
noremap <C-k> <C-w>k
noremap <C-l> <C-w>l

Plug 'christoomey/vim-tmux-navigator'
if &term == 'screen-256color'
    " Seamless CTRL-h/j/k/l navigation between Vim splits  and tmux panes.
    " Note, only set up mappings if running inside tmux.
    let g:tmux_navigator_no_mappings = 1
    nnoremap <silent> <C-h> :TmuxNavigateLeft<cr>
    nnoremap <silent> <C-j> :TmuxNavigateDown<cr>
    nnoremap <silent> <C-k> :TmuxNavigateUp<cr>
    nnoremap <silent> <C-l> :TmuxNavigateRight<cr>
endif
```

```conf
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
    | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n C-h if-shell "$is_vim" "send-keys C-h"  "select-pane -L"
bind-key -n C-j if-shell "$is_vim" "send-keys C-j"  "select-pane -D"
bind-key -n C-k if-shell "$is_vim" "send-keys C-k"  "select-pane -U"
bind-key -n C-l if-shell "$is_vim" "send-keys C-l"  "select-pane -R"
```

Create a *tmux* window
----------------------
```conf
bind-key -n M-w new-window
```

Create a *Vim* tab page
-----------------------
```viml
noremap <silent> <A-t> :$tabnew<CR>
```

Navigate between *tmux* windows
-------------------------------
*tmux* windows are the effectively numbered tabs which are usually navigated by
`<prefix>-<number>`. However I find using a `<prefix>` based mapping a bit
cumbersome if I want to quickly flick between windows.

Instead I prefer to use `<Alt>-<number>`:

```conf
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

Navigate between *Vim* tab pages
--------------------------------
```viml
noremap <A-n> gt
noremap <A-p> gT
```

*Vim* splits
------------
```conf
noremap <leader>s :split<CR>
noremap <leader>v :vsplit<CR>
noremap <leader>q :close<CR>
```

Duplicate semicolon to colon
----------------------------
Command mode in *Vim* is entered by typing `:`, this necessarily involves
holding the **shift** key. A simple and handy shortcut is to map `;` to `:` as
follows:

```viml
noremap ; :
```

This will mean losing the `;` repeat of the last `f`, `t`, `F` or `T` commands.
In my case that is not an issue since I use the
[clever-f](https://github.com/rhysd/clever-f.vim) plugin which provides `f` and
`F` as repeat operators among other benefits.

Center next *Vim* search match
------------------------------
```conf
noremap n nzz
noremap N Nzz
```

Navigate *quickfix* list and center matches
-------------------------------------------
```viml
noremap <silent> <A-Up> :cp<CR>zz
noremap <silent> <A-Down> :cn<CR>zz
```

Quit Vim and confirm saves
--------------------------
```viml
noremap <C-q> :confirm qall<CR>
```

Replay a macro
--------------

```viml
nnoremap Q @q
xnoremap Q :'<,'>:normal @q<CR>
```

Simply record a macro using `qq` and replay that macro using `Q`.

Equalize splits
---------------
```viml
noremap <leader>= <C-w>=
```

Navigate MVC web framework code
-------------------------------
```viml
if filereadable('config/environment.rb') && isdirectory('app')
    " This looks like a Rails layout.
    nnoremap <leader>ec :CtrlP app/controllers<CR>
    nnoremap <leader>eh :CtrlP app/helpers<CR>
    nnoremap <leader>em :CtrlP app/models<CR>
    nnoremap <leader>ev :CtrlP app/views<CR>
elseif filereadable('config/config.exs') && isdirectory('web')
    " This looks like an Elixir Phoenix layout.
    nnoremap <leader>ec :CtrlP web/controllers<CR>
    nnoremap <leader>em :CtrlP web/models<CR>
    nnoremap <leader>et :CtrlP web/templates<CR>
    nnoremap <leader>ev :CtrlP web/views<CR>
endif
```

Zoom a *Vim* split
------------------
```viml
noremap <silent> <leader>z :tab split<CR>
```
