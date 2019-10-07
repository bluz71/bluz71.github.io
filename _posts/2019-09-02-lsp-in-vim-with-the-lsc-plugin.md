---
title: LSP in Vim with the LSC Plugin
layout: default
comments: true
published: false
---

LSP in Vim with the LSC Plugin
===============================

The recent emergence of [Language
Servers](https://microsoft.github.io/language-server-protocol/implementors/servers)
and asynchronous job support has given rise to a myriad of code completion
plugins and frameworks for the [Vim](https://www.vim.org) and
[Neovim](https://neovim.io) editors.

Having so many choices is both a benefit and curse; the benefit being that the
savy Vim user can craft a configuration specific to their need, the curse,
especially for a novice, is the classic [paradox of
choice](https://whatis.techtarget.com/definition/paradox-of-choice), where to
start and what to choose?

This post will discuss my code completion setup, and more broadly my LSP setup,
for [Ruby](https://www.ruby-lang.org/en/) and
[JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) filetypes
using the [Vim LSC](https://github.com/natebosch/vim-lsc) plugin.

Note, my choices may not necessarily suit you, but they do offer a starting
point for users wishing to enter the world of LSP with the Vim editor.

Language Server Protocol (LSP)
------------------------------

Created by [Microsoft](https://www.microsoft.com), the [Language Server
Protocol](https://microsoft.github.io/language-server-protocol/) (LSP) was
originally developed for the [Visual Studio Code](https://code.visualstudio.com)
editor to decouple code editing and presentation from language-specific
actions.

Certain language-aware actions, such as: auto-completion, code refactoring, go
to definition and find all references, that used to be the purview of
heavyweight
[IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment) such as
[Visual Studio](https://www.visualstudio.com) are now available to LSP-client
editors when connected to an appropriate language server. LSP simply transfers
the responsibility of such actions out of the editor to an independent language
server.

As an open JSON-RPC-based standard, LSP now has multi-vendor support which has
rapidly lead to the development of numerous [language
servers](https://langserver.org/#implementations-server) and
[clients](https://langserver.org/#implementations-client).

Vim omnicompletion
------------------

Discuss Vim omni completion, benefits and issues. One issue being blocking.
Another being tern vs clang-complete vs alchemist vs vim-ruby etc etc. So many
different choices, some great, some not so great. But sometimes still not as
good as an IDE and blocking.

Now use the same completion engine as used by VSCode.

Discuss async and why it is good.

LSP vs Code Completion Plugins
------------------------------

What is the difference

Plugin Choices
--------------

Coc
Deoplete + LCM
vim-lsp + asyncomplete.vim
Ncm2 + LCM
ale

Prerequisites
-------------

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
