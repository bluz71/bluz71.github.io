---
title: Current treats & future wants of Neovim
layout: default
comments: true
published: true
---

Current treats & future wants of Neovim
=======================================

[Neovim](https://neovim.io) bills itself as *literally the future of Vim*. A
bold claim indeed.

For those that haven't been following, Neovim is a modern fork of the
[Vim](http://www.vim.org) text editor that began in 2014. The
[Neovim charter](https://neovim.io/charter) outlines the vision of the project
quite succinctly.

Having used *Neovim* for a little while now I decided it would be worthwhile to
write a post listing the treats, pains and wants, as I see them, with Neovim.

Note, the features of Neovim discussed below pertain to Neovim version 0.2 as
against Vim 8 circa June 2017.

Installation
============

[This page](https://github.com/neovim/neovim/wiki/Installing-Neovim) contains
installation details for Neovim.

Configuration
=============

Setting up Neovim does require some minor configuration since Neovim, by
default, follows the
[XDG base directory specification](https://standards.freedesktop.org/basedir-spec/basedir-spec-latest.html)
unlike Vim which does not

vimrc
-----

I find having *one* configuration that works for both Vim and Neovim extremely
beneficial. Hence, my combined configuration still lives in the traditional
*~/.vimrc* file; basically I abstract away the *XDG base directories.*

To connect Neovim to *~/.vimrc* create a *~/.config/nvim/init.vim* file with
the following content:

```viml
set runtimepath^=~/.vim runtimepath+=~/.vim/after
let &packpath = &runtimepath
source ~/.vimrc
```

Inside a *vimrc* file use the following `if` style conditional for any Neovim
specific statements:

```viml
if has("nvim")
  " Neovim
else
  " Traditional Vim
endif
```

[My vimrc](https://github.com/bluz71/dotfiles/blob/master/vimrc) is an example
of a combined configuration.

Cut n' paste
------------

Neovim requires an external clipboard provider for seamless *cut n' paste.*

That means making sure the following utilities are installed:

- `pbcopy/pbpaste` on *macOS*, this is usually provided by default
- `xclip` or `xsel` on Linux, install either with your system package
  manager if not already installed

Compatibility
-------------

Once setup as such, Neovim and Vim will be quite interchangeable.

In my experience, 3rd-party plugin compatibility is excellent and for the most
part users will rarely be able to tell the difference between both editors. So
why even bother with Neovim at all? Read on and find out.

Minor Neovim features
=====================

Note, a full list of Neovim feature differences to Vim is listed
[here](https://neovim.io/doc/user/vim_diff.html#nvim-features).

Cursor shape changing
---------------------

Neovim provides terminal cursor shape changing out-of-the-box, no configuration
needed.

Basically that means the cursor shape will differ depending on mode:

- A box in *normal* mode
- An I-beam in *insert* mode
- An underscore in *replace* mode

This works in *iTerm2* (on macOS) and *xterm* (on Linux). Best of all the
cursor shape change will only effect the current Neovim edit session unlike
Vim's crude `t_SI/t_EI` based cursor shape change functionality which will
bleed into all panes and windows of a *tmux* session.

*Whitespace* highlight group
----------------------------

Another small, but useful, enhancement is the
[Whitespace](https://github.com/neovim/neovim/pull/6367) highlight group.

Vim provides only a single highlight group `SpecialKey` that is used to
visibly highlight special characters such as *space*, *tab* and *return*
characters (among others).

I like seeing leading whitespaces as I type them in a low contrast color.
However, when I want to see trailing *returns*, done by toggling the `list`
option, I like to use a high contrast color. In Vim this is a challenge since
it only provides a single highlight group for both contexts. Neovim provides
two highlight groups to solve this dilemma.

Note, my own [moonfly](https://github.com/bluz71/vim-moonfly-colors)
*colorscheme* is *Neovim* `Whitespace` aware.

Substitution previews with *inccommand*
---------------------------------------

Neovim provides live substitution previews.

To enable, add the following to your *vimrc:*

```viml
if has("nvim")
    set inccommand=nosplit
endif
```

As you type a substitution the results will immediately be visible in the edit
window. This feature is best highlighted in this video:

[Neovim incremental substitution](http://www.youtube.com/watch?v=sA3z6gsqOuw)

*silent make*
-------------

In Vim invoking an external `makeprg` tool such as **eslint** will result in
a visible screen flash even when `silent` is specified. This is due to
Vim context switching to the terminal to invoke the tool and then switching
back to Vim with the results.

Neovim has no such flashing since it invokes such system commands in the
background and uses pipes to connect results back to the edit session.

Note however, modern async-linting plugins such as
[Neomake](https://github.com/neomake/neomake) and
[ALE](https://github.com/w0rp/ale) render this `makeprg`/ `errorformat`-based
tool launching somewhat moot these days.

*Control-q* mapping
-------------------

I have a *Control-q* mapping to safely quit my Vim session:

```viml
noremap <C-q> :confirm qall<CR>
```

In terminal Vim this mapping does not work unless one launches Vim via `stty
-ixon && vim`. This prevents the terminal from freezing/unfreezing with the
*Control-s/Constrol-q* key combinations.

However, in Neovim, it is not necessary to do anything special.
*Control-s/Constrol-q* mappings are available for use however you see fit, no
special `stty` magic is needed.

*Alt* based mappings
--------------------

Somewhat related to the above I assume, `Alt` based mapping work as one would
expect in Neovim.

For example, the following mapping to create a new tab page, via `Alt-t`, works
in Neovim but not in terminal Vim:

```viml
noremap <silent> <A-t> :$tabnew<CR>
```

Note, it is possible to configure an `Alt-t` mapping in terminal Vim, but it
involves coding in the specific terminal sequence, this is not as nice nor as
intuitive as Neovim's approach.

Medium Neovim features
======================

Inbuilt terminal
----------------

Neovim provides a fully fledged built-in terminal, invoked by the `:terminal`
command (or the `:te` shorthand).

This feature provides a *tmux-lite* alternative for those not wanting or
needing a full [tmux](https://tmux.github.io) setup.

Tip, I find the following settings result in a nicer Neovim terminal
experience:

```viml
if has("nvim")
  " Make escape work in the Neovim terminal.
  tnoremap <Esc> <C-\><C-n>

  " Make navigation into and out of Neovim terminal splits nicer.
  tnoremap <C-h> <C-\><C-N><C-w>h
  tnoremap <C-j> <C-\><C-N><C-w>j
  tnoremap <C-k> <C-\><C-N><C-w>k
  tnoremap <C-l> <C-\><C-N><C-w>l

  " I like relative numbering when in normal mode.
  autocmd TermOpen * setlocal conceallevel=0 colorcolumn=0 relativenumber

  " Prefer Neovim terminal insert mode to normal mode.
  autocmd BufEnter term://* startinsert
endif
```

As an ongoing *tmux* user myself I currently find the Neovim terminal useful
for:

- easy text copying out of a terminal and into an edit session
- running tests in a split terminal

Copying large amounts of text from a tmux window into a Vim session can be
fiddly.  The Neovim terminal however makes this operation a breeze via
traditional visual selection and yanking.

The [vim-test](https://github.com/janko-m/vim-test) plugin provides direct
Neovim terminal support when running tests. In Vim, by default, when invoking a
test through the *vim-test* plugin, the edit session will context switch to the
host terminal and run the test. In that case it is not possible to
simultaneously view the test code and invoked test result unless one uses
something like [vimux](https://github.com/benmills/vimux). On the other hand,
in Neovim such a test will be invoked in a split-below terminal window (if
set), hence both test code and test result are visible next to each other. This
greatly enhances the code, test, fix cycle.

It is also my understanding that the Neovim terminal is scriptable; thus far I
haven't toyed with that capability. However, I do look forward to Drew Neil's
upcoming book [Modern
Vim](http://vimcasts.org/blog/2017/05/working-title-modern-vim) book which will
cover that capability (and others).

Working Ruby on Rails autocomplete
----------------------------------

The [vim-ruby](https://github.com/vim-ruby) plugin provides omni-completion for
Ruby on Rails code when `let g:rubycomplete_rails = 1` is set.

When set accordingly, Rails code omni-completes successfully when using Neovim
but for unknown reasons does not work, at least for me, when using Vim 8.

I have an existing [issue](https://github.com/vim-ruby/vim-ruby/issues/349) on
in the *vim-ruby* issue tracker in regards to that Vim failure.

Major Neovim features
=====================

In the grand scheme the above list of enhancements are, mostly, minor in
nature. The more substantive Neovim enhancements are primarily occurring under
the covers and in the community at large, some of those enhancements being:

- Refactored and modernized code base. See
  [this](https://geoff.greer.fm/2015/01/15/why-neovim-is-better-than-vim/) post
  for details.

- A development community that can survive the comings and goings of key
  developers. The initial Neovim lead, Thiago de Arruda, did depart the Neovim
  project yet Neovim has continued on just fine. Would Vim development continue
  if Bram Moolenaar, Vim's creator and on-going maintainer, was no longer
  active?

- Neovim's remote API functionality provides a pathway where Neovim can be a
  true component in another application. This Neovim component could exist in
  an [Atom](https://atom.io) or
  [Thunderbird](https://www.mozilla.org/en-US/thunderbird) session. This same
  *client/server* API has also lead to the development of some very interesting
  Neovim [clients](https://github.com/neovim/neovim/wiki/Related-projects#gui)
  such as the [Electron](http://electron.atom.io) based
  [NyaoVim](https://github.com/rhysd/NyaoVim).

- Integration of a [Lua](https://www.lua.org/) interpreter directly into the
  [runtime](https://github.com/neovim/neovim/pull/4411) to run natively
  alongside Vimscript. Lua is a nicer language than Vimscript and with
  [LuaJIT](http://luajit.org) it is a language that should run orders of
  magnitude faster as well. This will allow Neovim plugin authors scope to
  execute compute-heavy tasks **with** good performance without resorting to
  external processes written in other languages. Going forward,
  [YouCompleteMe](https://github.com/Valloric/YouCompleteMe) type
  plugins would have less
  [need](https://github.com/Valloric/YouCompleteMe#why-isnt-ycm-just-written-in-plain-vimscript-ffs)
  to be written in Python (or similar).

- Asynchronous support is now less of a differentiator between Neovim and Vim
  than it used to be. Vim 8 includes JSON based asynchronous support whilst
  Neovim has long had a
  [MessagePack](https://github.com/msgpack-rpc/msgpack-rpc) based asynchronous
  API.
  [This thread](https://www.reddit.com/r/neovim/comments/58nrwv/neovim_api_comparison_with_vim_channels/)
  explains the approaches the two projects have used.

- Mutually beneficial competitive tension between Vim and Neovim benefits both
  projects. It certainly appears that Neovim's asynchronous support spurred the
  development of Vim 8's equivalent.

Minor future Neovim wants
=========================

Visual block pasting
--------------------

Visual block pasting is currently
[broken](https://github.com/neovim/neovim/issues/1822) in Neovim when
`clipboard=unnamed` is set; pastes will incorrectly occur below rather than to
the side of the cursor. I hope this issue is fixed soon, when encountered it is
very annoying.

Medium future Neovim wants
==========================

Encryption support
------------------

I would like encryption support behaviourally similar to Vim's existing
[blowfish2](http://vim.wikia.com/wiki/Encryption) feature.  Neovim stripped out
all direct encryption support a while
[ago](https://github.com/neovim/neovim/issues/694). I understand why they did
that, and I am not asking that encryption code be put back into the core
editor. However, it seems possible to seamlessly delegate to an external
encryption provider, similar to how Neovim currently delegates to an external
clipboard provider. Such an encryption provider could be
[GPG](https://gnupg.org/). Something like the
[vim-gnupg](https://github.com/jamessan/vim-gnupg) plugin, but built into
Neovim, is what I would like. I still use Vim's `blowfish2` feature, in
preference to *vim-gnupg*, since I'm not sure of the long-term viability of
that plugin.

Improved *relativenumber* scroll performance
--------------------------------------------

This performance issue effects both Vim **and** Neovim. Currently when
`relativenumber` is enabled any type of scroll event will cause a full redraw
for all visible lines which in turn results in every line having its syntax
re-evaluated even though no text has actually changed (only line numbers
change). For languages like Ruby, with its over 200 *regex* rules, this does
result in visible performance problems when scrolling as noted by these issues:
[vim #282](https://github.com/vim/vim/issues/282) and
[vim-ruby #243](https://github.com/vim-ruby/vim-ruby/issues/243). Basically I
would like
[this feature request](https://github.com/vim/vim/issues/1735) implemented;
please thumb it up if you can.

Major future Neovim wants
=========================

Language Server Protocol (LSP)
------------------------------

Integrated [Language Server Protocol](http://langserver.org/) support. LSP
supposedly provides high performance editor-agnostic: code completion, hovered
tooltips, jump-to-definition and code refactoring as explained in
[this](https://code.visualstudio.com/blogs/2016/06/27/common-language-protocol)
Microsoft blog post. Language servers already exist for many
[languages](https://github.com/Microsoft/language-server-protocol/wiki/Protocol-Implementations).
A language-client implementation for Neovim is available as a
[plugin](https://github.com/autozimu/LanguageClient-neovim), but in the longer
term, once LSP and its clients and servers mature, it would seem desirable to
integrate the LSP client directly in core Neovim.

Summary
=======

Is *Neovim is the future of Vim*? I say it *can* indeed be a part of the
future.

As of mid-2017 both the Vim and Neovim projects appear healthy. The competitive
tension seems to have benefited the Vim user community at large. Hence, I say
that every Vim user should want Neovim to continue and more than that
**thrive**, whether they use Neovim or they don't.

P.S. I still continue to use both Vim and Neovim.
