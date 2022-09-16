---
title: "neovim customization"
date: 2022-07-03T08:48:38+05:30
draft: true
tags: ["neovim", "vi", "ricing", "productivity"]
draft: false
description: "A Basic Minimal Neovim Config in vimscript"
ShowRssButtonInSectionTermList: true
UseHugoToc: true
---

### Why use vim and neovim over other text editors and IDEs?
*For me, I've always loved minimalism and performance over ease of use and other flashy bloat no one really uses and cares about. This why I personally use Terminal-based text editors. Preferably VIM or Neovim. Tho I currently, am running Neovim, but both of them are basically the same. NeoVIM is a little bit more extensible than VIM and easy to manage imo. But enough talking, lets start configuring our VIM/NVIM to make it more like an actual developing environment.*  

**IMPORTANT:** Since I mainly use Neovim, some of the stuff written in this blog might not work for vim users, for them just avoid the once that are not compatible, if I find any alternatives to those plugins and setting, I will update the blogs with the details.

### Step 1: Setting up the config files and directory for vim and neovim[^1]
Follow the commands given below to make all the necessary files and directories for neovim

```bash
cd
mkdir -p ~/.config/nvim
touch ~/.config/nvim/init.vim
```
The **init.vim** file is the config file that we will edit in this post to make our base neovim to look and act like an actual IDE.

### Step 2: Editing the init.vim file
To edit this **init.vim** open the file with the text editor of your choice and paste the following text

```vim
:set number
:set relativenumber
:set autoindent
:set expandtab
:set tabstop = 4
:set shiftwidth = 4
:set smarttab
:set softtabstop = 4
```

These parameters set basic functionality for vim such as number lines, auto indentation, amd smart tabs. By default vim doesnt support mouse and its kinda normal to not use mouse since everything in vim can be done using the keyboard but if you are a soy and want to use mouse type this this too
```vim
:set mouse = a
```

For now we have done all the basic settings.

### Step 3: Adding a Plug-In Manager
For this setup we will use Vim-Plug to manage our plugins.  
1. Install Curl  
    **Debian-based**
    ```bash
    apt install curl
    ```
    
    **Gentoo**
    ```bash
    emerge curl
    ```
    
    **OpenBSD**
    ```bash
    pkg_add curl
    ```
    
    **Arch-based**
    ```bash
    pacman -S curl
    ```
    
    **Void**
    ```bash
    xbps-install -S curl
    ```

2. Create the installation directories, download, and install Vim-Plug. 
```bash
 curl -fLo ~/.vim/autoload/plug.vim --create-dirs https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim
```

### Step 4: Installing Plug-ins
In your init.vim file, add these lines at the bottom of what we previously wrote.  
```vim
call plug#begin()



call plug#end()
```

We will be able to add plugins in between the two commands.  

Now we will add our first plug-in  

