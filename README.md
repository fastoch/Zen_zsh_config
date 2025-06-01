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

The `~/.zshrc` file is where all of our zsh config is going to live.  
The first thing we're going to configure being a **plugin manager**.  

When it comes to zsh, there are a number of different plugin managers.  
We're going to use **zinit**, which provides pretty much all the bells & whistles you could possibly need.  

## zinit location

In order to add zinit, we first need to define where it's going to live.  
```bash
# Set the directory we want to store zinit and plugins
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

## zinit installation

If there is no directory at the ZINIT_HOME env var, then create this directory and clone down zinit into it:
```bash
# Download zinit if it's not there yet
if [ ! -d "$ZINIT_HOME" ]; then
  mkdir -p "$(dirname $ZINIT_HOME)"
  git clone https://github.com/zdharma-continuum/zinit.git "$ZINIT_HOME"
fi
```

All of the above makes sure that zinit is installed the first time the zshrc file is sourced.  

Now, we can add this line to our `.zshrc` file:
```bash
# Source/Load zinit
source "${ZINIT_HOME}/zinit.zsh"
```

Here's how our final `.zshrc` file looks like:
```bash
# Set the directory we want to store zinit and plugins
ZINIT_HOME="${XDG_DATA_HOME:-${HOME}/.local/share}/zinit/zinit.git"

# Download zinit if it's not there yet
if [ ! -d "$ZINIT_HOME" ]; then
  mkdir -p "$(dirname $ZINIT_HOME)"
  git clone https://github.com/zdharma-continuum/zinit.git "$ZINIT_HOME"
fi

# Source/Load zinit
source "${ZINIT_HOME}/zinit.zsh"
```

## Make sure everything works as expected

After saving and exiting our .zshrc file, run `zinit zstatus`  

---

# Zenful Prompt


@3/17
