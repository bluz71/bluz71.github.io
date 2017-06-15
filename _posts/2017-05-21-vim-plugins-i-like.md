---
title: Vim Plugins I Like
layout: default
comments: true
published: false
---

Vim Plugins I Like
==================

Vim gains much functionality through the inclusion of *plugins*.

This post contains a curated set of my most favoured plugins. Note, many of
these plugins are *the usual suspects*, hence don't expect any revelations. I
also fully acknowledge that there are many useful Vim plugins beyond those
discussed here, hence, I encourage Vim users to explore, test and discuss those
they appreciate.

The full set of plugins and mappings I use are available in my
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
[vim-plug](https://github.com/junegunn/vim-plug) plugin manager is the one I
like most.

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

The [vim-easy-align](https://github.com/junegunn/vim-easy-align) and
[tabular](https://github.com/godlygeek/tabular) plugins also align text with
more functionality than *vim-lion* but with slightly higher complexity.

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
display indentation guide markers, as seen in *Sublime* and *Atom* editors.
This is a simple, yet useful, visual aid. I do hope one day Vim itself may
provide this feature.

The two *let* options documented **should** be set for maximum performance. The
default settings for the *indentLine* plugin **will** have a negative impact on
Vim scroll performance.

supertab
--------

```viml
Plug 'ervandew/supertab'
let g:SuperTabDefaultCompletionType = "context"
let g:SuperTabContextDefaultCompletionType = "<c-n>"
```

I [supertab](https://github.com/ervandew/supertab) plugin allows one to simply
use the **TAB** character to carry out completions whilst in *insert* mode. The
plugin itself determines the appropriate type of completion, be it *text*,
*omni* or *file* completion.

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

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) is a fuzzy file finder.

I highly recommend installing [ripgrep](https://github.com/BurntSushi/ripgrep)
and configuring *CtrlP* to use it and to **not** cache results. *ripgrep* is a
highly performant modern search utility and it mates very well with *CtrlP*.

Also, here are some handy mappings for [Ruby on Rails](http://rubyonrails.org/)
or [Elixir/Phoneix](http://www.phoenixframework.org/) developers:

```viml
if filereadable('config/environment.rb') && isdirectory('app')
    " This looks like a Rails app.
    nnoremap <leader>cc :CtrlP app/controllers<CR>
    nnoremap <leader>cm :CtrlP app/models<CR>
    nnoremap <leader>cv :CtrlP app/views<CR>
    nnoremap <leader>ch :CtrlP app/helpers<CR>
    nnoremap <leader>cs :CtrlP spec<CR>
elseif filereadable('config/prod.exs') && isdirectory('web')
    " This looks like an Elixir/Phoenix app.
    nnoremap <leader>cc :CtrlP web/controllers<CR>
    nnoremap <leader>cm :CtrlP web/models<CR>
    nnoremap <leader>cv :CtrlP web/views<CR>
    nnoremap <leader>cp :CtrlP web/templates<CR>
    nnoremap <leader>ct :CtrlP test<CR>
endif
```

These mappings, which can also easily be extended for other frameworks, provide
quick and direct access to *model/view/controller* files in their appropriate
directories.

Lastly, quite a few Vim users are now using
[fzf.vim](https://github.com/junegunn/fzf.vim) for their fuzzy file finding
needs instead of *CtrlP*. Personally I've not played with it since *CtrlP* with
*ripgrep* works perfectly fine for me.

NERDTree
--------

```viml
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
noremap <silent> <leader>n :NERDTreeToggle<CR> <C-w>=
```

Most Vim users are aware of
[NERDTree](https://github.com/scrooloose/nerdtree). Not much explanation is
needed, *NERDTree* is a simple file explorer that open up on the left-hand side
of a Vim workspace. I use `<leader>n` to toggle *NERDTree* whilst also
equalizing all existing splits.

One inconvenience is that *NERDTree*, by default, will not refresh itself when
one enters the file-tree window. For instance, it won't display new files not
created within *NERDTree* unless a manual *refresh* is executed. This can be
overcome with the following *vimrc* snippet:

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

I find the visual information provided by this plugin to be genuinely useful.

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

Neomake
-------

```viml
Plug 'neomake/neomake'
let g:neomake_open_list = 1
noremap <silent> <leader>m :Neomake<CR>
```

The [Neomake](https://github.com/neomake/neomake) plugin is used to
asynchronously run language linters, or compilers, within modern versions of
Vim.

Neomake ships with configurations for most common languages, such as JavaScript
and Ruby to name a couple, hence very little configuration is required within
Vim. However, the tools that *Neomake* uses, such as *eslint* or *rubocop*,
will need to be installed on the host.

I like to use a mapping, `<leader>m`, to invoke Neomake when I desire, others
however prefer to hook into to the file save event as follows:

```viml
autocmd! BufWritePost * Neomake
```

Neomake is not the only asynchronous linting solution for Vim, an alternative
is [ALE](https://github.com/w0rp/ale) which will live lint unlike Neomake.

Developers should integrate linting into their development flow, either one of
these two modern plugins should satisfy that need.

vim-test
--------

```viml
Plug 'janko-m/vim-test'
noremap <silent> <leader>T  :TestNearest<CR>
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
which will shell-out by default.

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

- a smart spell corrector
- a smart substituter
- a name coercer.

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

Projectionist
-------------

Configuration for Elixir/Phoenix:

```viml
Plug 'tpope/vim-projectionist'
let g:projectionist_heuristics = {
      \  "config/prod.exs": {
      \    "web/controllers/*_controller.ex": {
      \      "type": "controller",
      \      "alternate": "test/controllers/{}_controller_test.exs",
      \    },
      \    "web/models/*.ex": {
      \      "type": "model",
      \      "alternate": "test/models/{}_test.exs",
      \    },
      \    "web/views/*_view.ex": {
      \      "type": "view",
      \      "alternate": "test/views/{}_view_test.exs",
      \    },
      \    "web/templates/*.html.eex": {
      \      "type": "template",
      \      "alternate": "web/views/{dirname|basename}_view.ex"
      \    },
      \    "test/*_test.exs": {
      \      "type": "test",
      \      "alternate": "web/{}.ex",
      \    }
      \  }
      \}
noremap <leader>ec :Econtroller<Space>
noremap <leader>em :Emodel<Space>
noremap <leader>ev :Eview<Space>
noremap <leader>ep :Etemplate<Space>
noremap <leader>et :Etest<Space>
noremap <leader>A  :A<CR>
```

The [vim-projectionist](https://github.com/tpope/vim-projectionist) plugin,
primarily, provides infrastructure to navigate around projects. This plugin is
effectively the core of the [vim-rails](https://github.com/tpope/vim-rails)
plugin extracted into a standalone plugin. Note, Rails developers should still
use *vim-rails* in preference to *vim-projectionist*, think of it as a
pre-configured *vim-projectionist* with a little bit of added sugar on top;
however *vim-rails* and *vim-projectionist* can happily live side by side.

Listed above is an **example** configuration to navigate
[Elixir/Phoenix](http://www.phoenixframework.org/) applications. For example,
`<leader>ec TAB` will list all available controllers allowing a developer to
quickly go to the controller they wish. The `<leader>A` mapping provides quick
switching to an alternate file, which will usually be the associated test suite
for the current file.

Configurating *vim-projectionist* for other frameworks like
[react](https://facebook.github.io/react/) or [Ember](https://www.emberjs.com/)
should not be too difficult.

Setting up this plugin does require a little bit of upfront work, but once
done, and then used, you will appreciate the capabilities this plugin provides.
