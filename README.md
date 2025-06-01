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
For that, let's edit the `~/.zshrc` file:
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

Let's keep editing our `~/.zshrc` file.  

If there is no directory at the ZINIT_HOME env var, then create this directory and clone down zinit into it:
```bash
# Download zinit if it's not there yet
if [ ! -d "$ZINIT_HOME" ]; then
  mkdir -p "$(dirname $ZINIT_HOME)"
  git clone https://github.com/zdharma-continuum/zinit.git "$ZINIT_HOME"
fi
```

The above config makes sure that zinit is installed the first time the zshrc file is sourced.  

Now, we can add this line to our `.zshrc` file:
```bash
# Source/Load zinit
source "${ZINIT_HOME}/zinit.zsh"
```
This will source our zsh config file next time we open a terminal.  

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

## Make sure that everything works as expected

After saving and exiting our .zshrc file, run `zinit zstatus`  
If everything is set up correctly, you should see something like this:
![image](https://github.com/user-attachments/assets/9500fd30-05bb-4300-abf6-e39d95dcfdf7)

---

# Zenful Prompt

Now that our plugin manager for zsh has been set up and is ready-to-go, we can add some plugins.  
The first plugin we're going to add is our zenful prompt.  

When it comes to zsh, there are a number of different prompt options available, one of the most popular one being **starship.rs**.  
But in this zsh config, we will use **Powerlevel10k**.  

Before we can install it however, we need to get a nerd font set up on our system.  
We will use the **JetBrainsMono** Nerd Font.  

## If you're using Ghostty as your terminal application

To set JetBrains Mono as the default font in **Ghostty**, you generally do not need to do anything since JetBrains Mono is the embedded default font.  






@4/17
