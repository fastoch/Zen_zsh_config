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

## Set up the default font 

Before we can install it however, we need to get a nerd font set up on our system.  
We will use the **JetBrainsMono** Nerd Font.  

If you're using **Ghostty** as your terminal application, you don't need to do anything since JetBrains Mono is the embedded default font.  

## Install Powerlevel10k

Edit your ~/.zshrc file and add the following:
```bash
# Add in Powerlevel10k
zinit ice depth=1; zinit light romkatv/powerlevel10k
```

About the above command:
- The `ice` command allows us to pass arguments to the next zinit command
- `zinit ice depth=1;` passes `depth=1` to the `zinit light` command
- `zinit` makes use of `git` under the hood, which is where the `depth=1` argument comes from
- zinit has 2 commands for installing packages: `zinit load` & `zinit light`
  - both commands pretty much do the same thing, except `zinit load` has reporting and investigation built-in

## Configuring Powerlevel10k

Open up a new terminal window and you'll be greeted with the powerlevel10k configuration wizard.  
- The first questions are easy to answer and depend on whether you've properly set up your nerd font
- Then choose your favorite prompt style between lean, classic, rainbow, and pure
- Select your favorite prompt color between original and snazzy
- decide if you want to show current time
- select the prompt height and prompt spacing
- Enable the transient prompt if you want to remove the header from previous commands (helps focusing on current cmd)
- select the instant prompt mode (verbose is the recommended mode)
- apply changes to `~/.zshrc`

Our awesome zenful prompt is now ready to use!   

Note that a couple of lines were added at the start of our .zshrc file. These are to enable the **transient prompt**.  
Make sure to keep these lines at the top of the file so that the transient prompt is loaded first.  

We also have an additional line at the bottom of our config file.  
This line checks for the existence of a `.p10k.zsh` file, and then sources it if it exists.  
This file contains our powerlevel10k configuration, which we can actually edit to customize our prompt a bit further.  

Finally, we can also call the `p10k configure` command in order to restart the configuration wizard.  

# Big 3 Plugins

These three plugins are used to provide the foundation for our setup:
- zsh-syntax-highlighting
- zsh-completions
- zsh-autosuggestions

## Syntax Highlighting

We can add the first one adding this line to our `~/.zshrc` file:
```bash
# Add in zsh plugins (the big three)
zinit light zsh-users/zsh-syntax-highlighting
```
This plugin, as the name suggests, enables nice syntax highlighting for our commands.  

## Z shell completions

The next plugin to add is zsh-completions, which provides autocomplete functionality for a number of CLI tools.  
```bash
# Add in zsh plugins (the big three)
zinit light zsh-users/zsh-syntax-highlighting
zinit light zsh-users/zsh-completions
```

However, we also need to tell zsh to automatically load our completions whenever it starts:
```bash
# Load completions
autoload -U compinit && compinit
```
Now, when I open up a new terminal window and start typing out a command, I can press the Tab key to see any completions associated with it.  

You can see which tools this plug-in provides completions for on the GitHub repo: https://github.com/zsh-users/zsh-completions  

## Z shell autosuggestions

The last of the Big Three:
```bash
# Add in zsh plugins (the big three)
zinit light zsh-users/zsh-syntax-highlighting
zinit light zsh-users/zsh-completions
zinit light zsh-users/zsh-autosuggestions
```
This plug-in provides autosuggestions based on your command history, similar to what the **fish** shell provides.  
If the suggested command matches the one you want to run, just press the **right arrow** to confirm it and press Enter.  

# Old habits die hard

Of course, the usual keybindings still work:
- ctrl + E to jump to the end of a command 
- ctrl + A to jump to the start
- ctrl + U to delete a command (undo)
- ctrl + Y to write the previous command back (redo)
- ctrl + F to move forwards through the command
- ctrl + B to move backwards
- ctrl + L to move the prompt at the top
- up and down arrows to move through your command history

# Command history persistence

We currently have a bit of an issue.  
If we open up a new terminal window, none of the commands from my other shell session are being suggested.  

In order for our command history to persist between sessions, we need to set up and enable a few options inside of our `.zshrc` file.  
```bash
# History settings
HISTSIZE=4000
HISTFILE=~/.zsh_history
SAVEHIST=$HISTSIZE
HISTDUP=erase
setopt appendhistory
setopt sharehistory
setopt hist_ignore_space
```
- The first variable sets the number of commands saved in our history
- The second variable sets our history file
- The third variable needs to be the same size as the first one
- The fourth variable is set to erase any duplicate command from our history
- The `appendhistory` option causes zsh to append any commands to the history file rather than overwriting it
- The next option will share our command history across all zsh sessions
- The `hist_ignore_space` allows us to prevent a command from being written to the history file by adding a space before it
  - This is useful to prevent any sensitive information from being saved in our history file


@11/17
