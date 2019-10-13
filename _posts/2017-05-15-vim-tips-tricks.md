---
title: Vim Tips & Tricks
layout: default
comments: true
published: true
---

Vim Tips & Tricks
=================

**UPDATED JULY 2019**

As a long-term Vim user myself, here are some of my favourite *tips n' tricks*
that I have picked up along the journey.

Note, by no means is this an exclusive list, many of these *tips* will be
well-known to the Vim community at large. However, even the most seasoned of Vim
users can sometimes learn a *trick* or two from posts like this.

Quite a few of these *tips n' tricks* are baked into my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc).

Set *relativenumber*
--------------------

I encourage Vim users to enable relative line numbering. This setting makes it
easy to figure out how many lines up or down you have to jump, via **j** and
**k** movements, to get to where you want to go when in normal mode.

```viml
set relativenumber
```

One time normal mode command whilst in insert mode
--------------------------------------------------

Whilst in insert mode you can quickly execute a single normal operation with:

```
Control-o <<command>>
```

Once the normal mode command has completed you will be returned to insert mode
where you were last editing.

A useful example would be centering the text being edited in the current window:

```
Control-o zz
```

Expression register in insert mode
----------------------------------

Use the expression register, whilst in insert mode, to edit-in simple math
values.

```
Control-r= <<math expression>>
```

An example could be:

```
Control-r= 43 + 139 + 761
```

The inserted value would be *943*.

Set global replacement as the default
-------------------------------------

Force Vim to always do global substitutions.

```viml
set gdefault
```

This removes the need to tack on **g** at the end of *substitute* commands.
Once `gdefault` is set the following will be a global substitute command:

```
:%s/old/new
```

All the following substitute examples will assume `gdefault` has been set.

Substitute in a visual block
----------------------------

Do the following to substitute *old* with *new* only within a rectangular
visual block, that being a `Control-v` style visual selection:

```
:'<,'>s/\%Vold/new
```

Count the number of pattern matches
-----------------------------------

Use the `substitute` command to count the number of matches for a particular
pattern.

First, execute a search:

```
/<<some-pattern>>
```

Then execute a counting substitute:

```
:%s///n
```

Note, this particular form of *substitute* will **not** actually substitute
anything, it will instead just print out the number of matches.

**UPDATE**: In the comments *Smylers* suggested this even more compact variant:

```
:%~n
```

Sorting
-------

Use the `sort` command to sort a selection, say a visual selection:

```viml
:'<,'>sort
```

By default, lines starting with *0-9* will be sorted before *A-Z* followed
lastly by lines starting with *a-z*.

Use the `i` option to ignore case when sorting, more often than not you want to
do this:

```viml
:'<,'>sort i
```

Use a `!` to reverse the sort:

```viml
:'<,'>sort! i
```

Lastly the `u` option can be used to remove duplicates much like the **uniq**
system command:

```viml
:'<,'>sort u
```

Diffing
-------

```viml
if has('nvim-0.3.2') || has("patch-8.1.0360")
    set diffopt=filler,internal,algorithm:histogram,indent-heuristic
endif
```

Vim, starting with patch 8.1.0360, and Neovim, starting with version 0.3.2,
incorporates Git's [xdiff](https://github.com/git/git/tree/master/xdiff) library.

