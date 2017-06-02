---
title: The Benefits of Neovim
layout: default
comments: true
---

The Benefits of Neovim
======================
[Neovim](https://neovim.io) bills itself as *literally the future of Vim*. This
post will delve into Neovim to see if it lives up to that billing.

Firstly, for those that haven't been following, Neovim is a modern fork of the
[Vim](http://www.vim.org/) text editor that was started in 2014. 
If you *why Neovim, what does it
have to offer, I am completely happy with Vim?*

This post will try and explain the advantages, as I see them, with Neovim. It
may also help explain why Neovim even exists at all. I do feel sorry sometimes
for the Neovim leads who often need to answer *why Neovim, Vim is perfect
right?*

Do note however, this should post not be interpreted as any slight on Vim; far
from it, I still use Vim daily myself.

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
I have a *Control-q* mapping to safely quit my Vim session:
```viml
noremap <C-q> :confirm qall<CR>
```

In terminal Vim this mapping does not work, one needs to launch Vim via ```stty
-ixon && vim```. This prevents the terminal from freezing/unfreezing with the
*Control-s/Constrol-q* key combinations.

However, in Neovim, one does not need to do anything special.
*Control-s/Constrol-q* mappings are available for use however you choose.

*Alt* based mappings
--------------------
Somewhat related to the above, I assume, *Alt* based mapping work as one would
expect in Neovim.

For example, the following mapping to create a tab via *Alt-t*, works in Neovim
but not in terminal Vim:
```viml
noremap <silent> <A-t> :$tabnew<CR>
```

It is possible to configure the above *Alt-t* mapping in terminal Vim, but it
involves coding in the specific terminal sequence such as:
```viml
noremap t :$tabnew<CR>
```

This is not as nice nor as intuitive.

Architectural Neovim benefits
=============================
In the grand scheme the above list really are incremental improvements. The
bigger Neovim benefits right now are occurring unders the covers and in the
community, those being:

- Massively cleaned up and modernized code base. See
  [this](https://geoff.greer.fm/2015/01/15/why-neovim-is-better-than-vim/) post
  for details. Note, this cleanup includes a comprehensive test suite providing
  confidence to the Neovim development community to expand, enhance and
  refactor with confidence.

- A fully fledged development community that can survive the comings and goings
  of key developers. The initial Neovim lead,  Thiago de Arruda, did in fact
  depart Neovim yet the project has continued on just fine. Can Vim survive if
  Bram Moolenaar is no longer around, who will maintain the legacy code in the
  long term?

- Neovim's remote API functionality provides a pathway where Neovim can be a true
  component. This Neovim component theoretically could exist in an
  [Atom](https://atom.io) or
  [Thunderbid](https://www.mozilla.org/en-US/thunderbird) session. This same
  *client/server* API has lead to the development of a host of Neovim
  [clients](https://github.com/neovim/neovim/wiki/Related-projects#gui).

- Integration of [Lua](https://www.lua.org/) interpreter into the runtime to
  run alongside the Vimscript. Lua is a far nicer language than VimL and with
  [LuaJIT](http://luajit.org) it is a language that should run orders of
  magnitude faster as well. This should allows Neovim plugin authors far
  greater scope to offer complex functionality with great performance.

- Asynchronous support is now less of a differentiator between Neovim and Vim
  than it used to be. The relatively new Vim 8 includes JSON based asynchronous
  support whilst Neovim has long had a
  [MessagePack](https://github.com/msgpack-rpc/msgpack-rpc) asynchronous API.
  [This](https://www.reddit.com/r/neovim/comments/58nrwv/neovim_api_comparison_with_vim_channels/)
  thread explains the two approaches.

Personal Wishlist
=================
If I was king here is what I would want:

- Encryption support somewhat similar to Vim's
  [blowfish2](http://vim.wikia.com/wiki/Encryption) feature.  Neovim stripped
  out all direct encryption support. I understand why they did that, and I am
  not asking they ask they directly add encryption handling back into the core
  editor. However, it should be possible to seamlessly delegate to an external
  encryption provider similar to how Neovim delegates to an external clipboard
  provider. Such an encryption provider could be [GPG](https://gnupg.org/).
  Something like the [vim-gnupg](https://github.com/jamessan/vim-gnupg), but
  built-in is what I would like.

- Indent guide markers. Both Sublime and Atom editors provide guide markers, in
  Vim one can achieve a similar result using the
  [indentLine](https://github.com/Yggdroot/indentLine) plugin. However, this
  plugin is quirky and its performance at times is problematic. My hope would
  be guide markers would be built into the core editor similar to how
  ```colorcolumn``` is built directly into Vim.

- AST language aware syntax highlighters, whether that be in the core or as
  plugins. Currently highlighting is regular expression based, historically
  this has been good enough and like will remain good enough it does have it's
  issues. On slower machines, especially when ```relativenumber``` is enabled
  this regular expression based highlighting can lead to performance
  [issues](https://github.com/vim/vim/issues/282), but even worse more than
  that, the syntax highlighting can often get tricked out. I am sure every Vim
  user has experienced that.
  [This](https://github.com/neovim/neovim/issues/719) issue is pertinent.

Summary
=======
Is *Neovim is the future of Vim*? I say an emphatic **yes**.

The cleaned up codebase, the development community, the architectural changes
mean that Neovim will innovate at a faster rate than Vim. Every Vim user should
want Neovim to survive and thrive, it is the future of Vim.
