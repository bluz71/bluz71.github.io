---
title: Maximize Productivity Of The Bash Shell
layout: default
comments: true
published: true
---

Maximize Productivity Of The Bash Shell
=======================================

This is a 2023 followup to my 2018 [Bash Shell Tweaks &
Tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html) article.

Since that last article there has been a renaissance in shell-agnostic command
line tooling, often implemented in high-performance
[Rust](https://www.rust-lang.org), that has elevated the interactive shell
experience. Hence, now is the right time to revisit Bash with such tooling in
mind.

In this article we will: enable a host of useful Bash shell options, script
automatic `pushd` along with matching directory stack navigation bindings,
integrate an interactive completion interface and we will use modern command
line tooling to sensibly improve the interactive experience, among other tips
and suggestions.

Note, my [bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files
integrate all the suggestions documented in this article.

Dispelling Bash Misconceptions
------------------------------

Before proceeding, it is worth correcting some Bash misconceptions that still
intermittently persist:

- Bash supports changing directories without entering the `cd` command
- Bash supports simple file and directory path *autocorrection*
- `**` recursive globbing is supported
- Command `history` can be shared between concurrent Bash instances
- Bash does support *tab-completion* cycling
- Command and context-aware completion is supported through the Bash Completion
  package
- Interactive completion is supported by way of a 3rd party package

Keep reading for details about all the above.

Installation And Setup For macOS
--------------------------------

Since OS Catalina (2019), [Zsh](https://www.zsh.org) is now the default shell
for macOS. Apple still ships with Bash, but only version 3.2 which dates back to
2006; such a legacy version of Bash should be avoided for interactive use these
days.

Updating to a modern version of Bash is easily accomplished with the
[Homebrew](https://brew.sh) package manager:

```sh
brew install bash
echo /opt/homebrew/bin/bash | sudo tee -a /etc/shells
chsh -s /opt/homebrew/bin/bash
```

Note, if using an older Intel-based Mac, please replace `/opt/homebrew` with
`/usr/local`.

Please also install the Bash Completion package as follows:

```sh
brew install bash-completion@2
```

And lastly add the following to your `~/.bash_profile`:

```sh
[[ -r "$HOMEBREW_PREFIX/etc/profile.d/bash_completion.sh" ]] && . "$HOMEBREW_PREFIX/etc/profile.d/bash_completion.sh"
```

Readline Configuration
----------------------

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html) library
is used by Bash, and certain other utilities, for line-editing and history
management.

Many beneficial Readline features are disabled by default; thankfully, it is
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
useful shell options that are also disabled by default.

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

It is somewhat baffling that these options are disabled by default.

History
-------

In its out of the box configuration, the [Bash
history](https://www.gnu.org/software/bash/manual/html_node/Bash-History-Facilities.html)
experience is quite primitive:

- Each running shell will have its own history, and when a shell ends the
  running history of the current shell will entirely replace the contents of
  `~/.bash_history` file
- Hence, history from the next exiting shell will overwrite the history of the
  previously exited shell
- History will not be shared between concurrent shell sessions
- Duplicates will exist in the current session history
- Only the last 500 commands are preserved

Fortunately, it is easy to greatly improve Bash history.

Firstly, in the previous *Shell Options* section, we enabled the important
`shopt -s histappend` option which appends the current shell history to the
history file rather than overwriting it.

And to compliment that shell option change, these are history controls I
recommend in `~/.bashrc`:

```sh
HISTCONTROL=ignoreboth:erasedups # Erase duplicates
HISTIGNORE=?:??                  # Ignore one and two letter commands
HISTFILESIZE=99999               # Max size of history file
HISTSIZE=99999                   # Amount of history to preserve
```

Lastly, to share history between concurrent Bash sessions:

```
PROMPT_COMMAND="history -a; history -n"
```

This will immediately append the current shell history to the history file and
load history from other active sessions in the current shell history each time
the prompt is updated.

Prompt
------

Speaking of prompts, an informative, colorful and configurable prompt is easily
attained nowadays. The cross-shell [Starship](https://starship.rs) prompt has
become a fan-favourite due to its compatibility and customization simplicity.
For many, Starship is all the prompt they will ever need.

Install as per [Starship instructions](https://starship.rs/#quick-install), then
add the following to `~/.bashrc`:

```sh
eval "$(starship init bash)"
```

And configure according to [the documentation](https://starship.rs/config) on
the Starship website.

Be aware, Starship has now taken control of the `PROMPT_COMMAND` and the history
sharing noted in the previous section will no longer apply. I am led to believe
the following `~/.bashrc` configuration will restore history sharing:

```sh
function history_sharing(){
    history -a && history -n
}
starship_precmd_user_func="history_sharing"
```

With all that said, I still use my own
[bash-seafly-prompt](https://github.com/bluz71/bash-seafly-prompt) package
instead, which predates Starship, and which also has [superior Git status
performance](https://github.com/romkatv/gitstatus/issues/385#issuecomment-1532411950)
when combined with either the
[git-status-fly](https://github.com/bluz71/git-status-fly) or
[gitstatus](https://github.com/romkatv/gitstatus) utilities.

Other popular prompt choices include:

- [bash-git-prompt](https://github.com/magicmonty/bash-git-prompt)
- [Liquid Prompt](https://github.com/nojhan/liquidprompt)

Fuzzy Finding
-------------

These days, it is hard to imagine using an interactive shell without fuzzy
finding integration. [fzf](https://github.com/junegunn/fzf) is the most
popular such fuzzy finding tool.

I wrote [Fuzzy Finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
article a few years ago. That article is still relevant and applicable these
days; if time, permits I do recommend reading it.

Install as per [fzf instructions](https://github.com/junegunn/fzf#installation),
then add the following key-binding configuration to `~/.bashrc`:

```sh
. /LOCATION/OF/FZF/INSTALLATION/shell/key-bindings.bash
```

That will add the following three key-bindings:

- `Control-r` - fuzzy reverse history search
- `Control-t` - append fuzzy selections to the current shell command
- `Alt-c` - change to the fuzzy found directory

An assortment of custom [search
scripts](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html#search-scripts)
are documented in the previously mentioned [Fuzzy Find in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
article, they are well worth inspecting. These include: editing a fuzzy found
file, killing a fuzzy found process, Git staging fuzzy found files and fuzzy Git
log browsing to name a few.

The [forgit](https://github.com/wfxr/forgit) project, which combines *fzf*
with interactive Git, may also be of interest.

Interactive Completion Powered By fzf
-------------------------------------

Unlike [Zsh](https://www.zsh.org) and [Fish](https://fishshell.com), Bash does
not provide a native interactive completion mechanism; that is, completion that
can be navigated with arrow keys to quickly accept to the desired completion.

As noted above in the Readline section, Bash does support completion cycling,
but when there are many completions there is no ability to interactively
navigate the displayed completions.

The excellent
[fzf-tab-completion](https://github.com/lincheney/fzf-tab-completion) package
however does provide interactive completions powered by the aforementioned *fzf*
utility.

Install by cloning the repository:

```sh
git clone --depth 1 https://github.com/lincheney/fzf-tab-completion ~/.fzf-tab-completion
```

Then add the following to `~/.bashrc`:

```sh
source $HOME/.fzf-tab-completion/bash/fzf-bash-completion.sh
bind -x '"\C-f": fzf_bash_completion'
```

This binds `<Control-f>` (`f` for fuzzy) to *fzf-tab-completion* whilst keeping
`<TAB>` bound to native Bash completion. I find native Bash `<TAB>` completion
preferable for most simple completions, whilst reserving *fzf-tab-completion*
for the few occasions when there are many completion matches. Note, a `<TAB>`
initiated completion can be converted to *fzf-tab-completion* just by pressing
`<Control-f>`.

Lastly, `<Control-f>` is just my preferred binding, another binding, such as
`<TAB>` itself, can be defined instead.

Modern Shell Tools
------------------

As mentioned in the introduction, there has been resurgence in command line tool
development in recent years. Some of these new tools are  direct replacements
for long-established core utilities.

A few that I actively use are:

- [bat](https://github.com/sharkdp/bat), an enhanced `cat` clone

- [delta](https://github.com/dandavison/delta), a syntax-highlighting pager for
  `diff` and `git` output

- [dust](https://github.com/bootandy/dust), an intuitive version of `du`

- [exa](https://github.com/ogham/exa), a modern replacement for `ls`

- [fd](https://github.com/sharkdp/fd), a simple, fast and user-friendly
  alternative to `find`

- [hyperfine](https://github.com/sharkdp/hyperfine), a modern benchmarking
  alternative to `time`

- [ripgrep](https://github.com/BurntSushi/ripgrep), a recursive pattern search
  alternative to `grep`

- [sd](https://github.com/chmln/sd), an intuitive `sed` alternative

- [zoxide](https://github.com/ajeetdsouza/zoxide), a smarter `cd` command,
  inspired by [z](https://github.com/rupa/z) and
  [autojump](https://github.com/wting/autojump), that remembers visited
  directories for instant navigation

Whilst not a new tool, I would also like to shout out (again), the brilliant
[qmv](https://www.nongnu.org/renameutils) utility which makes bulk renames a
breeze by way of your current `$EDITOR`. I discuss it in greater [detail
here](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html#the-qmv-rename-utility).

Similarly, for those interested, I detail the *ripgrep* and *fd* utilities
[here](https://bluz71.github.io/2018/06/07/ripgrep-fd-command-line-search-tools.html).

Miscellaneous Helpers
=====================

Lastly, I will end with a couple of simple Bash helpers, inspired by other
shells, that have enhanced my shell interaction.

Automatic pushd And Associated Navigation Bindings
--------------------------------------------------

The Fish shell automatically provides [directory
history](https://fishshell.com/docs/current/interactive.html#navigating-directories)
when changing directories along with associated `prevd` and `nextd` commands,
which themselves are bound to `<Alt-Left>` and `<Alt-Right>`, for directory
history traversal.

That functionality can be mimicked in Bash as follows:

```sh
cd() {
    local target="$@"
    if [[ $# -eq 0 ]]; then
        # Handle 'cd' without arguments; change to the $HOME directory.
        target="$HOME"
    elif [[ $1 == "--" ]]; then
        # Handle 'autocd' shopt, that is just a directory name entered without
        # a preceding 'cd' command. In that case the first argument will be '--'
        # with the target directory defined by the remaining arguments.
        shift
        target="$@"
    fi

    # Note, if the target directory is the same as the current directory do
    # nothing since we don't want to populate the directory stack with
    # consecutive repeat entries.
    if [[ "$target" != "$PWD" ]]; then
        builtin pushd "$target" 1>/dev/null
    fi
}

# Alt-Left: rotate back through the directory stack.
bind -x '"\C-x\C-p": "pushd +1 &>/dev/null"'
bind '"\e[1;3D":"\C-x\C-p\n"'
# Alt-Right rotate forward through the directory stack.
bind -x '"\C-x\C-n": "pushd -0 &>/dev/null"'
bind '"\e[1;3C":"\C-x\C-n\n"'
```

All `cd` executions will be recorded to the directory stack and `<Alt-Left>` and
`<Alt-Right>` will now act like a browser's back and forward buttons, but this
time for stacked directories.

Web Searching
-------------

The [Oh My Zsh](https://github.com/ohmyzsh/ohmyzsh) framework provides a handy
[web-search](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/web-search)
plugin.

We can mimic that as follows:

```sh
alias web='web_search'

web_search() {
    GOLD=$(tput setaf 222)
    GREEN=$(tput setaf 79)
    NC=$(tput sgr0)

    read -ep "$(echo -e "${GOLD}Search ${GREEN}➜ ${NC}")" search_term
    open "https://duckduckgo.com/?q=${search_term}" &>/dev/null
}
```

This will execute a [DuckDuckGo](https://duckduckgo.com) search using the
default browser when `web` is executed. I like DuckDuckGo because it provides
[bang](https://duckduckgo.com/bangs) shortcuts. For example, the following will
do a GitHub search, via the `!gh` bang, for Neovim:

```
Search➜ !gh Neovim
```

Copy Directory
--------------

Also inspired by *Oh My Zsh*, a clone of the
[copydir](https://github.com/mwilc0x/ohmyzsh/tree/master/plugins/copydir) plugin
which simply copies the current working directory to the system clipboard:

```sh
alias cwd='copy_working_directory'

copy_working_directory() {
    if [[ $OS == Linux ]]; then
        echo -n ${PWD/#$HOME/\~} | tr -d "\r\n" | xclip -selection clipboard -i
    elif [[ $OS = Darwin ]]; then
        echo -n ${PWD/#$HOME/\~} | tr -d "\r\n" | pbcopy
    fi
    # Also copy current directory to a tmux paste buffer if tmux is active.
    if [[ -n $TMUX ]]; then
        echo -n ${PWD/#$HOME/\~} | tr -d "\r\n" | tmux load-buffer -
    fi
}
```

Conclusion
==========

Hopefully this assortment of settings, utilities and helpers provides
inspiration to improve your interactive Bash experience.
