---
title: My Favourite Vim Plugins
layout: default
comments: true
---

My Favourite Vim Plugins
========================

Vim gains much functionality through the inclusion of *plugins*. This post
contains a curated set of my favourite Vim *plugins*.

Note, the full set of plugins and mappings I use are available in my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc).

vim-plug
--------
The first decision a Vim user, looking to enter the plugin world, must decide
is which plugin manager to use. There are many to choose from:
[Vundle](https://github.com/VundleVim/Vundle.vim),
[Pathogen](https://github.com/tpope/vim-pathogen),
[vim-plug](https://github.com/junegunn/vim-plug), and
[Dein](https://github.com/Shougo/dein.vim) to name but a few.

Each will do the job, but for *simplicity* and *performance* the
[vim-plug](https://github.com/junegunn/vim-plug) plugin manager is hard to
beat. It is was I recommend.

Note, if you are a *Vundle* user, like I was, transfering over to *vim-plug* is
simple, just follow
[this](https://github.com/junegunn/vim-plug/wiki/faq#migrating-from-other-plugin-managers)
advice.

All snippets for the remainder of this post will use *vim-plug* notation.

vim-moonfly-colors
------------------
```viml
Plug 'bluz71/vim-moonfly-colors'
```

Some self advertising, I have written my own Vim **colorscheme** named
[moonfly](https://github.com/bluz71/vim-moonfly-colors).

The *moonfly* colorscheme is yet another dark theme, but unlike all other dark
themes this one is **my** dark theme. That means it has been tuned to my
particular tastes.  Whether those tastes match up with other people's tastes
will be in the eye of the beholder.

vim-moonfly-statusline
----------------------
```viml
Plug 'bluz71/vim-moonfly-statusline'
```

Personally I am not a fan of heavy *statusline* plugins like
[powerline](https://github.com/powerline/powerline) and
[airline](https://github.com/vim-airline/vim-airline).

Instead I have created a simple matching *statusline* for the *moonfly*
colorscheme named appropriately
[moonfly-statusline](https://github.com/bluz71/vim-moonfly-statusline). It
provides all relevant information I find useful whilst also clearly indicating
which mode you are in: normal, insert, replace or visual modes.

visual-star-search
------------------
```viml
Plug 'nelstrom/vim-visual-star-search'
```

The
[vim-visual-star-search](https://github.com/nelstrom/vim-visual-star-search)
plugin allows __*__ and **#** searches to occur on the current visual selection.

indentLine
----------

```viml
Plug 'Yggdroot/indentLine'
let g:indentLine_faster = 1
let g:indentLine_setConceal = 0
```

The [indentLine](https://github.com/Yggdroot/indentLine) plugin is used to
display indentation guide markers. This is a simple, but extremely useful,
visual aid. In my opinion it is a feature that Vim itself should provide but
does not.

The two *let* options documented **should** be set for maximum performance.
The default settings for the *indentLine* plugin **will** have a negative
impact on Vim scroll performance.

supertab
--------
```viml
Plug 'ervandew/supertab'
let g:SuperTabDefaultCompletionType = "context"
let g:SuperTabContextDefaultCompletionType = "<c-n>"
```

I consider the [supertab](https://github.com/ervandew/supertab) plugin to be an
essential plugin. This plugin allows one to simply use the **TAB** character to
carry out completion whilst in insert mode. The plugin itself determines the
appropriate type of completion, be it *text*, *omni* or *file* completion.

clever-f
--------
```viml
Plug 'rhysd/clever-f.vim'
let g:clever_f_across_no_line = 1
let g:clever_f_timeout_ms = 3000
```

The [clever-f](https://github.com/rhysd/clever-f.vim) plugin makes **f**,
**F**, **t** and **T** movements more informative and convenient.

The more informative part is achieved by *clever-f* highlighting all the
matches for the movement.

The more convenient part is achieved by simly using the **f** and **F**
characters to navigate forward and backward through the matches unlike Vim's
inconvienent and hard to remembers default of **;** and **,** repeats. In my
case I map the **leader** key to **,** and I map **;** as a duplicate of **:**,
hence those repeat characters are not available.

CtrlP
-----
```viml
Plug 'ctrlpvim/ctrlp.vim'
let g:ctrlp_user_command = 'ag %s -l --nocolor -g ""'
let g:ctrlp_use_caching = 0
let g:ctrlp_match_window_reversed = 0
```

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) is another essential plugin.
It is a fuzzy file finder.

I highly recommend installing the [Silver
Searcher](https://github.com/ggreer/the_silver_searcher) and configuring
*CtrlP* to use it and to not cache results. The *Silver Searcher* is an
extremely performant search utility and it mates very well with *CtrlP*.

NERDTree
--------
```viml
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
```

Most Vim folk are aware or use
[NERDTree](https://github.com/scrooloose/nerdtree). Not much explanation is
needed, *NERDTree* is a simple file explorer that open up on the left-hand side
of a Vim session.

One inconvenience is that *NERDTree*, by default, will not refresh itself when
one enters the file-tree window. This can be overcome with the following
*vimrc* snippet:

```viml
function! NERDTreeRefresh()
    if &filetype == "nerdtree"
        silent exe substitute(mapcheck("R"), "<CR>", "", "")
    endif
endfunction

autocmd BufEnter * call NERDTreeRefresh()
```

NERDTree Git Plugin
-------------------
```viml
Plug 'Xuyuanp/nerdtree-git-plugin', { 'on': 'NERDTreeToggle' }
let g:NERDTreeUpdateOnCursorHold = 0
```

The [NERDTree Git Plugin](https://github.com/Xuyuanp/nerdtree-git-plugin) adds
**git** status indicators in the *NERDTree* file-tree window.

The visual information provided by this plugin is surprisingly useful.

Ag
--
```viml
Plug 'rking/ag.vim'
let g:ag_mapping_message = 0
let g:ag_highlight = 1
```

The [Ag](https://github.com/rking/ag.vim) plugin is a simple Vim interface to
the [Silver Searcher](https://github.com/ggreer/the_silver_searcher) search
tool. Search matches will populate Vim's *quickfix* list allowing easy
navigation through the hits.

Note, the *Ag* plugin is deprecated, but remains fully functional. For now,
ignore the warnings at the [Github](https://github.com/rking/ag.vim) repo.

vim-test
--------
```viml
Plug 'janko-m/vim-test'
noremap <silent> <leader>ts :TestNearest<CR>
noremap <silent> <leader>tf :TestFile<CR>
noremap <silent> <leader>ta :TestSuite<CR>
noremap <silent> <leader>tl :TestLast<CR>
if has("nvim")
    let test#strategy = "neovim"
endif
```

The [vim-test](https://github.com/janko-m/vim-test) plugin provides a universal
interface to various back-end testing frameworks. This plugin allows one to
agnostically run tests for different languages and their associated testing
frameworks.

Note, Neovim inbuilt terminal is extremely well integrated with *vim-test*. The
above *has neovim* configuration will run tests in a split terminal window
which is very nice when test-driven-design.

Tim Pope Plugins
================

A special mention should given to Tim Pope who has crafted some of Vim's most
useful plugins.

Abolish
-------
```viml
```

Commentry
---------
```viml
```

Endwise
-------
```viml
```

Surround
--------
```viml
```
