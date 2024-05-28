# .dotfiles

This is my dotfiles config for setting up the same configs on multiple systems if they're in need of a rebuild or they're just a new setup.

I'm using GNU Stow to create symlinks from the files in the home directory to the files located in the .dotfiles directory.

## Initial Setup

### Installing stow

Stow can be installed easily on Debian based systems with apt

``` sh
sudo apt install stow
```

### Folder structure

To get stow working first we need to create folder structure in .dotfiles that has parity with what we want to deploy.

``` sh
mkdir ~/.dotfiles && cd ~/.dotfiles
git init
```

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

Once the folder structure has been created we can start using stow to create the symlinks. There's a few flags that will be useful:

``` sh
-t          Set target to DIR (default is parent of stow dir)
-n          Do not actually make any filesystem changes
-v          Adds verbosity
--adopt     (Use with care!)  Import existing files into stow package from target. This will be used to do the initial setup
```

Essentially the -n and -v flags will be used to test, without commiting, the setup applies to the correct files. The -t flag will be used to set the parent target directory (ie: $HOME). The --adopt flag will pull the contents of the existing files (ie: $HOME/.tmux.conf) in to the file we created in the .dotfiles directory.

``` sh
stow -nvt $HOME * --adopt # where * is everything

## or to be more specific
stow -nvt $HOME git/ tmux/ nvim/ bashrc/ --adopt
```

Once you've done that take off the -n flag and it wil commit the changes and create the symlinks

## Migrating to another system

On a new system that we want to clone our .dotfiles in to you'll have to back up the existing config files. Just rename them to .bak.

```sh
git clone https://github.com/calorushex/.dotfiles.git ~/.dotfiles
```

Another way to do this is to try get stow to delete the files (this won't back them up).

``` sh
cd ~/.dotfiles
stow -nvDt $HOME *
```

Notice we added the -D flag to delete. If the output in simulation mode (-n) looks ok then you can go ahead. If not then going to back to renaming the files might be the best bet.

```sh
stow -nvt $HOME git/ tmux/ nvim/ bashrc/
```

And again once you're happy you can remove the -n flag.