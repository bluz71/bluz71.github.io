---
title: Vim Plugins I Like
layout: default
comments: true
published: true
---

Vim Plugins I Like
==================

Vim gains much functionality through the inclusion of *plugins*.

This post contains a curated set of my favourite Vim plugins. Note, most of
these plugins are *the usual suspects*. I also fully acknowledge that there are
**many** useful Vim plugins beyond those discussed here, hence, I do encourage
Vim users to explore, test and discuss those plugins they appreciate.

The full set of plugins and mappings I use are available in my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc).

vim-plug
--------

The first decision a Vim user, looking to enter the plugin world, must decide
is which plugin manager to use. There are many to choose from:
[Vundle](https://github.com/VundleVim/Vundle.vim),
[Pathogen](https://github.com/tpope/vim-pathogen),
[vim-plug](https://github.com/junegunn/vim-plug), and
[Dein](https://github.com/Shougo/dein.vim) to name a few.

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

Some self-advertising, I have written my own Vim `colorscheme` named
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

Instead I have created a simple matching `statusline` for the *moonfly*
color scheme named appropriately
[moonfly-statusline](https://github.com/bluz71/vim-moonfly-statusline). It
provides all relevant information I find useful whilst also clearly indicating
which mode you are in: normal, insert, replace or visual modes.

vim-one
-------

```viml
Plug 'rakr/vim-one'
```

The [vim-one](https://github.com/rakr/vim-one) `colorscheme` advertises itself
as *a light and dark Vim colorscheme, shamelessly stolen from Atom*. It is
basically a port of Atom's
[One](http://blog.atom.io/2015/02/18/one-themes.html) theme. Whenever I get
bored with my own *moonfly* color scheme I change over to *One*.

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
text around a chosen character.

I find it easiest to select a visual region and then invoke `gl<character>` to
re-align text around a chosen character (which will often be equals).

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

The alternative [vim-easy-align](https://github.com/junegunn/vim-easy-align)
and [tabular](https://github.com/godlygeek/tabular) plugins can also align
text.

targets.vim
-----------

```viml
Plug 'wellle/targets.vim'
```

The [Targets.vim](https://github.com/wellle/targets.vim) plugin provides
additional text objects. The highest compliment I can provide a plugin is to
say that it feels like a natural part of Vim itself, this tremendous plugin
exhibits that nice characteristic.

Vim's text objects allow for easy selection and operation on regions of text.

Common text object operations, provided by default with Vim, include:

- `ciw` - change inside *word*
- `yi)` - yank inside parenthesis
- `vat` - visually select around *tag*
- `di"` - delete inside double *quotes*

The *Targets.vim* plugin provides additional text object separators such as:
`*`, `|`, `=`, and `_` to name a few.

Examples of those separators:

- `ci*` - change inside *star*
- `va|` - visually select around *pipe*
- `ci_` - change inside *underscore*

The `*`-based text object is handy for changing emphasized text in Mardown
files whilst `_`-based text objects are useful for language that use
*snake_case* such as Ruby and Elixir.

I also appreciate the comma smarts this plugin provides.

For example, given this text with the cursor positioned inside *second:*

```
foobar(first, second, third)
```

The operation `da,` will delete the comma before *second* but not the one
trailing it. Most of the time this *is* the preferred result when dealing with
source code.

I highly recommend this excellent plugin to all Vim users.

vim-indent-object
-----------------

```viml
Plug 'michaeljsmith/vim-indent-object'
```

The [vim-indent-object](https://github.com/michaeljsmith/vim-indent-object)
plugin adds yet another text object to Vim, this one based on the indentation
of the current cursor line. This new text object is invoked by either `i` or
`I`. Some examples:

- `vii` - visually select *inside* code block using current indentation
- `vaI` - visually select *around* code block using current indentation AND
    include trailing line (for example the `end` delimiter in Ruby)

These indent based text objects are handy because they are language agnostic,
they work just as well for Python code as they do for JavaScript code.

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
characters are not even available.

CtrlP
-----

```viml
Plug 'ctrlpvim/ctrlp.vim'
let g:ctrlp_user_command = 'fd --type f --color=never "" %s'
let g:ctrlp_use_caching = 0
let g:ctrlp_match_window_reversed = 0
```

The [CtrlP](https://github.com/ctrlpvim/ctrlp.vim) plugin is a fuzzy file
finder.

I highly recommend installing [fd](https://github.com/sharkdp/fd) and
configuring *CtrlP* to use it and to **not** cache results. *fd* is a highly
performant file find utility and it mates very well with *CtrlP*. Turning off
caching means that every time *CtrlP* is run it will select from all available
files including new ones.

Also, here are some handy mappings for [Ruby on Rails](http://rubyonrails.org/)
or [Elixir/Phoneix](http://www.phoenixframework.org/) developers:

```viml
if filereadable('config/environment.rb') && isdirectory('app')
    " This looks like a Rails app.
    noremap <localleader>ec :CtrlP app/controllers<CR>
    noremap <localleader>eh :CtrlP app/helpers<CR>
    noremap <localleader>em :CtrlP app/models<CR>
    noremap <localleader>es :CtrlP spec<CR>
    noremap <localleader>eT :CtrlP test<CR>
    noremap <localleader>ev :CtrlP app/views<CR>
elseif filereadable('web/router.ex')
    " This looks like an Elixir/Phoenix app.
    noremap <localleader>ec :CtrlP web/controllers<CR>
    noremap <localleader>em :CtrlP web/models<CR>
    noremap <localleader>eT :CtrlP test<CR>
    noremap <localleader>et :CtrlP web/templates<CR>
    noremap <localleader>ev :CtrlP web/views<CR>
endif
```

Those mappings, which can also easily be extended for other frameworks, provide
quick and direct access to *model/view/controller* files (among others) in
their appropriate directories.

Lastly, be aware that quite a few Vim users are now using
[fzf.vim](https://github.com/junegunn/fzf.vim) for their fuzzy file finding
needs instead of *CtrlP*. Personally I've not played with it since *CtrlP* with
*ripgrep* works perfectly fine for me.

UltiSnips
---------

```viml
Plug 'SirVer/ultisnips'
let g:UltiSnipsExpandTrigger       = "<C-j>"
let g:UltiSnipsJumpForwardTrigger  = "<C-j>"
let g:UltiSnipsJumpBackwardTrigger = "<C-k>"
```

The [UltiSnips](https://github.com/SirVer/ultisnips) plugin allows one to
easily insert predefined text segments in the current buffer.

The following Vimcasts are an excellent introduction to *UltiSnips*, please
view:

- [Meet UltiSnips](http://vimcasts.org/episodes/meet-ultisnips/)
- [Using Python interpolation in UltiSnips snippets](http://vimcasts.org/episodes/ultisnips-python-interpolation/)
- [Using selected text in UltiSnips snippets](http://vimcasts.org/episodes/ultisnips-visual-placeholder/)

Note, by default *UltiSnips* will use the **TAB** character to expand a
snippet, however that will conflict with the *supertab* plugin. Instead use
`Control-j`, as defined above, to expand and then navigate forward through a
snippet's tabstops and `Control-k` to navigate back up through the tabstops.

The small set of snippets I use are declared [here](https://github.com/bluz71/dotfiles/tree/master/vim/UltiSnips).

NERDTree
--------

```viml
Plug 'scrooloose/nerdtree', { 'on': 'NERDTreeToggle' }
let NERDTreeHijackNetrw = 0
noremap <silent> <leader>n :NERDTreeToggle<CR> <C-w>=
noremap <silent> <leader>f :NERDTreeFind<CR> <C-w>=
```

Most Vim users are aware of
[NERDTree](https://github.com/scrooloose/nerdtree). Not much explanation is
needed, *NERDTree* is a simple file explorer that open up on the left-hand side
of a Vim workspace.

I use `<leader>n` to toggle *NERDTree* whilst also equalizing all existing
splits. I also have a `<leader>f` mapping to open NERDTree and reveal the
current buffer in the file tree.

One inconvenience is that *NERDTree*, by default, will not refresh itself when
one enters the file-tree window. For instance, it won't display new files not
created within *NERDTree* unless a manual *refresh* is executed. This can be
overcome with the following auto-refreshing snippet:

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
let g:NERDTreeUpdateOnWrite      = 0
```

The [NERDTree Git](https://github.com/Xuyuanp/nerdtree-git-plugin) plugin adds
**git** status indicators in the *NERDTree* window.

I find the visual information provided by this plugin to be genuinely useful. I
recommend all NERDTree users, who manage code via Git repositories, to give
this plugin a try.

supertab
--------

```viml
Plug 'ervandew/supertab'
let g:SuperTabDefaultCompletionType = "context"
let g:SuperTabContextDefaultCompletionType = "<c-n>"

autocmd FileType css,scss let g:SuperTabDefaultCompletionType = "<c-x><c-o>"
```

The [supertab](https://github.com/ervandew/supertab) plugin uses the **TAB**
character to carry out completions, using Vim's various existing completion
options, whilst in *insert* mode. The plugin itself determines the appropriate
type of completion, be it *keyword*, *file* or *omni* completion, based on the
current context.

Note, the above listed `autocmd` is required for nice *supertab* completions in
**CSS** files.

Similar to *CtrlP* and *NERDTree*, it seems that the mature *supertab* plugin
is a little uncool to like these days especially with modern alternatives
available. It is often accused of having a crufty codebase, which may well be
true, but I continue to use it simply because it *just works.* But it must be
said, my completion needs are modest and I find Vim's existing *omni*-based
completion to be quite good, not *Jetbrains* IDE good, but good enough for me.

If *supertab* doesn't float your boat then rest assured there are many
completion choices available ranging from the simple to the advanced. Such Vim
plugin completion choices include:

- [VimCompletesMe](https://github.com/ajh17/VimCompletesMe)
- [MUcomplete](https://github.com/lifepillar/vim-mucomplete)
- [vim-simple-complete](https://github.com/maxboisvert/vim-simple-complete)
- [YouCompleteMe](https://github.com/Valloric/YouCompleteMe)
- [neocomplete](https://github.com/Shougo/neocomplete.vim)/[deoplete](https://github.com/Shougo/deoplete.nvim)
- [nvim-completion-manager](https://github.com/roxma/nvim-completion-manager)
- [completor.vim](https://github.com/maralla/completor.vim)

I would classify *supertab*, *VimCompletesMe*, *MUcomplete*  and
*vim-simple-complete* as being towards the simple/light end of completion
spectrum whilst the others I would classify as being towards the advanced,
sometimes heavy, end of the spectrum. Personally, I recommend starting simple
then moving up only when necessary.

indentLine
----------

```viml
Plug 'Yggdroot/indentLine'
let g:indentLine_faster = 1
let g:indentLine_setConceal = 0
```

The [indentLine](https://github.com/Yggdroot/indentLine) plugin is used to
display indentation guide markers as often seen in *Sublime* and *Atom*
editors. This is a simple and useful visual aid, though *indentLine* is not
quite as slick-looking as the guide markers in *Sublime* and *Atom*.

Note, the two *let* options listed **should** be set for maximum performance.
The default settings for the *indentLine* plugin **will** have a negative
impact on Vim scroll performance.

<a id="vim-grepper"></a>vim-grepper
-----------------------------------

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
and Ruby to name a few, hence very little configuration is usually required.
Note, the tools that *Neomake* uses, such as *eslint* or *rubocop*, **will**
need to be installed on the host.

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
noremap <silent> <leader>.  :TestNearest<CR>
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

vim-closer
----------

```viml
Plug 'rstacruz/vim-closer'
```

The [vim-closer](https://github.com/rstacruz/vim-closer) plugin will
automatically close `(`, `{` and `[` after `enter` is pressed when in insert
mode for supported languages such as: C, C++, JavaScript and Go (to name a
few). This plugin is a natural companion to the *vim-endwise* plugin noted
below.

vim-auto-save
-------------

```viml
Plug '907th/vim-auto-save'
let g:auto_save        = 1
let g:auto_save_silent = 1
let g:auto_save_events = ["InsertLeave", "TextChanged", "FocusLost"]
```

The [vim-auto-save](https://github.com/907th/vim-auto-save) plugin
automatically saves changes to disk without required manual `:w` invocations. I
prefer to automatically save after: normal mode changes (`TextChanged`), exiting
insert mode (`InsertLeave`) and when focussing away from Vim (`FocusLost`).

Tim Pope Plugins
================

A special mention should be given to [Tim Pope](https://github.com/tpope) who
has crafted some of Vim's most useful plugins. He deserves a place in the Vim
hall of fame alongside Bram Moolenaar himself.

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
simple language agnostic commenter. I usually use it with a visual line
selection to comment out or uncomment out a block of code with the `gc`
command the plugin provides.

No need to remember what the comment characters are for a certain language, is
it **//** or **#** or **"**, just `gc` it.

Fugitive
--------

```viml
Plug 'tpope/vim-fugitive'
noremap <leader>gb :Gblame<CR>
```

The [vim-fugitive](https://github.com/tpope/vim-fugitive) plugin is a Git
wrapper. I do most of my **git** work at the command line, however I find
*fugitive's* `Gblame` command to be supremely useful within Vim.

Endwise
-------

```viml
Plug 'tpope/vim-endwise'
```

The [vim-endwise](https://github.com/tpope/vim-endwise) plugin will
automatically insert **end**, in insert mode, to code blocks for languages such
as: Ruby, Elixir and Crystal. This plugin is a natural companion to the
*vim-closer* plugin noted above.

Projectionist
-------------

The [vim-projectionist](https://github.com/tpope/vim-projectionist) plugin,
primarily, provides infrastructure to navigate around projects. This plugin is
effectively the core of the [vim-rails](https://github.com/tpope/vim-rails)
plugin extracted into a standalone plugin.

Here is a simple configuration for Elixir/Phoenix projects:

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
noremap <leader>et :Etemplate<Space>
noremap <leader>eT :Etest<Space>
noremap <leader>ev :Eview<Space>
noremap <leader>A  :A<CR>
```

The above configuration will result in the following commands being created:
`Econtroller`, `Emodel`, `Eview`, `Etemplate` and `Etest`. Those commands are
then mapped for quick access. Hence, `<leader>ec <TAB>` will list all available
controllers, in the status line if `wildmenu` and `wildmode` are set
appropriately, allowing a developer to quickly go to the controller they wish.
The `<leader>A` mapping provides quick switching to an *alternate* file, which
will usually be the associated test suite for the current file.

Configuring *vim-projectionist* for other frameworks like
[react](https://facebook.github.io/react/) or [Ember](https://www.emberjs.com/)
should not be too difficult.

Setting up this plugin does require a little bit of upfront work, but once
done, and then used, you will really appreciate the navigation capabilities
this plugin provides.

Note, Rails developers should still use *vim-rails* in preference to
*vim-projectionist*, think of *vim-rails* as a pre-configured
*vim-projectionist* with a little bit of added sugar on top; also *vim-rails*
and *vim-projectionist* do happily live side by side.

sleuth
------

```viml
Plug 'tpope/vim-sleuth'
```

The [vim-sleuth](https://github.com/tpope/vim-sleuth) plugin automatically
adjusts `shiftwidth` and `expandtab` intelligently based on the existing
indentation within the file or within the directory tree for like files. With
this plugin in effect there is little need to manually define indentation
settings.

Ragtag
------

```viml
Plug 'tpope/vim-endwise'
```

The [vim-ragtag](https://github.com/tpope/vim-ragtag) plugins provides a set of
helpers for TAG-based languages such as HTML, XML and JSX.

These are the *ragtag* helpers I find most handy whilst in *insert* mode:

- `<CTRL-x>/` close the previous open tag
- `<CTRL-x><Space>` convert the current word into open and close tags
- `<CTRL-x><Enter>` same as above except split the tag over multiple lines
- `<CTRL-x>_` add `<% %>` template tag
- `<CTRL-x>+` add `<%= %>` templage tag

Surround
--------

```viml
Plug 'tpope/vim-surround'
```

The [vim-surround](https://github.com/tpope/vim-surround) plugin allows one to
add, change or delete *surrounding pairs*.

What is a *surrounding pair*? It may be the quote characters or **<div>**
tags or anything else that surrounds some text.

To delete a *surrounding pair* use **d**. Here are some examples, the first
example will delete double quotes, the second will delete a tag (like *<div>*)
and the third will delete _*_ :

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
single `Ctrl-S` followed by the surround character(s) to create a surround on
the current line. Use a double `Ctrl-S-S` to spread the surround over multiple
lines. Note, in both cases the cursor will be inserted *between* the
*surrounding pair*. The double `Ctrl-S-S` is especially useful for inserting
curly braces in *C/C++/Java/JavaScript* type languages.

Note, terminal Vim users need to make sure flow-control is disable otherwise
the above `Ctrl-S` will lock your terminal. I recommend starting Vim this way
to avoid that issue, `stty -ixon && vim`.

This plugin is a little harder to explain than it is to use, however once you
*get it* you can't imagine life without it.

Repeat
------

```viml
Plug 'tpope/vim-repeat'
```

The [vim-repeat](https://github.com/tpope/vim-repeat) enhances the `.` operator
to work as one would expect with a number of Vim plugins, most notably the
*vim-surround* plugin noted above.

Unimpaired
----------

```viml
Plug 'tpope/vim-unimpaired'
```

The [vim-unimpaired](https://github.com/tpope/vim-unimpaired) plugin provides a
set of mappings for many operations that have natural pairings. A pairing may
be: up and down, or forward and backward, set or unset or above and below.

Of the mappings provided by this plugin these are the mappings I use most
often:

- `[q` / `]q` - navigate up and down through the **q**uickfix list, for instance
  through *vim-grepper* results

- `[l` / `]l` - navigate up and down through the **l**ocation list, for instance
  through *neomake* results

- `[a` / `]a` - navigate backward and forward through the file list

- `[<Space>` / `]<Space>` - add a blank line above or below the current line

- `[p` / `]p` - linewise paste above or below the current line

The full set of mappings is documented
[here](https://github.com/tpope/vim-unimpaired/blob/master/doc/unimpaired.txt).

The *vim-unimpaired* plugin negates the need to provide your own custom set of
mappings for these types of operation.
