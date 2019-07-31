---
title: Fuzzy Finding in Vim with fzf
layout: default
comments: true
published: true
---

Fuzzy Finding in Vim with fzf
=============================

Following on from [fuzzy finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html),
this post will discuss usage of the [fzf](https://github.com/junegunn/fzf) tool
within the [Vim](https://www.vim.org) and [Neovim](https://neovim.io) editors.

Please do read [fuzzy finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
to first gain insight about the fzf tool. That post notes how to install,
configure and then use fzf within the [Bash](https://www.gnu.org/software/bash)
shell.

Fuzzy finding is a powerful search technique that can be just as useful inside
an editor as it is at the command-line. Useful fuzzy finding operations within
an editor, such as Vim, may include: opening a project file in a far-away
location by short fuzzy name, switching to an open buffer by fuzzy name or
opening a file by fuzzy
[tag](http://vim.wikia.com/wiki/Browsing_programs_with_tags).

As always, feel free to refer to my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc) files to view my
fzf configurations.

Requirements
------------

For the best user experience, please use a modern version of Vim or Neovim that
provides a built-in terminal. For Vim that means using version 8.1 or later,
and for Neovim that means using version 0.2 or later. GUI-based Vims, such as
gVim and [MacVim](https://github.com/macvim-dev/macvim), especially benefit
since versions pre-terminal integration, when running fzf, would spawn an ugly
external terminal window which was highly unpleasant.

I recommend installing and updating Vim, or Neovim, using
[Homebrew](https://brew.sh/) on macOS and Linux.

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
[fzf.vim](https://github.com/junegunn/fzf.vim) plugin  which smartly wraps the
fzf command-line tool whilst also providing a number of powerful Vim commands.
Note, both fzf and fzf.vim were created and are maintained by [Junegunn
Choi](https://github.com/junegunn), hence, they will always be synchronized if
installed and updated simultaneously.

Just like the command-line tool, fzf.vim provides responsive real-time updates
as fuzzy text is entered.

If using [vim-plug](https://github.com/junegunn/vim-plug), please add the
following to your `~/.vimrc` file:

```viml
Plug 'junegunn/fzf', { 'dir': '~/.fzf', 'do': './install --no-bash' }
Plug 'junegunn/fzf.vim'
```

Then run `:PlugInstall` to install both the fzf command-line tool and
associated Vim plugin.

You may wonder, if one has already installed the fzf command-line tool, why
bother with installing it again via a Vim plugin? Basically to maintain runtime
synchronization between the Vim plugin and command-line tool.

If using a different plugin manager, please adjust the above statements
appropriately.

Usage
-----

With [fzf.vim](https://github.com/junegunn/fzf.vim) installed we can now
focus on how to use fzf within Vim.

Hint, use the ESC key to quickly dismiss a fzf window.

Files command
-------------

The `:Files` command is a fuzzy file finder. Without an argument `:Files` will
recursively search files that match the supplied fuzzy query from the current
directory downwards.

Note, when the fzf command line tool is configured to use the
[fd](https://github.com/sharkdp/fd) tool, Git ignores will be respected, which
can greatly increase fuzzy find performance. Please refer to [fuzzy finding in
Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
for details about how to configure fzf to use fd.

Refine a search by entering in more characters at the prompt. Use the UP and
DOWN keys to move the selection line. Hit the ENTER key to edit a selected file
in the current window, or use TAB to first select multiple files and then hit
ENTER to edit all the selected files. To open the selected file(s) in a new
tab, rather than the current window, use `Control-t` instead of ENTER.
Similarly, use `Control-x` or `Control-v` to open file selections in new
horizontal or vertical splits.

### Example key mapping

Typing `:Files` soon becomes a burden, instead please use a mapping like the
following.

```viml
let mapleader = " "
nnoremap <silent> <Leader><Space> :Files<CR>
```

Hitting SPACE SPACE quickly brings up the fuzzy file finder. An alternative
would be to map `Control-p` to `:Files` if transitioning over from the
[CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin.

### Screenshot

<img width="800" alt="vim_fzf_files" src="https://raw.githubusercontent.com/bluz71/misc-binaries/master/blog/vim_fzf_files.png">

Sibling files
-------------

Sometimes one may need to edit a sibling file of the currently edited file,
that being a file in the same directory with that current file.

This SPACE DOT mapping will open the fuzzy finder just for the directory
containing the currently edited file.

```viml
nnoremap <silent> <Leader>. :Files <C-r>=expand("%:h")<CR>/<CR>
```

Project navigation
------------------

Modern project frameworks, such as [Ruby on Rails](https://rubyonrails.org) and
[Create React App](https://github.com/facebook/create-react-app), are
opinionated in regards to directory structure and project layout.

Those opinionated project layouts, as created by the above listed frameworks
and others, can form the basis for fuzzy project navigation.

In a Rails application, controller files are located in `app/controllers`,
whilst models are located in `app/models` and lastly views are located in
`app/views`. With that in mind the following mappings will provide quick fuzzy
access to controller, model and view files.

```viml
nnoremap <silent> <Leader>ec :Files app/controllers<CR>
nnoremap <silent> <Leader>em :Files app/models<CR>
nnoremap <silent> <Leader>ev :Files app/views<CR>
```

A React application, on the other hand, may be navigated with the following
mappings.

```viml
nnoremap <silent> <Leader>ec :Files src/components<CR>
nnoremap <silent> <Leader>et :Files src/__tests__/components<CR>
```

When dealing with multiple frameworks I recommend scoping such key mappings by
searching for a distinguishing file, that way key mappings can be reused such
as the `<Leader>ec` mapping noted above.

For example:

```viml
if filereadable('config/routes.rb')
  " This looks like a Rails app, add Rails specific mappings here.
elseif filereadable('src/index.js')
  " This looks like a React app, add React specific mappings here.
endif
```

Note, the [vim-rails](https://github.com/tpope/vim-rails) and
[vim-projectionist](https://github.com/tpope/vim-projectionist) plugins can also
be used to navigate projects. All these project navigation techniques are
complimentary, they can happily co-exist.

Buffers command
---------------

The `:Buffers` command is used to quickly switch to an open buffer.

### Example key mapping

```viml
nnoremap <silent> <Leader>b :Buffers<CR>
```

GFiles? command
----------------

The `:GFiles?` command is used to display the status of the current Git
repository whilst also allowing easy navigation to modified files.

### Example key mapping

```viml
nnoremap <silent> <Leader>g :GFiles?
```

Tags command
------------

The `:Tags` and `:BTags` commands are used to navigate a project by fuzzy
[tag](http://vim.wikia.com/wiki/Browsing_programs_with_tags). The `:BTags`
command will limit navigation to the tags associated with the current buffer
whilst `:Tags` will use the complete project tags.

### Example key mapping

```viml
nnoremap <silent> <Leader>]  :Tags<CR>
nnoremap <silent> <Leader>b] :BTags<CR>
```

Commits command
---------------

The `:Commits` and `:BCommits` commands are used to explore a project's
[Git](https://git-scm.com) history. The `:BCommits` command will limit
exploration to the history associated with the current buffer whilst the
`:Commits` command will explore the complete history of the project.

Note, these commands depend on Tim Pope's
[fugitive](https://github.com/tpope/vim-fugitive) plugin. If not already
installed, please add the following to your `~/.vimrc` file, then run
`:PlugInstall` if using [vim-plug](https://github.com/junegunn/vim-plug),
otherwise adjust appropriately if using another plugin manager.

```viml
Plug 'tpope/vim-fugitive'
```

### Example configuration and key mappings

```viml
let g:fzf_commits_log_options = '--graph --color=always
  \ --format="%C(yellow)%h%C(red)%d%C(reset)
  \ - %C(bold green)(%ar)%C(reset) %s %C(blue)<%an>%C(reset)"'

nnoremap <silent> <Leader>c  :Commits<CR>
nnoremap <silent> <Leader>bc :BCommits<CR>
```

The `g:fzf_commits_log_options` option customizes the appearance of Git log
command used by the `:Commits` and `:BCommits` commands.

### Screenshot

<img width="800" alt="vim_fzf_commits" src="https://raw.githubusercontent.com/bluz71/misc-binaries/master/blog/vim_fzf_commits.png">

Pattern search with the Rg command
----------------------------------

[ripgrep](https://github.com/BurntSushi/ripgrep) is an excellent command-line
text search utility that I have previously [posted
about](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html#ripgrep).

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin provides access
to ripgrep with the `:Rg` and `:Rg!` commands. Both commands should be
launched with a search term, for example `:Rg my_search_term`. The bang
version, `:Rg!`, launches a fullscreen window whilst `:Rg` launches the usual
bottom-of-screen split window.

Once a search has been completed, fzf can be used to filter the results to just
those of interest. If only one selection is chosen then that match will be
opened, otherwise if multiple selections are chosen then the first match will
be opened along with the
[quickfix](http://vimdoc.sourceforge.net/htmldoc/quickfix.html) window listing
all matches. Note, use `Alt-a` and `Alt-d` to select and deselect all matches.

### Example key mapping

```viml
nnoremap <Leader>rg :Rg<Space>
nnoremap <Leader>!  :Rg!<Space>
```

The preview window can be toggled with the QUESTION MARK key.

Most Recently Used Files
------------------------

Sometimes one may want to open an out-of-project file that had been previously
edited in Vim. Such a need is best served by an MRU (most-recently-used) cache
that can provide access to a list of recently opened files.

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin unfortunately does
not provide any MRU functionality, however, the
[fzf-mru](https://github.com/pbogut/fzf-mru.vim) plugin does provide a bridge
between fzf and a MRU cache.

If using [vim-plug](https://github.com/junegunn/vim-plug), please add the
following to your `~/.vimrc` file:

```viml
Plug 'pbogut/fzf-mru.vim'
```

Run `:PlugInstall` to install the plugin. If using a different plugin manager,
please adjust the above statement appropriately.

### Example key mapping

```viml
nnoremap <silent> <Leader>m :FZFMru<CR>
```

The SPACE m key combination will launch the fzf window with a list of recently
opened-by-Vim files.

Other commands
--------------

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin also provides the
following useful commands.

- `:Lines` - list lines of all open buffers, then navigate to the selection

- `:BLines` - list lines of the current buffer, then navigate to the selection

- `:Marks` - list all [marks](http://vim.wikia.com/wiki/Using_marks), then
  navigate to the selection

- `:Snippets` - list [UltiSnips](https://github.com/SirVer/ultisnips) snippets,
  then run the selected snippet

- `:Maps` - list all
  [mappings](http://vim.wikia.com/wiki/Mapping_keys_in_Vim_-_Tutorial_(Part_1))

- `:Helptags` - explore Vim's `:help` documentation, then navigate to the
  selected topic

- `:Colors` - list color schemes, then change `colorscheme` to the selection

Why not just use CtrlP
----------------------

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin is the precursor to
most Vim fuzzy finding plugins. It is an excellent plugin which is still
actively maintained.

I used CtrlP for many years. I even posted how to [turbocharge the CtrlP Vim
plugin](https://bluz71.github.io/2017/10/26/turbocharge-the-ctrlp-vim-plugin.html).

So why am I not using CtrlP anymore? Answer, performance.

Even with the turbocharging noted above CtrlP can still slowdown when
navigating huge source trees or when navigating a very large tags file. Neovim
especially, when running the [cpsm](https://github.com/nixprime/cpsm) CtrlP
matcher, can be sluggish due to the cost of Python RPCs
(remote-procedure-calls) as noted in this
[issue](https://github.com/neovim/neovim/issues/7063).

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin is highly performant,
even when dealing with very large source trees or tags files. Pattern matching
is asynchronous and progressive, so it usually feels very fast and rarely
laggy.

I will note, if you are a CtrlP user, and its performance is fine to you, then
there really is no need to change over to fzf.vim unless some of its unique
features, such as `:Commits` or `:Rg`, appeal to you.

Conclusion
----------

I have found fzf-based fuzzy finding, when editing in Vim, to be a productivity
game-changer.

So I encourage you to be curious and to not be afraid to go down the fzf rabbit
hole :rabbit2:
