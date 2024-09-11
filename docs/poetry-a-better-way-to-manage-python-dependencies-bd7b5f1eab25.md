# Python 依赖管理：你应该选择哪个工具？

> 原文：[https://towardsdatascience.com/poetry-a-better-way-to-manage-python-dependencies-bd7b5f1eab25?source=collection_archive---------7-----------------------#2023-06-13](https://towardsdatascience.com/poetry-a-better-way-to-manage-python-dependencies-bd7b5f1eab25?source=collection_archive---------7-----------------------#2023-06-13)

## 对 Poetry、Pip 和 Conda 的深入比较

[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----bd7b5f1eab25--------------------------------) [![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----bd7b5f1eab25--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----bd7b5f1eab25--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpoetry-a-better-way-to-manage-python-dependencies-bd7b5f1eab25&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----bd7b5f1eab25---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bd7b5f1eab25--------------------------------) · 10分钟阅读 · 2023年6月13日

--

![](../Images/a87494b4a6efa276955127728c116a55.png)

作者提供的图片

*最初发布于* [*https://mathdatasimplified.com*](https://mathdatasimplified.com/2023/06/12/poetry-a-better-way-to-manage-python-dependencies/) *于2023年6月13日。*

# 动机

随着数据科学项目的扩展，依赖项的数量也会增加。为了保持项目环境的可重现性和可维护性，使用高效的依赖管理工具非常重要。

因此，我决定比较三种流行的依赖管理工具：Pip、Conda 和 Poetry。经过仔细评估，我确信 Poetry 在效果和性能方面优于其他两个选项。

在这篇文章中，我们将*深入探讨* Poetry 的优势，并突出其与 Pip 和 Conda 的*主要区别*。

# 可用的软件包

拥有广泛的软件包选择可以让开发人员更容易找到最适合其需求的特定软件包和版本。

## Conda

一些软件包，例如“snscrape”，无法通过 conda 安装。此外，某些版本，如 Pandas 2.0，可能无法通过 Conda 安装。
