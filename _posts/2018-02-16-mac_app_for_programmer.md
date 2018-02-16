---
layout: post
title: "Mac新机配置记录"
description: ""
category: misc
tags: []
modify: 2018-02-16 16:44:52
---

update: 2018-02-16


今天是农历大年初一，新年快乐！

最近换了工作，因为新公司数据安全政策，不能使用Mac Time Machine（时光机器）进行系统迁移，所以只能重新手配一次系统。我自己有些配置偏好，林林杂杂，为了以后参考方便，趁今天时间富裕，整理集于一处。

我比较偏爱键盘流，又习惯了VIM键绑定风格，所以配置基本都是围绕着快速操作的目的设计。


### 常用配置

+ 下载安装配置文件：[facaiy/facai-tools](https://github.com/facaiy/facai-tools)

+ 下载配色方案：[altercation/solarized](https://github.com/altercation/solarized)


### 包管理器

+ 下载安装[Homebrew](https://brew.sh/)

```bash
# 更新
brew update
```

+ 开启FileVault
+ 开启Time Machine


### 基础工具

+ Git: `brew install git`

+ VIM: `brew install vim --with-lua`
  - 安装VIM包管理器：[VundleVim/Vundle.vim](https://github.com/VundleVim/Vundle.vim)，根据使用说明，安装全部插件。

+ 终端: `brew cask install iterm2`
  - 切换Solarized Dark配色方案: Profiles --> Colors --> Color Presets
  - 变更字体：Profiles --> Text --> Font:
    1. Font: 16pt Monaco
    2. Non-ASCII Font: 18pt DFKai-SB
  - 恢复Alt键：Profiles --> Keys: 变更Option Keys为`ESC+`


### 基础环境

+ 切换Shell: 从默认Bash切换到Zsh，下载安装[prezto](https://github.com/sorin-ionescu/prezto)

+ 安装Python: [anaconda](https://www.anaconda.com/)发行版

```bash
brew cask install anaconda
```

+ 安装Java8:

```bash
brew install caskroom/versions/java8
```


### 生产力工具

+ 分屏
  1. 安装Tmux：`brew install tmux`
  2. 安装包管理器：[tmux-plugins/tpm](https://github.com/tmux-plugins/tpm)，并根据说明，更新全部插件。


```bash
# 快速跳转：j键
brew install autojump

# 统一命令行和X剪贴板，见Tmux配置：
brew install reattach-user-namespace

# 命令速查表
brew install tldr
```

+ 本地笔记应用：jupyter安装扩展：[jupyter_contrib](https://github.com/ipython-contrib/jupyter_contrib_nbextensions)
  1. `conda install -c conda-forge jupyter_contrib_nbextensions`
  2. 根据说明，安装VIM键绑定。

+ 笔记应用：evernote: `brew cask install evernote`

+ 浏览器：
  1. 安装Firefox-ESL: `brew install caskroom/versions/firefox-esr`
  2. 常用扩展：
     + Adblock Plus: 广告屏蔽
     + Nightly Tester Tools: 强制兼容
     + Vimperator: VIM绑定
     + All-in-One Sidebar: 侧边栏
     + Menu Wizard: 订制菜单栏
     + Tab Groups: 标签分组
     + Tab Mix Plus: 标签管理
	   - Peference --> Links --> Force to open in new tab: Links to other sites
	   - Peference --> Events --> New Tabs --> Open other tabs next to current one
     + Evernote Web Clipper
     + Lastpass: 密码管理
     + FlashGot: 下载管理
     + FoxyProxy Standard: 代理
     + gtranslate: 翻译
  3. 地址栏安装搜索引擎：YouDao, Github

+ Finder窗口管理器：
  - 10.11及以下：[XtraFinder](https://www.trankynam.com/xtrafinder/)
  - 10.12及以上：[TotalFinder](https://totalfinder.binaryage.com/)，收费软件


### 编程

+ Python IDE: `brew cask install pycharm-ce`
  - python切换到anaconda版本
  - 安装IdeaVim
+ Java/Scala IDE: `brew cask install intellij-idea-ce`
  - 安装IdeaVim
+ C++: `brew cask install visual-studio-code`


### 其他

+ [pomotodo](https://pomotodo.com/): 时间管理 `brew cask install pomotodo`
+ mactex: LaTeX `brew cask install mactex`
  - 编译参考： [facaiy/banyuan-ppt](https://github.com/facaiy/banyuan-ppt)

+ 输入法：[Squirrel](http://rime.im/) `brew cask install squirrel`
  - 安装：[仓颉单字方案](https://github.com/facaiy/facai-tools/tree/master/config/squirrel)

+ StarUML: 框架图 `brew cask install staruml`
+ xmind: 思维导图 `brew cask install xmind`

+ Haroopad: markdown编辑器 `brew cask install haroopad`
+ 下载软件：
   - `brew cask install progressive-downloader`
   - `brew install aria2`

+ 科学上网：`brew cask install shadowsocksx-ng`

<br/>
<br/>

参考阅读：

+ [程序员如何优雅地使用 macOS？](https://www.zhihu.com/question/20873070)
