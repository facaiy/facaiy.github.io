---
layout: post
title: "vim nerdtree tab关闭标签页报错WinEnter Autocommands"
description: ""
category: misc
tags: [vim]
modify: 2023-09-04 13:45:39
---

update: 2023-09-04

年初开始，vim关闭标签页就会报错：

```
Error detected while processing WinEnter Autocommands for "*"..function <SNR>147_WinEnterHandler
[10]..<SNR>147_CloseIfOnlyNerdTreeLeft:
```

去vim-nerdtree-tabs查了下，发现网友们已经讨论出修复方法（[issue #102](https://github.com/jistr/vim-nerdtree-tabs/issues/102)）。

具体做法是找到插件：
```bash
~/.vim/bundle/vim-nerdtree-tabs/nerdtree_plugin/vim-nerdtree-tabs.vim
```

将函数`CloseIfOnlyNerdTreeLeft`实现改成： 

```vim
fun! s:CloseIfOnlyNerdTreeLeft()
  if exists("t:NERDTreeBufName") && bufwinnr(t:NERDTreeBufName) != -1 && winnr("$") == 1
    call timer_start(1, {-> execute('q') }) " close buffer after we exit autocmd
    call timer_start(200, {-> execute('vertical resize 31') }) " window sizing is goofed up, so fix it
    call timer_start(250, {-> execute('wincmd w') }) " shift focus from NerdTree window to buffer window
  endif
endfun
```

测了下，完美。
