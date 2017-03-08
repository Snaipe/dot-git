# Dot Git

A git-centric way to manage your dotfiles.

Inspired by this [hackernews comment by StreakyCobra](https://news.ycombinator.com/item?id=11071754).

## Perks

* [x] **Minimal**: only needs `git` and `coreutils` (+ `e2fsprogs` if you want to use lock/unlock).
* [x] **No symlinks**: git manages your files inplace.
* [x] **Runnable from any directory**: no need to `cd ~` before doing some changes.
* [x] **Profile management**: Keep all your configs in a repository, only link what you need (run `dot profile --help` for info).
* [x] **Git-powered**: git commands are used to manage your files.
* [x] **Useful aliases**: extended with repo-specific aliases like `files` and `edit`.

## Quick Start

### Install

#### From scratch:

1. [Fork this repository](https://github.com/Snaipe/dot-git/fork), and copy its push url.
2. Run the following commands (make sure to change `your_repo_url` and `your_shell_rc`):

    ```bash
    $ git clone --bare your_repo_url ~/.config/dotgit
    $ echo "alias dot='PATH=\"\$HOME/.config/dotgit/bin:\$PATH\" git --git-dir=\"\$HOME/.config/dotgit/repo\" --work-tree=\"\$HOME\"'" >> ~/.your_shell_rc
    $ source ~/.your_shell_rc
    $ dot checkout -f --
    $ dot remote add origin your_repo_url
    $ dot add .config/dotgit/config
    $ dot commit -m "dot: added own remote to dotgit"
    ```

3. You're done! Enjoy your new git-flavored dotfile manager.
4. (optional) `dot rm README.md && dot commit -m "removed readme"`

#### From an existing dot-git repo:

Run the following commands (make sure to change `your_repo_url` and `your_shell_rc`):
```bash
$ git clone --bare your_repo_url .config/dotgit
$ git --git-dir="$HOME/.config/dotgit" --work-tree="$HOME" checkout -f --
$ source ~/.your_shell_rc
```

You may have to add the `dot` alias back if you did not version your shell rc.

### Usage

* Add some dotfiles to your repository:

```bash
$ dot add ~/.vimrc
$ dot commit -m "vim: added my configuration"
$ dot push
```

* Edit the dotfile matching some keywords:

```bash
$ dot edit vimrc        # will edit .vimrc
$ dot edit config dot   # will edit .config/dotgit/config
```

* List all commited dotfiles:

```bash
$ dot files             # all files
$ dot files vim         # all vim-related files
```

* Write-Lock/Unlock files (needs chattr)

```bash
$ dot files .vimrc      # check the permissions on .vimrc
-rw-r--r-- '.vimrc'
$ dot lock .vimrc       # lock vimrc
[sudo] Password for user :
$ dot files .vimrc      # .vimrc now has the 'i'mmutable flag.
irw-r--r-- '.vimrc'
$ dot edit .vimrc       # dot edit will unlock the file and re-lock it after editing
```

* Make a profile for your x11 config

```bash
$ dot profile add x11
$ dot profile x11 add .xinitrc .Xresources
$ dot profile x11 commit -m "added my xorg configuration"
$ dot push origin dot-x11
```

* Link a profile on another machine

```bash
$ dot fetch origin
$ dot branch -t dot-x11 origin/dot-x11
$ dot profile add x11
```

## F.A.Q.

**Q: I need to call `dot` to draw graphs, but you thoughtlessly
   replaced it with an alias. What do I do?**  
A: I don't know about you, but I only *need* to call dot once every full moon, so
   when I do, I just call `\dot`, which bypasses alias expansion. You're welcome.  

**Q: `dot profile --help` doesn't seem to work, how can I read that manual page?**  
A: Your manpath program may not be searching the PATH variable. Try setting
   `MANPATH=":$HOME/.config/dotgit/share/man"`.
