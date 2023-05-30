---
title: Maximize Productivity Of The Bash Shell
layout: default
comments: false
published: false
---

Maximize Productivity Of The Bash Shell
=======================================

This is a 2023 followup to the [Bash Shell Tweaks &
Tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html) article
from 2018.

Since that last article there has been a renaissance in shell-agnostic command
line tooling, often implemented in high-performance
[Rust](https://www.rust-lang.org), that have elevated the interactive shell
experience.

In this article: we will enable a host of useful available Bash shell options,
we will script automatic `pushd` along with matching directory stack navigation
bindings, we will integrate an interactive completion interface and we will use
modern command line tooling to sensibly improve the interactive experience,
among other tips and suggestions.

Note, my [bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files
integrate all the upcoming suggestions listed in this article.

Dispelling Bash Misconceptions
------------------------------

Before proceeding, it is worth correcting some Bash misconceptions that still
intermittently persist:

- Bash supports changing directories without entering the `cd` command
- Bash supports simple file and directory path *autocorrection*
- `**` recursive globbing is supported
- Command `history` can be shared between Bash instances
- Bash does support *tab-completion* cycling
- Command and context-aware completion is supported through the Bash Completion
  package
- Interactive completion is supported by way of a 3rd party package

Keep reading for details about all the above.

Installation And Setup For macOS
--------------------------------

Since OS Catalina (2019), [Zsh](https://www.zsh.org) is now the default shell
for macOS. Apple still ships with Bash, but only version 3.2 which dates back to
2006; such a legacy version of Bash should be avoided for interactive use.

Updating to a modern version of Bash is most easily accomplished with the
[Homebrew](https://brew.sh) package manager:

```sh
brew install bash
echo /opt/homebrew/bin/bash | sudo tee -a /etc/shells
chsh -s /opt/homebrew/bin/bash
```

Note, if using an older Intel-based Mac, please replace `/opt/homebrew` with
`/usr/local`.

The Bash Completion package greatly enhances the interactive shell experience,
please install it as follows:

```sh
brew install bash-completion@2
```

And then add the following to your `~/.bash_profile`:

```sh
[[ -r "$HOMEBREW_PREFIX/etc/profile.d/bash_completion.sh" ]] && . "$HOMEBREW_PREFIX/etc/profile.d/bash_completion.sh"
```

Terminal Emulators
------------------

In recent years there has been a resurgence in terminal emulator development,
mirroring the previously mentioned renaissance in shell-agnostic command line
tooling.

I recommend the use of a terminal emulator that: is cross platform, is
GPU-accelerated and supports a text configuration that can be stored in a Git
repository.

Any of these fine terminal emulators will will meet those expectations:

- [Alacritty](https://alacritty.org)

- [kitty](https://sw.kovidgoyal.net/kitty)

- [Wezterm](https://wezfurlong.org/wezterm)

Readline Configuration
----------------------

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) library
is used by Bash, and certain other utilities, for line-editing and certain
history management.

Many beneficial Readline capabilities are disabled by default; thankfully it is
easy to enable these features for the betterment of the interactive experience.

The Readline library is configured through the `~/.inputrc` file. I use and
recommend these settings, with comments provided detailing each setting:

```sh
# TAB cycles forward through and Shift-TAB cycles backward through completion
# choices.
TAB: menu-complete
"\e[Z": menu-complete-backward

# Substring history search using UP and DOWN arrow keys.
"\e[A": history-substring-search-backward
"\e[B": history-substring-search-forward

# Launch $EDITOR on the current command and execute when finished editing.
"\ee": edit-and-execute-command

# Enable completion coloring.
set colored-completion-prefix on
set colored-stats on

# Ignore case when completing.
set completion-ignore-case on

# Treat hypen and underscores as equivalent.
set completion-map-case on

# The number of completions to display without prompt; when exceeded a
# prompt-to-display will appear.
set completion-query-items 200

# Automatically add slash to the end of symlinked directories when completing.
set mark-symlinked-directories on

# Don't automatically match files beginning with dot.
set match-hidden-files off

# Display the common prefix choices on the first completion then cycle the
# available choices on the next completion.
set menu-complete-display-prefix on

# Turn off the completions pager.
set page-completions off

# Immediately display completion matches.
set show-all-if-ambiguous on

# Smartly complete items when the cursor is not at the end of the line.
set skip-completed-text on
```

It is instructive to highlight a few capabilities enabled above:

- The `<TAB>` and `<Shift-Tab>` keys will now cycle completions choices

- Completions will commence after pressing the `<TAB>` just once

- Partially typing a command and pressing `<Up>` will engage `history` substring
  matching, not just start of line matching

- The `<Alt-e>` edit-and-execute binding is a reasonable Vim-mode substitute if
  Vim or Neovim is the configured `$EDITOR`; noting that Readline only supports
  a basic Vi-mode if configured

Shell Options
-------------

Somewhat similar to the previous Readline section, Bash provides a number of
useful shell options that are disabled by default.

Shell options should be set in `~/.bashrc`. I use and recommend these options,
with comments detailing each option:

```sh
#  - autocd - change directory without entering the 'cd' command
#  - cdspell - automatically fix directory typos when changing directory
#  - direxpand - automatically expand directory globs when completing
#  - dirspell - automatically fix directory typos when completing
#  - globstar - ** recursive glob
#  - histappend - append to history, don't overwrite
#  - histverify - expand, but don't automatically execute, history expansions
#  - nocaseglob - case-insensitive globbing
#  - no_empty_cmd_completion - do not TAB expand empty lines
shopt -s autocd cdspell direxpand dirspell globstar histappend histverify \
    nocaseglob no_empty_cmd_completion
```

These options elevate the interactive experience, it is somewhat baffling why
they are disabled by default.

History
-------

In its out of the box configuration, the [Bash
history](https://www.gnu.org/software/bash/manual/html_node/Bash-History-Facilities.html)
experience is quite primitive:

- Each running shell will have it's own history, and when a shell ends the
  running history of the current shell will entirely replace the contents of
  `~/.bash_history` file
- Hence, history from the next exiting shell will then overwrite the history of
  the previously exited shell
- History will not be shared between concurrent shell sessions
- Duplicates will exist in the current session history
- Only the last 500 commands are preserved

Fortunately it is easy to greatly improve Bash history.

Firstly, in the previous *Shell Options* section we enabled the important `shopt
-s histappend` option which appends the current shell history to the history
file rather than overwriting it.

And to compliment that shell option change these are history controls I
recommend in `~/.bashrc`:

```sh
HISTCONTROL=ignoreboth:erasedups # Erase duplicates
HISTIGNORE=?:??                  # Ignore one and two letter commands
HISTFILESIZE=99999               # Max size of history file
HISTSIZE=99999                   # Amount of history to preserve
```

Lastly to share history between concurrent shell sessions:

```
PROMPT_COMMAND="history -a; history -n"
```

This will immediately append the current shell history to the history file and
load history from other active sessions in the the current shell history each
time the prompt is updated.

Prompt
------

Speaking of prompts, an informative, colorful and configurable prompt is greatly
beneficial nowadays. The cross-shell [Starship](https://starship.rs) prompt has
quickly become a favourite due to its compatibility and customization
simplicity. For many, Starship is all the prompt they will ever need.

If using Homebrew, install as follows:

```sh
brew install starship
```

Then add the following to `~/.bashrc`:

```sh
eval "$(starship init bash)"
```

And configure according to [the documentation](https://starship.rs/config) on
the Starship website.

Be aware, Starship has now taken control of the `PROMPT_COMMAND` and the history
sharing noted in the previous section will no longer apply. I am lead to believe
the following `~/.bashrc` configuration will restore history sharing:

```sh
function history_sharing(){
    history -a && history -n
}
starship_precmd_user_func="history_sharing"
```

With all that said, I still use my own
[bash-seafly-prompt](https://github.com/bluz71/bash-seafly-prompt) extension
instead, which predates Starship, and which also has [superior Git status
performance](https://github.com/romkatv/gitstatus/issues/385#issuecomment-1532411950)
especially when combined with either the
[git-status-fly](https://github.com/bluz71/git-status-fly) or
[gitstatus](https://github.com/romkatv/gitstatus) utilities.

Other popular choices include:

- [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt)
- [Liquid Prompt](https://github.com/nojhan/liquidprompt)

Fuzzy Finding
-------------

These days it is hard to imagine using an interactive shell without fuzzy
finding integration. The [fzf](https://github.com/junegunn/fzf) is the most
popular such fuzzy finding tool.

I wrote [Fuzzy Finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
article a few years ago. That article is still relevant and applicable these
days; if time permits I do recommend reading it.

In terms of the Bash shell, if using Homebrew install `fzf` as follows:

```sh
brew install fzf
```

Then add the following key-binding configuration to `~/.bashrc`:

```sh
. $HOMEBREW_PREFIX/opt/fzf/shell/key-bindings.bash
```

That will add the following three key-bindings:

- `Control-r` - fuzzy reverse history search
- `Control-t` - append fuzzy selections to the current shell command
- `Alt-c` - change to the fuzzy found directory

An assortment of custom [search
scripts](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html#search-scripts)
are also documented in the [Fuzzy Find in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
article, they are well worth inspecting. These include: editing a fuzzy found
file, killing a fuzzy found file, Git staging fuzzy found files and fuzzy Git
log browsing to name a few.

Modern Tooling
--------------

- Tools
  - exa talk about LS_COLORS
  - zoxide
  - bat
  - delta
  - qmv
- fzf-tab-completion

Timesaving Tips
---------------
- recipes & bindings
  - Fish-like autopushd & dirhistory
  - www / web_search
  - Zsh-like copybuffer, Control-o
  (https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/copybuffer)
  - Zsh-like copydir, current_working_directory / cwd
  (https://github.com/mwilc0x/ohmyzsh/tree/master/plugins/copydir)




Refer to:
  - 7 Command-Line Tools That Make Your Life Easier:
    https://levelup.gitconnected.com/7-command-line-tools-that-make-your-life-easier-d69c38850d6c
  - https://dev.to/zaiste/15-command-line-tools-to-make-you-better-at-shell-cli-35n6
  - https://news.ycombinator.com/item?id=30976207
