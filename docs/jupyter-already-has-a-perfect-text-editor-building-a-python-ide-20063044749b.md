# Jupyter 已经拥有一个完美的文本编辑器：构建一个 Python IDE

> 原文：[`towardsdatascience.com/jupyter-already-has-a-perfect-text-editor-building-a-python-ide-20063044749b?source=collection_archive---------12-----------------------#2023-03-08`](https://towardsdatascience.com/jupyter-already-has-a-perfect-text-editor-building-a-python-ide-20063044749b?source=collection_archive---------12-----------------------#2023-03-08)

## 让 Jupyter 成为你的一体化 IDE，提升你的 Python 开发体验

[](https://dpoulopoulos.medium.com/?source=post_page-----20063044749b--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----20063044749b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20063044749b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----20063044749b--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----20063044749b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjupyter-already-has-a-perfect-text-editor-building-a-python-ide-20063044749b&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----20063044749b---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20063044749b--------------------------------) ·6 分钟阅读·2023 年 3 月 8 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F20063044749b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjupyter-already-has-a-perfect-text-editor-building-a-python-ide-20063044749b&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----20063044749b---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20063044749b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjupyter-already-has-a-perfect-text-editor-building-a-python-ide-20063044749b&source=-----20063044749b---------------------bookmark_footer-----------)![](img/d9c19f9805a3a34318db168c864ad8f1.png)

图片来源：[Fotis Fotopoulos](https://unsplash.com/@ffstop?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> *这篇文章是系列的第三部分。查看完整系列：* *第一部分**，* *第二部分*，*第四部分**。*

在系列的前几部分中，我们讨论了为什么许多开发者不将 Jupyter 视为一个完全的集成开发环境，以及缺乏强大文本编辑器是主要原因之一。

因此，我们决定将 JupyterLab 视为一个平台，用于从零开始创建我们自己的 Python IDE。将相同的想法扩展到其他编程语言只需进行一次 Google 搜索即可。

为此，我们使用了 JupyterLab 终端仿真器来安装 Neovim，并使其表现和外观类似于 VS Code。这样，我们可以构建一个 docker 镜像来打包我们整个工作空间，根据我们的喜好进行自定义。这个想法超越了 JupyterLab 和 Neovim。我们可以拥有一个预配置工具的工作环境，比如用于版本控制的 Git，用于创建虚拟环境的 pyenv，以及用于终端仿真的 tmux。

在系列的前两部分中，我们安装了 Neovim，学习了 Vim 绑定，并配置了编辑器的外观和基本设置。今天，我们将安装几个插件来使……
