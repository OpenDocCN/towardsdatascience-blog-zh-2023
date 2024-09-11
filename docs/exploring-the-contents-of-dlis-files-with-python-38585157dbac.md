# 使用 Python 探索 DLIS 文件的内容

> 原文：[https://towardsdatascience.com/exploring-the-contents-of-dlis-files-with-python-38585157dbac?source=collection_archive---------13-----------------------#2023-07-14](https://towardsdatascience.com/exploring-the-contents-of-dlis-files-with-python-38585157dbac?source=collection_archive---------13-----------------------#2023-07-14)

## 使用 Pandas 和 dlisio 探索井测数据

[](https://andymcdonaldgeo.medium.com/?source=post_page-----38585157dbac--------------------------------)[![Andy McDonald](../Images/df11d647be032aeb3d31852affb33a64.png)](https://andymcdonaldgeo.medium.com/?source=post_page-----38585157dbac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38585157dbac--------------------------------)[![数据科学的前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----38585157dbac--------------------------------) [Andy McDonald](https://andymcdonaldgeo.medium.com/?source=post_page-----38585157dbac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9c280f85f15c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-the-contents-of-dlis-files-with-python-38585157dbac&user=Andy+McDonald&userId=9c280f85f15c&source=post_page-9c280f85f15c----38585157dbac---------------------post_header-----------) 发表在 [数据科学的前沿](https://towardsdatascience.com/?source=post_page-----38585157dbac--------------------------------) ·8 分钟阅读·2023年7月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38585157dbac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-the-contents-of-dlis-files-with-python-38585157dbac&user=Andy+McDonald&userId=9c280f85f15c&source=-----38585157dbac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38585157dbac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexploring-the-contents-of-dlis-files-with-python-38585157dbac&source=-----38585157dbac---------------------bookmark_footer-----------)![](../Images/36ce0ae3f2ac656818b83f047ec06850.png)

图片由 [Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

[DLIS 文件](http://w3.energistics.org/RP66/V1/Toc/main.html) 是一种标准的石油和天然气行业数据格式。它们是结构化的二进制文件，包含了井信息、工具信息和井测数据的表格。与简单的 LAS（日志 ASCII 标准）文件相比，它们要复杂得多，打开起来也更困难。这使得它们在处理时可能更加困难，通常需要专门的工具来查看和探索其内容。

幸运的是，Equinor 发布了一个名为 [dlisio](https://github.com/equinor/dlisio) 的 Python 库，这使得探索这些文件的过程变得更加容易。

[dlsio](https://github.com/equinor/dlisio) 是由 Equinor ASA 开发的一个 Python 库，用于读取 dlis 文件和 Log Information Standard 79 (LIS79) 文件。开发该库的主要目的是减少探索和提取这些文件中数据的负担和工作量，而无需完全了解它们的结构。这使得用户可以专注于访问和处理数据。

欲了解有关 dlisio 库的更多信息，您可以查看下面的文档
