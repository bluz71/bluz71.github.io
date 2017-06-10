---
title: My Favourite Vim Plugins
layout: default
comments: true
published: false
---

My Favourite Vim Plugins
========================

Vim gains much functionality through the inclusion of *plugins*. This post
contains a curated set of my favourite Vim plugins.

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
beat. It is what I recommend.

Note, if you are a *Vundle* user, like I was, transferring over to *vim-plug*
is simple, just follow
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
particular tastes.  Whether those tastes match up with anyone else's taste
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

vim-lion
--------
```viml
Plug 'tommcdo/vim-lion'
let g:lion_squeeze_spaces = 1
```

The [vim-lion](https://github.com/tommcdo/vim-lion) plugin is used to align
text around a chosen character. I find it easiest to select a visual region and
then invoke `gl<character>` to re-align text.

For example, `gl=` will convert this:
```ruby
i = 5;
username = 'tommcdo';
stuff = [1, 2, 3];
```

into this:
```ruby
i        = 5;
username = 'tommcdo';
stuff    = [1, 2, 3];
```

vim-indent-object
-----------------
```viml
Plug 'michaeljsmith/vim-indent-object'
```

Vim's text object allow easy selection and operation on regions of text.

Common text object operations include:

- `ciw` - change inside *word*
- `vat` - visually selection around *tag*
- `di"` - delete inside double *quotes*

The [vim-indent-object](https://github.com/michaeljsmith/vim-indent-object)
adds another text object, this one based on the indentation of the current
cursor line. This new text object is invoked by either `i` or `I`. Some
examples:

- `vii` - visually select *inside* code block using current indentation
- `vaI` - visually select *around* code block using current indentation AND
    include trailing line (for example the `end` delimiter in Ruby)

The `i` based text object is very handy since it is language agnostic, it works
just as well for Python code as it does for JavaScript code.

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
currently does not.

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
carry out completions whilst in *insert* mode. The plugin itself determines the
appropriate type of completion, be it *text*, *omni* or *file* completion.

clever-f
--------
```viml
Plug 'rhysd/clever-f.vim'
let g:clever_f_across_no_line = 1
let g:clever_f_timeout_ms = 3000
```

The [clever-f](https://github.com/rhysd/clever-f.vim) plugin makes `f`,
`F`, `t` and `T` movements more informative and convenient.

The more informative part is achieved by *clever-f* highlighting all the
matches for the chosen movement.

The more convenient part is achieved by simply using the `f` and `F`
characters to navigate forward and backward through the matches unlike Vim's
inconvenient and hard to remember defaults of `;` and `,`. In my case I map the
**leader** key to `,` and I map `;` as a duplicate of `:`, hence those repeat
characters are not available.

CtrlP
-----
```viml
Plug 'ctrlpvim/ctrlp.vim'
let g:ctrlp_user_command = 'rg %s --files --color=never --glob ""'
let g:ctrlp_use_caching = 0
let g:ctrlp_match_window_reversed = 0
let g:ctrlp_switch_buffer = 'e'
```

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) is another essential plugin.
It is a fuzzy file finder.

I highly recommend installing [ripgrep](https://github.com/BurntSushi/ripgrep)
and configuring *CtrlP* to use it and to not cache results. *ripgrep* is a
highly performant modern search utility and it mates very well with *CtrlP*.

NERDTree
--------
```viml
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
```

Most Vim folk are aware or use
[NERDTree](https://github.com/scrooloose/nerdtree). Not much explanation is
needed, *NERDTree* is a simple file explorer that open up on the left-hand side
of a Vim workspace.

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
**git** status indicators in the *NERDTree* window.

The visual information provided by this plugin is genuinely useful.

vim-grepper
-----------
```viml
Plug 'mhinz/vim-grepper'
let g:grepper = {}
runtime autoload/grepper.vim
let g:grepper.jump = 1
let g:grepper.stop = 500
noremap <leader>gr :GrepperRg<Space>
```

The [vim-grepper](https://github.com/mhinz/vim-grepper) plugin is a simple Vim
interface to various text search utilities including my favourite, the
[ripgrep](https://github.com/BurntSushi/ripgrep) search utility.

Upon execution *vim-grepper* search matches will populate Vim's *quickfix* list
allowing easy navigation through the hits. Note, when run on a modern version
of Vim or Neovim the search will be executed asynchronously. I also limit
search matches to a max of 500 matches since I want speedy results and also
because I never usually have that many hits.

I have a simple mapping `<leader>gr` to invoke *vim-grepper*'s *ripgrep* search.

vim-polyglot
------------
```viml
Plug 'sheerun/vim-polyglot'
```

The [vim-polyglot](https://github.com/sheerun/vim-polyglot) plugin is a
**comprehensive** language pack collection for Vim. This plugin consolidates
all the best standalone language plugins, such as
[vim-ruby](https://github.com/vim-ruby/vim-ruby) and
[vim-go](https://github.com/fatih/vim-go), into one master-plugin. And best
of all this plugin will configure all language scripts to only load when
required.

vim-test
--------
```viml
Plug 'janko-m/vim-test'
noremap <silent> <leader>T :TestNearest<CR>
noremap <silent> <leader>tf :TestFile<CR>
noremap <silent> <leader>ts :TestSuite<CR>
noremap <silent> <leader>tl :TestLast<CR>
if has("nvim")
    let test#strategy = "neovim"
endif
```

The [vim-test](https://github.com/janko-m/vim-test) plugin provides a universal
interface to various back-end testing frameworks. This plugin allows one to
agnostically run tests for different languages and their associated testing
frameworks.

Note, Neovim's inbuilt terminal is well integrated with *vim-test*. The above
`has neovim` configuration will run tests in a split terminal window unlike Vim
which will shell-out.

Tim Pope Plugins
================

A special mention should given to [Tim Pope](https://github.com/tpope) who has
crafted some of Vim's most useful plugins. He deserves a place in the Vim hall
of fame alongside Bram Moolenaar himself.

Abolish
-------
```viml
Plug 'tpope/vim-abolish'
```

The [abolish](https://github.com/tpope/vim-abolish) plugin is really a couple
plugins in one, those being: 

* a smart spell corrector
* a smart substituter
* a name coercer.

I primarily use the first two.

The *abolish* plugin can be set to automatically correct text as you type it.
An example use is correcting *seperate* into *separate* and *delimeter* into
*delimiter*. It can do this no matter the case and even with pluralization. One
sets up these corrections in their own *~/.vim/after/plugin/abolish.vim* file.

Here are my *abolish* corrections:
```
Abolish {despa,sepe}rat{e,es,ed,ing,ely,ion,ions,or} {despe,sepa}rat{}
Abolish {,in}consistant{,ly}                         {}consistent{}
Abolish lan{gauge,gue,guege,guegae,ague,agueg}       language
Abolish delimeter{,s}                                delimiter{}
Abolish {,non}existan{ce,t}                          {}existen{}
Abolish d{e,i}screp{e,a}nc{y,ies}                    d{i}screp{a}nc{}
Abolish {,un}nec{ce,ces,e}sar{y,ily}                 {}nec{es}sar{}
Abolish persistan{ce,t,tly}                          persisten{}
Abolish {,ir}releven{ce,cy,t,tly}                    {}relevan{}
Abolish cal{a,e}nder{,s}                             cal{e}ndar{}
Abolish reproducable                                 reproducible
Abolish retreive                                     retrieve
Abolish compeletly                                   completely
```

The abolish plugin can also carry smart substitutions. What is a *smart
substitution*? Such a substitution would intelligently change *old* to *new*
and *Old* to *New* and *OLD* to *NEW* in one command. The *abolish* *substitute*
command does just that and more. 

An example *abolish* substitute:
```
:%S/facilit{y,ies}/building{,s}/
```

This plugin does more than I have documented here, please refer to the
[abolish documentation](https://github.com/tpope/vim-abolish).

Commentry
---------
```viml
Plug 'tpope/vim-commentary'
```

The [vim-commentary](https://github.com/tpope/vim-commentary) plugin is a
simple language agnostic comment plugin. I use it with a visual line selection
to comment out or uncomment out a block of code with the `gc` operation the
plugin provides.

No need to remember what the comment characters are for a certain language, is
it **//** or **#** or **"**, just `gc` it.

Endwise
-------
```viml
Plug 'tpope/vim-endwise'
```

The [vim-endwise](https://github.com/tpope/vim-endwise) plugin will
automatically insert **end**, in insert mode, to code blocks for languages such
as: Ruby, Elixir and Crystal.

Surround
--------
```viml
Plug 'tpope/vim-surround'
```

The [vim-surround](https://github.com/tpope/vim-surround) allows one to add,
change or delete *surrounding pairs*.

What is a *surrounding pair*? It may be the quote characters or **<div>**
tags or anything else that surrounds some text.

To delete a *surrounding pair* use **d**. Here are some examples, the first
example will delete the double quotes, the second will delete a tag (like
*<div>*) and the third will delete _*_ :
```
ds"
dst
ds*
```

To change a *surrounding pair* use **c**. Note, you must provide the *old* and
*new* surround:
```
cs'"
cs*<div>
```

To add a *surround pair* one can visually select the candidate text and enter
**S** followed by the *surround* character(s) of choice.

It is even possible to add in *surrounding pairs* whilst in insert mode. Use a
single `Control-S` followed by the surround character(s). Use a double
`Control-S-S` to spread the surround over multiple lines. Note, in both cases
the cursor will be inserted *between* the *surrounding pair*. The double
`Control-S-S` is especially useful for inserting curly braces in
*C/C++/Java/JavaScript* type languages. 

Note, this plugin is harder to explain than it is to use, however once you *get
it* you can't imagine life without it.
