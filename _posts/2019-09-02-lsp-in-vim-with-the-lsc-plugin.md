---
title: LSP in Vim with the LSC Plugin
layout: default
comments: true
published: false
---

LSP in Vim with the LSC Plugin
===============================

The recent emergence of [Language
Server Protocol](https://microsoft.github.io/language-server-protocol) (LSP)
and asynchronous job support has given rise to a myriad of code completion
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

Language Server Protocol (LSP)
------------------------------

Created by [Microsoft](https://www.microsoft.com),
[LSP](https://microsoft.github.io/language-server-protocol/) was originally
developed for the [Visual Studio Code](https://code.visualstudio.com) editor to
decouple code editing and presentation from language-specific actions.

Language-specific actions, such as: auto-completion, code refactoring, go to
definition and find all references, that used to be the purview of heavyweight
[IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment) are now
available to any LSP-capable editor when associated with an appropriate language
server. LSP transfers the responsibility of such language-specific actions out
of the editor to an independent language server.

As an open JSON-RPC-based standard, LSP now has multi-vendor support which has
rapidly lead to the development of numerous [language
servers](https://langserver.org/#implementations-server) and
[clients](https://langserver.org/#implementations-client).

Vim Omni Completion
-------------------

The 2006 release of Vim 7 saw the introduction a new form of completion,
omni-completion. Omni-completion, orthogonal to existing forms of completion
such as `keyword` or `dictionary` completion, is performed by the defined
`omnifunc` that will usually offer filetype-specific completions.

Invoked by `<Control-x><Control-o>`, the intelligence of omni-completion depends
on the sophistication of the `omnifunc` in use. Vim ships with set of
rudimentary omni completion implementations. Users can install more advanced
omni completion plugins, such as: [Tern](https://github.com/ternjs/tern_for_vim)
for JavaScript or [clang_complete](https://github.com/xavierd/clang_complete)
for C or C++, to improve the quality of such completions.

Nonetheless, there are a few issues with omni-completion:

* completion is a synchronous, invoking omni-completion will block the editor
  until the operation is concluded

* omni-completion plugins needs to be coded and maintained specifically for Vim

However, with LSP-based completion, Vim can leverage and use the same language
servers used by [Visual Studio Code](https://code.visualstudio.com). One can be
confident that the major language servers are actively developed and maintained.
On the other hand, some omni-completion plugins, such as [Tern for
Vim](https://github.com/ternjs/tern_for_vim), are no longer being mantained.

Also, LSP-based solutions can leverage Vim & Neovim's asychronous job control to
**not** block the editor whilst editing. Auto-completion, where completion
results are displayed as one types, should be asychronous otherwise
editing will become painful due to synchronous completion gathering.

:bulb: LSP offers more than just code completion, a full-featured language
server can provide context-aware navigation and [code
refactoring](https://en.wikipedia.org/wiki/Code_refactoring).

:gift: My LSP configuration, documented below, readily allows for both
non-blocking auto-completion or manually invoked omni-completion via Vim's
existing `omnifunc` setting using the same language servers in both cases. You
choose which type of completion suits.

Completion Frameworks and LSP-clients
-------------------------------------

A completion framework is responsible for collating completion results and
displaying those choices to the user. Completion framworks often operate in
auto-completion mode, rather than manually invoked, and the more advanced
frameworks usually operate asychronously.

An LSP client on the otherhand is editor tooling that supports communication
with a language server using the Language Server Protocol. As of the time of
this post (October 2019) neither Vim nor Neovim provide out-of-the-box support
for LSP. However, a future version of Neovim will provide LSP support as noted
[in this pull request](https://github.com/neovim/neovim/pull/10222). In the
meantime there are multiple Vim plugins that provide LSP support.

Note, Some code completions frameworks include LSP support whilst others depend
on 3rd party LSP-client plugins.

Notable completion and LSP-related plugins for Vim and Neovim:

- [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe), a comprehensive
  and mature code completion engine. Very heavy plugin. No longer the cool kid.
  The grand daddy. Complex, big, sometimes slow.

- [deoplete](https://github.com/Shougo/deoplete.nvim), an asynchronous
  completion framework. A 3rd party LSP plugin, such as
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim)
  will be required. Can combine completions from multiple sources, for example
  LSP-based completions mixed with keyword completions. Python3 plugin. Primary
  use case is auto-completion. Lots of related plugins, completion sources.

- [ncm2](https://github.com/ncm2/ncm2), a Neovim-specific asynchronous code
  completion framework. A 3rd party LSP plugin, such as
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim)
  will be required. Can combine completions from multiple sources, for example
  LSP-based completions mixed with keyword completions. Python3 plugin. Primary
  use case is auto-completion.

- [Coc](https://github.com/neoclide/coc.nvim), an all inclusive code completion
  framework with LSP support. Like Deoplete and ncm2, does combine completions
  from multiple sources. Typescript based. Is it's own ecosystem with it's own
  plugin system and separate configuration. Written in TypeScript. Designed to
  more directly leverage plugins coded for Visual Studio Code than other Vim
  plugins.

- [Completor](https://github.com/maralla/completor.vim), an asynchronous
  completion framework for Vim 8 with LSP support. Requires Vim Python support.
  Now support Vim 8 and Neovim.

- [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim), an
  LSP client that is often used with other asynchronous completion frameworks
  such as: deoplete or ncm2. May have Rust implementation. Works for both Vim 8
  and Neovim even though the name implies Neovim only support. Delagates
  completion to completion frameworks.

- [vim-lsp](https://github.com/prabirshrestha/vim-lsp), an LSP client written in
  pure Vimscript. Usually combined with asyncomplete.vim. Kind-of like
  LanguageClient-neovim, but pure Vimscript. Delegates completion to completion
  frameworks.

- [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim), an
  ayschronous autocompletion framework written that supports both Vim 8 and
  Neovim. Pure Vimscript. No LSP support. Should combine with vim-lsp by the
  same author.

- [ale](https://github.com/dense-analysis/ale), primarily an asynchronous
  linting infrastructure. Extended to support LSP, initially to support
  LSP-based linting, now with LSP-based completion (among other). Pure
  Vimscript.

- [vim-lsc](https://github.com/natebosch/vim-lsc), a full-featured LSP client
  with LSP-based auto or manual completion. Written in pure Vimscript

- [MUcomplete](https://github.com/lifepillar/vim-mucomplete), a minimalist
  auto-completion plugin that leverages Vim's existing completion
  infrastructure. Does not support asynchronous operation.

- [Supertab](https://github.com/ervandew/supertab), a tab completion plugin for
  Vim.

- [VimCompletesMe](https://github.com/ajh17/VimCompletesMe), a tab completion
  plugin for Vim, similar to Supertab but simpler.

Prerequisites
-------------

Modern Vim for popup / float

Handy suggestion
----------------

VimCompletes me plugin
Insert mode completion mappings (ctrl-d/k etc)

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

Ruby Completion
---------------

JavaScript Completion
---------------------

Language Servers for other Languages
------------------------------------

- Dart

- Python

- Rust

- C++
