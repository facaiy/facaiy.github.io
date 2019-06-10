---
layout: post
title: "MacTex: LaTeX minted包报错，TexShop未开启shell escape"
description: ""
category: misc
tags: [latex]
modify: 2019-06-11 07:22:48
---

update: 2019-06-11


LaTeX minted包对代码的渲染效果很漂亮。因为它需要使用python Pygmentize库，所以LaTeX引擎必须开启shell escape模式才能调用外部命令。如果未开启，则会报错：minted Error: You must invoke LaTeX with the `-shell-escape` flag

对于pdflatex或者xelatex来说，只要在命令行参数中追加`--shell-escape`就好，但对于使用图形界面MacTex的中文用户，在TexShop里会找不到xelatex对应的变更选项。为了解决这个问题，我们需要仿照xelatex，手动创建一份引摮配置，具体如下。

1. 打开配置目录：`~/Library/TeXShop/Engines` （是家目录下的Library文件夹）
2. 复制XeLaTeX.engine为XeLaTeX-shellescape.engine
3. 用文本编辑器打开XeLaTeX-shellescape.engine，在xelatex命令行后追加`--shell-escape`，内容似：
   ```bash
   xelatex  -file-line-error -synctex=1 --shell-escape "$1"
   ```
4. [可选] 设为默认配置。前往菜单栏，按路径：Preference -- Typesetting -- Default Command -- Command Listed Below里录入XeLaTeX-shellescape
5. 保存，重新打开TexShop

另外，如果编译时报错找不到Pygmentize，则需要运行如下命令，建立软链接：
```bash
sudo ln -s "$(which pygmentize)" /Library/TeX/texbin/pygmentize
```

参考：
+ [如何优雅地用XeLaTeX设置PDF元数据](https://zhuanlan.zhihu.com/p/38286971)
+ [How to invoke latex with the -shell-escape flag in TeXStudio (former TeXMakerX)?](https://tex.stackexchange.com/questions/99475/how-to-invoke-latex-with-the-shell-escape-flag-in-texstudio-former-texmakerx)
