---
title: LSP in Vim with the LSC Plugin
layout: default
comments: true
published: false
---

LSP in Vim with the LSC Plugin
===============================

The recent emergence of the [Language Server
Protocol](https://microsoft.github.io/language-server-protocol) (LSP) and
asynchronous job support has given rise to a myriad of code completion
frameworks for the [Vim](https://www.vim.org) and [Neovim](https://neovim.io)
editors.

So many choices is both a benefit and curse; the benefit being that the savy Vim
user can craft a configuration specific to their need, the curse, especially for
a novice, is the classic [paradox of
choice](https://whatis.techtarget.com/definition/paradox-of-choice), where to
start and what to choose?

This post will discuss my code completion setup, and more broadly my LSP setup,
for [Ruby](https://www.ruby-lang.org/en/) and
[JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) filetypes
using the [Vim LSC](https://github.com/natebosch/vim-lsc) plugin.

Note, my choices may not necessarily suit you, but they do offer a starting
point for users wishing to use modern LSP-based editing features in Vim.

Feel free to refer to my [dotfiles](https://github.com/bluz71/dotfiles) to view
my configuration.

Prerequisites
-------------

A version of Vim, version 8 or above or Neovim, that supports asynchronous job
control is required.

Preferably, a recent version of Vim, version 8.1.2050 or Neovim 0.4.0, is
strongly recommended since the LSP hover operation of the LSC plugin can use
either Vim's [Popup Window](https://github.com/vim/vim/issues/4063) or Neovim's
[Floating Window](https://github.com/neovim/neovim/pull/6619) functionality.

I recommend installing and updating Vim, or Neovim, using
[Homebrew](https://brew.sh/) on macOS and Linux.

For example, to install Neovim using Brew:

```sh
brew install neovim
```

And to update Neovim:

```sh
brew upgrade neovim
```

Language Server Protocol (LSP)
------------------------------

Created by [Microsoft](https://www.microsoft.com),
[LSP](https://microsoft.github.io/language-server-protocol/) was originally
developed for the [Visual Studio Code](https://code.visualstudio.com) editor to
decouple code editing and presentation from language-specific actions.

Language-specific actions, such as: auto-completion, refactoring and navigation,
that used to be the purview of heavyweight
[IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment) are now
available to any LSP-capable editor when associated with an appropriate language
server. LSP transfers the responsibility of such language-specific actions out
of the editor to an independent language server.

As an open JSON-RPC-based standard, LSP now has multi-vendor support which has
rapidly lead to the development of numerous [language
clients](https://langserver.org/#implementations-client) and
[servers](https://langserver.org/#implementations-server).

Vim Omni Completion
-------------------

The 2006 release of Vim 7 saw the introduction a new form of completion,
omni-completion. Omni-completion, orthogonal to existing forms of completion
such as `keyword` or `dictionary` completion, is performed by the defined
`omnifunc` which offers filetype-specific completions.

Invoked by `<Control-x><Control-o>`, the intelligence of omni-completion depends
on the sophistication of the `omnifunc` in use. Vim ships with set of
rudimentary omni completion implementations. Users can install more advanced
omni completion plugins, such as: [Tern](https://github.com/ternjs/tern_for_vim)
for JavaScript or [clang_complete](https://github.com/xavierd/clang_complete)
for C or C++, to improve the quality of such completions.

Nonetheless, there are a few issues with omni-completion:

* completion is a synchronous operation, invoking omni-completion will block the
  editor until the operation is concluded

* omni-completion plugins needs to be coded and maintained specifically for Vim

However, with LSP-based completion, Vim can leverage and use the same language
servers used by [Visual Studio Code](https://code.visualstudio.com). One can be
confident that the major language servers are actively developed and maintained.
On the other hand, some omni-completion plugins, such as [Tern for
Vim](https://github.com/ternjs/tern_for_vim), are no longer being mantained.

Also, LSP-based solutions can leverage Vim & Neovim's asychronous job control to
**not** block the editor whilst editing. Auto-completion, where completion
results are displayed as one types, should be asychronous otherwise
editing will become painful due to the stalls resulting from synchronous
operation.

:bulb: LSP offers more than just code completion, a full-featured language
server can provide context-aware navigation and [code
refactoring](https://en.wikipedia.org/wiki/Code_refactoring).

:gift: My LSP configuration, documented below, readily allows for both
non-blocking auto-completion or manually invoked omni-completion using the same
language servers in both cases. You choose which type of completion suits.

Completion Frameworks and LSP-clients
-------------------------------------

A Vim completion framework is responsible for collating completion results and
displaying those choices to the user. Completion framworks often operate, by
default, in asynchronous auto-completion mode.

An LSP client on the otherhand is editor tooling that supports communication
with a language server employing the Language Server Protocol. As of the time of
this post (October 2019) neither Vim nor Neovim provide out-of-the-box support
for LSP. However, a future version of Neovim will provide LSP support as noted
[in this pull request](https://github.com/neovim/neovim/pull/10222). In the
meantime there are multiple Vim plugins that do provide LSP support.

Certain code completions frameworks include direct LSP support whilst others
delegate such duties to a separate LSP-client plugin.

Notable code completion and LSP-client plugins:

- [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe), a monolithic code
  completion engine that predates LSP and asynchronous job support in Vim, both
  of which have now been incorporated. For many years this was the go to code
  completion plugin for Vim.

- [deoplete](https://github.com/Shougo/deoplete.nvim) by
  [Shogo](https://github.com/Shougo), the first asynchronous code completion
  framework for Neovim, but which now also supports Vim. Auto-completion results
  are gathered from deoplete-compatible sources, however LSP-based results
  require integration with the
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim)
  plugin.

- [ncm2](https://github.com/ncm2/ncm2) by [roxma](https://github.com/roxma), is
  also an asynchronous code completion framework for Vim and Neovim.
  Conceptually similar to deoplete, ncm2 likewise requires
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim) to
  integate LSP-based results.

- [Coc](https://github.com/neoclide/coc.nvim), a contemporary code completion
  framework for Neovim and Vim with inbuilt LSP support. Being
  [Typescript](http://www.typescriptlang.org/)-based allows Coc to leverage
  existing plugins used by [Visual Studio Code](https://code.visualstudio.com).
  Somewhat against the norm, Coc operates its own configuration and extension
  system.

- [Completor](https://github.com/maralla/completor.vim), an asynchronous
  completion framework for Vim, and now Neovim-compatible, with LSP support.
  Leaner and more accessible than the plugins mentioned above.

- [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim), an
  LSP client commonly used in combination with an asynchronous completion
  framework such as deoplete or ncm2.

- [vim-lsp](https://github.com/prabirshrestha/vim-lsp), an LSP client written in
  Vimscript; not Python-based like some of the above-mentioned plugins. This
  plugin is regularly used with
  [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) by the
  same author.

- [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim), an
  ayschronous autocompletion framework written Vimscript that supports both Vim
  and Neovim's asynchronous job control. Like deoplete and ncm2, LSP-based results
  require integration an LSP-client plugin, this time with the
  [vim-lsp](https://github.com/prabirshrestha/vim-lsp) plugin.

- [ALE](https://github.com/dense-analysis/ale), primarily an asynchronous
  linting and fixing plugin, but now exended to support LSP. Language servers
  can provide linting, hence the reason why ALE integrated LSP. ALE now includes
  LSP-based code completion in addition to other LSP functionality.

- [vim-lsc](https://github.com/natebosch/vim-lsc), a performant LSP client,
  written in Vimscript, that also supports asynchronous auto-completion in both
  Vim and Neovim.

- [MUcomplete](https://github.com/lifepillar/vim-mucomplete), a minimalist
  auto-completion plugin that leverages Vim's existing completion
  infrastructure. Does not support asynchronous operation.

- [Supertab](https://github.com/ervandew/supertab), a tab completion plugin for
  Vim. This plugin simply maps the **TAB** key to Vim's existing completion
  kinds.

- [VimCompletesMe](https://github.com/ajh17/VimCompletesMe), another tab
  completion plugin for Vim, similar to Supertab but simpler.

Why LSC?
--------

- simple configuration (vimrc only)

- works with both Vim 8 and Neovim

- Language servers installed and maintained outside (similar to Ale linters)

- vimscript only (no Python dependency) and works for both Vim and Neovim

- only provides LSP completion choices (no mixing in of other choices)

- Very easy to configure on a per-filetype basis, I did not want auto-completion
  for all filetypes

- excellent non-blocking performance, including the use of [incremental
  updates](https://www.reddit.com/r/vim/comments/9zo98c/what_languageserver_client_does_everyone_use/eadqshn/)

- Easy configuration toggle to choose between auto-completion and manual
  completion; some users like the former, some only like the latter. Easily done
  via `omnifunc`

- Makes of quickfix list (unlike Coc or Ale)

- Makes use of floats/popups for hovers (on modern Vim/Neovim)

- Launches Language Server, in the background, once Vim is entered unlike Ale

- Works well with Ale once completion is disabled in Ale and Linting is disabled
  in LSC. I prefer Ale to do linting since I use Standard and StandardRB which
  are not LSP based

LSC Installation & Configuration
--------------------------------

```viml
set completeopt=menu,menuone,noinsert,noselect
```

VimCompletes me plugin as well

List LSP commands to be used

Ruby Completion
---------------

Solargraph needs to be in the $PATH

JavaScript Completion
---------------------

typescript-language-server needs to be in the $PATH

Language Servers for other Languages
------------------------------------

- Dart

- Python

- Rust

- C++
