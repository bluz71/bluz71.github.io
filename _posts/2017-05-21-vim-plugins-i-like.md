---
title: Vim Plugins I Like
layout: default
comments: true
published: true
---

Vim Plugins I Like
==================

**UPDATED AUGUST 2020**

Vim gains much functionality through the inclusion of *plugins*.

This post contains a curated set of my favourite Vim plugins. Note, many of
these plugins are *the usual suspects*. I also fully acknowledge that there are
**numerous** useful plugins beyond those discussed here, hence, I do encourage
Vim users to explore, test and discuss those plugins they appreciate.

The full set of plugins and mappings I use are available in my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc).

vim-plug
--------

The first decision a Vim user, looking to enter the plugin world, must decide is
which plugin manager to use. There are many to choose from:
[Vundle](https://github.com/VundleVim/Vundle.vim),
[Pathogen](https://github.com/tpope/vim-pathogen),
[vim-plug](https://github.com/junegunn/vim-plug),
[Dein](https://github.com/Shougo/dein.vim) and [Vim 8 native package
management](http://vimcasts.org/episodes/packages/) to name a few.

Each will do the job, but for *simplicity*, *performance* and *features*
the [vim-plug](https://github.com/junegunn/vim-plug) plugin manager is the one I
use.

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

Some self-advertising, I have created my own Vim `colorscheme` named
[moonfly](https://github.com/bluz71/vim-moonfly-colors).

The *moonfly* color scheme is a dark theme, but unlike all other dark themes
this one is **my** dark theme. That means it has been tuned to my particular
tastes.  Whether those tastes match up with anyone else's taste will be in the
eye of the beholder.

vim-nightfly-guicolors
----------------------

```viml
Plug 'bluz71/vim-nightfly-guicolors'
```

More recently, I have created another Vim `colorscheme` named
[nightfly](https://github.com/bluz71/vim-nightfly-guicolors).

The *nightfly* color scheme is also a dark theme, but unlike the
charcoal-colored *moonfly* color scheme this one skews slightly toward blue. The
*nightfly* color scheme is heavily inspired by the Visual Studio Code [Night
Owl](https://github.com/sdras/night-owl-vscode-theme) theme.

vim-moonfly-statusline
----------------------

```viml
Plug 'bluz71/vim-moonfly-statusline'
```

I am not a fan of heavy *statusline* plugins like
[powerline](https://github.com/powerline/powerline) and
[airline](https://github.com/vim-airline/vim-airline).

Instead, I have created the simple, yet informative,
[moonfly-statusline](https://github.com/bluz71/vim-moonfly-statusline) plugin
which by default uses the
[moonfly](https://github.com/bluz71/vim-moonfly-colors) color palette. If the
colors do not suit then they can easily be
[customized](https://github.com/bluz71/vim-moonfly-statusline#gmoonflyignoredefaultcolors)
if desired.

Note, the newer _nightfly_ color scheme will automatically adapt to
_moonfly-statusline_.

:bulb: With `let g:moonflyIgnoreDefaultColors = 1` set, it is quite simple to
define a custom status color scheme for _moonfly-statusline_. For example this
simple theme should work well with most existing Vim color schemes:

```viml
let g:moonflyIgnoreDefaultColors = 1

highlight! link User1 DiffText
highlight! link User2 DiffAdd
highlight! link User3 Search
highlight! link User4 IncSearch
highlight! link User5 StatusLine
highlight! link User6 StatusLine
highlight! link User7 StatusLine
```

indentLine
----------

```viml
Plug 'Yggdroot/indentLine'
let g:indentLine_char       = '▏'
let g:indentLine_setConceal = 0
```

The [indentLine](https://github.com/Yggdroot/indentLine) plugin is used to
display indentation guide markers as often seen in *Sublime* and *Atom*
editors. This is a simple and useful visual aid, though *indentLine* is not
quite as slick-looking as the guide markers in *Sublime* and *Atom*.

visual-star-search
------------------

```viml
Plug 'nelstrom/vim-visual-star-search'
```

The
[vim-visual-star-search](https://github.com/nelstrom/vim-visual-star-search)
plugin allows __*__ and **#** searches to occur on the current visual selection.

targets.vim
-----------

```viml
Plug 'wellle/targets.vim'
```

The [Targets.vim](https://github.com/wellle/targets.vim) plugin provides
additional text objects. The highest compliment I can provide a plugin is to
say that it feels like a natural part of Vim itself, this tremendous plugin
exhibits that characteristic.

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

clever-f
--------

```viml
Plug 'rhysd/clever-f.vim'
let g:clever_f_across_no_line    = 1
let g:clever_f_fix_key_direction = 1
```

The [clever-f](https://github.com/rhysd/clever-f.vim) plugin makes `f`,
`F`, `t` and `T` movements more informative and convenient.

The more informative aspect is achieved by *clever-f* highlighting all the
matches for the chosen movement.

The more convenient aspect is achieved by simply using the `f` and `F`
characters to navigate forward and backward through the matches unlike Vim's
inconvenient and hard to remember defaults of `;` and `,`. In my case I map the
**leader** key to `,` and I map `;` as a duplicate of `:`, hence those repeat
characters are not even available.

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

vim-wordmotion
---------------

```viml
Plug 'chaoren/vim-wordmotion'
nmap cw ce
nmap cW cE
```

The [wordmotion](https://github.com/chaoren/vim-wordmotion) plugin expands Vim's
definition of a word. This plugin will take into account programming-related
camel and snake-case (and other unusal word definitions) and allow navigation,
using `w` and `b`, within such *words*.

This plugin will not suit everyone, but for certain language, such as
[Ruby](https://www.ruby-lang.org/) which uses both camel and snake-case, it has
proven invaluable in practice.

Note, the `cw` / `cW` mapping will restore standard Vim behaviour, that being to
preserve whitespace between words.

Pear Tree
---------

```viml
Plug 'tmsvg/pear-tree'
let g:pear_tree_repeatable_expand = 0
let g:pear_tree_smart_backspace   = 1
let g:pear_tree_smart_closers     = 1
let g:pear_tree_smart_openers     = 1
```

The [Pear Tree](https://github.com/tmsvg/pear-tree) plugin will automatically
close `(`, `{`, `[` and quote pairs whilst in insert mode.

:bulb: If I was doing less JavaScript and JSON I would likely skip this and all
auto-closing pairs plugins.

fzf.vim
-------

```viml
Plug 'junegunn/fzf', { 'do': { -> fzf#install() } }
Plug 'junegunn/fzf.vim'
```

The [fzf.vim](https://github.com/junegunn/fzf.vim) plugin is a performant fuzzy
finder.

Please refer to [fuzzy finding in Vim with
fzf](https://bluz71.github.io/2018/12/04/fuzzy-finding-in-vim-with-fzf.html) for
comprehensive details about [fzf](https://github.com/junegunn/fzf) and the
[fzf.vim](https://github.com/junegunn/fzf.vim) plugin.

fern.vim
--------

```viml
Plug 'lambdalisue/fern.vim'
let g:fern#disable_default_mappings   = 1
let g:fern#disable_drawer_auto_quit   = 1
let g:fern#disable_viewer_hide_cursor = 1
```

The [fern](https://github.com/lambdalisue/fern.vim) plugin is a simple, yet
powerful, file explorer.

Historically, [NERDTree](https://github.com/preservim/nerdtree) has been the
go-to file explorer for Vim; however, in my experience *NERDTree* does have
certain performance issues that can make it frustrating to use. In contrast,
*fern* is a modern asynchronous file explorer that has excellent performance.

Candidate *fern* launch mappings:

```viml
noremap <silent> <Leader>d :Fern . -drawer -width=35 -toggle<CR><C-w>=
noremap <silent> <Leader>f :Fern . -drawer -reveal=% -width=35<CR><C-w>=
noremap <silent> <Leader>. :Fern %:h -drawer -width=35<CR><C-w>=
```

- The `<Leader>d` mapping toggles a left-hand side project drawer whilst also
  equalizing existing splits

- The `<Leader>f` mapping opens *fern* and reveals the current buffer in the
  project tree

- The `<Leader>.` opens just the directory of sibling files of the current
  buffer

Candidate *fern* operation mappings:

```viml
function! FernInit() abort
  nmap <buffer><expr>
        \ <Plug>(fern-my-open-expand-collapse)
        \ fern#smart#leaf(
        \   "\<Plug>(fern-action-open:select)",
        \   "\<Plug>(fern-action-expand)",
        \   "\<Plug>(fern-action-collapse)",
        \ )
  nmap <buffer> <CR> <Plug>(fern-my-open-expand-collapse)
  nmap <buffer> <2-LeftMouse> <Plug>(fern-my-open-expand-collapse)
  nmap <buffer> m <Plug>(fern-action-mark-toggle)j
  nmap <buffer> N <Plug>(fern-action-new-file)
  nmap <buffer> K <Plug>(fern-action-new-dir)
  nmap <buffer> D <Plug>(fern-action-remove)
  nmap <buffer> R <Plug>(fern-action-move)
  nmap <buffer> s <Plug>(fern-action-open:split)
  nmap <buffer> v <Plug>(fern-action-open:vsplit)
  nmap <buffer> r <Plug>(fern-action-reload)
  nmap <buffer> <nowait> d <Plug>(fern-action-hidden-toggle)
  nmap <buffer> <nowait> < <Plug>(fern-action-leave)
  nmap <buffer> <nowait> > <Plug>(fern-action-enter)
endfunction

augroup FernGroup
  autocmd!
  autocmd FileType fern call FernInit()
augroup END
```

- *Enter* and *double-click* open and collapse directories, or opens files.
  Note, if multiple splits exist *fern* will prompt the user asking which
  split to open the chosen file into thus avoiding the [oil and
  vinegar](http://vimcasts.org/blog/2013/01/oil-and-vinegar-split-windows-and-project-drawer)
  problem.

- `m` marks entries for bulk operation such as deletion or opening

- `N` and `K` create new files and directories respectively

- `D` removes content

- `R` renames content

- `s` and `v` will open files in either horizontal or vertical splits

- `r` reloads the current tree, use this to update *fern* with filesystem
  changes that occur outside Vim

- 'd' toggles dot files

- `<` and `>` will change up and down the directory hierarchy.

Candidate style settings for those who like a NERDTree-like presentation:

```viml
let g:fern#mark_symbol                       = '●'
let g:fern#renderer#default#collapsed_symbol = '▷ '
let g:fern#renderer#default#expanded_symbol  = '▼ '
let g:fern#renderer#default#leading          = ' '
let g:fern#renderer#default#leaf_symbol      = ' '
let g:fern#renderer#default#root_symbol      = '~ '
```

Lastly, if one prefers to automatically update the current tree upon entering
the *fern* window then please add the following to the above `FernInit`
function (not the `FernGroup`):

```viml
augroup FernTypeGroup
    autocmd! * <buffer>
    autocmd BufEnter <buffer> silent execute "normal \<Plug>(fern-action-reload)"
augroup END
```

:exclamation: Certain Vim elitists consider file explorers an anti-pattern. I
primarily use *fern* as a project tree visualizer and occasional file manager. I
do **not** recommend you use *fern* as your prime method to open files,
alternatives such as: *fzf*, *projectionist* and the standard `gf` command are
better and faster.

fern-git-status
---------------

```viml
Plug 'lambdalisue/fern-git-status.vim'
let g:fern_git_status#disable_ignored    = 1
let g:fern_git_status#disable_untracked  = 1
let g:fern_git_status#disable_submodules = 1
```

The [fern-git-status](https://github.com/lambdalisue/fern-git-status.vim) plugin
augments the above documented *fern* plugin with Git status badges when in a Git
working tree.

A screenshot of *fern-git-status* in action:

![fern-git-status](https://user-images.githubusercontent.com/546312/89777703-2483cd80-db47-11ea-84dc-7690d2996d89.png)

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

vim-gitgutter
-------------

```viml
Plug 'airblade/vim-gitgutter'
let g:gitgutter_grep                    = 'rg'
let g:gitgutter_map_keys                = 0
let g:gitgutter_sign_added              = '▎'
let g:gitgutter_sign_modified           = '▎'
let g:gitgutter_sign_modified_removed   = '▌'
let g:gitgutter_sign_removed            = '▎'
let g:gitgutter_sign_removed_first_line = '▎'
nmap [g <Plug>GitGutterPrevHunkzz
nmap ]g <Plug>GitGutterNextHunkzz
nmap <Leader>p <Plug>GitGutterPreviewHunk
nmap <Leader>+ <Plug>GitGutterStageHunk
nmap <Leader>- <Plug>GitGutterUndoHunk
```

The [vim-gitgutter](https://github.com/airblade/vim-gitgutter) plugin highlights
Git repository modifications via the signs column whilst also providing
functionality to navigate, preview, stage and undo those modified Git chunks,
aka hunks.

The speed with which signs appear and change is governed by Vim's `updatetime`
option. I suggest setting it to 100ms:

```viml
set updatetime=100
```

Candidate mappings:

- Square-brackets `g` navigates between hunks

- `<Leader>p` displays the current hunk as a diff in the preview window

- `<Leader>+` stages the curent hunk

- `<Leader>-` reverts the curent hunk back to `HEAD`

This plugin shines when dealing with modified Git chunks; that being easy
navigation and staging of those hunks.

Code Completion with VimCompletesMe and LSP-based LSC
-----------------------------------------------------

```viml
Plug 'ajh17/VimCompletesMe'
Plug 'natebosch/vim-lsc'
```

The [VimCompletesMe](https://github.com/ajh17/VimCompletesMe) plugin uses the
**TAB** character, whilst in *insert* mode, to carry out completions using
Vim's various built-in completions. The plugin itself usually determines the
appropriate type of completion, be it *keyword*, *file* or
[omni](http://vim.wikia.com/wiki/Omni_completion) completion, based on the
current context.

Where I require more language-aware intelligence I use the
[LSC](https://github.com/natebosch/vim-lsc) plugin with appropriate
LSP-compliant language servers. Please refer to the [LSP in Vim with the LSC
Plugin](https://bluz71.github.io/2019/10/16/lsp-in-vim-with-the-lsc-plugin.html)
post for details about the Language Server Protocol (LSP) and the LSC
plugin.

ALE
---

```viml
Plug 'w0rp/ale'
let g:ale_fixers = {
\  'css':        ['prettier'],
\  'javascript': ['prettier-standard'],
\  'json':       ['prettier'],
\  'ruby':       ['standardrb'],
\  'scss':       ['prettier'],
\  'yml':        ['prettier']
\}
let g:ale_linters = {
\  'css':        ['csslint'],
\  'javascript': ['standard'],
\  'json':       ['jsonlint'],
\  'ruby':       ['standardrb'],
\  'scss':       ['sasslint'],
\  'yaml':       ['yamllint']
\}
let g:ale_linters_explicit = 1
let g:ale_open_list        = 0
```

The [ALE](https://github.com/w0rp/ale) plugin is used to asynchronously run
language linters and fixers within modern versions of Vim.

Be aware, a fixer can also format code, hence there is no need to install code
formatting plugins such as
[vim-prettier](https://github.com/prettier/vim-prettier) when using ALE.

ALE ships with configurations for most common languages, such as JavaScript and
Ruby to name a few, hence little configuration is required. Note, the tools that
*ALE* uses, such as *eslint* or *standard*, **will** need to be installed on the
host, the ALE plugin will not install the underlying lint or fix tools.

One could define mappings, `<Leader>l` for linting and `<Leader>f` for fixing,
and run them manually, others however prefer to have ALE lint code is it is
being written (the default behaviour). Candidate ALE settings:

```viml
let g:ale_lint_on_enter            = 0
let g:ale_lint_on_filetype_changed = 0
let g:ale_lint_on_insert_leave     = 0
let g:ale_lint_on_save             = 1
let g:ale_lint_on_text_changed     = 'never'
nmap <Leader>l    <Plug>(ale_lint)
nmap <Leader>f    <Plug>(ale_fix)
```

Note, use the `:ALEInfo` command to display runtime information per the
current file type, use it when you need to debug any ALE issues.

ALE is not the only asynchronous linting solution for Vim, an alternative is
[Neomake](https://github.com/neomake/neomake) which does much the same job. I
prefer ALE since it also incorporates fixing.

vim-vsnip
---------

```viml
Plug 'hrsh7th/vim-vsnip'
imap <expr> <C-j> vsnip#available(1) ? "<Plug>(vsnip-expand-or-jump)" : "<C-j>"
imap <expr> <C-k> vsnip#jumpable(-1) ? "<Plug>(vsnip-jump-prev)"      : "<C-k>"
```

The [vim-vsnip](https://github.com/hrsh7th/vim-vsnip) plugin allows easy
insertion of predefined text segments, named snippets, in the current buffer.

The following Vimcasts are an excellent introduction to snippets, in this case
via the older, but similar, *UltiSnips* plugin, please view:

- [Meet UltiSnips](http://vimcasts.org/episodes/meet-ultisnips/)
- [Using selected text in UltiSnips snippets](http://vimcasts.org/episodes/ultisnips-visual-placeholder/)

The legacy *UltiSnips* plugin uses a different in snippet syntax compared with
*vim-vsnip*, but the core concept of snippets are common to both plugins.

So why choose *vim-vsnip* in preference to *UltiSnips*? In my case it is mainly
due to the Python dependency of the *UltiSnips* plugin. Python-based Vim plugins
are fragile to system level Python updates. *vim-vsnip* on the otherhand is a
pure VimScript plugin and is not prone to such breakages.

By default, I do not recommend the use of the **TAB** character to expand or
navigate a snippet since that will conflict with the *VimCompletesMe* plugin.
Instead, I recommend the use of `Control-j`, as defined above, to expand and
then navigate forward through a snippet's tabstops and `Control-k` to navigate
back up through the tabstops.

The small set of snippets I use are declared
[here](https://github.com/bluz71/dotfiles/tree/master/vim/vsnip).

:gift: I also define the following `<Control-s>` insert mode mapping to list
available snippets in the completion menu, this is useful for the times I can't
recall the name of a seldom used snippet:

```viml
inoremap <silent> <C-s> <C-r>=SnippetsComplete()<CR>

function! SnippetsComplete() abort
    let wordToComplete = matchstr(strpart(getline('.'), 0, col('.') - 1), '\S\+$')
    let fromWhere      = col('.') - len(wordToComplete)
    let containWord    = "stridx(v:val.word, wordToComplete)>=0"
    let candidates     = vsnip#get_complete_items(bufnr("%"))
    let matches        = map(filter(candidates, containWord),
                \  "{
                \      'word': v:val.word,
                \      'menu': v:val.kind,
                \      'dup' : 1,
                \   }")


    if !empty(matches)
        call complete(fromWhere, matches)
    endif

    return ""
endfunction
```

vim-test
--------

```viml
Plug 'janko-m/vim-test'
let test#javascript#jest#executable = 'CI=true yarn test --colors'
nnoremap <silent> <Leader>tt :TestNearest<CR>
nnoremap <silent> <Leader>tf :TestFile<CR>
nnoremap <silent> <Leader>ts :TestSuite<CR>
nnoremap <silent> <Leader>tl :TestLast<CR>
if has("nvim")
    let test#strategy = "neovim"
else
    let test#strategy = "vimterminal"
endif
```

The [vim-test](https://github.com/janko-m/vim-test) plugin provides a universal
interface to various back-end testing frameworks. This plugin allows one to
agnostically run tests for different languages and their associated testing
frameworks.

Since we are using modern versions of Vim, let's use the terminal capabilities
provided to run tests in a split terminal window.

Note, the following configuration is required to use *vim-test* with
[Create React App](https://github.com/facebook/create-react-app) projects:

```viml
let test#javascript#jest#executable = 'CI=true yarn test --colors'
```

<a id="vim-grepper"></a>vim-grepper
-----------------------------------

```viml
Plug 'mhinz/vim-grepper'
let g:grepper = {}
let g:grepper.tools = ["rg"]
runtime autoload/grepper.vim
let g:grepper.jump = 1
nnoremap <Leader>/ :GrepperRg<Space>
nnoremap gs :Grepper -cword -noprompt<CR>
xmap gs <Plug>(GrepperOperator)
```

The [vim-grepper](https://github.com/mhinz/vim-grepper) plugin is a simple Vim
interface to various text search utilities including my favourite, the
[ripgrep](https://github.com/BurntSushi/ripgrep) search utility.

For more details about *ripgrep* please read [this
post](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html).
Short summary, *ripgrep* is fast and is repository aware (aka it will skip
ignores).

Upon execution *Grepper* search matches will populate Vim's *quickfix* list
allowing easy navigation through the matches. Note, when run on a modern version
of Vim or Neovim the search will be executed asynchronously.

I have a simple mapping `<Leader>/` to invoke an interactive *Grepper* search.
The normal mode mapping `gs` will invoke a search on the word under the cursor
whilst the visual mode mapping `gs` will invoke a search on the current visual
selection.

Undotree
--------

```viml
Plug 'mbbill/undotree'
let g:undotree_HighlightChangedWithSign = 0
let g:undotree_WindowLayout             = 4
let g:undotree_SetFocusWhenToggle       = 1
nnoremap <Leader>u :UndotreeToggle<CR>
```

The [Undotree](https://github.com/mbbill/undotree) plugin visualizes your undo
history and provides easy navigation back and forth through that history. This
plugin proves handy for certain kinds of non-linear edits that may prove
unreachable via the normal `u` undo command.

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

Cheat40
-------

```vim
Plug 'lifepillar/vim-cheat40'
```

The [Cheat40](https://github.com/lifepillar/vim-cheat40) plugin is a convenient,
and customizable, 40 character wide cheat sheet. Cheat40 can display a cheat
sheet of your key mappings or exotic Vim commands for instant recall. For
example, if you keep forgetting your seldom used key mappings, just add them to
your cheat sheet and then display the cheat sheet when required.

Use the `<leader>?` mapping, or bind your own key to `:Cheat40`, to toggle the
right-hand side full height cheat sheet. Note, sections will usually be folded
to avoid needless scrolling; just go to the section of interest and unfold it.

The *Cheat40* plugin provides a default [cheat
sheet](https://github.com/lifepillar/vim-cheat40/blob/master/cheat40.txt),
however, I strongly recommend creating your own cheat sheet.

To disable the default cheat sheet provided by the plugin:

```viml
let g:cheat40_use_default = 0
```

Your cheat sheet should reside at `~/.vim/cheat40.txt`.

As an example, [here is my cheat
sheet](https://github.com/bluz71/dotfiles/blob/master/vim/cheat40.txt). Note,
formatting is important, please adhere to the 40 character column layout.

If you find yourself often looking up your `~/.vimrc` file for a particular
mapping, please instead take the time to craft a cheat sheet; that will involve
a little bit of upfront work, but the payoff will be worth it in the long run.

Tim Pope Plugins
================

A special mention should be given to [Tim Pope](https://github.com/tpope) who
has crafted some of Vim's most useful plugins. He deserves a place in the Vim
hall of fame alongside Bram Moolenaar himself.

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
nnoremap <silent> <Leader>B :Gblame<CR>
nnoremap <silent> <Leader>C :Gclog %<CR>
nnoremap <silent> <Leader>G :Gstatus<CR>
```

The [vim-fugitive](https://github.com/tpope/vim-fugitive) plugin is a Git
wrapper. I do most of my **git** work at the command line, however I find
*fugitive's* `Gblame` command to be supremely useful within Vim.

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
  through *ALE* results

- `[a` / `]a` - navigate backward and forward through the file list

- `[b` / `]b` - navigate backward and forward through the buffer list

- `[<Space>` / `]<Space>` - add a blank line above or below the current line

- `[e` / `]e` - move the current line, or visual lines, up or down

- `[p` / `]p` - linewise paste above or below the current line

The full set of mappings is documented
[here](https://github.com/tpope/vim-unimpaired/blob/master/doc/unimpaired.txt).

The *vim-unimpaired* plugin negates the need to provide your own custom set of
mappings for these types of operation.

Endwise
-------

```viml
Plug 'tpope/vim-endwise'
```

The [vim-endwise](https://github.com/tpope/vim-endwise) plugin will
automatically insert **end**, in insert mode, to code blocks for languages such
as: Ruby, Elixir, Crystal and Vim.

Projectionist
-------------

The [vim-projectionist](https://github.com/tpope/vim-projectionist) plugin,
primarily, provides infrastructure to navigate around projects. This plugin is
effectively the core of the [vim-rails](https://github.com/tpope/vim-rails)
plugin extracted into a standalone plugin.

Here is a simple configuration for
[create-react-app](https://github.com/facebook/create-react-app) projects:

```viml
Plug 'tpope/vim-projectionist'
if filereadable('src/App.js')
    " This looks like a React app.
    let g:projectionist_heuristics = {
    \  'src/App.js': {
    \    'src/components/*.js': {
    \      'type': 'component',
    \      'alternate': 'src/__tests__/components/{}.test.js'
    \    },
    \    'src/__tests__/components/*.test.js': {
    \      'type': 'test',
    \      'alternate': 'src/components/{}.js'
    \    },
    \    'src/styles/*.css': {
    \      'type': 'stylesheet',
    \      'alternate': 'src/components/{}.js'
    \    }
    \  }
    \}
    nnoremap <Leader>ec :Ecomponent<Space>
    nnoremap <Leader>es :Estylesheet<Space>
    nnoremap <leader>et :Etest<Space>
    nnoremap <Leader>a  :A<CR>
endif
```

The above configuration will result in the following commands being created:
`Ecomponent`, `Estylesheet` and `Etest`. Those commands are then mapped for
quick access. Hence, `<Leader>ec <TAB>` will list all available components, in
the status line if `wildmenu` and `wildmode` are set appropriately, allowing a
developer to quickly go to the component they wish. The `<Leader>a` mapping
provides quick switching to an *alternate* file, which will usually be the
associated test suite for the current component file.

Configuring *vim-projectionist* for other frameworks like
[Phoenix](https://phoenixframework.org) or [Ember](https://www.emberjs.com/)
should not be difficult.

Setting up this plugin does require a bit of upfront work, but once done, and
then used, you will really appreciate the navigation capabilities this plugin
provides.

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
