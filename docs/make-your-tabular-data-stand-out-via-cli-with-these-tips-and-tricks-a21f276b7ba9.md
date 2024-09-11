# 通过命令行让你的表格数据更突出，提供这些技巧和窍门

> 原文：[https://towardsdatascience.com/make-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9?source=collection_archive---------11-----------------------#2023-04-12](https://towardsdatascience.com/make-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9?source=collection_archive---------11-----------------------#2023-04-12)

## 增强可读性：在命令行中显示数据集的技巧

[](https://federicotrotta.medium.com/?source=post_page-----a21f276b7ba9--------------------------------)[![Federico Trotta](../Images/e997e3a96940c16ab5071629016d82fd.png)](https://federicotrotta.medium.com/?source=post_page-----a21f276b7ba9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a21f276b7ba9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a21f276b7ba9--------------------------------) [Federico Trotta](https://federicotrotta.medium.com/?source=post_page-----a21f276b7ba9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F654cd4bbe899&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9&user=Federico+Trotta&userId=654cd4bbe899&source=post_page-654cd4bbe899----a21f276b7ba9---------------------post_header-----------) 发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a21f276b7ba9--------------------------------) ·6分钟阅读·2023年4月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa21f276b7ba9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9&user=Federico+Trotta&userId=654cd4bbe899&source=-----a21f276b7ba9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa21f276b7ba9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmake-your-tabular-data-stand-out-via-cli-with-these-tips-and-tricks-a21f276b7ba9&source=-----a21f276b7ba9---------------------bookmark_footer-----------)![](../Images/cffe7e7f8f14546db47248ae34a16223.png)

图片由[Dorothe](https://pixabay.com/it/users/darkmoon_art-1664300/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3224768)提供于[Pixabay](https://pixabay.com/it//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=3224768)

几天前，我想帮助我父亲解决一个问题。他的需求是尽快汇总、过滤和显示一些数据。嗯……事实上，他是打印数据（每次大约10页！！）然后手动查找数据！我看到了他的困难，决定立即帮助他。

对于像我这样能分析数据的人来说没有什么困难：数据已经是Excel格式，因此Jupyter Notebook和Pandas是完美的选择。

问题是我并不为我的父亲工作。而且，我们住在不同的城市，每隔几周才见一次面。因此，我需要给他一个可以使用的工具，具备以下特点：

+   简单的使用方法。

+   如果他写错了东西，不会导致电脑崩溃。

这就是我想到创建一个可以通过CLI管理的小程序的原因。我想创建的很简单：用户通过命令行输入内容。然后，程序展示所有与之相关的数据，就像Pandas在终端中做的那样。

所以，在这篇文章中，我将向你展示如何通过CLI展示表格数据……
