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
using the [LSC](https://github.com/natebosch/vim-lsc) plugin.

Note, my choices may not necessarily suit you, but they do offer a starting
point for users wishing to use LSP-based code completion and other
advanced language-aware actions in Vim.

Feel free to refer to my [dotfiles](https://github.com/bluz71/dotfiles) to view
my configuration.

Prerequisites
-------------

A modern version of Vim or Neovim that supports asynchronous job control is
required. That means Vim 8 or any modern version of Neovim.

Preferably, a very recent version of Vim, version 8.1.2050 or Neovim 0.4.0 at
the time of this writing, is strongly recommended since the LSP hover operation
of the LSC plugin can use either Vim's [Popup
Window](https://github.com/vim/vim/issues/4063) or Neovim's [Floating
Window](https://github.com/neovim/neovim/pull/6619) functionality if
available.

I also recommend installing and updating Vim, or Neovim, using
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

Language-specific actions, such as: auto-completion, hovering and navigation,
that used to be the purview of heavyweight
[IDEs](https://en.wikipedia.org/wiki/Integrated_development_environment) are now
available to LSP-capable editors when associated with an appropriate
LSP-compliant language server. LSP transfers the responsibility of such
language-specific actions out of the editor to a vendor-agnostic language
server that runs as a separate background process on the host.

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
Vim](https://github.com/ternjs/tern_for_vim), are no longer maintained.

Also, LSP-based solutions can leverage Vim & Neovim's asychronous job control to
**not** block the editor whilst editing. Auto-completion, where completion
candidates are displayed as one types, **should be** asychronous otherwise
editing will become painful due to the stalls arising from the synchronous
invocation of the `omnifunc`.

:bulb: LSP offers more than just code completion, a full-featured language
server can provide context-aware navigation, [code
refactoring](https://en.wikipedia.org/wiki/Code_refactoring) and hovering
tooltips among other capabilities.

:gift: My LSP configuration, documented below, readily allows for both
non-blocking auto-completion or manually invoked omni-completion using the same
language servers in both cases. You choose which type of completion suits.

Completion Frameworks and LSP-clients
-------------------------------------

A Vim completion framework is responsible for collating completion candidates
and displaying those choices to the user. Advanced Completion framworks often
operate, by default, in asynchronous auto-completion mode.

An LSP client on the otherhand is editor tooling that supports communication
with a language server employing the Language Server Protocol. As of the time of
this post, October 2019, neither Vim nor Neovim provide out-of-the-box support
for LSP. However, a future version of Neovim will provide LSP support as noted
[in this pull request](https://github.com/neovim/neovim/pull/10222). In the
meantime there are multiple Vim plugins that do provide LSP support.

Certain code completions frameworks also include direct LSP support whilst
others delegate such duties to a separate LSP-client plugin.

Notable code completion and LSP-client plugins for Vim and Neovim:

- [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe), a monolithic code
  completion engine that predates LSP and asynchronous job control in Vim, both
  of which have now been incorporated. For many years this was the go to code
  completion plugin for Vim, nowadays there are many lighter weight
  alternatives.

- [deoplete](https://github.com/Shougo/deoplete.nvim) by
  [Shogo](https://github.com/Shougo), the first asynchronous code completion
  framework for Neovim, but which now also supports Vim. Completion candidates
  are gathered from [deoplete-compatible
  sources](https://github.com/Shougo/deoplete.nvim/wiki/Completion-Sources);
  note, LSP-based results require integration with the
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim)
  plugin.

- [ncm2](https://github.com/ncm2/ncm2) by [roxma](https://github.com/roxma), is
  another asynchronous code completion framework for Vim and Neovim.
  Conceptually similar to deoplete, completion candidates are gathered from
  [ncm2-compatible
  sources](https://github.com/ncm2/ncm2/wiki#completion-source); however,
  LSP-based candidates  require integration with either the
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim) or
  [ncm2-vim-lsp](https://github.com/ncm2/ncm2-vim-lsp) plugins.

- [Coc](https://github.com/neoclide/coc.nvim), a contemporary code completion
  framework for Neovim and Vim with inbuilt LSP support. Being
  [Typescript](http://www.typescriptlang.org/)-based allows Coc to leverage
  existing plugins used by [Visual Studio Code](https://code.visualstudio.com).
  Somewhat against the norm, Coc operates its own configuration and extension
  system.

- [Completor](https://github.com/maralla/completor.vim), an asynchronous
  completion framework for Vim, and now also Neovim-compatible, with inbuilt LSP
  support. Leaner and more accessible than the plugins mentioned above.

- [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim), an
  LSP client commonly used in combination with an asynchronous completion
  framework such as deoplete or ncm2. The name implies Neovim-only support, but
  nowawdays it also support Vim.

- [vim-lsp](https://github.com/prabirshrestha/vim-lsp), an LSP client written in
  Vimscript; not Python-based like some of the above-mentioned plugins. This
  plugin is frequently used with the
  [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) plugin
  by the same author.

- [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim), an
  ayschronous auto-completion framework written in Vimscript that supports both
  Vim and Neovim's asynchronous job control APIs. Like deoplete and ncm2,
  LSP-based candidates  require integration with an LSP-client plugin, this time
  with the [vim-lsp](https://github.com/prabirshrestha/vim-lsp) plugin.

- [ALE](https://github.com/dense-analysis/ale), primarily an asynchronous
  linting and fixing plugin, but now exended to support LSP. Language servers
  can provide linting, hence the reason why ALE integrated LSP. ALE now includes
  LSP-based code completion in addition to other LSP functionality.

- [LSC](https://github.com/natebosch/vim-lsc) by [Nate
  Bosch](https://github.com/natebosch), a performant LSP client, written in
  Vimscript, that supports LSP-based asynchronous auto-completion in both Vim
  and Neovim.

- [MUcomplete](https://github.com/lifepillar/vim-mucomplete), a minimalist
  auto-completion plugin that leverages Vim's existing completion
  infrastructure. This plugin does not support asynchronous operation.

- [Supertab](https://github.com/ervandew/supertab), a tab completion plugin for
  Vim. This plugin simply maps the **TAB** key to Vim's existing completion
  kinds.

- [VimCompletesMe](https://github.com/ajh17/VimCompletesMe), another tab
  completion plugin for Vim, similar to Supertab but simpler.

The LSC Plugin
--------------

After much trialing I chose [LSC](https://github.com/natebosch/vim-lsc) due
to the following characteristics of the plugin:

- LSC is a combination LSP-client and completion plugin that stands alone, there
  is no multi-plugin dance required

- Implemented in pure Vimscript, this eases installation and avoids certain
  kinds of [upgrade
  pain](https://www.reddit.com/r/vim/comments/aexw9t/deoplete_replacement) that
  may be experienced by Python-based alternatives

- Compatibility with both Vim and Neovim's differing asynchronous job control
  APIs

- Light in weight, less than three thousand lines of Vimscript; LSC augments
  Vim, it does not take over unlike certain other plugins

- Excellent performance due to: asynchronous operation and use of performance
  optimizations such as [incremental
  updates](https://www.reddit.com/r/vim/comments/9zo98c/what_languageserver_client_does_everyone_use/eadqshn/)
  and
  [debouncing](https://www.reddit.com/r/vim/comments/9zo98c/what_languageserver_client_does_everyone_use/eaf2pqi/)

- Pristine completions, candidates will only be sourced from the language
  server, there will be no mixing of candidates from other completion sources

- Straightforward and concise `~/.vimrc`-based configuration

- Referenced language servers are installed and maintained externally, similar
  to how the [ALE](https://github.com/dense-analysis/ale) plugin uses external
  linters and fixers

- Simple configuration option to select either asynchronous auto-completion or
  synchronous manual completion; the latter requires setting the `omnifunc`
  option appropriately

- Auto-completion, if enabled, will only apply to filetypes that are associated
  with a language server

- The `find all references` operation will populate the
  [quickfix](https://vimhelp.org/quickfix.txt.html) list; no custom UIs in
  contrast to a few other plugins

- The `symbol hover` operation can use either Vim's
  [popup](https://vimhelp.org/popup.txt.html) window or Neovim's
  [floating](https://neovim.io/doc/user/api.html#api-floatwin) windows if
  available; this feature markedly enhances hover usability

The simplicity LSC may not suit you, especially if you wish to combine
completion candidates from multiple sources, say LSP-based candidates with
keyword-in-file candidates. In my case I already have `ctrl`-based [mappings
that allow easy switching from one completion kind to
another](https://bluz71.github.io/2017/06/14/a-few-vim-tmux-mappings.html#insert-mode-completion-mappings).

:star: I augment LSC with the
[VimCompletesMe](https://github.com/ajh17/VimCompletesMe) plugin since I like
using the **TAB** key to scroll through completion candidates, among other
benefits of that plugin.

LSC Installation & Configuration
--------------------------------

If using the [vim-plug](https://github.com/junegunn/vim-plug) plugin manager,
please add the following to your `~/.vimrc` file:

```viml
Plug 'natebosch/vim-lsc'
Plug 'ajh17/VimCompletesMe'
```

Then run `:PlugInstall` to install the plugins. Please change the notation
appropriately if using an alternate plugin manager for Vim.

My Ruby and JavaScript LSC configuration:

```viml
let g:lsc_server_commands = {
 \  'ruby': {
 \    'command': 'solargraph stdio',
 \    'log_level': -1,
 \    'suppress_stderr': v:true
 \  },
 \  'javascript': {
 \    'command': 'typescript-language-server --stdio',
 \    'log_level': -1,
 \    'suppress_stderr': v:true
 \  }
 \}
let g:lsc_auto_map = {
 \  'GoToDefinition': 'gd',
 \  'FindReferences': 'gr',
 \  'Rename': 'gR',
 \  'ShowHover': 'K'
 \}
let g:lsc_enable_autocomplete  = v:true
let g:lsc_enable_diagnostics   = v:false
let g:lsc_reference_highlights = v:false
let g:lsc_trace_level          = 'off'
```

For a given filetype the LSC plugin will take care of launching, communicating
and shutting down the named language server `command`. The specified language
servers, listed above, will be discussed in greater detail in the following Ruby
and JavaScript sections.

In addition to LSP-based auto-completion, I define and use the following four
LSP actions:

LSP Action | LSC Command | Vim Mapping
--- | --- | ---
*Go to definition* for the symbol under the cursor | `GoToDefinition` | `gd`
*Find all references* for the symbol under the cursor | `FindReferences` | `gr`
*Rename* the symbol under the cursor and all references | `Rename` | `gR`
*Show hover* tooltip for the symbol under the cursor | `ShowHover` | `K`

I **strongly** recommend the following `completeopt` setting when using
auto-completion:

```viml
set completeopt=menu,menuone,noinsert,noselect
```

If you do not care for auto-completion but do wish to use LSP-based
omni-completion, via `<Control-x><Control-o>`, then add the following to your
`~/.vimrc`:

```viml
let g:lsc_enable_autocomplete = v:false
autocmd FileType ruby,javascript setlocal omnifunc=lsc#complete#complete
```

I use the [ALE](https://github.com/dense-analysis/ale) plugin for linting and
fixing, specifically [StandardRB](https://github.com/testdouble/standard) for
Ruby and [StandardJS](https://standardjs.com/) for JavaScript, hence, I disable
LSC diagnostics. However, if you wish to use LSP-based real-time linting then
specify `let g:lsc_enable_diagnostics = v:true`.

Lastly, I configure LSC to suppress all client/server messages; by default the
LSC plugin is a little too chatty with regards to displaying all messages, even
when they are not that useful.

:exclamation: Whilst debugging a recalcitrant language server please do enable
LSC diagnostics.

Ruby Language Server
--------------------

[Solargraph](https://github.com/castwide/solargraph) is the prime LSP-compliant
language server for [Ruby](https://www.ruby-lang.org).

Install Solargraph with the following command:

```sh
gem install solargraph
```

For LSC to function correctly please make sure `solargraph` is available in your
`$PATH`.

:bulb: The intelligence of Solargraph, when operating in a `Gemfile`-managed
project, can be improved by running following command in the project's base
directory:

```sh
solargraph bundle
```

Solargraph will now use the documentation from the project's Gems for improved
completions.

:bulb: Similarly, the quality of [Solargraph completions will be further enhaced
for Rails](https://github.com/castwide/solargraph/issues/87) projects by also
copying [this
Gist](https://gist.github.com/castwide/28b349566a223dfb439a337aea29713e) into
your project, I copied that file into the `initializers` directory.

Lastly, Solargraph is a still maturing technology, so please install updates
when they become available.

JavaScript Language Server
--------------------------

typescript-language-server needs to be in the $PATH

But this one is buggy and slow
```
$ npm install -g javascript-typescript-langserver
```

This one is better
```
npm install -g typescript-language-server
```

https://github.com/eclipse/wildwebdeveloper/issues/22
https://www.reddit.com/r/emacs/comments/ca9fto/javascripttypescriptlangserver_or_tide/
https://github.com/theia-ide/typescript-language-server
https://github.com/emacs-lsp/lsp-mode/pull/509

ADD SCREENSHOT!!!

Language Servers for other Languages
------------------------------------

- Dart

- Python

  ```
  pip install python-language-server
  ```

  `pyls`

- Rust

- C++

Conclusion
----------

<iframe width="672" height="378" src="https://www.youtube.com/embed/8PZZkIr5Dcc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
