---
title: Git Tips & Aliases
layout: default
comments: true
published: false
---

# Git Tips & Aliases

The [Git](https://git-scm.com) version control system is a complex beast,
especially when used at the command-line with its myriad of tools and switches.

Over time however, most users will settle on their preferred subset of `git`
commands and shortcuts. This post contains my curated set of tips and shortcuts
(aka `git aliases`). Note, many maybe even most, of these suggestions have
been gleamed from the interwebs over many years. I do not claim authorship on
anything noted here.

The full set of Git aliases I use are listed in my
[gitconfig](https://github.com/bluz71/dotfiles/blob/master/gitconfig).

## g alias for Bash

The `git` command is three letters long. I am extremely lazy, I only want to
type one letter, `g`, to invoke Git.

Add the following to your `~/.bashrc`:

```sh
alias g=git
```

If using [bash-completion](https://github.com/scop/bash-completion), please
also add:

```sh
complete -o default -o nospace -F _git g
```

## Bash completion

If you are a Bash user and are not using
[bash-completion](https://github.com/scop/bash-completion), then I **strongly**
suggest you setup it up. Please refer to my [Bash Shell Tweaks &
Tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html) for
details.

Once enabled you will be able to use TAB-completely for most Git commands and
related paths.

## Git branch details in Bash prompt

Once bash-completion has been enabled I suggest enabling Git branch information
in your prompt.

Add the following to your `~/.bashrc`:

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

## Nicer diffs with diff-so-fancy

The [diff-so-fancy](https://github.com/so-fancy/diff-so-fancy) utility replaces
Git's default machine-readable `diff` output with one that is much more human
readable.

Installation is most easily accomplished via [Homebrew](https://brew.sh) on
macOS and [Linuxbrew](http://linuxbrew.sh) on Linux.

```sh
brew install diff-so-fancy
```

Now enable diff-so-fancy.

```sh
git config --global core.pager "diff-so-fancy | less --tabs=4 -RFX"
```

`g diff` output will now be much nicer.

## General aliases

Note, most of these above suggestions should already be enabled by default with
modern versions of Git, but I still like to explicity enable them.

Only push the current branch, not multiple branches.

```sh
git config --global push.default simple
```

Automatically convert accidental DOS format text files back to Unix format when
committing from a Unix-like host.

```sh
git config --global core.autocrlf input
```

Enable on colorization.

```sh
git config --global color.ui auto
```

Shutup warning message **if** using a command-line editor such as Vim.

```sh
git config --global advice.waitingForEditor false
```

## Diff aliases

```sh
git config --global alias.di 'difftool'
git config --global alias.dis 'difftool --staged'
```

Usage:

```sh
g di  # List unstaged differences
g dis # List staged differences
```

## Status alias

```sh
git config --global alias.st 'status --short --branch'
```

Usage:

```sh
g st # Working tree status in compact notation
```

## Stage alias

Incrementally stage changes including new files.

```sh
git config --global alias.sg '!git add . -N && git add -p'
```

Usage:

```sh
g sg # Individually stage all changes in the current working tree
```

## Commit aliases

```sh
git config --global alias.ci 'commit'
git config --global alias.unci 'reset --soft HEAD~1'
```

Usage:

```sh
g ci   # Commit stage changes
g unci # Undo the last commit. Do NOT do this for pushed commits.
```

## Branch aliases

```sh
git config --global alias.bb 'branch -vv'
git config --global alias.bd 'branch -d'
```

Usage:

```sh
g bb          # List all branches
g bd <branch> # Delete specified branch
```

## Check-out aliases

```sh
git config --global alias.co 'checkout'
git config --global alias.cob 'checkout -b'
```

Usage:

```sh
g co <path-or-branch> # Check-out a particular path or branch
g cob <branch>        # Create a new branch and immediately check-out
```

## Log aliases

```sh
git config --global alias.li 'log --graph --format=format:\"%C(yellow)%h%C(red)%d%C(reset) - %C(bold green)(%ar)%C(reset) %s %C(blue)<%an>%C(reset)\"'
git config --global alias.llm '!git ll master..HEAD'
git config --global alias.llom '!git ll origin/master..HEAD'
```

Usage:

```sh
g ll   # List commits in a compact colorful format
g llm  # List commits that differ between current branch and master
g llom # List commits that differ between current branch and origin/master
```

## Merge aliases

```sh
git config --global alias.mg 'merge'
git config --global alias.mgs 'merge --squash'
```

Usage:

```sh
g mg <branch>  # Merge branch
g mgs <branch> # Squash merge the commits of the specified branch
```

## Remote aliases

```sh
git config --global alias.rr 'remote -v'
git config --global alias.rso 'remote show origin'
git config --global alias.rsu 'remote show upstream'
```

Usage:

```sh
g rr  # Show remotes
g rso # Show state of local and remote tracking branches
g rsu # Show state of local and remote upstream branches
```

## Stash aliases

```sh
git config --global alias.sa '!sh -c "git stash apply stash@{$1}" -'
git config --global alias.sd '!sh -c "git stash drop stash@{$1}" -'
git config --global alias.sl 'stash list'
git config --global alias.ss 'stash save'
```

Usage:

```sh
g sl     # List current stashes
g ss     # Save current changes with auto-generated name
g ss foo # Save current changes with specified name
g sa 0   # Apply top stash, use 'g sl' to list stash numbers
g sd 0   # Delete specified stash, use 'g sl' to list stash numbers
```

Note, most of these stash aliases were provided by the following post:
[6 Git Aliases to Make Stashing
Easier](http://thesimplesynthesis.com/post/6-git-aliases-to-make-stashing-easier)

## Conclusion

Hopefully there will be a tip or alias contained in the above post that even
the most seasoned of Git user will find useful.