Find the desireable plug-ins from [VIM Awesome](https://vimawesome.com/) this is one of the best places to find vim and neovim plugins.  
For this tutorial we will add couple of plug-ins to make our neovim look good and work like an ide.  
```vim
Plug 'tpope/vim-fugitive'                   " Git integration in to nvim
Plug 'Yggdroot/indentLine'                  " Line Indentations
Plug 'farmergreg/vim-lastplace'             " Continue from where you left last time
Plug 'raimondi/delimitmate'                 " Provides insert mode auto-completion for special-characters
Plug 'tpope/vim-markdown'                   " Markdown runtime files
Plug 'tpope/vim-surround'                   " Change paranthesis and quotes into other forms quickly
Plug 'scrooloose/nerdtree'                  " File navigator
Plug 'vim-scripts/indentpython.vim'         " Indentation script for python
Plug 'alvan/vim-closetag'                   " Makes a close tag for html quickly
Plug 'luochen1990/rainbow'                  " Provides different colors to different paranthesis
Plug 'airblade/vim-gitgutter'               " Shows git diffs in the sign columns
Plug 'lilydjwg/colorizer'                   " Provides color for the #rrggbb or #rgb color format in files
Plug 'vim-airline/vim-airline'              " Powerline Theme / Status line
Plug 'vim-airline/vim-airline-themes'       " Themes for vim-airline
Plug 'rafi/awesome-vim-colorschemes'        " Change colorschemes on the fly for vim and nvim
Plug 'ryanoasis/vim-devicons'               " Icons
Plug 'SirVer/ultisnips'                     " Code completion using snippets from vim-snippets and custom snippets
Plug 'honza/vim-snippets'                   " Provides snippets for ultisnips
```

First of all add few keybinding to open up NERDTree and make vertical and horizontal splits in your `init.nvim`.  
```vim
nnoremap <C-f> :NERDTreeFocus<CR>
nnoremap <C-n> :NERDTree<CR>
nnoremap <C-t> :NERDTreeToggle<CR>
nnoremap <A-h> :vsplit<CR>
nnoremap <A-k> :split<CR>
```

After you've saved and exited the file now open up neovim run `:PlugInstall`. This will install all the plugins in the plugged directory in `.local/share/nvim/plugged/`.  

Since we installed `vim airline` and `vim colorschemes` we can change the look and feel of neovim/vim. Find your favorite vim colorscheme [here](https://github.com/rafi/awesome-vim-colorschemes) also find your favorite airline theme [here](https://github.com/vim-airline/vim-airline-themes)  
For this blog, we will use the gruvbox theme along with the base16 airline theme. To make this permanent write these in your nvim config. 

```vim
colorscheme gruvbox

let g:airline_left_sep = ''
let g:airline_left_alt_sep = ''
let g:airline_right_sep = ''
let g:airline_right_alt_sep = ''
let g:airline_theme='base16'
```

Since we also have a plugin for autoindentation and indent lines. We have to add these lines to the config too.

```vim
let g:indentLine_fileTypeExclude = ["help", "nerdtree", "diff"]
let g:indentLine_fileTypeExclude = ["help", "nerdtree", "diff", "markdown"]
let g:indentLine_bufTypeExclude = ["help", "terminal"]
let g:indentLine_showFirstIndentLevel = 1
let g:indentLine_indentLevel = 7
let g:indentLine_char_list = ['|', '¦', '┆', '┊']
```

### (OPTIONAL)Adding Code-suggestions
This section is a bit tidious and is only available for neovim users so if you use vim this section is irrelevnet to you.  
Add this plug-in in your init.vim file.
```vim
Plug 'neoclide/coc.nvim'                    " Code suggestions and completion
```

Now this plugin wont run without nodejs in your machine so follow these steps to make it work.  

1. Install Nodejs and npm 

    **Debian**
    ```bash
    apt install nodejs npm
    ```
    
    **Arch**
    ```bash
    pacman -S nodejs npm
    ```
    
    **Gentoo**
    ```bash
    emerge nodejs
    ```
    
    **OpenBSD**
    ```bash
    pkg_add node
    ```
    
    **Void**
    ```bash
    xbps-install nodejs
    ```

    **For Other Distos and operating systems visit [https://nodejs.org/en/download/package-manager/](https://nodejs.org/en/download/package-manager/)**

2. Additional Packages
    Install yarn
    ```bash
    npm install yarn
    ```
    After yarn is finished installing, execute the following command  
    ```bash
    cd ~/.local/share/nvim/plugged/coc.nvim/
    yarn install
    yarn build
    ```

    Now open up an nvim install and check if any error is shown, if not then it is successfully installed. Otherwise check online for errors. 

    From here on out check [this github repository](https://github.com/neoclide/coc.nvim/wiki) to find your desired language to use and ways to download the language server. This is a bit tidious process but here is one example. 
#### (EXAMPLE)Installing the python language server
1. Install python3 and python3-pip  
2. Install jedi from the pip repository
```bash
pip install jedi
```
3. Open up and nvim instance and run `:CocInstall coc-python` this will install the python module for Coc.


*Follow this type of procedure to install all of your favorite language servers and have fun coding*  

<!-- Footer -->
[^1]: Jump back to the top