This library can improve [diffing](https://github.com/vim/vim/pull/2732) within
Vim for certain kinds of diff. With this enhancement, there is now no need to
install [diff-specific plugins](https://github.com/chrisbra/vim-diff-enhanced).

I like and recommend the
[histogram](http://download.eclipse.org/jgit/docs/jgit-2.0.0.201206130900-r/apidocs/org/eclipse/jgit/diff/HistogramDiff.html)
algorithm with [indent
heuristic](https://github.com/libgit2/libgit2/commit/19f1a8e6f289b07389d525a12a13a4aaeaabe443).

<a id="cfdo"></a>Project wide substitution using *cfdo*
-------------------------------------------------------

Historically it has been awkward to carry out multi-file substitutions in
Vim. Many possibilities exist, some involving `argo`, others involving
**sed**, but all are convoluted and none are elegant.

The new `cfdo` command is ideal for substituting, or refactoring in programmer
speak, across multiple files. The `cfdo` command allows a normal mode command,
such as a substitute, to be invoked only on the files in the *quickfix* list.

Note, `cfdo` is only available in relatively recent versions of Vim or Neovim.
Please upgrade to Vim 8 or the newest version of Neovim.

To carry out a `cfdo` substitute one must first populate the *quickfix* list
with a candidate set of files containing the *term* wanting to be refactored.

Using *vimgrep:*

```
:vimgrep oldterm **
```

Or using the excellent [ripgrep](https://github.com/BurntSushi/ripgrep)
utility via the [vim-grepper](https://github.com/mhinz/vim-grepper) plugin:

```
:GrepperRg -i oldterm
```

From there one simply executes the desired substitution over the list of files
in the *quickfix* list.

Using standard Vim substitution:

```
:cfdo %s/oldterm/newterm/ | update
```

Or using Tim Pope's superb case-smart
[Abolish](https://github.com/tpope/tpope-vim-abolish) plugin:

```
:cfdo %S/oldterm/newterm/ | update
```

**UPDATE (MAR 2019)**: This [find & replace helpers for Vim
post](https://bluz71.github.io/2019/03/11/find-replace-helpers-for-vim.html)
may be of interest.

Substitute word under cursor and dot repeat
-------------------------------------------

```viml
nnoremap <silent> <Leader>c :let @/='\<'.expand('<cword>').'\>'<CR>cgn
xnoremap <silent> <Leader>c "sy:let @/=@s<CR>cgn
```

The relatively new `gn` command allows easy operation on the *next* match of a
completed search. These `<Leader>c` normal and visual mode mappings make use of
`gn` to provide easy *word-under-cursor* or *visual-selected-term* substitution,
aka in-file refactoring. Best of all simply use `.` (dot) to repeat that
substitution for the next match instead of `n.` as has usually been necessary in
Vim when doing such changes.

Star search that does not move the cursor
-----------------------------------------

The `*` operator will find and navigate to the next instance of the word under
the cursor. The
[visual-star-search](https://github.com/nelstrom/vim-visual-star-search) plugin
expands that paradigm to visual selections.

But in certain instances one may want to search for the word under the cursor or
the current visual selection **but** not move to the next instance. For instance
one may wish to only highlight matches.

These `g*` mappings will behave like `*` whilst keeping the cursor where it
currently is.

```viml
nnoremap <silent> g* :let @/='\<'.expand('<cword>').'\>'<CR>
xnoremap <silent> g* "sy:let @/=@s<CR>
```

Complete a line with *Control-x Control-l*
------------------------------------------

Sometimes in insert mode you may wish to repeat an existing full line; *line
completion* is up to the task:

```
Control-x Control-l
```

The above is a little difficult to type, I prefer to use the following mapping:

```viml
inoremap <C-l> <C-x><C-l>
```

One only need type *Control-l* whilst in insert mode to complete the current
line by repeating an existing line.

Dictionary complete current word with *Control-x Control-k*
-----------------------------------------------------------

Complete the current word with *dictionary completion* whilst in insert mode:

```
Control-x Control-k
```

This requires that the `dictionary` option be set, for Unix-based systems I
recommend:

```viml
set dictionary=/usr/share/dict/words
```

If that key combination is a little difficult to type, then it may be worthwhile
to set the following mapping:

```viml
inoremap <C-d> <C-x><C-k>
```

One only need type *Control-d* (d for dictionary) whilst in insert mode to
complete the current word by using dictionary-based completion.

Repeat last visual selection with *gv*
--------------------------------------

Simply enter `gv` to reselect the last visual selection.

Launch browser with *gx* command
------------------------------

Launch a browser window by moving the cursor, in normal mode, into a URL text
area and simply enter the `gx` command. I only recently came across this nice
tip, I wish I knew this years ago!

Delete all lines containing pattern
-----------------------------------

Use the *global* command with the *delete* option to remove all lines that
contain the desired pattern.

```
:g/<<some-pattern>>/d
```

Delete all lines not containing pattern
---------------------------------------

Use the *vglobal* command to achieve the opposite effect, that is to delete
all lines **not** containing the pattern:

```
:v/<<some-pattern>>/d
```

Vim as a *sed* replacement
--------------------------

A Vim script file can be used as a *poor man's* **sed** replacement.

Create a file with the desired operations. For example this file, *do.vim*,
will substitute *new* for *old:*

```
%s/old/new
wq
```

Then use the **-es** option of Vim to execute the Vim script.

This example will execute the above *do.vim* script over all Ruby files in the
current directory tree:

```
vim -es **/*.rb < do.vim
```

If one uses Bash, please enable `globstar` in your `~/.bashrc` file via the
`shopt -s globstar` statement. For more details please read [Bash shell tweaks
& tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html).

Better wrapping with *breakindent*
----------------------------------

The relatively new `breakindent` indent option is an *excellent* way to wrap
long code lines. When set, long lines will wrap *with* an indentation thus
preserving the clean indented look of code.

Note, a modern version of Vim, and Neovim, will be required. All the more reason
to upgrade!

Set the following options to wrap long lines with indentation:

```viml
set breakindent
set breakindentopt=shift:2
set showbreak=\\\\\
```

**UPDATE (MAR 2019)**: When using a modern font, such as
[Iosevka](https://github.com/be5invis/iosevka), then I recommend using a
bent-arrow glyph as the `showbreak`:

```viml
set showbreak=↳
```

Smarter *j* and *k* navigation
------------------------------

When any form of wrapping is in effect, I recommend `breakindent` as noted
above, then it is natural to convert the **j** and **k** movement commands from
strict linewise movements to onscreen display line movements via the **gj** and
**gk** commands. However, when preceded with a count, useful when
`relativenumber` is in effect, then we *want* to go back to strict linewise
movements.

The following mapping achieves both aims, display line movements unless
preceded by a count:

```viml
nnoremap <expr> j v:count ? 'j' : 'gj'
nnoremap <expr> k v:count ? 'k' : 'gk'
```

**UPDATE**: In the comments *p1xelHer0* suggested this enhancement:

```viml
nnoremap <expr> j v:count ? (v:count > 5 ? "m'" . v:count : '') . 'j' : 'gj'
nnoremap <expr> k v:count ? (v:count > 5 ? "m'" . v:count : '') . 'k' : 'gk'
```

Similar to the first version except this version will automatically save
movements larger than 5 lines to the *jumplist*. Use `Ctrl-o`/`Ctrl-i` to
navigate backwards and forwards through the *jumplist*. Yes, a nice
enhancement, thanks *p1xelHer0*.

Set *infercase*
---------------

Most folks set `ignorecase` when searching. However, that option does not play
nicely with completion which will then completely ignore case. Set the
`infercase` option for smarter completions that will be case aware:

```viml
set infercase
```

Improve performance for files with long lines
---------------------------------------------

Very long lines will cause performance problems with Vim. One of *the* main
culprits for this performance issue is the syntax highlighter. I recommend only
syntax highlighting the first 200 characters of each line.

```viml
set synmaxcol=200
```

The `relativenumber` settings can also cause problems with files with long
lines. I suggest having a quick toggle to disable `relativenumber` when
required.

Enable *wildmenu* and *wildmode*
--------------------------------

The `wildmenu` option makes setting an option, or opening new files via `:e`, a
breeze with **TAB** expansion.

I recommend these options.

```viml
set wildmenu
set wildmode=full
```

For example, once set you can quickly tab complete an option via:

```
:set auto<<TAB>>
```

The **TAB** invocation will then list the available options in the status line,
use **TAB** again to quickly scroll through the options (left and right arrows
also work). Note, when opening a file via `:e` use up and down arrows to
enter or leave directories.

Fast alternate buffer switching
---------------------------------

Hit `Control-6` to switch to the previously edited file, hit `Control-6` again
to switch forward to the original file.

Use this handy key sequence to toggle between the last two buffers.

Fast buffer switching via the *wildmenu*
----------------------------------------

Expanding on the previous tips, use `wildmenu` to quickly switch buffers.

```viml
set wildcharm=<Tab>
nnoremap <Leader><Tab> :buffer<Space><Tab>
```

Hitting **Leader + TAB** in normal mode will list the open buffers in the status
line via `wildmenu`, use **TAB** or arrow-keys to navigate the choices and then
hit **Enter** to select the desired buffer.

Note, do **NOT** use raw **TAB** as the normal-mode mapping, as I initially
did, since it will break `Control-i` inward navigation.

Make dot work over visual line selections
-----------------------------------------

By default, the **.** repeat operator does not work on visual selections.

Add this mapping in your *vimrc* file to enable a simple form of dot repetition
over visual line selections.

```viml
xnoremap . :norm.<CR>
```

It is recommended that only simple operations that start from the beginning of
a line be dot repeated. For example `ct=` (change up until =) or `5dw`
(delete the first five words of the line) are good candidates for visual dot
repetition.

Execute a macro over visual line selections
-------------------------------------------

Somewhat related to the previous tip is having the ability to run a macro only
over a visual line selection.

I use the `qq` command to record a macro into the *q register*. I then setup
the following `Q` mapping:

```viml
xnoremap Q :'<,'>:normal @q<CR>
```

Typing `Q` with a visual line selection in effect will execute the `q` macro
over just the selected lines.

Automatically equalize splits when Vim is resized
-------------------------------------------------

It is an annoyance to have to manually equalize Vim splits that have been
munged by some type of resize event, for example zooming in and out a *tmux*
pane.

The following `autocmd` will take care of split equalization for you:

```
autocmd VimResized * wincmd =
```

Autoread
--------

Add the following snippet to your *vimrc* to enable functional autoread
behaviour in terminal Vim:

```viml
set autoread

augroup autoRead
    autocmd!
    autocmd CursorHold * silent! checktime
augroup END
```

Autoread will automatically update an open buffer if it has been changed
outside the current edit session, usually by an external program.

A natural companion for this tip is the autosaving
[vim-auto_save](https://github.com/907th/vim-auto-save) plugin. For more
details please refer to the appropriate section of the [Vim Plugins I
Like](https://bluz71.github.io/2017/05/21/vim-plugins-i-like.html) post.

Improve scroll performance
--------------------------

Add the following snippet to your *vimrc* to improve scroll performance for
certain file types:

```viml
augroup syntaxSyncMinLines
    autocmd!
    autocmd Syntax * syntax sync minlines=2000
augroup END
```

Computing syntax highlight information on the fly whilst scrolling can be slow
in Vim. How much of a file Vim highlight's is determined by the `filetype` in
use. For some languages the value of `minlines` is small hence Vim can
occasionally stutter whilst scrolling.

The above setting can markedly improve scroll performance for certain file
types. Also, I have yet to encounter a detriment with this setting on modern
machines.
