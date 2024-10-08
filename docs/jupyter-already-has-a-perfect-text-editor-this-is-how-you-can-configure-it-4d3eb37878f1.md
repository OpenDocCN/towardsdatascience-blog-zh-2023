# Jupyter 已经有了完美的文本编辑器：这就是你可以配置它的方法

> 原文：[`towardsdatascience.com/jupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1`](https://towardsdatascience.com/jupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1)

## 如何在 Jupyter 中获得类似 VS Code 的体验，使用一个优秀的文本编辑器

[](https://dpoulopoulos.medium.com/?source=post_page-----4d3eb37878f1--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----4d3eb37878f1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d3eb37878f1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d3eb37878f1--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----4d3eb37878f1--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d3eb37878f1--------------------------------) ·7 分钟阅读·2023 年 3 月 3 日

--

![](img/c40ed0aa20061ca652dd94fb8205a3a4.png)

[Douglas Lopes](https://unsplash.com/@douglasamarelo?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上的照片

这篇文章是系列文章的第二部分。查看完整系列： 第一部分， 第三部分， 第四部分。

我们之前的文章指出，许多工程师并不认为 JupyterLab 是一个完整的 IDE。一个主要原因是 JupyterLab 缺乏像 VS Code 或 Sublime Text 这样强大的文本编辑器。

JupyterLab 允许用户创建和共享包含实时代码、方程式、可视化和叙述文本的文档。这是进行这种互动工作的完美工具。然而，事实是它的文本编辑器原始得像 Windows 记事本一样；你可以编写代码，但体验远非理想。

我们能对此做些什么吗？我们希望能有一个 Docker 镜像，可以在任何机器上随时运行并准备好工作区。用 VS Code 实现这一点并不容易，除非你愿意付费。但 JupyterLab 是免费的；它拥有一个 [充满活力的社区](https://youtu.be/9Q6sLbz37gk)，如果我们能够结合两者的优点，就能打造一个免费的、强大的、便携的工作空间。

让我们退一步，把 JupyterLab 看作一个平台，而不是 IDE。JupyterLab 有一个终端模拟器。这意味着你可以做几乎所有你能想到的事情。基于这个想法，在我们之前的文章中，我们开始安装 Neovim 并将其配置成像 VS Code 一样。

[](/unlocking-the-potential-of-jupyterlab-discover-the-powerful-text-editor-you-never-knew-you-had-af18bf5bce3f?source=post_page-----4d3eb37878f1--------------------------------) ## 释放 JupyterLab 的潜力：发现你从未知道的强大文本编辑器

### 在 JupyterLab 中通过一个优秀的文本编辑器释放你的编码效率和生产力

towardsdatascience.com

我们的目标是创建一个包含强大笔记本编辑器和功能丰富的 Python IDE 的 JupyterLab 镜像。

![](img/bf26f9b2d357744f69b3527f9800cfe0.png)

作者提供的图像

今天，我们从配置编辑器的核心功能和外观开始。

> [学习速递](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=neovim-basic-settings-and-looks) 是一个针对那些对 ML 和 MLOps 世界感兴趣的人的新闻通讯。如果你想了解更多类似的主题，请在[这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=neovim-basic-settings-and-looks)订阅。你将会在每个月的最后一个星期天收到我的更新和对最新 MLOps 新闻和文章的见解！

# 核心配置

我们从设置编辑器的核心设置开始。在你的 `home` 目录下，有一个名为 `.config` 的隐藏文件夹。进入该文件夹并创建一个名为 `nvim` 的新文件夹：

```py
cd ~/.config && mkdir nvim
```

接下来，在 `nvim` 文件夹中创建一个名为 `init.vim` 的新文件：

```py
cd nvim && touch init.vim
```

`init.vim` 是你将用来配置 Neovim 行为和外观的文件。

在上一篇文章中，我建议你通过完成内置的 Vim Tutor 教程来学习你的 Vim 绑定。我假设你已经完成了这个教程，因此我们将使用 Neovim 编辑任何文件。唯一习惯它的方式就是尽可能多地使用它，相信我，一旦你习惯了，你会惊讶于你可以更快地完成任何想做的事情。

那么，废话不多说，让我们打开并编辑你创建的文件：

```py
nvim init.vim
```

我们的第一个设置将启用简单的功能，如行号、文本编码和语法高亮。将以下行复制粘贴到你的 `init.vim` 文件中：

```py
" General setup
set number
set relativenumber
set encoding=utf-8
set mouse=a
set scrolloff=10
set colorcolumn=80
set textwidth=80

" Set syntax highlighting
syntax on
```

每当你看到以引号开头的行时，那是一条注释。你可以在正常模式下使用 `:wq` 保存并退出文件，然后重新打开它以查看更改。

接下来，Vim 和 Neovim 使用`h j k l`键进行导航。我不想移动手指来在文件中导航。因此，我喜欢将它们重新映射到手指已放置的位置：`j k l ;`。为此，请在`init.vim`中添加以下行：

```py
" Remap navigation keys
noremap ; l
noremap l k
noremap k j
noremap j h
```

最后，让我们处理 Python 特定的设置，比如自动缩进和在`79`个字符处设置换行。将以下行复制到`init.vim`中：

```py
" Python specific settings
au BufNewFile,BufRead *.py
    \ set tabstop=4 |
    \ set fileformat=unix |
    \ set softtabstop=4 |
    \ set shiftwidth=4 |
    \ set textwidth=79 |
    \ set expandtab |
    \ set autoindent

au BufRead, BufNewFile *.py,*.pyw,*.c,*.h match BadWhitespace /\s\+$/ " Highlight trailing whitespace
```

现在我们完成了这些基本设置，让我们处理外观问题。毕竟，每个人都希望在一个美丽的环境中工作。

# 让它变得美丽

为了让我们的编辑器达到我们想要的外观，我们将深入探讨插件的世界。Vim 和 Neovim 有一个充满活力的社区，发布和维护成千上万的插件。这些插件改变了编辑器中每个小细节的行为。如果你遇到 Neovim 的问题，很可能其他人已经遇到过并发布了一个让事情变得更简单的插件。

我们的第一步是安装一个插件管理器，它将安装、卸载和更新插件。为此，我喜欢使用[vim-plug](https://github.com/junegunn/vim-plug)。要安装 vim-plug，请运行[docs](https://github.com/junegunn/vim-plug#unix-linux)中指定的适用于你平台的命令。如果你在我们上一篇文章中启动的 Docker 镜像中运行，你会想使用适用于 Linux 的那个。

现在你有了一个插件管理器，安装 Neovim 插件变得非常简单。因此，为了让我们的编辑器更美观，我们将使用四个插件：

+   vim-airline：一个精简而高效的 vim 状态栏/标签行

+   vim-airline-themes：一个 vim-airline 的主题集合

+   awesome-vim-color-schemes：Neo/vim 的精彩配色方案集合

+   vim-devicons：Neovim 的文件类型图标集合

要安装这些插件，首先，在`init.vim`中添加以下行：

```py
call plug#begin()

Plug 'https://github.com/vim-airline/vim-airline' " Show status bar
Plug 'https://github.com/vim-airline/vim-airline-themes.git' " Customize status bar
Plug 'https://github.com/ryanoasis/vim-devicons' " Display developer icons
Plug 'https://github.com/rafi/awesome-vim-colorschemes' " Change color Schemes

call plug#end()
```

保存并退出文件，然后重新打开并运行以下 Neovim 命令：`:PlugInstall`。一个新面板将启动，你的插件将被安装：

![](img/4a0ec09635fa8b482aa3e2d5a4972bed.png)

作者提供的图片

关闭所有窗口，使用命令 `:qa`，然后重新打开文件。更改应该会立即可见。现在，让我们修复一些问题。首先，你会注意到终端没有正确显示某些图标。这是因为你需要安装[Nerd Fonts](https://www.nerdfonts.com/)并在终端中使用它们。我将安装[source-code-pro](https://www.programmingfonts.org/#source-code-pro)。首先，在你的`home`下创建一个`.fonts`目录：

```py
mkdir .fonts && cd .fonts
```

获取 source-code-pro Nerd Font：

```py
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v2.3.3/SourceCodePro.zip
```

解压它：

```py
unzip SourceCodePro.zip && rm SourceCodePro.zip
```

最后，运行`fc-cache -fv`来手动重建字体缓存。然后，进入`settings > advanced settings editor`菜单，找到左侧边栏中的`terminal`条目，将“Font Family”选项更改为“SauceCodePro Nerd Font Mono”。就这样！再用 Neovim 打开`init.vim`文件，图标应该就会出现：

![](img/00cee70fb0caada478a44442d4e26013.png)

作者提供的图片

然而，有些东西仍然显得有些古怪。让我们修复这些问题，并确定我们的核心主题。我喜欢`molokai`，所以我会使用这个。在你的`init.vim`中添加以下行：

```py
" Set default colorscheme
colorscheme molokai

" Customize status bar
let g:airline_theme='molokai'

if !exists('g:airline_symbols')
    let g:airline_symbols = {}
endif

" airline symbols
let g:airline_symbols.branch = ''
let g:airline_symbols.readonly = ''
let g:airline_symbols.maxlinenr = ''
```

再次保存并退出，重新打开文件，一切应该都会正常工作。恭喜你，你现在已经为在 JupyterLab 中将 Neovim 设置为完美的 Python IDE 打下了基础。

# 保存你的工作

如果你正在使用我们在上一篇文章中启动的 Docker 容器（你应该这样做），我会给你一个最后的提示。要将你到目前为止所做的工作保存到你自己的镜像中，请运行以下命令：

```py
docker commit <name-of-the-running-image> <new-image-name>
```

这将创建一个新的层，在当前运行的容器镜像之上，并将所有内容打包成一个你可以推送到 DockerHub 账户的新镜像。

例如，对我来说，这个命令将是：

```py
docker commit jupyter dpoulopoulos/jupyter:v0.0.1
```

这意味着你可以拉取 `dpoulopoulos/jupyter:v0.0.1` 并在其中找到我们在这里做的所有内容！

接下来，我们将深入探讨更多高级功能，比如如何添加文件系统浏览窗格和代码导航窗口。在此之前，练习你的 Vim 快捷键吧！

# 关于作者

我的名字是 [Dimitris Poulopoulos](https://www.dimpo.me/)，我是一名为 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我为包括欧洲委员会、Eurostat、国际货币基金组织、欧洲中央银行、经济合作与发展组织以及宜家在内的主要客户设计和实施了人工智能和软件解决方案。

如果你对阅读更多关于机器学习、深度学习、数据科学和数据运维的文章感兴趣，可以在 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow)、[LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或者 Twitter 上关注我 [@james2pl](https://twitter.com/james2pl)。

所表达的意见仅代表我个人的观点，并不代表我雇主的观点或意见。
