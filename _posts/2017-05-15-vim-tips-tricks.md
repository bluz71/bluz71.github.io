---
title: Vim Tips & Tricks
layout: default
comments: true
published: true
---

Vim Tips & Tricks
=================

As a long-term Vim user myself, here are some of my favourite *tips n' tricks*
that I have picked up along the journey.

Note, by no means is this an exclusive list, many of these *tips* will be well
known to the Vim community at large. However, even the most seasoned of Vim
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

When the normal mode command has completed you will be returned back to insert
mode where you were last editing.

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

This removes the need to tack on **g** to the end of *substitute* commands.
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

Sorting
-------

Use the `sort` command to sort a selection, say a visual selection:

```viml
:'<,'>sort
```

By default lines starting with *0-9* will be sorted before *A-Z* followed
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

Project wide substitution using *cfdo*
-------------------------------------

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

Complete a line with *Control-x Control-l*
------------------------------------------

Sometimes in insert mode you may wish to repeat an existing full line; *omni
line completion* is up to the task:

```
Control-x Control-l
```

The above is a little difficult to type, I prefer to use the following mapping:

```viml
inoremap <C-l> <C-x><C-l>
```

One only need type *Control-l* whilst in insert mode to complete the current
line by repeating an existing line.

Repeat last visual selection with *gv*
--------------------------------------

Simply enter `gv` to reselect the last visual selection.

Launch browser with *gx* command
------------------------------

Launch a browser window by moving the cursor, in normal mode, into a URL text
area and simply enter the `gx` command. I only recently came across this nice
tip, I wish I knew this one years ago!

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
:%s/old/new
:wq
```

Then use the **-es** option of Vim to execute the Vim script.

This example will execute the above *do.vim* script over all Ruby files in the
current directory tree:

```
vim -es $(find . -name '*.rb') < do.vim
```

Completion for spellings
------------------------

Vim insert mode completion, via *Control-n* and *Control-p*, can be used to
complete using dictionary words. This is useful when writing text.

Simply enable the spell option and append *kspell* to the complete options:

```viml
set spell
set complete+=kspell
```

Since I am not always editing text I prefer to toggle the above settings *on*
and *off* when I desire. I have a **Spelling** function hooked up to the **F5**
function key as seen in my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc).

Better wrapping with *breakindent*
----------------------------------

The relatively new `breakindent` indent option is an *excellent* way to wrap
long code lines. When set, long lines will wrap *with* an indentation thus
preserving the clean indented look of code.

Note, a very modern version of Vim, and Neovim, will be required. All the more
reason to upgrade!

Set the following options to wrap long lines with indentation:

```viml
set breakindent
set showbreak=\\\\\
```

Smarter *j* and *k* navigation
------------------------------

When any form of wrapping is in effect, I recommend `breakindent` as noted
above, then it is natural to convert the **j** and **k** movement commands from
strict linewise movements to onscreen display line movements via the **gj** and
**gk** commands. However when preceded with a count, useful when
`relativenumber` is in effect, then we *want* to go back to strict linewise
movements.

The following mapping achieves both aims, display line movements unless
preceded by a count:

```viml
nnoremap <expr> j v:count ? 'j' : 'gj'
nnoremap <expr> k v:count ? 'k' : 'gk'
```

Set *infercase*
---------------

Most folks set `ignorecase` when searching. However that option does not play
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

Make dot work over visual line selections
-----------------------------------------

By default the **.** repeat operator does not work on visual selections.

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

Autosave and autoread
-----------------------

Add the following snippet to your *vimrc* to enable functional autosave and
autoread behaviour in Vim:

```viml
set autoread

augroup autoSaveAndRead
    autocmd!
    autocmd TextChanged,InsertLeave,FocusLost * silent! wall
    autocmd CursorHold * silent! checktime
augroup END
```

Autosave will automatically save to disk the currently edited buffer upon
leaving *insert* mode as well as after a text edit has occurred.

Autoread will automatically update an open buffer if it has been changed
outside the current edit session, usually by an external program.

With these `autocmds` in effect one will rarely need to manually trigger an
explicit save, which will be of benefit if one context switches often, for
example a web developer switching between an edit terminal window and a test
browser.
