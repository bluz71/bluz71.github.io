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
to gain insight about the fzf tool, how to install, configure and then use it
within the [Bash](https://www.gnu.org/software/bash) command-line.

Fuzzy finding is a powerful search technique that can be just as useful inside
an editor as it is at the command-line. Useful fuzzy finding operations within
an editor, such as Vim, may include: opening a project file in a far-away
location by short fuzzy name, switching to an open file by fuzzy name or
opening a file by fuzzy
[tag](http://vim.wikia.com/wiki/Browsing_programs_with_tags).

Requirements
------------

For the best user experience, please use a modern versions of Vim or Neovim
that provides a built-in terminal. For Vim that means using version 8.1 or
later, and for Neovim that means using version 0.2 or later. GUI-based Vim's,
such as gVim and MacVim, especially benefit since prior to the integration of a
terminal fzf-within-Vim would spawn an ugly external terminal window which was
highly unpleasant.

I recommend installing and updating Vim, or Neovim, using
[Homebrew](https://brew.sh/) on macOS and [Linuxbrew](http://linuxbrew.sh) on
Linux.

For example, to install Neovim using Brew:

```sh
brew install neovim
```

And to update Neovim:

```sh
brew upgrade neovim
```

Plugin
------

The [fzf](https://github.com/junegunn/fzf) tool provides basic [Vim
integration](https://github.com/junegunn/fzf/blob/master/README-VIM.md) out of
the box.

However, I strongly recommend installing and using the
[fzf.vim](https://github.com/junegunn/fzf.vim) plugin instead. Note, both
projects were created and are maintained by [Junegunn
Choi](https://github.com/junegunn), hence, they will always be synchronized if
installed and updated simultaneously.

If using [vim-plug](https://github.com/junegunn/vim-plug), please add the
following to your `~/.vimrc` file:

```viml
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --no-bash' }
Plug 'junegunn/fzf.vim'
```

Then run `:PlugInstall` to install the fzf command-line tool and associated Vim
plugin. Note, the Vim plugin uses the fzf command line tool, hence the two
plugin lines.

If using a different plugin manager please adjust the above statements
appropriately.

Usage
-----

With [fzf.vim](https://github.com/junegunn/fzf.vim) installed we can now
focus on how to use fzf within Vim.

Note, use the ESC key to quickly dismiss an fzf window.

Files command
-------------

The `:Files` command is a fuzzy file finder. Without an argument `:Files` will
recursively search files that match the supplied fuzzy query, from the current
directory downwards.

Note, when the fzf command line tool is configured to use the
[fd](https://github.com/sharkdp/fd) tool, Git ignores will be respected, which
can greatly increase fuzzy find performance. Please refer to [Fuzzy Finding in
Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
for details about how to configure fzf to use fd.

Refine a search by entering in more characters in the query, use the UP and
DOWN keys to move the selection. Hit the ENTER key to edit a selected file in
the current window, or use TAB to first select multiple files and then hit
ENTER to edit all the selected files.

To open the selected file(s) in a new tab, rather than the current window, use
`Control-t` instead of ENTER. Similarly use `Control-x` or `Control-v` to open
file selections in new horizontal or vertical splits.

### Example key mapping

Typing `:Files` soon becomes a burden, instead I use the following mapping:

```viml
let maplocalleader = " "
nnoremap <silent> <localleader><Space> :Files<CR>
```

Hitting SPACE SPACE quickly brings up the fuzzy file finder. An alternative
would be to map `Control-p` to `:Files` if transitioning over from the
[CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin.

Sibling files
-------------

Sometimes one may need to edit a sibling file of the currently edited file,
that being a file in the same directory with the current file.

The following SPACE DASH mapping will open the fuzzy finder just in the
directory containing the currently edited file.

```viml
nnoremap <silent> <localleader>- :Files <C-r>=expand("%:h")<CR>/<CR>
```

DASH was chosen due to my experience with Tim Pope's
[Vinegar](https://github.com/tpope/vim-vinegar) plugin.

Project navigation
------------------

Modern project frameworks, such as [Ruby on Rails](https://rubyonrails.org) and
[Create React App](https://github.com/facebook/create-react-app), are
opinionated with regards to directory structure and project layout.

Those opinionated project layouts, as created by the above listed frameworks
and others, can form the basis for fuzzy project navigation.

In a Rails application, controller files are located in `app/controllers`,
whilst models are located in `app/models` and lastly views are located in
`app/views`. With that in mind the following mappings will provide quick fuzzy
access to controller, model, view and spec files.

```viml
noremap <silent> <localleader>ec :Files app/controllers<CR>
noremap <silent> <localleader>em :Files app/models<CR>
noremap <silent> <localleader>ev :Files app/views<CR>
```

A React application on the other hand may be navigated with the following
mappings.

```viml
noremap <silent> <localleader>ec :Files src/components<CR>
noremap <silent> <localleader>et :Files src/__tests__/components<CR>
```

When dealing with multiple frameworks I recommend scoping key mappings by
searching for a distinguishing file, that way key mappings can be reused such
as the `<localleader>ec` mapping noted above.

For example:

```viml
if filereadable('config/routes.rb')
  " This looks like a Rails app, add Rails specific mapping here.
elseif filereadable('src/index.js')
  " This looks like a React app, add React specific mappings here.
endif
```

Note, the [vim-rails](https://github.com/tpope/vim-rails) and
[vim-projectionist](https://github.com/tpope/vim-projectionist) plugins can
also be used be used to navigate projects. All these project navigation
techniques are complimentary, and they can happily co-exist.

Buffers command
---------------

The `:Buffers` command is used to quickly switch to an open buffer.

### Example key mapping

```viml
nnoremap <silent> <localleader>, :Buffers<CR>
```

Tags command
------------

The `:Tags` and `:BTags` commands are used to navigate a project by fuzzy
[tag](http://vim.wikia.com/wiki/Browsing_programs_with_tags). The `:BTags`
command will limit navigation to the tags associated with the current buffer
whilst `:Tags` will use the complete project tags.

### Example key mapping

```viml
nnoremap <silent> <localleader>]  :Tags<CR>
nnoremap <silent> <localleader>b] :BTags<CR>
```

Commits command
---------------

The `:Commits` and `:BCommits` commands are used to explore a project's
[Git](https://git-scm.com) history. The `:BCommits` command will limit
exploration to the history associated with the current buffer whilst the
`:Commits` command will explore the complete history of the project.

### Example configuration and key mappings

```viml
let g:fzf_commits_log_options = '--graph --color=always
  \ --format="%C(yellow)%h%C(red)%d%C(reset)
  \ - %C(bold green)(%ar)%C(reset) %s %C(blue)<%an>%C(reset)"'

nnoremap <silent> <localleader>c  :Commits<CR>
nnoremap <silent> <localleader>bc :BCommits<CR>
```

The `g:fzf_commits_log_options` option customizes the appearance of Git log
command used by the `:Commits` and `:Bcommits` commands.

Pattern search with Rg command
------------------------------

[ripgrep](https://github.com/BurntSushi/ripgrep) is an excellent command-line
text search utility that I have previously [posted
about](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html#ripgrep).

The [fzf.vim](https://github.com/junegunn/fzf.vim) utility provides integration
with ripgrep with the `:Rg` and `:Rg!` commands. Both commands should be
launched with an search term, for example `:Rg my_search_term`. The bang
version, `:Rg!`, launches a fullscreen window whilst `:Rg` launches the usual
bottom-of-screen split window.

Once a search has been completed, fzf can be used to filter the results to just
those of interest. If only one selection is chosen then that match will be
opened, otherwise if multiple selections are chosen the first match will be
opened along with the
[quickfix](http://vimdoc.sourceforge.net/htmldoc/quickfix.html) window listing
all matches. Note, use `Alt-a` and `Alt-d` to select and deselect all matches.

### Example configuration and key mapping

```viml
command! -bang -nargs=* Rg
  \ call fzf#vim#grep(
  \   'rg --column --line-number --no-heading --color=always --smart-case '.shellescape(<q-args>), 1,
  \   fzf#vim#with_preview('right:50%', '?'),
  \   <bang>0)

nnoremap <localleader>rg :Rg<Space>
nnoremap <localleader>! :Rg!<Space>
```

The preview window can be hidden with the typing the question mark key.

Most Recently Used Files
------------------------

Sometimes one may want to open an out-of-project file that had been previously
edited in Vim. Such a need is best served by an MRU (most-recently-used) that
provides access to a cached list of recently opened files.

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin unfortunately does
not provide any MRU functionality, however, the
[fzf-mru](https://github.com/pbogut/fzf-mru.vim) does provide a bridge between
FZF and an MRU.

If using [vim-plug](https://github.com/junegunn/vim-plug), please add the
following to your `~/.vimrc` file:

```viml
Plug 'pbogut/fzf-mru.vim'
```

Run `:PlugInstall` to install the plugin. If using a different plugin manager
please adjust the above statement appropriately.

### Example key mapping

```viml
nnoremap <silent> <localleader>m :FZFMru<CR>
```

The SPACE-m key combination will launch the fzf window with a list of recently
opened-by-Vim files. Simply search, then select the file of interest for
editing.

Other commands
--------------

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin also provides the
following notable commands.

- `:Colors` - list color schemes, then change `colorscheme` to the selection

- `:Lines` - list lines of all open buffer, then navigate to selection

- `:Blines` - list lines of current buffer, then navigate to selection

- `:Marks` - list all marks, then navigate to selection

- `:Maps` - list all mappings

- `:Helptags` - explore Vim documentation, then navigate to the selected topic

- `:Snippets` - list [UltiSnips](https://github.com/SirVer/ultisnips) snippets,
  then run the selected snippet

Why not just use CtrlP
----------------------

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin is the precursor to
most Vim fuzzing finding plugins. It is an excellent plugin which is still
actively maintained.

I happily used CtrlP for many years. I even posted how to [turbocharge the
CtrlP Vim
plugin](https://bluz71.github.io/2017/10/26/turbocharge-the-ctrlp-vim-plugin.html).

So why am I not using CtrlP anymore? Answer, performance.

Even with the turbocharging noted above CtrlP can still slowdown when
navigating huge source trees or when navigating a very large tag file. Neovim
especially, when running the [cpsm](https://github.com/nixprime/cpsm) CtrlP
matcher, can be noticably slowdown due to the cost of Python RPC
(remote-procedure-calls) as noted in this
[issue](https://github.com/neovim/neovim/issues/7063).

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin is highly performant,
even when dealing with very large source trees or tag files. Pattern matching
is asynchronous and progressive, so it always feels fast and never laggy.

I will note, if you are a CtrlP user, and its performance is fine to you, then
there really is no need to change over fzf.vim unless some of its unique
features, such as `:Commits` or `:Rg`, appeal to you.

Conclusion
----------

Fuzzing finding, when editing, is a productivity game-changer, no matter which
underlying fuzzy finding technology is used.

Please give fzf.vim, or CtrlP, a try, I am sure whichever plugin you use will
prove itself extremely useful :thumbsup:
