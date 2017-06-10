---
title: A few Vim & tmux mappings
layout: default
comments: true
published: false
---

A few Vim & tmux mappings
=========================
Power *Vim* usage involves configuring mappings, oftimes many of them. This
post will list some of *my* most used mappings and why I find them useful. This
post is **not** be about convincing anyone to change *their* mappings, rather
the intention is to simply provide yet another resouce of possibilities.

Note, both *Vim* and *tmux* mappings will be intermingled in this post since I
use both as a unified whole.

I will let others explain the benefits of *tmux* and *Vim* together:
[vim + tmux: A Perfect Match](https://teamgaslight.com/blog/vim-plus-tmux-a-perfect-match),
and [Benefits of using tmux](https://blog.bugsnag.com/benefits-of-using-tmux)

Note, the following mappings are backed into my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc) and
[tmux.conf](https://github.com/bluz71/dotfiles/blob/master/tmux.conf) files.

*tmux* prefix
-------------
*tmux* operations are usually invoked with a `<prefix>` plus `<command>` key
combination.

The default *tmux* prefix is `Ctrl-b`. That is an awkward combination to type
with one hand, hence many folks change it to `Ctrl-a`. In my case I find
`Ctrl-w` even nicer to type with just my left hand.

In *~/.tmux.conf:*
```sh
unbind-key C-b
set -g prefix C-w
```

*Vim* leader
------------
The *Vim* leader is a user definable key that can be used as a prefix for
custom mappings. Think of it as being similar to `Alt` or `Ctrl`,  but in
addition to those modifiers.

The default Vim leader key is backslash which, much like the *tmux* default
prefix, is an awkward key. The two most common replacements are `Space` and
`,`.

I like comma, in *~/.vimrc:*
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

When using *Vim* with *tmux* it makes sense to configure *vi* style bindings.
The above configuration allows for *Vim* style visual selection and yanking
when in *tmux* copy mode.


Nicer *tmux* pane splitting
---------------------------
In *~/.tmux.conf:*
```sh
unbind-key %
bind-key | split-window -h
unbind-key '"'
bind-key - split-window -v
```

Nice pane creation in *tmux*, use `<prefix>-|` to create a vertical pane and
`<prefix>--` to create a horizontal plane. The direction of the line indicates
the type of pane that will be created.

Seamlessly navigate *Vim* splits and *tmux* panes
-------------------------------------------------
In *~/.vimrc:*
```viml
noremap <C-h> <C-w>h
noremap <C-j> <C-w>j
noremap <C-k> <C-w>k
noremap <C-l> <C-w>l

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

*Vim* splits divide up a workspace, and similarly *tmux* panes divide up a
terminal window. From a usability perspective we want to use the same mappings
to navigate between *splits* and *panes*.

The above configuration provides `Ctrl-h` (left), `Ctrl-j` (down), `Ctrl-k`
(up), and `Ctrl-l` (right) seamless navigation between *Vim* splits *tmux*
panes.

The [vim-tmux-navigator](https://github.com/christoomey/vim-tmux-navigator)
plugin is a key requirement for these mappings to function. Please install with
your preferred *Vim* plugin manager.

Create a *tmux* window
----------------------
In *~/.tmux.conf:*
```sh
bind-key -n M-w new-window
```
`Alt-w` creates a new *tmux* window.

Create a *Vim* tab page
-----------------------
In *~/.vimrc:*
```viml
noremap <silent> <A-t> :$tabnew<CR>
```

`Alt-t` creates a new *Vim* tab page.

Note, the above mapping works with GUI Vims, like *MacVim* and *gVim*, and also
*Neovim*, but will not work with terminal *Vim*. If using terminal *Vim* please
replace `<A-t>` with the actual key-sequence which involves entering insert
mode, then typing `Ctrl-v` followed by `Alt-t`. One more reason to use Neovim,
simpler mappings.

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

*tmux* windows are the effectively numbered tabs which are usually navigated by
`<prefix>-<number>`. However I find using a `<prefix>` based mapping a bit
cumbersome if I want to quickly flick between windows.

Instead I prefer to use `<Alt>-<number>` to quickly get to the window I want.

Navigate between *Vim* tab pages
--------------------------------
In *~/.vimrc:*
```viml
Plug 'gcmt/taboo.vim'
let g:taboo_tab_format = " tab:%N%m "

noremap <A-n> gt
noremap <A-p> gT
noremap <A-!> 1gt
noremap <A-@> 2gt
noremap <A-#> 3gt
noremap <A-$> 4gt
noremap <A-%> 5gt
```

This provides `Alt-n`/`Alt-p` to navigate the next and previous tabs and
`Alt-Shift-1`/`Alt-Shift-2`/`...` to switch to a specific numbered tab. I use
the [vim-taboo](https://github.com/gcmt/taboo.vim) plugin to obtain numbered
tabs rather than *Vim's* default tab naming convention.

*Vim* splits
------------
```sh
noremap <leader>s :split<CR>
noremap <leader>v :vsplit<CR>
noremap <leader>q :close<CR>
```

Vim splits are central to the editing experience, having simple mappings to
create and close splits is a must.

Enter *Vim* command mode
------------------------
Command mode in *Vim* is entered by typing `:`, this necessarily involves
holding the **shift** key. A simple and handy shortcut is to map `;` to `:` as
follows to avoid the **shift** key altogether:

```sh
noremap ; :
```

This will mean losing the `;` repeat of the last `f`, `t`, `F` or `T` command.
In my case that is a non-issue since I use the
[clever-f](https://github.com/rhysd/clever-f.vim) plugin which provides `f` and
`F` as repeat operators among other benefits.

Center next *Vim* search match
------------------------------
```sh
noremap n nzz
noremap N Nzz
```

Navigating search results is something often done. These mappings simply center
the `n` (next) or `N` (previous) match. Much less of a need to search for the
cursor on the screen; the next match, more often than not, will be in the
center of the screen.

Navigate *quickfix* list and center matches
-------------------------------------------
```sh
noremap <silent> <A-Up> :cp<CR>zz
noremap <silent> <A-Down> :cn<CR>zz
```

The [vim-grepper](https://github.com/mhinz/vim-grepper) plugin, with the
[ripgrep](https://github.com/BurntSushi/ripgrep) search utility, is a favourite
of mine when I want to do project wide searching. *vim-grepper* will populate
the *quickfix* list. The above mappings, `Alt-Up` and `Alt-Down`, navigate the
next and previous search matches whilst also centering the match.

Fold code in Vim
----------------
```viml
set foldmethod=indent
nnoremap <leader><Space> za
```

There are a few choices available when choosing a Vim fold method. I just to
use `indent` since it is simple and performant. `<leader><Space>` simply
toggles the desired fold.


Quit *Vim* and confirm saves
----------------------------
```viml
noremap <C-q> :confirm qall<CR>
```

Use `Ctrl-q` to exit *Vim* with confirmation.

Replay a *Vim* macro
--------------------
```viml
nnoremap Q @q
xnoremap Q :'<,'>:normal @q<CR>
```

Simply record a macro using `qq` and replay that macro using `Q`.

Equalize *Vim* splits
---------------------
```viml
noremap <leader>= <C-w>=
```

The above mapping equalizes the splits of the current *Vim* workspace.

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

These mappings use the [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin to
navigate the Model/View/Controller (MVC) code of a
[Ruby on Rails](http://rubyonrails.org/) or
[Elixir Phoenix](http://www.phoenixframework.org/) application using mostly
the same mappings. The above mappings can also easily be extended for other
frameworks.


Zoom a *Vim* split
------------------
```viml
noremap <silent> <leader>z :tab split<CR>
```

This mapping is handy, it zooms a split into its own full tab page. This is
useful when the current workspace has been divided multiple times making edits
to any single split difficult due to lack of space. In that case simply *zoom*
that split.
