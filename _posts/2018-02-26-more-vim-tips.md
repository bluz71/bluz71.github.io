---
title: A Couple More Vim Tips
layout: default
comments: true
published: true
---

A Couple More Vim Tips
======================

Vim enlightenment is a never ending process.

Following on from my 2017
[Vim Tips & Tricks](https://bluz71.github.io/2017/05/15/vim-tips-tricks.html)
article, this post will list a couple more tips that I have added to my
tool belt since that post.

As per usual, many *tips n' tricks* are baked into my
[vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc).

Persistent Undo
---------------

Vim has had persistent undo capability since version 7.3. Due to apathy I only
recently became annoyed enough about lost *undos* that I enabled it, now I
can't imagine living without it.

By default Vim does not enable persistent undo meaning change history will only
be saved for the active buffer, changing to another file results in change
history starting from scratch even when navigating back to the original file.
This is how one loses *undos*.

Persistent undo remedies this issue by saving change history to disk.
Navigating between files or even exiting and returning to Vim will reference
these *saved* change histories allowing undo and redo to naturally work as one
would expect.

I favour saving these changes to fast temporary storage which means change
history will only be saved for the uptime of the workstation, which in my case
is all I need.

Add this snippet to your *vimrc:*

```viml
let s:undoDir = "/tmp/.undodir_" . $USER
if !isdirectory(s:undoDir)
    call mkdir(s:undoDir, "", 0700)
endif
let &undodir=s:undoDir
set undofile
```

The above configuration will store Vim change histories in a private
subdirectory of  `/tmp`, which on modern Linux and Mac workstations is carved
from RAM. If one desires to persist *undos* across reboots then please replace
`/tmp` with a directory in your home directory such as `$HOME/.vim/undodir` for
example. Note, in the latter case you may need to periodically clean the undo
directory since it will accumulate changes forever.

Point in time undoing and redoing
---------------------------------

The *undo* (`u`) and *redo* (`ctrl-r`) commands allow for rollback and
roll forward through edit history. Vim actually stores these changes in a tree
structure, changes can fork off branches, some of which may be unaccessible
using the standard undo and redo commands which traverse only along a single
linear path.

Plugins such as [Gundo](https://github.com/sjl/gundo.vim) and
[undotree](https://github.com/mbbill/undotree) provide undo visualisation and
navigation of the complete change tree.

However, a simpler approach than navigating around the change tree is to simply
undo to a point in time using the `earlier` and `later` commands.

Say you wanted to undo to 10 minutes ago then enter:

```
:earlier 10m
```

Or to redo forward 50 seconds:

```
:later 50s
```

I strongly recommend enabling persistent undo to extract maximum benefit for
the `earlier` and `later` commands.

Incrementing and decrementing
-----------------------------

Vim provides the very useful `ctrl-a` and `ctrl-x` commands to increment and
decrement numbers.

In normal mode, entering `ctrl-a` will increment the first number to the right
of the cursor on the current line. Preceded by a count `ctrl-a` will increment
by the count amount. The `ctrl-x` command will do the reverse operation of
decrementing.

Personally, I don't like increment and decrement to take octal and hex
numbers into account, I prefer an increment on `07` to result in `08` and not
`010`.

Blanking the `nrformats` option will force decimal-based arithmetic:

```viml
set nrformats=
```

Mapping the `+` and `-` keys to increment and decrement feels more natural than
using the `ctrl` based defaults:

```viml
noremap + <C-a>
noremap - <C-x>
```

Simply hit `+` or `-` to adjust numbers on the current line.

A very useful Vim increment capability is the `g ctrl-a` command which can
increment a sequence of numbers in a vertical visual selection.

Say for example you have the following three lines in a visual selection (aka
using `ctrl-v` block selection):

```
0
0
0
```

Entering `g ctrl-a` will result in:

```
1
2
3
```

Prepending a count to the `g ctrl-a` command will determine the step size of
the increments. The `g ctrl-x` command will do decrementing instead of
incrementing.

Again, I like to use `+` and `-` instead of `ctrl-a` and `ctrl-x`, hence I
use these mappings instead for visual sequence incrementing and decrementing:

```viml
xnoremap + g<C-a>
xnoremap - g<C-x>
```

Whether you use the mappings listed above or the defaults of `ctrl-a` and
`ctrl-x` incrementing and decrementing numbers in Vim is very easy.
