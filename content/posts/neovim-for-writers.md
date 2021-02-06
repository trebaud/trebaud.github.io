---
title: "Neovim for Writers"
date: 2022-07-31T05:56:32-04:00
draft: false
---

In this quick post I share how to optimize your Neovim setup for writing. Neovim is a great option for writers, it combines an ergonomic resting default position with powerful text manipulation capabilities.

First let's install a good syntax highlighter that includes Markdown, I use `polyglot` which is pretty complete and works well.

```
Plug 'sheerun/vim-polyglot'

```

Let's also install the Ranger plugin for file navigation (note that you will need to install the `ranger` program also independently) and a markdown previewer plugin to check your results as you write.

```
Plug 'francoiscabrol/ranger.vim'
Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && yarn install' }
```

Next I use the `Goyo` plugin to get that minimalistic and distraction free look for a good writing experience.

```
Plug 'junegunn/goyo.vim'
```

Next let's install the very good `vim-pencil` plugin to manage line and word wraps to make neovim look like a real text editor.

```
Plug 'preservim/vim-pencil'
```

To activate this new "writing" mode simply type the following command in normal mode which concatenates two commands from the previous two plugins we just installed:

```
`:Goyo | SoftPencil`
```

Last tip, be sure to set the native vim spell checker so you don't have to make any mistakes ever again.

```
:set spell
```

Putting it all together we can write our init.vim configuration file to start Neovim automatically in this "writer" mode. That's it, you can use this basic setup and improve on it with your own ideas.

```vim
"--------------------------------------------------------------------------
" General settings
"--------------------------------------------------------------------------
set spell
set termguicolors

"--------------------------------------------------------------------------
" Key mappings
"--------------------------------------------------------------------------
" Use Ctrl-c for copy 
nmap <C-c> "+y
vmap <C-c> "+y
nmap <C-v> "+p
inoremap <C-v> <c-r>+
cnoremap <C-v> <c-r>+

"--------------------------------------------------------------------------
" Plugins
"--------------------------------------------------------------------------
" Automatically install vim-plug
let data_dir = has('nvim') ? stdpath('data') . '/site' : '~/.vim'
if empty(glob(data_dir . '/autoload/plug.vim'))
  silent execute '!curl -fLo '.data_dir.'/autoload/plug.vim --create-dirs  https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim'
  autocmd VimEnter * PlugInstall --sync | source $MYVIMRC
endif

call plug#begin(data_dir . '/plugins')
    Plug 'sheerun/vim-polyglot'
    Plug 'junegunn/goyo.vim'
    Plug 'preservim/vim-pencil'
    Plug 'iamcco/markdown-preview.nvim', { 'do': 'cd app && yarn install' }
    Plug 'francoiscabrol/ranger.vim'
    Plug 'rbgrouleff/bclose.vim'
    Plug 'sainnhe/sonokai'
call plug#end()
doautocmd User PlugLoaded

color sonokai

let g:mkdp_auto_start = 1

autocmd VimEnter * Goyo|SoftPencil

nnoremap <C-p> :Ranger<CR>
```

![](https://user-images.githubusercontent.com/8050949/185811620-04ca4de4-609a-4de7-851a-cf3abfb8fb47.png)
