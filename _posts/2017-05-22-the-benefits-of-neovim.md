---
title: The Benefits of Neovim
layout: default
---

The Benefits of Neovim
======================
[Neovim](https://neovim.io/) is a recent fork of the [Vim](http://www.vim.org/)
text editor. Started in 2014, Neovim bills itself as *literally the future of
Vim*.

A question I often see asked in various Vim places is *why Neovim, what does it
have to offer, I am completely happy with Vim?*

This post will try and explain the benefits, as I see it, with Neovim.
Currently the user visible benefits are small, but in the longer term the
benefits should be substantial.

Note however, this should post not be interpreted as any slight on Vim; far
from it, I still use it daily myself.

Lastly, I am assuming the reader is already a Vim user with an functional setup
already.

Setup
=====
Neovim does require some pre-configuration.

vimrc
-----
Firstly, I find having one configuration that works for both Vim and Neovim
extremely useful. Hence, my combined configuration still lives in the
traditional ```~/.vimrc``` file.

To connect Neovim to the ```~/.vimrc``` create a ```~/.config/nvim/init.vim```
file with the following content:

```viml
set runtimepath+=~/.vim,~/.vim/after
source ~/.vimrc
```

Inside the *vimrc* file one uses the following if type conditional for Neovim
specific code:

```viml
if has("nvim")
  " Neovim specific  
else
  " Traditional Vim
endif
```

Use my [vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc) as an
example of a combined configuration.

Cut n' paste
------------
Neovim requires an external clipboard provider for functional cut n' paste
support.

Basically that means making sure the following utilities is installed on your
platform:

- ```pbcopy/pbpaste``` on *macOS*, this is usually provided by default
- ```xclip``` or ```xsel``` on Linux, install it via your default package
  manager if not installed by default.

Current user-visible Neovim benefits
====================================
This is the list of small, but still worthwhile, user-visible enchancements
that Neovim provides over and above Vim. Note, I am only listing the
enhancements that I appreciate, a full list of  Neovim differences is listed
[here](https://neovim.io/doc/user/vim_diff.html#nvim-features).

Cursor shape changing
---------------------
This feature is the one I appreciate most; Neovim provides terminal cursor
shape changing out-of-the-box, no configuration needed.

Basically that means the cursor shape will differ depending on mode:
- A box in *normal* mode
- An I-beam in *insert* mode
- An underscore in *replace* mode

This works in *iTerm2* (on Mac) and *xterm* (on Linux). Best of all the cursor
shape change will only effect the Neovim edit session unlike Vim's crude
```t_SI/t_EI``` based cursor shape change functionality which will bleed into
all panes and windows of a *tmux* session.

*Whitespace* highlight group
----------------------------
Another small, but extremely useful, enhancement is the
[Whitespace](https://github.com/neovim/neovim/pull/6367) highlight group.

Vim provides only a single highlight group ```SpecialKey``` that is used to
visibly highlight special characters such as *space*, *tab* and *return*
characters (among others). 

I like seeing leading whitespaces as I type them in a low contrast color.
However, when I want to see trailing *returns*, done by toggling the ```list```
option I like to use a high contrast color. In Vim this is a challenge since it
only provides a single highlight group for both contexts.

My own [moonfly](https://github.com/bluz71/vim-moonfly-colors) *colorscheme* is
```Whitespace``` aware.

Substitution previews via *inccommand*
--------------------------------------
Neovim provides live substitution previews.

To enable, add the following to your ```vimrc```:
```viml
if has("nvim")
    set inccommand=nosplit
endif
```

As you type a substitution the results will be immediately updated in the edit
window. This feature is best highlighted in this video:

[![Incremental substitution](https://i.ytimg.com/vi/sA3z6gsqOuw/hqdefault.jpg?custom=true&w=336&h=188&stc=true&jpg444=true&jpgq=90&sp=68&sigh=Zr-su2aAfVYE-tawG-lB3_-YPQ0)](http://www.youtube.com/watch?v=sA3z6gsqOuw)

Inbuilt terminal
----------------
Neovim provides a fully fledged terminal, invoked via ```:terminal``` (or
```:te``` shorthand).

This provides a *tmux-lite* alternative for those not wanting or needing a full
*tmux* setup.

As a *tmux* user myself the Neovim terminal is still useful for:

- easy text copying out of the terminal and into an edit session
- running tests in a split terminal

Copy large amounts of text from a tmux window into a Vim session can be fiddly.
The inbuilt Neovim terminal however makes this operation a breeze via
traditional Vim visual selection and yanking.

The [vim-test](https://github.com/janko-m/vim-test) plugin provides direct
Neovim terminal support when running tests (for example Rails RSpec tests). 

In Vim, when invoking a test through the *vim-test* plugin, the edit session
will context switch back to the host terminal and run the test. Hence, it is not
possible to simultaneously view the test code and invoked test result. On the
other hand in Neovim such a test will be invoked in a split terminal window
below the current edit edit window hence both test code and test result are
seen next to each other. This greatly enhances the code, test, fix cycle.

*silent make*
-------------
In Vim invoking an external ```make``` tool such as ```eslint``` will result in
a visible screen flash even when ```silent``` is specified. This results due to
Vim context switching to the terminal to invoke the tool and then switching
back to Vim with the results.

Neovim has no such flashing since it invokes such system commands in the
background and uses pipes to connect results back to the edit session. The lack
of the terminal flash simply looks and feels better.


*Control-q* mapping
-------------------

*Alt* based mappings
--------------------
