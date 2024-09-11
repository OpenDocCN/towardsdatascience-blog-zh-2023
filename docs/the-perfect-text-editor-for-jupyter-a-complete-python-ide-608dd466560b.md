# Jupyter 的完美文本编辑器：一个完整的 Python IDE

> 原文：[`towardsdatascience.com/the-perfect-text-editor-for-jupyter-a-complete-python-ide-608dd466560b?source=collection_archive---------4-----------------------#2023-03-09`](https://towardsdatascience.com/the-perfect-text-editor-for-jupyter-a-complete-python-ide-608dd466560b?source=collection_archive---------4-----------------------#2023-03-09)

## 从语法高亮到代码补全，一个完整的 Python IDE 内置于 Jupyter

[](https://dpoulopoulos.medium.com/?source=post_page-----608dd466560b--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----608dd466560b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----608dd466560b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----608dd466560b--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----608dd466560b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-perfect-text-editor-for-jupyter-a-complete-python-ide-608dd466560b&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----608dd466560b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----608dd466560b--------------------------------) ·5 min 阅读·2023 年 3 月 9 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F608dd466560b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-perfect-text-editor-for-jupyter-a-complete-python-ide-608dd466560b&source=-----608dd466560b---------------------bookmark_footer-----------)![](img/074816bb2f0c1badf878e10ebac705da.png)

图片由 [Max Duzij](https://unsplash.com/@max_duz?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 本文是系列文章的第四部分。查看完整系列：第一部分，第二部分，第三部分。

在过去的几天里，我们一直在 Jupyter 内部构建一个完整的 Python IDE。在本文中，我们将添加最后的润色，并将所有内容打包成一个 Docker 镜像，以创建一个便于数据科学家和机器学习工程师使用的可移植工作环境。

Jupyter 并不完全是一个 IDE。它甚至不是一个 IPython UI，尽管许多人可能认为如此。我认为 Jupyter 是一个开发平台，你可以按照自己的方式创建工作空间。由于它包括一个终端仿真器，你可以做任何你能想到的。因此，我们将利用它创建一个完整、功能丰富的 Python IDE。

因此，在之前的文章中，我们安装了 Neovim 并将其配置为与当前最流行的文本编辑器：Visual Studio Code，类似的外观和功能。在本文中，我们将安装一种万能工具：代码补全、代码格式化、Git 集成、拼写检查和代码对齐。让我们开始吧！

> [学习速率](https://www.dimpo.me/newsletter?utm_source=medium&utm_medium=article&utm_campaign=neovim-coc)是一个针对那些对 ML 和 MLOps 世界感兴趣的人的新闻通讯。如果你想了解更多关于……
