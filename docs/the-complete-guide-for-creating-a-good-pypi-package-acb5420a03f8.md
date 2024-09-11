# 最完整的 PyPI 包创建指南

> 原文：[`towardsdatascience.com/the-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8?source=collection_archive---------15-----------------------#2023-03-27`](https://towardsdatascience.com/the-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8?source=collection_archive---------15-----------------------#2023-03-27)

## 您需要了解的一切——无论是新手还是有经验的用户

[](https://eliselandman.medium.com/?source=post_page-----acb5420a03f8--------------------------------)![Elise Landman](https://eliselandman.medium.com/?source=post_page-----acb5420a03f8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----acb5420a03f8--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----acb5420a03f8--------------------------------) [Elise Landman](https://eliselandman.medium.com/?source=post_page-----acb5420a03f8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdbd14e538474&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8&user=Elise+Landman&userId=dbd14e538474&source=post_page-dbd14e538474----acb5420a03f8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----acb5420a03f8--------------------------------) · 11 分钟阅读 · 2023 年 3 月 27 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Facb5420a03f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8&user=Elise+Landman&userId=dbd14e538474&source=-----acb5420a03f8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Facb5420a03f8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complete-guide-for-creating-a-good-pypi-package-acb5420a03f8&source=-----acb5420a03f8---------------------bookmark_footer-----------)![](img/726e161623b0a5e82e1c2c669eee378e.png)

图片来自 [Unsplash](https://unsplash.com/photos/DYHx6h3lMdY)。

**Python 包索引**（或 [PyP](https://pypi.org/)I）是第三方 **Python 包** 的官方 **软件仓库**。这是 Python 程序员获取任何预构建和可重用的软件组件的一站式服务——使我们的编码生活更加轻松高效。只需简单的 `pip install`，任何 PyPI 包都可以轻松下载并安装到我们的机器上。

在对 [我自己的 PyPI 包](https://github.com/elisemercury/Duplicate-Image-Finder) 进行 2 年的持续开发和更新之后，我学到了许多从一开始就希望知道的东西。在本指南中，我将**分享我所有的学习和技巧**，这些技巧可以**让你的 PyPI 包上传**和**维护**变得**更加成功**。

本指南分为 2 部分：

👉 如果你是*首次接触 PyPI*，这是你第一个包，从这里开始：**1 | 为 PyPI 准备你的包**

👉 如果你*已经是 PyPI 用户*并且已经上传了你的包，从这里开始：**2 | 上传和维护你的 PyPI 包**

# 1 | 为 PyPI 准备你的包

当你将你的包上传到 [PyPI](https://pypi.org/) 时，你使得任何人都可以通过 `pip` 安装程序来安装它。然后任何人都可以运行…
