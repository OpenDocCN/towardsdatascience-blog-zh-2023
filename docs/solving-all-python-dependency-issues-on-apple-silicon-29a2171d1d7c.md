# 解决所有 Apple Silicon 上的 Python 依赖问题

> 原文：[`towardsdatascience.com/solving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c`](https://towardsdatascience.com/solving-all-python-dependency-issues-on-apple-silicon-29a2171d1d7c)

## 使用 pyenv 在 Mac M1 上管理多个 Python 架构的指南

[](https://david-farrugia.medium.com/?source=post_page-----29a2171d1d7c--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----29a2171d1d7c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----29a2171d1d7c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----29a2171d1d7c--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----29a2171d1d7c--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----29a2171d1d7c--------------------------------) ·4 min read·2023 年 2 月 21 日

--

![](img/35b90b1468e13b350e7dfa0cc53604f6.png)

图片来源：[Jonathan Francisca](https://unsplash.com/@jonathan_francisca?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我爱 MacOS。

也许是它的认证 Unix 特性，简约和优雅，或者是它的无缝集成。但我爱 MacOS。

根据我的经验，一切都会顺利进行。

在我职业生涯的开始阶段，我主要使用 Windows（同时玩弄一些 Linux 发行版）。一旦我切换到苹果，尽管这可能有些陈词滥调，但我从未回头。

那种跟随逐步教程却面临奇怪错误的日子早已一去不复返。

那种为了让某些 Python 包在 Windows 系统上特定工作而需要找寻模板代码的日子已经一去不复返。

而且这种情况经常发生。作为一名数据科学家，我不断地尝试和实验新的 Python 包。

自从 2019 年发布以来，我一直在使用我的 2019 年款 Macbook Pro 处理数字。但最近，我有机会在 2020 年款 Macbook Pro M1 上工作了一段时间。

我很兴奋。我听说 M1 芯片在性能上比 Intel 芯片强大得多。

首先，必须提到，这颗芯片真是太棒了！M1 芯片非常快，能效高，远远领先于竞争对手。从我的旧系统到 M1 的性能差异立刻显而易见。

然而，当我开始为数据科学和机器学习设置我的机器时，我很快意识到我最喜欢的 Mac 功能已经迅速消失。我无法完成最基本的操作——通过 pip 安装 Python 包。M1 芯片不断给我抛出各种依赖错误和奇怪的冲突。

这主要是由于从 x86 转向 ARM64 的切换。尽管现在的情况已经大大改善，许多开发者更新他们的软件包以支持 ARM64 系统，但仍有大量 Python 软件包尚未更新。

# 进入 pyenv

[](https://github.com/pyenv/pyenv?source=post_page-----29a2171d1d7c--------------------------------) [## GitHub - pyenv/pyenv: 简单的 Python 版本管理]

### pyenv 允许你轻松切换多个 Python 版本。它简单、不干扰，并遵循 UNIX…

github.com](https://github.com/pyenv/pyenv?source=post_page-----29a2171d1d7c--------------------------------)

在我的研究中，我遇到了 Pyenv——一个 Python 版本管理工具。

类似于我们使用 `conda` 或 `venv` 管理不同的软件包和环境，Pyenv 允许我们在同一台机器上拥有多个 Python 版本。除了实际的版本（例如，Python 3.8.1 和 Python 3.9.7），通过 Pyenv 我们还可以决定是否需要 x86 版本的 Python。

因此，在一台机器上，我们可以同时拥有 Python 3.9.7 的 x86 版本和 ARM64 版本。我们可以像处理系统本身的版本一样切换不同的 Python 版本。

我们仍然可以与 `conda` 或 `venv` 一起使用它们。

那么，我们该如何开始使用 Pyenv 呢？

**第 1 步：在系统上安装 xcode**

```py
xcode-select --install
```

**第 2 步：安装 Homebrew**

确保你已经安装了 `brew`。你可以在终端运行 `brew help` 来检查是否已安装。

如果你需要安装 brew，可以按照他们页面上的说明进行操作。

[](https://brew.sh/?source=post_page-----29a2171d1d7c--------------------------------) [## Homebrew]

### macOS（或 Linux）的缺失包管理器。

brew.sh](https://brew.sh/?source=post_page-----29a2171d1d7c--------------------------------)

**第 3 步：安装 Pyenv**

我们可以通过 Homebrew 安装 Pyenv，方法如下：

```py
brew install pyenv
```

**第 4 步：更新 Shell 配置文件**

我们需要更新我们的 shell 配置文件 PATH（例如，~/.bash_profile、~/.zshrc 等）以包含 Pyenv。

同时，为了使 Pyenv 默认加载，我们需要初始化它。

```py
##### ~/.zprofile #####
eval "$(pyenv init --path)"

##### ~/.zshrc #####
if command -v pyenv 1>/dev/null 2>&1; then
    eval "$(pyenv init -)"
fi
```

**第 5 步：安装 Python 版本**

要列出所有可用的版本，我们可以使用：

```py
pyenv install --list
```

要安装特定的 Python 版本，我们可以使用：

```py
pyenv install 3.9.7
```

安装后，我们还可以将新版本设置为全局 Python 版本。

```py
pyenv global 3.9.7
```

**第 6 步：安装软件包**

一切设置好后，我们现在可以像使用本机 Python 版本一样开始使用我们的新 Python 版本。我们可以创建不同的虚拟环境，并通过 `pip` 或 `setup.py` 安装不同的软件包，就像我们通常做的那样。

# 但是，x86 Python 环境怎么办？

我们可以使用 Rosetta 和 Pyenv 在 Apple Silicon 上设置 x86 版本的 Python。

Rosetta 是由 Apple 构建的一个工具，用于将 x86 架构转换为 ARM64。

我们可以使用以下命令安装 Rosetta：

```py
softwareupdate --install-rosetta
```

安装完成后，我们可以设置终端使用 Rosetta 启动，这样终端将转换为 x86 模式。

为此，右键点击你的终端应用程序，点击 `获取信息` 并勾选 `使用 Rosetta 打开`。

> **提示：** 复制终端应用程序，这样你将拥有一个使用 Rosetta（即 x86）启动的终端，以及一个用于 ARM64 的终端。

你现在可以按照上述 x86 终端上的相同步骤来访问 x86 Python 环境。

# 结束语

在这篇文章中，我们将深入探讨一个简单但有效的解决方案，用于在 Apple Silicon 上使用 Pyenv 处理 x86 Python 版本。除了这个好处外，Pyenv 还提供了许多其他优势，包括在一台机器上管理多个 Python 版本。

**你喜欢这篇文章吗？每月 $5，你可以成为会员，解锁对 Medium 的无限访问权限。这将直接支持我以及你在 Medium 上其他喜爱的作者。非常感谢！**

[## 使用我的推荐链接加入 Medium - David Farrugia](https://david-farrugia.medium.com/membership?source=post_page-----29a2171d1d7c--------------------------------)

### 阅读 David Farrugia 的每个故事（以及 Medium 上其他成千上万的作家）。你的会员费将直接支持…

[订阅](https://david-farrugia.medium.com/membership?source=post_page-----29a2171d1d7c--------------------------------)

**也许你还可以考虑订阅我的邮件列表，以便在我发布新内容时收到通知。这个服务是免费的 :)**

[## 每当 David Farrugia 发布新内容时，收到一封电子邮件。](https://david-farrugia.medium.com/subscribe?source=post_page-----29a2171d1d7c--------------------------------)

### 每当 David Farrugia 发布新内容时，会收到一封电子邮件。通过注册，如果你还没有 Medium 账户，你将创建一个账户…

[订阅](https://david-farrugia.medium.com/subscribe?source=post_page-----29a2171d1d7c--------------------------------)  

# 想要联系我吗？

我很想听听你对这个话题的看法，或者对 AI 和数据的任何想法。

如果你希望联系我，请发电子邮件到 ***davidfarrugia53@gmail.com***。

[Linkedin](https://www.linkedin.com/in/david-farrugia/)
