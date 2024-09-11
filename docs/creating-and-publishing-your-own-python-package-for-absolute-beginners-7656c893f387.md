# 为绝对初学者创建和发布自己的 Python 包

> 原文：[https://towardsdatascience.com/creating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387?source=collection_archive---------6-----------------------#2023-09-23](https://towardsdatascience.com/creating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387?source=collection_archive---------6-----------------------#2023-09-23)

## 在5分钟内创建、构建和发布一个 Python 包

[](https://mikehuls.medium.com/?source=post_page-----7656c893f387--------------------------------)[![Mike Huls](../Images/8f9f55a0d25db00799c5d37383b7f5b6.png)](https://mikehuls.medium.com/?source=post_page-----7656c893f387--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7656c893f387--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7656c893f387--------------------------------) [Mike Huls](https://mikehuls.medium.com/?source=post_page-----7656c893f387--------------------------------)

·

[跟随](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ffb62c607ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387&user=Mike+Huls&userId=7ffb62c607ee&source=post_page-7ffb62c607ee----7656c893f387---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7656c893f387--------------------------------) ·6分钟阅读·2023年9月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7656c893f387&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387&user=Mike+Huls&userId=7ffb62c607ee&source=-----7656c893f387---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7656c893f387&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-and-publishing-your-own-python-package-for-absolute-beginners-7656c893f387&source=-----7656c893f387---------------------bookmark_footer-----------)![](../Images/376b56645a3b29e6dba99d79a2a5fd01.png)

（图片由 [Erda Estremera](https://unsplash.com/@erdaest) 提供，来源于 [Unsplash](https://unsplash.com/photos/sxNt9g77PE0)）

Python 包是可重复使用的代码集合，可以轻松地在项目之间共享和实施。我们可以一次编写代码，并在许多地方多次使用。包允许我们与同事甚至全球开发者社区共享代码。作为数据科学家，你可以分享包而不是 Jupyter 笔记本，以确保轻松更新、重用和版本控制。

在本文中，我们将详细介绍创建、构建和发布你自己的包到 Python 包索引 (PyPI；你从中`pip install`) 的现代方法。我们将创建一个名为“**mikes-toolbox2**”的[真实包](https://pypi.org/project/mikes-toolbox2/)并将其部署到 PyPI，以便我们可以使用`pip install mikes-toolbox2`来安装它。开始编程吧！

## 在我们开始之前……

本文详细介绍了如何将包发布到*公共* Python 包索引。这意味着，一旦发布，你的包将对任何人开放。我正在撰写一篇关于如何设置自己的私有 PyPI 的文章，如果你感兴趣，请务必[**关注我**](http://mikehuls.medium.com/membership)。

如果你在这篇文章中的代码示例中迷失了方向：[**在这里查看源代码**](https://github.com/mike-huls/mikes-toolbox2)。

## 1\. 设置一个 Python 环境……
