# Python 到 Rust：突破三大障碍

> 原文：[https://towardsdatascience.com/python-to-rust-breaking-down-3-big-obstacles-094eb99e331d?source=collection_archive---------1-----------------------#2023-12-12](https://towardsdatascience.com/python-to-rust-breaking-down-3-big-obstacles-094eb99e331d?source=collection_archive---------1-----------------------#2023-12-12)

## Python 专家到 Rust 新手 —— 一个数据科学家的转变故事

[](https://dennisbakhuis.medium.com/?source=post_page-----094eb99e331d--------------------------------)[![丹尼斯·巴赫伊斯](../Images/4dc6dca031cdedbb044a1d0a6b142186.png)](https://dennisbakhuis.medium.com/?source=post_page-----094eb99e331d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----094eb99e331d--------------------------------)[![向数据科学进发](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----094eb99e331d--------------------------------) [丹尼斯·巴赫伊斯](https://dennisbakhuis.medium.com/?source=post_page-----094eb99e331d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b8617eb89bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-to-rust-breaking-down-3-big-obstacles-094eb99e331d&user=Dennis+Bakhuis&userId=5b8617eb89bb&source=post_page-5b8617eb89bb----094eb99e331d---------------------post_header-----------) 发表在 [向数据科学进发](https://towardsdatascience.com/?source=post_page-----094eb99e331d--------------------------------) ·8 分钟阅读·2023 年 12 月 12 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F094eb99e331d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-to-rust-breaking-down-3-big-obstacles-094eb99e331d&user=Dennis+Bakhuis&userId=5b8617eb89bb&source=-----094eb99e331d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F094eb99e331d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-to-rust-breaking-down-3-big-obstacles-094eb99e331d&source=-----094eb99e331d---------------------bookmark_footer-----------)![](../Images/2c60e597f6041cb1288d3e3032c05b6f.png)

图 1：蛇和蟹。（蟹：[Romina BM](https://unsplash.com/photos/a-group-of-red-mushrooms-ZWrsjySNfxY); 蛇：[Mohan Moolepetlu](https://unsplash.com/photos/close-up-photo-of-brown-and-gray-snake-VUr5nmC1IM4); 作者组合）。

我周围的每个人都知道我非常喜欢🐍 Python。我大约15年前开始使用Python，那时我对[Mathworks Matlab](https://www.mathworks.com/products/matlab.html)感到厌倦。尽管Matlab的概念看起来不错，但在[精通Python](https://medium.com/towards-data-science/master-python-in-10-minutes-a-day-ac32996b5ded)之后，我从未回头。我甚至在我的大学里成为了Python的布道者，并“传播”这一理念。

> 编写代码的能力并不意味着你是一个软件开发人员。

在我目前的雇主[特能特](https://www.tennet.eu/)，一个在荷兰和德国的大型[输电系统运营商](https://en.wikipedia.org/wiki/Transmission_system_operator)，我们正与大约10人的团队一起构建一个*文档解析和验证解决方案*。构建这样的解决方案，尤其是在团队中，远比我想象的要困难得多。这也让我对软件工程的正确范式产生了更多兴趣。我一直认为我的代码还不错，但在看到软件工程师朋友们的工作后：哇，还有这么多要学习的东西！

当我学习了像[强类型](https://en.wikipedia.org/wiki/Strong_and_weak_typing)、[SOLID原则](https://en.wikipedia.org/wiki/SOLID)和通用编程架构等话题时，我也浏览了其他语言以及*它们*如何解决这些问题。尤其是[Rust](https://www.rust-lang.org)引起了我的注意，因为我经常看到基于Rust的Python包（例如[Polars](https://pola.rs/)）。
