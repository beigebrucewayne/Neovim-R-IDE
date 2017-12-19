# Neovim R IDE

A step-by-step walkthrough of how to convert Neovim into an R IDE, with a slight preference for Mac OS.

![cover-image](https://i.imgur.com/dnDB1o1.png)

## Table of Contents

- [Homebrew](#installing-homebrew)
- [R](#installing-r)
- [Neovim](#installing-neovim)
- [Vim-Plug](#installing-vim-plug)
- [Plugins](#plugins)
- [Key Mappings](#key-mappings)

I'm going to start from the bare bones basics. So, if you have Homebew, R, or Neovim installed you can skip ahead. If not, you're at the right place!

### Installing Homebrew

[Homebrew](https://brew.sh/) is an amazing package manager for Mac OS. If you aren't using it, you should. To install, head to your terminal and paste the following command:

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Installing R

Next up we're going to install R from Homebrew. You may think that [CRAN](https://cran.r-project.org/) is the preferred vendor, but Homebrew will help with updating us to the most current version.

```bash
brew tap homebrew/core
brew install Caskroom/cask/xquartz
brew install r
```

### Installing Neovim

Finally, the last brew we need is good 'ole Neovim. If you're unsure what the difference between Neovim and Vim is head [here](https://neovim.io/charter/).

```bash
brew install neovim
```

### Installing Vim-Plug

We are now past the basics and delving into crafting our `init.vim`, which is analogous to Vim's `.vimrc`. In order to make plugin management a breeze we're going to install [Vim-Plug](https://github.com/junegunn/vim-plug). Vim-Plug makes plugin management enjoyable, and provides a great framework to easily test new solutions.

Head to your terminal yet again and proceed:

```bash
curl -fLo ~/.local/share/nvim/site/autoload/plug.vim --create-dirs \
    https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

Next, you'll need to initialize it within your `init.vim`.

```vim
call plug#begin('~/.local/share/nvim/plugged')

" your plugins go here
Plug "junegunn/vim-easy-align"

call plug#end
```

Lastly, you'll need to reload your `init.vim` and type the command `:PlugInstall`

### Installing Plugins

### Key Mappings
