---
title: Git - Shortcuts & Bash Tips
layout: default
comments: true
published: true
---

Git - Shortcuts & Bash Tips
===========================

The [Git](https://git-scm.com) version control system is a complex beast,
especially when used in the command-line with its myriad of tools and switches.

Over time and with experience most users will settle on their preferred set of
commands, shortcuts and helpers. This post contains my curated set of Git tips
and shortcuts.

Note, many, maybe even most, of these suggestions have been gleamed from the
interwebs over the years. I do not claim authorship on anything noted here.

The full set of Git aliases I use are listed in my
[gitconfig](https://github.com/bluz71/dotfiles/blob/master/gitconfig).

Lastly, this post assumes a basic knowledge of Git in the command-line. If you
are a Git-inside-editor person then this is not the post you are looking for.

g alias for Bash
----------------

The `git` command is three letters long. I am extremely lazy, I only want to
type one letter, `g`, to invoke Git because I do it so often.

Add the following to your `~/.bashrc`.

```sh
alias g=git
```

If using [bash-completion](https://github.com/scop/bash-completion), please
also add the following to allow the `g` alias to complete just like `git`
command.

```sh
complete -o default -o nospace -F _git g
```

**UPDATE (JAN 2019)**: Stealing an idea from the [thoughbot
dotfiles](https://github.com/thoughtbot/dotfiles). Instead of simply aliasing
`g` to `git`, as noted above, make it a smart alias:

```sh
alias g='_f() { if [[ $# == 0 ]]; then git status --short --branch; else git "$@"; fi }; _f'
```

When invoked without arguments `g` will do a short Git status, otherwise it
will just pass on the given arguments to the `git` command. Status is likely to
be the Git command one will execute the most, hence this simple enhancement does
prove very useful in practice.

Bash completion
---------------

If you are a Bash user and are not using
[bash-completion](https://github.com/scop/bash-completion), then I **strongly**
suggest you setup it up. Please refer to my [Bash Shell Tweaks &
Tips](https://bluz71.github.io/2018/03/15/bash-shell-tweaks-tips.html#bash-completion)
post for details.

Once enabled, you will be able to use TAB-completion for most Git commands and
related paths. This will be a large time-saver.

Git branch details in Bash prompt
---------------------------------

Once bash-completion has been enabled, I also suggest adding Git branch
information to your prompt.

As a starting point, add the following to your `~/.bashrc`:

```sh
if [[ -f /usr/local/etc/bash_completion.d/git-prompt.sh ]]; then
    local GIT_PROMPT_PATH="/usr/local/etc/bash_completion.d/git-prompt.sh"
elif [[ -f /etc/bash_completion.d/git-prompt ]]; then
    local GIT_PROMPT_PATH="/etc/bash_completion.d/git-prompt"
else
    local GIT_PROMPT_PATH="/usr/share/git-core/contrib/completion/git-prompt.sh"
fi
GIT_PS1_SHOWDIRTYSTATE=1
GIT_PS1_SHOWUPSTREAM="auto"
GIT_PS1_SHOWSTASHSTATE=1
. $GIT_PROMPT_PATH
PS1="\h\$(__git_ps1) \w > "
```

Note, this is a rudimentary prompt configuration, usually you will want to
spruce it up.

For example, one can install and use a Bash prompt script, such as my own
[seafly](https://github.com/bluz71/bash-seafly-prompt) prompt or
[bash-git-prompt](https://github.com/magicmonty/bash-git-prompt), which will
both emit Git details with visual flair.

Nicer diffs with git-delta
--------------------------

The [git-delta](https://github.com/so-fancy/diff-so-fancy) utility replaces
Git's default machine-readable `diff` output with one that is more human
readable.

Installation is most easily accomplished via [Homebrew](https://brew.sh) on
macOS and Linux.

```sh
brew install git-delta
```

Now enable diff-so-fancy.

```sh
git config --global core.pager 'delta'
```

General aliases
---------------

Note, some of these suggestions are enabled by default with modern versions of
Git, but I still like to explicitly enable them just in-case I inadvertently use
an old version of Git.

Only push the current branch, not multiple branches.

```sh
git config --global push.default simple
```

Automatically convert stray DOS format text files back to Unix format when
committing from Unix-style hosts.

```sh
git config --global core.autocrlf input
```

Enable colorization.

```sh
git config --global color.ui auto
```

Use nicer histogram diff'ing.

```sh
git config --global diff.algorithm histogram
```

Shut up command-line commit warning message **if** using a command-line editor
such as Vim.

```sh
git config --global advice.waitingForEditor false
```

Diff aliases
------------

```sh
git config --global alias.di 'difftool'
git config --global alias.dis 'difftool --staged'
```

Usage:

```sh
g di  # List unstaged differences
g dis # List staged differences
```

Stage alias
-----------

Interactively stage changes on a per-chunk basis, including new files.

```sh
git config --global alias.sg '!git add . -N && git add -p'
```

Usage:

```sh
g sg # Individually stage changes in the current branch
```

Unadd alias
-----------

```sh
git config --global alias.unadd 'reset HEAD'
```

Usage:

```sh
g unadd <file> # Unstage a file
```

Commit aliases
--------------

```sh
git config --global alias.ci 'commit'
git config --global alias.unci 'reset --soft HEAD~1'
git config --global alias.amend 'commit --amend'
```

Usage:

```sh
g ci    # Commit staged changes
g unci  # Undo the last commit. Note, do NOT do this for pushed commits.
g amend # Modify the most recent commit. Note, do NOT do this for pushed commits.
```

Branch aliases
--------------

```sh
git config --global alias.bb 'branch -vv'
git config --global alias.bd 'branch -d'
```

Usage:

```sh
g bb          # List all branches
g bd <branch> # Delete specified branch
```

Check-out aliases
-----------------

```sh
git config --global alias.co 'checkout'
git config --global alias.cob 'checkout -b'
```

Usage:

```sh
g co <path-or-branch> # Check-out a particular path or branch
g cob <branch>        # Create a new branch and immediately check-out into it
```

Log aliases
-----------

```sh
git config --global alias.ll 'log --graph --format="%C(yellow)%h%C(red)%d%C(reset) - %C(bold green)(%ar)%C(reset) %s %C(blue)<%an>%C(reset)"'
git config --global alias.llm '!git ll master..HEAD'
git config --global alias.llu '!git ll @{upstream}..HEAD'
git config --global alias.lld '!git ll HEAD..@{upstream}'
```

Usage:

```sh
g ll  # List commits in a compact colorful format
g llm # List commits that exist in current branch but not in master
g llu # List commits in current branch that are ready to be pushed up
g lld # List commits in the remote tracking branch that a ready to be pulled down
```

Merge aliases
-------------

```sh
git config --global alias.mg 'merge'
git config --global alias.mgs 'merge --squash'
git config --global alias.unmg reset --merge ORIG_HEAD'
```

Usage:

```sh
g mg <branch>  # Merge branch
g mgs <branch> # Squash merge the commits of the specified branch
g unmg         # Undo the last merge. Note, do NOT do this for pushed merges.
```

Remote aliases
--------------

```sh
git config --global alias.rr 'remote -v'
git config --global alias.rro 'remote show origin'
git config --global alias.rru 'remote show upstream'
```

Usage:

```sh
g rr  # Show remotes
g rro # Show state of local and remote tracking branches
g rru # Show state of local and remote upstream branches
```

Stash aliases
-------------

```sh
git config --global alias.sa '!sh -c "git stash apply stash@{$1}" -'
git config --global alias.sd '!sh -c "git stash drop stash@{$1}" -'
git config --global alias.sl 'stash list'
git config --global alias.ss 'stash save --include-untracked'
git config --global alias.ssp '!sh -c 'git stash show -p stash@{$1}' -'
```

Usage:

```sh
g sl        # List current stashes
g ss        # Save current changes with auto-generated name
g ss <name> # Save current changes with specified name
g sa 0      # Apply specified stash, use 'g sl' to list stash numbers
g sd 0      # Delete specified stash, use 'g sl' to list stash numbers
g ssp 0     # Show the changes of the specified stash in diff preview format
```

Fuzzy-finding command-line Git browser
--------------------------------------

The [fzf](https://github.com/junegunn/fzf) utility is an excellent command-line
fuzzy finder. With just a small of amount of scripting it is easy to create a
command-line fuzzy-finding Git log browser with commit previewing.

First install fzf. If using Homebrew.

```sh
brew install fzf
```

For other platforms.

```sh
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
```

Now add the following Git browser tooling to your `~/.bashrc` file.

```sh
alias gll='fzf_git_log'

fzf_git_log() {
    local commits=$(
      git ll --color=always "$@" |
        fzf --ansi --no-sort --no-height \
            --preview "echo {} | grep -o '[a-f0-9]\{7\}' | head -1 |
                       xargs -I@ sh -c 'git show --color=always @'"
      )
    if [[ -n $commits ]]; then
        local hashes=$(printf "$commits" | cut -d' ' -f2 | tr '\n' ' ')
        git show $hashes
    fi
}
```

Type `gll` in a Git repository. This will display a compact log list that can
be narrowed down by entering in fuzzy text at the prompt. Also, one can
navigate up and down the commit list to preview the differences of each commit.
Github-style browsing in the command-line, almost.

Refer to [Fuzzy Finding in Bash with
fzf](https://bluz71.github.io/2018/11/26/fuzzy-finding-in-bash-with-fzf.html)
for more such Git examples.

References
==========

Some suggestions in the post originated from the following:

- [Must Have Git Aliases: Advanced Examples](http://durdn.com/blog/2012/11/22/must-have-git-aliases-advanced-examples)

- [6 Git Aliases to Make Stashing Easier](http://thesimplesynthesis.com/post/6-git-aliases-to-make-stashing-easier)

- [Browsing git commit history with fzf](https://gist.github.com/junegunn/f4fca918e937e6bf5bad)
