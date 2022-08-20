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

Next I use the `Goyo` plugin to get that minimalistic and distraction free look for a good writing experience.

```
Plug 'junegunn/goyo.vim'
```

Last but not least let's install the very good `vim-pencil` plugin to manage line and word wraps to make neovim look like a real text editor.

```
Plug 'preservim/vim-pencil'
```

To activate this new "writing" mode simply type the following command in normal mode which concatenates two commands from the previous two plugins we just installed:

```
`:Goyo | SoftPencil`
```

That's it, with this setup you should be good to go!
