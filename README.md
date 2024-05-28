# .dotfiles

This is my dotfiles config for setting up the same configs on multiple systems if they're in need of a rebuild or they're just a new setup.

I'm using GNU Stow to create symlinks from the files in the home directory to the files located in the .dotfiles directory.

# Initial Setup

## Installing stow

Stow can be installed easily on Debian based systems with apt

``` sh
sudo apt install stow
```

## Folder structure

To get stow working first we need to create folder structure in .dotfiles that has parity with what we want to deploy.

For my case it will look something like this (assuming $HOME is the parent directory we're going to use)

```
.dotfiles/
    git/
        .gitconfig
    bashrc/
        .bashrc
    tmux/
        .tmux.conf
    nvim/
        .config
            nvim/
                init.lua
            lua/
                mappings.lua
                plugins/
                    init.lua
```

# New System Setup