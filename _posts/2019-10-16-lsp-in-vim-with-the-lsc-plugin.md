---
title: LSP in Vim with the LSC Plugin
layout: default
comments: true
published: true
---

LSP in Vim with the LSC Plugin
===============================

The emergence of the [Language Server
Protocol](https://microsoft.github.io/language-server-protocol) (LSP) and
asynchronous job support has given rise to a myriad of code completion
frameworks for the [Vim](https://www.vim.org) and [Neovim](https://neovim.io)
editors.

So many choices is both a benefit and curse; the benefit being that the savvy
Vim user can craft a configuration specific to their need, the curse, especially
for a novice, is the classic [paradox of
choice](https://whatis.techtarget.com/definition/paradox-of-choice), where to
start and what to choose?

This post will discuss my code completion setup, and more broadly my LSP setup,
for [Ruby](https://www.ruby-lang.org/en/) and
[JavaScript](https://developer.mozilla.org/en-US/docs/Web/JavaScript) filetypes
using the [LSC](https://github.com/natebosch/vim-lsc) plugin.

Note, my choices may not necessarily suit you, but they do offer a starting
point for users wishing to use LSP-based code completion, and other advanced
language-aware actions, in Vim.

Feel free to refer to my [dotfiles](https://github.com/bluz71/dotfiles) to view
my particular LSP configuration.

**UPDATE (Sep 2021)**: I now use Neovim's inbuilt LSP client instead of the LSC
plugin. However, the core concepts discussed in this apply apply to whichever
LSP client one chooses to use.

Prerequisites
-------------

A modern version of Vim or Neovim that supports asynchronous job control is
required. That means Vim 8 or any modern version of Neovim.

Preferably, a very recent version of Vim, version 8.1.2050 or Neovim 0.4.0 at
the time of this writing, is strongly recommended since the LSP hover operation
of the LSC plugin can use either Vim's [Popup
Window](https://github.com/vim/vim/issues/4063) or Neovim's [Floating
Window](https://github.com/neovim/neovim/pull/6619) functionality if available.

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
language-specific actions out of the editor to a vendor-agnostic language server
that runs as a separate background process on the host.

As an open JSON-RPC-based standard, LSP now has multi-vendor support which has
led to the development of numerous [language
clients](https://langserver.org/#implementations-client) and
[servers](https://langserver.org/#implementations-server).

Vim Omni Completion
-------------------

The 2006 release of Vim 7 saw the introduction a new form of completion,
omni-completion. Omni-completion, orthogonal to existing forms of completion
such as `keyword` or `dictionary` completion, is performed by the defined
`omnifunc` and will usually offer filetype-specific completions.

Invoked by `<Control-x><Control-o>`, the intelligence of omni-completion depends
on the sophistication of the `omnifunc` in use. Vim ships with set of
rudimentary omni completion implementations. Users can install more advanced
omni completion plugins, such as: [Tern](https://github.com/ternjs/tern_for_vim)
for JavaScript or [clang_complete](https://github.com/xavierd/clang_complete)
for C or C++, to improve the quality of such completions.

Nonetheless, there are a few issues with omni-completion:

* completion is a synchronous operation, invoking omni-completion will block the
  editor until the operation is concluded

* omni-completion plugins need to be coded and maintained specifically for Vim

However, with LSP-based completion, Vim can leverage and use the same language
servers used by [Visual Studio Code](https://code.visualstudio.com). One can be
confident that the major language servers are actively developed and maintained.
On the other hand, some omni-completion plugins, such as [Tern for
Vim](https://github.com/ternjs/tern_for_vim), are no longer maintained.

Also, LSP-based solutions can leverage Vim & Neovim's asynchronous job control
to **not** block the editor whilst editing. Auto-completion, where completion
candidates are displayed as one types, **should be** asynchronous otherwise
editing may be painful due to stalls arising from the synchronous invocation of
the `omnifunc`.

:bulb: LSP offers more than just code completion; a full-featured language
server can provide context-aware navigation, [code
refactoring](https://en.wikipedia.org/wiki/Code_refactoring) and hovering
tooltips, among other capabilities.

:gift: My LSP configuration, documented below, readily allows for both
non-blocking auto-completion or manually invoked omni-completion using the same
language servers in both cases. You choose which type of completion suits.

Vim Completion Frameworks and LSP-clients
-----------------------------------------

A Vim completion framework is responsible for collating completion candidates
and displaying those choices to the user. Advanced completion frameworks often
operate, by default, in asynchronous auto-completion mode.

An LSP client on the other-hand is editor tooling that supports communication
with a language server employing the Language Server Protocol. As of the time of
this post, October 2019, neither Vim nor Neovim provide out-of-the-box support
for LSP. However, a future version of Neovim will provide LSP support as noted
[in this pull request](https://github.com/neovim/neovim/pull/10222).

**UPDATE (Sep 2021)**: Neovim 0.5 ships with an LSP client.

In the meantime there are multiple Vim plugins that do provide LSP support.

Certain code completions frameworks also include direct LSP support whilst
others delegate such duties to a separate LSP-client plugin.

Notable code completion and LSP-client plugins for Vim and Neovim:

* [YouCompleteMe](https://github.com/ycm-core/YouCompleteMe), a monolithic code
  completion engine that predates LSP and asynchronous job control in Vim, both
  of which have now been incorporated. For many years this was the go to code
  completion plugin for Vim, nowadays there are lighter weight alternatives.

* [deoplete](https://github.com/Shougo/deoplete.nvim) by
  [Shogo](https://github.com/Shougo), the first asynchronous code completion
  framework for Neovim, but which now also supports Vim. Completion candidates
  are gathered from [deoplete-compatible
  sources](https://github.com/Shougo/deoplete.nvim/wiki/Completion-Sources);
  note, LSP-based results require integration with the
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim)
  plugin.

* [ncm2](https://github.com/ncm2/ncm2) by [roxma](https://github.com/roxma), is
  another asynchronous code completion framework for Vim and Neovim.
  Conceptually similar to deoplete, completion candidates are gathered from
  [ncm2-compatible
  sources](https://github.com/ncm2/ncm2/wiki#completion-source); however,
  LSP-based candidates  require integration with either the
  [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim) or
  [ncm2-vim-lsp](https://github.com/ncm2/ncm2-vim-lsp) plugins.

* [Coc](https://github.com/neoclide/coc.nvim), a contemporary code completion
  framework for Neovim and Vim with inbuilt LSP support. Being
  [Typescript](http://www.typescriptlang.org/)-based allows Coc to leverage
  existing plugins used by [Visual Studio Code](https://code.visualstudio.com).
  Somewhat against the norm, Coc operates its own configuration and extension
  system.

* [Completor](https://github.com/maralla/completor.vim), an asynchronous
  completion framework for Vim, and now also Neovim-compatible, with inbuilt LSP
  support. Leaner and more accessible than the plugins mentioned above.

* [LanguageClient-neovim](https://github.com/autozimu/LanguageClient-neovim), an
  LSP client commonly used in combination with an asynchronous completion
  framework such as deoplete or ncm2. The name implies Neovim-only support, but
  nowadays it also supports Vim.

* [vim-lsp](https://github.com/prabirshrestha/vim-lsp), an LSP client written in
  Vimscript; unlike some Python-based clients listed above. This plugin is
  frequently used with the
  [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim) plugin
  by the same author.

* [asyncomplete.vim](https://github.com/prabirshrestha/asyncomplete.vim), an
  ayschronous auto-completion framework written in Vimscript that supports both
  Vim and Neovim's asynchronous job control APIs. Like deoplete and ncm2,
  LSP-based candidates  require integration with an LSP-client plugin, this time
  with the [vim-lsp](https://github.com/prabirshrestha/vim-lsp) plugin.

* [ALE](https://github.com/dense-analysis/ale), primarily an asynchronous
  linting and fixing plugin, but now extended to support LSP. Language servers
  can provide linting, hence the reason why ALE integrated LSP. ALE now includes
  LSP-based code completion in addition to other LSP functionality.

* [LSC](https://github.com/natebosch/vim-lsc) by [Nate
  Bosch](https://github.com/natebosch), a performant LSP client, written in
  Vimscript, that supports LSP-based asynchronous auto-completion in both Vim
  and Neovim.

* [MUcomplete](https://github.com/lifepillar/vim-mucomplete), a minimalist
  auto-completion plugin that leverages Vim's existing completion
  infrastructure. This plugin does not support asynchronous operation.

* [Supertab](https://github.com/ervandew/supertab), a tab completion plugin for
  Vim. This plugin simply maps the **TAB** key to Vim's existing completion
  kinds.

* [VimCompletesMe](https://github.com/ajh17/VimCompletesMe), another tab
  completion plugin for Vim, similar to Supertab but simpler.

The LSC Plugin
--------------

After much trialling I chose [LSC](https://github.com/natebosch/vim-lsc) due to
the following characteristics of the plugin:

* LSC is a combination LSP-client and completion plugin that stands alone, there
  is no multi-plugin dance required

* Implemented in pure Vimscript, this eases installation and avoids certain
  kinds of [upgrade
  pain](https://www.reddit.com/r/vim/comments/aexw9t/deoplete_replacement) that
  may be experienced by Python-based alternatives

* Compatible with both Vim and Neovim's differing asynchronous job control APIs

* Light in weight, less than three thousand lines of Vimscript; LSC augments
  Vim, it does not take over unlike certain other plugins

* Excellent performance due to: asynchronous operation and use of performance
  optimizations such as [incremental
  updates](https://www.reddit.com/r/vim/comments/9zo98c/what_languageserver_client_does_everyone_use/eadqshn/)
  and
  [debouncing](https://www.reddit.com/r/vim/comments/9zo98c/what_languageserver_client_does_everyone_use/eaf2pqi/)

* Pristine completions, candidates will only be sourced from the language
  server, there will be no mixing of candidates from other completion sources

* Straightforward and concise `~/.vimrc`-based configuration

* Referenced language servers are installed and maintained externally, similar
  to how the [ALE](https://github.com/dense-analysis/ale) plugin uses external
  linters and fixers

* Simple configuration option to select either asynchronous auto-completion or
  synchronous manual completion, once the `omnifunc` option is appropriately set

* Auto-completion, if enabled, will only apply to filetypes that are associated
  with a language server

* The `find all references` operation will populate the
  [quickfix](https://vimhelp.org/quickfix.txt.html) list; no custom UIs in
  contrast to certain other LSP plugins

* The `symbol hover` operation, often mapped to `K`, can use either Vim's
  [popup](https://vimhelp.org/popup.txt.html) window or Neovim's
  [floating](https://neovim.io/doc/user/api.html#api-floatwin) window APIs if
  available, whilst double `K` will convert a popup/floating window into
  split-preview window if hover persistence is desired

* Easy to use `code actions` operation, just select the listed number to
  carry out a specific language/framework action, for example wrapping a widget
  with another widget in Flutter

The simplicity LSC may not suit you, especially if you wish to combine
completion candidates from multiple sources, say LSP-based candidates with
keyword-in-file candidates. In my case I already have `ctrl`-based [mappings
that allow easy switching from one completion kind to
another](https://bluz71.github.io/2017/06/14/a-few-vim-tmux-mappings.html#insert-mode-completion-mappings).

:nut_and_bolt: I augment LSC with the
[VimCompletesMe](https://github.com/ajh17/VimCompletesMe) plugin since I like
using the **TAB** key to scroll through completion candidates, among other
benefits of that plugin.

LSC Installation & Configuration
--------------------------------

If using the [vim-plug](https://github.com/junegunn/vim-plug) plugin manager,
add the following to your `~/.vimrc` file:

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
 \    'suppress_stderr': v:true,
 \  },
 \  'javascript': {
 \    'command': 'typescript-language-server --stdio',
 \    'log_level': -1,
 \    'suppress_stderr': v:true,
 \  }
 \}
let g:lsc_auto_map = {
 \  'GoToDefinition': 'gd',
 \  'FindReferences': 'gr',
 \  'Rename': 'gR',
 \  'ShowHover': 'K',
 \  'FindCodeActions': 'ga',
 \  'Completion': 'omnifunc',
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
*Find code actions* at the cursor location | `FindCodeActions` | `ga`

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
```

I use the [ALE](https://github.com/dense-analysis/ale) plugin for linting and
fixing, specifically [StandardRB](https://github.com/testdouble/standard) for
Ruby and [StandardJS](https://standardjs.com/) for JavaScript, hence, I disable
LSC diagnostics. However, if you wish to use LSP-based real-time linting, and
your language server supports it, then specify `let g:lsc_enable_diagnostics =
v:true`.

Lastly, I configure LSC to suppress all client/server messages; by default the
LSC plugin is a little too chatty with regard to displaying all messages, even
when they are not that useful.

:exclamation: Whilst debugging a recalcitrant language server please do enable
LSC diagnostics.

Screenshot
----------

The LSC plugin auto-completing JavaScript.

<img width="800" alt="vim_lsc_completion" src="https://raw.githubusercontent.com/bluz71/misc-binaries/master/blog/vim_lsc_completion.png">

Ruby Language Server
--------------------

[Solargraph](https://github.com/castwide/solargraph) is an LSP-compliant
language server for [Ruby](https://www.ruby-lang.org).

Install Solargraph with the following command:

```sh
gem install solargraph
```

For LSC to function please ensure `solargraph` is available in your `$PATH`.

:gem: The intelligence of Solargraph, when operating in a `Gemfile`-managed
projects, can be improved by running following command in the project's base
directory:

```sh
solargraph bundle
```

Solargraph will now use the documentation from the project's Gems for improved
completions.

:steam_locomotive: Similarly, the quality of [Solargraph completions will be
further enhanced for Rails](https://github.com/castwide/solargraph/issues/87)
projects by also copying [this
Gist](https://gist.github.com/castwide/28b349566a223dfb439a337aea29713e) into
your project, I copied that file into the `initializers` directory.

:construction: Lastly, Solargraph is a still maturing technology, so please
install updates when they become available.

JavaScript Language Server
--------------------------

[Sourcegraph's language
server](https://github.com/sourcegraph/javascript-typescript-langserver),
`javascript-typescript-langserver`, is often noted as the language server to use
for JavaScript and TypeScript. However, I have found it to be a little slow and
somewhat buggy; for example the *find all references* action regularly failed
with my [React](https://reactjs.org) projects.

Interestingly, Visual Studio Code actually uses editor-provided TypeScript
technologies, instead of Sourcegraph's language server, for JavaScript and
TypeScript language actions. Unfortunately, Microsoft's stand-alone TypeScript
server, `tsserver`, is not LSP-compliant, but it does provide the core
capabilities needed by an LSP client.

:clap: Thankfully [TypeFox](https://www.typefox.io) does provide a
[shim](https://en.wikipedia.org/wiki/Shim_(computing)) between `tsserver` and
the Language Server Protocol with the [TypeScript Language
Server](https://github.com/theia-ide/typescript-language-server) package. This
is the language server I recommend for JavaScript and TypeScript filetypes.

Install the TypeScript Language Server with the following command:

```sh
npm install -g typescript-language-server
```

For LSC to function please ensure `typescript-language-server` is available in
your `$PATH`.

Language Servers
----------------

Available language servers for certain prevalent programming languages:

Language | Language Server | Command
--- | --- | ---
C/C++ | [clangd](https://clang.llvm.org/extra/clangd) | `clangd`
Go | [gopls](https://github.com/golang/tools/tree/master/gopls) | `gopls serve`
Python | [pyright](https://github.com/microsoft/pyright) | `pyright --stdio`
Rust | [Rust Analyzer](https://rust-analyzer.github.io) | `rust-analyzer`
Swift | [SourceKit-LSP](https://github.com/apple/sourcekit-lsp) | `sourcekit-lsp`

:warning: Note, I have not tested these language servers personally.

Conclusion
----------

Which plugin(s) one ultimately uses is not that interesting, what is genuinely
game-changing are the advanced editing capabilities that LSP provides.

This Language Server Protocol Vim screencast, by Greg Hurrell, is pertinent with
respect to that point:

<iframe width="672" height="378" src="https://www.youtube.com/embed/8PZZkIr5Dcc" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

Hopefully this post provides enough detail to start your LSP journey in Vim.
