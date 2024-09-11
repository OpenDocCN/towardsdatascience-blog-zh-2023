# Python 到 Rust：关于虚拟环境你必须知道的一切

> 原文：[https://towardsdatascience.com/python-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835?source=collection_archive---------0-----------------------#2023-12-26](https://towardsdatascience.com/python-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835?source=collection_archive---------0-----------------------#2023-12-26)

## Python 专家到 Rust 新手——数据科学家的过渡故事

[](https://dennisbakhuis.medium.com/?source=post_page-----c1cd0e529835--------------------------------)[![Dennis Bakhuis](../Images/4dc6dca031cdedbb044a1d0a6b142186.png)](https://dennisbakhuis.medium.com/?source=post_page-----c1cd0e529835--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1cd0e529835--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c1cd0e529835--------------------------------) [Dennis Bakhuis](https://dennisbakhuis.medium.com/?source=post_page-----c1cd0e529835--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b8617eb89bb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835&user=Dennis+Bakhuis&userId=5b8617eb89bb&source=post_page-5b8617eb89bb----c1cd0e529835---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1cd0e529835--------------------------------) · 7 分钟阅读 · 2023年12月26日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc1cd0e529835&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835&user=Dennis+Bakhuis&userId=5b8617eb89bb&source=-----c1cd0e529835---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc1cd0e529835&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpython-to-rust-everything-you-must-know-about-virtual-environments-c1cd0e529835&source=-----c1cd0e529835---------------------bookmark_footer-----------)![](../Images/114983101b49df51f54b21054860ffc1.png)

图1：货舱中的蛇和螃蟹。 ([螃蟹](https://unsplash.com/photos/a-group-of-red-mushrooms-ZWrsjySNfxY); [蛇](https://unsplash.com/photos/close-up-photo-of-brown-and-gray-snake-VUr5nmC1IM4); [集装箱](https://unsplash.com/photos/assorted-color-filed-intermodal-containers-tjX_sniNzgQ); 作者编排)

从 Python 转向 Rust 的旅程就像是用一种新型的刀刃替代了信赖的光剑——既令人兴奋又有些令人畏惧。作为一个对 Python 特性非常熟悉的数据科学家，深入 Rust 世界是一个激动人心的新挑战。在这篇文章中，我将分享我的经验和见解，比较这两种强大的语言如何处理软件开发的一个关键方面——特别是 (虚拟) 环境和依赖管理。

当使用 Python 时，你首先学到的事情之一就是在所谓的 *虚拟环境* 中工作。这是管理依赖关系和隔离项目特定包的关键工具，以便它们不会干扰其他项目或系统范围的 Python 安装。我几年前写了一篇关于我如何 [管理 Python](/environments-conda-pip-aaaaah-d2503877884c) 的文章，但它仍然适用（随着 [*micromamba*](https://mamba.readthedocs.io/en/latest/user_guide/micromamba.html) 和 [*poetry*](https://python-poetry.org/) 的变化有所调整，如果你希望我写一篇关于这些工具的文章，请告诉我）。

> TLDR: 只需使用 *cargo*，大多数情况下你就会没问题 —— Dennis

安装完 Rust 并使用 [*rustup*](https://rustup.rs/) 后，我的第一个问题是：*我应该如何创建虚拟环境？* 对我来说，这是一个非常合理的问题，因为 Rust 也可以使用……
