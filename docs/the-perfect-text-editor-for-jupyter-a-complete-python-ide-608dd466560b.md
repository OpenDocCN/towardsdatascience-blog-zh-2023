# Jupyter 的完美文本编辑器：一个完整的 Python IDE

> 原文：[`towardsdatascience.com/the-perfect-text-editor-for-jupyter-a-complete-python-ide-608dd466560b`](https://towardsdatascience.com/the-perfect-text-editor-for-jupyter-a-complete-python-ide-608dd466560b)

## 从语法高亮到代码补全，一个完整的 Python IDE 在 Jupyter 中

[](https://dpoulopoulos.medium.com/?source=post_page-----608dd466560b--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----608dd466560b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----608dd466560b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----608dd466560b--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----608dd466560b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----608dd466560b--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 9 日

--

![](img/074816bb2f0c1badf878e10ebac705da.png)

图片由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 本文是系列文章的第四部分。查看完整系列：第一部分，第二部分，第三部分。

在过去几天里，我们在 Jupyter 中构建了一个完整的 Python IDE。在本文中，我们将进行最后的调整，并将一切打包成 Docker 镜像，以创建一个便于数据科学家和机器学习工程师使用的便携工作环境。

Jupyter 并不是一个完整的 IDE。它甚至不是许多人认为的 IPython 用户界面。我认为 Jupyter 是一个开发平台，你可以按照自己的喜好创建工作区。由于它包含一个终端模拟器，你可以做你想做的任何事。因此，我们将利用它来创建一个功能丰富的 Python IDE。

因此，在之前的文章中，我们安装了 Neovim，并将其配置为与现在最流行的文本编辑器：Visual Studio Code 相似。在本文中，我们将安装一个全面的工具：代码补全、代码格式化、git 集成、拼写检查和代码对齐。让我们开始吧！

> [学习速率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=neovim-coc) 是一个面向对 ML 和 MLOps 世界感兴趣的人的新闻通讯。如果你想了解更多类似的主题，请[在这里](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=neovim-coc)订阅。每个月的最后一个星期天，你会收到我的更新和对最新 MLOps 新闻和文章的见解！

# Conquer of Completion

许多工具承诺在 Vim/Neovim 的使用上让开发者的生活更轻松，但我个人偏好的是[Conquer of Completion](https://github.com/neoclide/coc.nvim)。

Conquer of Completion (CoC) 是一个流行的 Vim 和 Neovim 插件，提供强大的自动补全引擎。以下是有关 CoC 的一些关键点：

+   CoC 支持多种语言：它可以为包括 Python、JavaScript、C、Go 等在内的各种编程语言提供自动补全建议。它使用语言服务器根据你的代码上下文提供智能推荐。

+   CoC 是高度可配置的：它允许用户以类似 VS Code 的方式自定义自动补全引擎。用户可以设置触发补全的自定义映射，调整补全源的优先级等。

+   CoC 拥有庞大的用户社区：这是一个流行的插件，拥有大量用户社区，这意味着有许多资源可以用来排除故障和进行自定义。该插件也在积极维护和更新。此外，社区提供了几个我们将使用的优秀扩展。

总的来说，Conquer of Completion 是一个强大的插件，可以显著提升你在 Vim 或 Neovim 中的自动补全体验，尤其是当你处理多种编程语言时。

# 安装

CoC 需要一些设置。你需要安装和配置一些额外的组件，包括 NodeJS 和 `npm` 包管理器。根据情况，你可能还需要为你喜欢的编程语言安装一个语言服务器。

在我们的情况下，首先安装 NodeJS。

## NodeJS 安装

NodeJS 和 `npm` 是安装 Neovim 的 CoC 插件并启用多种编程语言自动补全的硬性要求。

要安装这两个软件包，请执行以下命令：

1.  安装 PPA 以访问其软件包：

```py
curl -sL https://deb.nodesource.com/setup_18.x -o nodesource_setup.sh
sudo bash nodesource_setup.sh
```

2\. 安装 Node.js：

```py
sudo apt install nodejs
```

3\. 验证：

```py
node -v
```

如果你看到类似 `v18.15.0`（在撰写本文时的最新版本）的版本号，你就可以开始使用了。即使你安装了其他版本，也应该可以继续使用。

## 安装 CoC

安装 CoC 插件与其他插件一样简单。只需将以下行放入你的 `init.vim` 配置文件中：

```py
Plug 'neoclide/coc.nvim', {'branch': 'release'} " Auto complete Python
```

重启编辑器并运行 `:PlugInstall` Vim 命令。`vim-plug`，我们正在使用的插件管理器，会为你安装 CoC。为完成安装过程，请再次重启编辑器。

> 要详细查看 Neovim 插件的安装过程，请参考本系列的第二篇博客。

我们准备安装所需的扩展，将编辑器转变为一个功能齐全的 IDE。运行 `:CocInstall coc-pyright` Vim 命令以安装 Python 支持。安装完成后，退出编辑器的所有窗口，然后打开一个 Python 文件，如下所示：

```py
nvim test.py
```

自动补全应该启用；然而，它并不是很实用。因此，让我们配置 CoC。在你的 `init.vim` 配置文件中添加以下几行：

```py
" CoC customization

" Applying code actions to the selected code block
vmap <leader>a <Plug>(coc-codeaction-selected)<cr>
xmap <leader>a <Plug>(coc-codeaction-selected)<cr>

" Format action on <leader>
vmap <leader>f  <Plug>(coc-format-selected)
xmap <leader>f  <Plug>(coc-format-selected)

" GoTo code navigation
nmap <silent> gd <Plug>(coc-definition)
nmap <silent> gv :vsp<CR><Plug>(coc-definition)<C-W>L
nmap <silent> gy <Plug>(coc-type-definition)
nmap <silent> gi <Plug>(coc-implementation)
nmap <silent> gr <Plug>(coc-references)

" Symbol renaming
nmap <leader>rn <Plug>(coc-rename)

" Refactor
nmap <silent> <leader>re <Plug>(coc-codeaction-refactor)
xmap <silent> <leader>r  <Plug>(coc-codeaction-refactor-selected)
nmap <silent> <leader>r  <Plug>(coc-codeaction-refactor-selected)

" Confirm selection by pressing Tab
inoremap <expr> <Tab> pumvisible() ? coc#_select_confirm() : "<Tab>"

" Open autocomplete menu by pressing Ctrl + Space
inoremap <silent><expr> <c-space> coc#refresh()

" Open documentation by pressing K
nnoremap <silent> K :call ShowDocumentation()<cr>

function! ShowDocumentation()
  if CocAction('hasProvider', 'hover')
    call CocActionAsync('doHover')
  else
    call feedkeys('K', 'in')
  endif
endfunction
```

我在每一行上留下了评论，以便你理解它的作用。现在 CoC 已配置好，你可以使用这些快捷键来充分利用它。

然而，事情并没有就此结束。正如我们所说，CoC 有多个扩展（如 `coc-pyright`）可以安装。它们包括：

+   `coc-git`

+   `coc-docker`

+   `coc-yaml`

+   `coc-json`

+   `coc-prettier`

+   `coc-pairs`

+   `coc-spell-checker`

要安装它们中的任何一个，只需运行 `:CocInstall` 命令并提供扩展的名称。有关扩展的完整列表，请访问 `coc-extensions` 页面：[`github.com/topics/coc-extensions`](https://github.com/topics/coc-extensions)

最后，你可以像在 VS Code 中一样配置 CoC。运行 `:CocConfig` 命令，`coc-settings.json` 文件将弹出。我在其中设置了以下配置：

```py
{
  "python.venvPath": "/home/dimpo/.pyenv/versions/",                            
  "python.linting.flake8Enabled": true,                                         
  "python.formatting.provider": "autopep8",                                     
  "pyright.inlayHints.variableTypes": false,                                    
  "pyright.inlayHints.functionReturnTypes": false,                              
  "git.addGBlameToVirtualText": true,                                           
} 
```

每一行都是自解释的，但如果你想详细了解可以配置的内容，请参考每个 CoC 扩展的文档。

像往常一样，使用 `docker commit` 命令保存你的工作（如果你不知道这意味着什么，请参考本系列以前的文章）。你可以在 [DockerHub](https://hub.docker.com/layers/dpoulopoulos/jupyter/v0.0.3/images/sha256-3538e1a72d605071a6517bf157dabfe8a3953f7236d10abfa18dbc5a38f7951b?context=repo) 上找到我的镜像。

# 总结

在这一系列文章中，我们在 JupyterLab 环境中构建了一个完整的 Python IDE。我们从一个简单的 Neovim 安装开始，并将其配置为像今天最受欢迎的文本编辑器 VS Code 一样。

这篇文章结束了这个故事，但如果你想看到更多内容，如如何使用调试器或进一步配置特定插件，请留言，我会尽力而为！在此之前，祝编程愉快。

# 关于作者

我的名字是 [Dimitris Poulopoulos](https://www.dimpo.me/?utm_source=medium&utm_medium=article&utm_campaign=neovim-coc)，我是一名在 [Arrikto](https://www.arrikto.com/) 工作的机器学习工程师。我为包括欧盟委员会、欧洲统计局、国际货币基金组织、欧洲中央银行、经济合作与发展组织和宜家在内的主要客户设计和实施了 AI 和软件解决方案。

如果你对机器学习、深度学习、数据科学和数据操作等更多帖子感兴趣，请在 [Medium](https://towardsdatascience.com/medium.com/@dpoulopoulos/follow)、[LinkedIn](https://www.linkedin.com/in/dpoulopoulos/) 或 [Twitter](https://twitter.com/james2pl) 上关注我。

所表达的观点仅代表我个人，并不代表我的雇主的观点或意见。
