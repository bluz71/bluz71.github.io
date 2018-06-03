---
title: Bash Shell Tweaks & Tips
layout: default
comments: true
published: true
---

Bash Shell Tweaks & Tips
========================

[Bash](https://www.gnu.org/software/bash) is the most common Unix shell. Bash
is highly ubiquitous due to it being the default user shell for various
flavours of Unix including: Linux, macOS and the Windows Subsystem for Linux
(WSL).

Due to this ubiquity, and the need to maintain backward compatibility, it is
rare that newer features of Bash are enabled by default. Some mistake this
*lack of default change* for stagnation and simply move across to a different
shell such as [Zsh](https://www.zsh.org) or [Fish](https://fishshell.com) for
their productivity enhancements.

This post will document a number of simple tweaks and tips, gleamed from the
interwebs, that will improve user productivity when using Bash. A special note
should be given to the [Sensible Bash](https://github.com/mrzool/bash-sensible)
project for providing a couple noteworthy tips.

Many *tweaks n' tips* noted in this post are baked into my
[bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) and
[inputrc](https://github.com/bluz71/dotfiles/blob/master/inputrc) files.

macOS, upgrade to Bash 4.x
--------------------------

macOS by default ships with a very old version of Bash, version 3.2, due to
licensing reasons. That version is over a decade old with many missing
features.

Please upgrade to version 4.x of Bash. This is most easily accomplished by
using the [Homebrew](https://brew.sh) package manager.

Install Homebrew followed by:

```sh
brew install bash
sudo echo /usr/local/bin/bash >> /etc/shells
chsh -s /usr/local/bin/bash
```

Launch a new shell and enter `echo $SHELL`, it should list
`/usr/local/bin/bash`.

I also recommend reading [this
post](https://www.topbug.net/blog/2013/04/14/install-and-use-gnu-command-line-tools-in-mac-os-x/)
followed by installation of the GNU command line utilities for macOS.

bash-completion
---------------

Out of the box Bash only provides rudimentary filesystem based completions
unlike the Zsh and Fish shells which offer command-aware completions. For
example, `shopt cd<TAB>` should command-aware complete with the options of
`cdable_vars` or `cdspell` as compared to completing with filenames or
directories starting with `cd` (which is what standard Bash will do).

The [bash-completion](https://github.com/scop/bash-completion) packages extends
the Bash shell with intelligent completions for many common commands.

Most Linux distributions install and enable bash-completion these days.

Homebrew based Bash on macOS on the other hand will require bash-completion
installation and invocation.

First install bash-completion:

```sh
brew install bash-completion
```

Then source bash-completion in your `~/.bashrc` file:

```sh
. $(brew --prefix)/etc/bash_completion
```

Confirm that bash-completion is enabled by trying the `shopt cd<TAB>` example
noted above, the aim is to have the `cdable_vars` and `cdspell` options
displayed.

If you have never used command-aware completion before it will be a revelation
how useful this capability is especially for commands such as `git` and `ssh`.
Basically you want to hit the TAB key early and often.

Readline Configuration
----------------------

The [GNU Readline](https://tiswww.case.edu/php/chet/readline/rltop.html)
library is used by Bash, and certain other utilities, for line-editing and
history management.

This library is configured by an `.inputrc` file in a user's home directory. A
list of recommended `~/.inputrc` settings follows.

- The *TAB* key cycles forward through the completion choices. Press an arrow
    key, such as right-arrow, to choose a selection.

    ```sh
    TAB: menu-complete
    ```

- The *Shift-TAB* key cycles backward through the completion choices. This is
    useful if you pressed *TAB* too many times and overshot the desired choice.
    Like *TAB*, press an arrow key, such as right-arrow, to choose a selection.

    ```sh
    "\e[Z": menu-complete-backward
    ```

- The first press of the completion key, *TAB*, will display a list of choices
    that match the given prefix, whilst the next press of the completion key
    will start cycling through the available choices.

    ```sh
    set menu-complete-display-prefix on
    ```

    The above listed completion settings will result in Zsh `AUTO_MENU`-like
    completion behaviour.

- Display completion matches upon the first press of the *TAB* key.

    ```sh
    set show-all-if-ambiguous on
    ```

- Enable colors when completing filenames and directories.

    ```sh
    set colored-stats on
    ```

    Filename and directory colors are defined in the `LS_COLORS` environment
    variable.

- When a completion matches multiple items highlight the common matching prefix
    in color.

    ```sh
    set colored-completion-prefix on
    ```

    The prefix color used will be the `so` option defined in the `LS_COLORS`
    environment variable.

    Note, this is a new option only available with the latest versions of
    Readline (version 7.0 and above) and Bash (version 4.4 and above).

- Ignore case when completing.

    ```sh
    set completion-ignore-case on
    ```

- Treat hypens and underscores as equivalent when completing.

    ```sh
    set completion-map-case on
    ```

- Automatically append the `/` slash character to the end of symlinked
    directories when completing.

    ```sh
    set mark-symlinked-directories on
    ```

- Enable incremental history navigation with the UP and DOWN arrow keys. This
    will use the already typed text as a required prefix when navigating
    through history.

    ```sh
    "\e[A": history-search-backward
    "\e[B": history-search-forward
    ```

- Disable beeps and do not display control characters.

    ```sh
    set bell-style none
    set echo-control-characters off
    ```

Readline Shortcuts
------------------

The Readline library also provides a number of useful default shortcuts.

- Go to the beginning of the current line.

    ```sh
    Control-a
    ```

- Go to the end of the current line.

    ```sh
    Control-e
    ```

- Go back one word on the current line.

    ```sh
    Alt-b
    ```

- Go forward one word on the current line.

    ```sh
    Alt-f
    ```

- Search back through history.

    ```sh
    Control-r
    ```

    This does a reverse search through history, from most recent to oldest, for
    commands that contain the chosen text. Hitting `Control-r` again cycles
    back through the matches whilst `Control-Shift-r` cycles forward.

- Append the last argument of the previous command to the end of the current
    command.

    ```sh
    Alt-.
    ```

    For example, after doing `mkdir some-long-directory` simply enter
    `cd <Alt-.>` to add in the `some-long-directory` component to the end of
    the `cd` command.

Bash Settings
-------------

A more productive Bash experience is achieved by enabling and using the newer
features the shell has made available.

The Bash shell is configured through a `.bashrc` file in a user's home
directory. A list of recommended `~/.bashrc` tweaks follows.

- Typing a directory name just by itself will automatically change into that
    directory.

    ```sh
    shopt -s autocd
    ```

- Automatically fix directory name typos when changing directory.

    ```sh
    shopt -s cdspell
    ```

- Automatically expand directory globs and fix directory name typos whilst
    completing. Note, this works in conjuction with the `cdspell` option listed
    above.

    ```sh
    shopt -s direxpand dirspell
    ```

- Enable the `**` globstar recursive pattern in file and directory expansions.

    ```sh
    shopt -s globstar
    ```

    For example, `ls **/*.txt` will list all text files in the current
    directory hierarchy.

- History settings.

    ```sh
    HISTCONTROL='erasedups:ignoreboth'
    HISTIGNORE=?:??
    HISTFILESIZE=9999
    HISTSIZE=9999
    PROMPT_COMMAND='history -a'
    shopt -s histappend
    ```

    The Readline incremental and reverse search commands are much more useful
    when a good amount of history is kept. Note, we will also ignore all
    commands that are only one or two characters long.

- Display Git branch details in the prompt.

    ```sh
    if [ -f /usr/local/etc/bash_completion.d/git-prompt.sh ]; then
        local GIT_PROMPT_PATH="/usr/local/etc/bash_completion.d/git-prompt.sh"
    elif [ -f /etc/bash_completion.d/git-prompt ]; then
        local GIT_PROMPT_PATH="/etc/bash_completion.d/git-prompt"
    else
        local GIT_PROMPT_PATH="/usr/share/git-core/contrib/completion/git-prompt.sh"
    fi
    GIT_PS1_SHOWUPSTREAM="auto"
    PS1="\h\$(__git_ps1) \w > "
    ```

    Note, this is only a rudimentary prompt configuration, usually you will
    want to pretty it up with colors. Refer to to the `prompt()` function in my
    [bashrc](https://github.com/bluz71/dotfiles/blob/master/bashrc) for a
    colorful prompt.

- Automatically shorten deep paths in the prompt. The `\w` option in `PS1`
    controls whether to display the path or not.

    ```sh
    PROMPT_DIRTRIM=4
    ```

    Choose a smaller number if you wish to display less path components.

- Enable history expansion with the *SPACE* key.

    ```sh
    bind Space:magic-space
    ```

    For example, `!!<SPACE>` will immediately expand to the last command.

Bash Aliases
-------------

A Bash alias is a user defined shortcut. Aliases save keystrokes. Do not be
afraid to alias long commands with shorter aliases, they will save you time.

As a starting point, here are some of the aliases from my `~/.bashrc` file.

- List command.

    ```sh
    alias ls='ls --color --classify --human-readable'
    alias ll='ls -l'
    alias ll.='ls -la'
    alias lls='ls -la --sort=size'
    alias llt='ls -la --sort=time'
    ```

  We want colorization with human-readable file sizes. The last two aliases
  will order by size (largest to smallest) or by time (newest to oldest).

- Easy navigation up a directory hierarchy.

    ```sh
    alias ..='cd ..'
    alias ..2='..; ..'
    alias ..3='..2; ..'
    alias ..4='..3; ..'
    alias ..5='..4; ..'
    ```

    No more need to type `cd ../../..`, just do `..3` instead.

- Git command.

    ```sh
    alias g=git
    complete -o default -o nospace -F _git g
    ```

    If you use git, then why not save a couple characters?

- Confirm unsafe file operations.

    ```sh
    alias cp='/bin/cp -i'
    alias mv='/bin/mv -i'
    alias rm='/bin/rm -i'
    ```

    This will prompt to confirm when copying over, moving over or deleting a
    file.

- List all files larger than a given size.

    ```sh
    alias llfs='find_by_size(){ find . -type f -size "$1" -exec ls --color --classify --human-readable -l {} \; ; }; find_by_size'
    ```

    For example, `llfs +10k` will find and  display all files larger than 10
    kilobytes in the currect directory hierarchy.

The z Directory Navigation Utility
----------------------------------

There are a number of ways to navigate a file system: plain `cd`, `pushd/popd`,
or defining a `CDPATH` environment variable as a short-circuit base directory.

The [z](https://github.com/rupa/z) utility provides a quicker way to change to
an already visited directory. The z utility, which works in both Bash and Zsh,
tracks the directories you visit with the `cd` command. Once trained you can
just use the `z` command with a portion of a path to jump to a visited
directory.

The *z* utility is most easily installed with Homebrew or Linuxbrew:

```sh
brew install z
```

Install the old fashioned way if you are not a Brew user.

Then source the `z.sh` file in your `~/.bashrc` file:

```sh
. $(brew --prefix)/etc/profile.d/z.sh
```

### Usage

First you need to train `z`:

```sh
cd ~/dotfiles
cd ~/projects/tickets_app
cd ~/projects/tickets_app/src
cd ~/projects/tickets
cd ~/projects/tickets/src
```

Now you can jump using the `z` command with the desired path hints:

```sh
z dot         # will jump to ~/dotfiles
z tic         # will jump to ~/projects/tickets
z tic app     # will jump to ~/projects/tickets_app
z tic app src # will jump to ~/projects/tickets_app/src
```

Though not Bash specific, the z utility is a fantastic tool that all Bash users
will find useful.

The qmv Rename Utility
----------------------

The [qmv](https://www.nongnu.org/renameutils) utility is used to rename files
by way of an auto-generated document of filenames that will be opened in your
editor ready for modification; the renames will be applied after the editor has
exited. This utility is especially useful for bulk renames where the power of
editor substitution can be used to quickly specify the desired renames.

`qmv` installation for macOS via Homebrew:

```
brew install renameutils
```

Linux (Debian flavoured) installation:

```
sudo apt install renameutils
```

By default `qmv` will use two editor columns, in my experience this consumes
too much valuable real-estate for little benefit, hence, I recommend the
following Bash alias (force `destination-only`):

```sh
alias qmv='qmv -f do'
```

### Usage

```sh
qmv **/*.JPG # rename to lowercase '.jpg' extension via substitution
qmv *.old    # rename to '.BAK' extension also via substitution
```

Like the z utility noted above, this `qmv` utility is not Bash specific, but it
will also prove very useful to all Bash users.

Style
-----

If you spend a lot of time in the command line then I recommend styling up the
terminal to suit your needs.

Some suggestions:

- Use a modern terminal and enable 256 colors.

- Use a nice monospace font. Fine choices being:
    [Hack](https://github.com/source-foundry/Hack),
    [Fira Code](https://github.com/tonsky/FiraCode),
    [Inconsolata](http://levien.com/type/myfonts/inconsolata.html) and my
    personal favourite [Ioveska](https://github.com/be5invis/Iosevka).

- Configure the `LS_COLORS` environment variable to highlight filetypes using
    256 colors.

- Display useful information in your prompt. Examples being: hostname,
    username, filesystem path and git branch to name a few.

- If you want to dabble into the world of Bash themes and other treats then
    have a look at [Bash-it](https://github.com/Bash-it/bash-it). Bash-it
    claims to be to Bash what
    [oh-my-zsh](https://github.com/robbyrussell/oh-my-zsh) is to Zsh.
    Personally I recommend configuring up the shell yourself, that will result
    in a leaner and faster shell.

Conclusion
----------

With just a little effort the Bash shell will become a more enjoyable and
productive environment.
