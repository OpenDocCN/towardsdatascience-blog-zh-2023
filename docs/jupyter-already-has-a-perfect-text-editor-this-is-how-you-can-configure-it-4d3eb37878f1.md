# Jupyter 已经拥有一款完美的文本编辑器：这就是如何配置它

> 原文：[`towardsdatascience.com/jupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1?source=collection_archive---------1-----------------------#2023-03-03`](https://towardsdatascience.com/jupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1?source=collection_archive---------1-----------------------#2023-03-03)

## 如何在 Jupyter 中获得类似 VS Code 的体验：一款优秀的文本编辑器

[](https://dpoulopoulos.medium.com/?source=post_page-----4d3eb37878f1--------------------------------)![Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----4d3eb37878f1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d3eb37878f1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d3eb37878f1--------------------------------) [Dimitris Poulopoulos](https://dpoulopoulos.medium.com/?source=post_page-----4d3eb37878f1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7cc87df5b1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=post_page-7cc87df5b1----4d3eb37878f1---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4d3eb37878f1--------------------------------) ·7 分钟阅读·2023 年 3 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4d3eb37878f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1&user=Dimitris+Poulopoulos&userId=7cc87df5b1&source=-----4d3eb37878f1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4d3eb37878f1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fjupyter-already-has-a-perfect-text-editor-this-is-how-you-can-configure-it-4d3eb37878f1&source=-----4d3eb37878f1---------------------bookmark_footer-----------)![](img/c40ed0aa20061ca652dd94fb8205a3a4.png)

图片由 [Douglas Lopes](https://unsplash.com/@douglasamarelo?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本文是系列文章的第二部分。查看完整系列：第一部分，第三部分，第四部分。

我们之前的文章提到，许多工程师并不认为 JupyterLab 是一个完整的 IDE。主要原因之一是 JupyterLab 没有像 VS Code 或 Sublime Text 那样强大的文本编辑器。

JupyterLab 允许用户创建和分享包含实时代码、方程式、可视化和叙述文本的文档。这是进行这种互动工作的完美工具。然而，事实是它的文本编辑器像 Windows 记事本一样原始；你可以写代码，但体验远非理想。

我们能对此做些什么吗？我们希望有一个 Docker 镜像，可以在任何机器上随时运行，随时准备好工作空间。使用 VS Code 实现这一点并不容易，除非你愿意付费。但 JupyterLab 是免费的；它有一个[充满活力的社区](https://youtu.be/9Q6sLbz37gk)，如果我们能将两者的优点结合起来，我们将拥有一个免费的、强大的、便携的工作空间。

让我们退一步，从平台的角度来看待 JupyterLab，而不是 IDE。JupyterLab 有一个终端模拟器。这意味着……
