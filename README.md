src = https://www.youtube.com/watch?v=ud7YxC33Z3w

# Getting started

- make sure you have zsh (Z shell) installed on your system
- make it your main login shell via `chsh -s $(which zsh)`
- After changing your shell with `chsh`, log out and log back in
- make sure Git is installed
- backup any .zshrc you may already have via `mv ~/.zshrc ~/.zshrc.bak`
- create a brand new zsh config file via `touch ~/.zshrc`
- open this config file with your favorite text editor (which should be Vim)

# Plugin manager for zsh

The .zshrc file is where all of our zsh config is going to live.  
The first thing we're going to configure being a **plugin manager**.  

When it comes to zsh, there are a number of different plugin managers.  
We're going to use **zinit**, which provides pretty much all the bells & whistles you could possibly need.  

In order to add zinit, we first need to define where it's going to live.  
```bash
ZINIT_HOME="${XDG_DATA_HOME:-${HOME}/.local/share}/zinit/zinit.git"
```

The above command sets the `ZINIT_HOME` environment variable to define the location where the Zinit plugin manager is installed.  
Here’s a breakdown of what it does:
- `${XDG_DATA_HOME:-${HOME}/.local/share}` uses Bash **parameter expansion**. It checks if the environment variable `XDG_DATA_HOME` is set:
  - If `XDG_DATA_HOME` is set, its value is used.
  - If not, it defaults to `${HOME}/.local/share`, where ${HOME} is your home directory.
- `/zinit/zinit.git` is appended to the chosen base path, forming the full path to where Zinit is (or will be) installed.
  - If `XDG_DATA_HOME` is set to `/home/alice/.data`, then `ZINIT_HOME` becomes `/home/alice/.data/zinit/zinit.git`.
  - If the variable not set, and HOME is `/home/alice`, then `ZINIT_HOME` becomes `/home/alice/.local/share/zinit/zinit.git`




@2/17
