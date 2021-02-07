---
title: "Neovim terminal fun"
description: "Configure your neovim terminal for maximum efficiency"
date: 2019-05-26T00:00:00-05:00
draft: false
tags: [neovim]
---

Neovim is Vim's forked cousin that aims to replace it by providing additional features and refactoring Vim's old code base. One of the cool stuff that comes with Neovim is its integrated terminal emulator. In this short blog post I will share how I use Neovim's terminal and how you can configure it to make it easier to use.

First install Neovim on your system, for that follow the official guide instructions here. To open Neovim's terminal you can to type:

```bash
  :te
  :term
  :terminal
```

To make it easier to use you'll want to modify some of the defaults that might refrain you from further using Neovim's terminal.

First map the weirdly `Ctrl-\ Ctrl-n` default binding for switching to normal mode to simply `Esc`.

```bash
  tnoremap <Esc> <C-\><C-n>
```

Then add this autocommand grouping for starting the terminal directly in insert mode, which I personally find should be the default expected behaviour when launching a terminal.

```bash
  augroup neovim_terminal
    autocmd!
    " Enter Terminal-mode (insert) automatically
    autocmd TermOpen * startinsert

    " Disables number lines on terminal buffers
    autocmd TermOpen * :set nonumber norelativenumber
  augroup END
```

Finally set the current directory in Neovim as the starting one in the terminal and open it in a new vertical pane. You can do all of that
with one single command. Here I map it to `Alt-t` (for Alternative terminal).

```bash
  map <A-t> :80vsp<CR>:let $VIM_DIR=expand('%:p:h')<CR>:terminal<CR>cd $VIM_DIR<CR>
```

And before I forget add this to your Neovim config file so as to close the terminal pane without too much embarrassment ;).

```bash
  nnoremap <Leader>d :bd!<CR>
```

(I personally map my Leader key to `\`)

With all of that in place you can now setup quite an interesting workflow. I usually use the terminal for launching a service associated with the codebase I'm working on,
like a server, or launching a set of unit tests. That way I can easily switch to normal mode and search on the terminal's output or copy some piece of standard ouput
and then paste it as a new terminal command. All of that using vim's classic shortcuts of course!

Neovim's terminal is a powerful addition to the now classic editor and with the right configuration it constitutes an interesting and simple alternative to other tools such as tmux or even i3
which also aim to integrate the terminal to the developer's workflow.

![neovim-term](/img/neovimterm.png)
