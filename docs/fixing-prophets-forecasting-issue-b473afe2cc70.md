# 修复 Prophet 的预测问题

> 原文：[https://towardsdatascience.com/fixing-prophets-forecasting-issue-b473afe2cc70?source=collection_archive---------5-----------------------#2023-01-24](https://towardsdatascience.com/fixing-prophets-forecasting-issue-b473afe2cc70?source=collection_archive---------5-----------------------#2023-01-24)

## 步骤 1：限制不合理的趋势

[](https://medium.com/@tylerblume?source=post_page-----b473afe2cc70--------------------------------)[![Tyler Blume](../Images/65885dd1d8c07a786764c6f898bafb35.png)](https://medium.com/@tylerblume?source=post_page-----b473afe2cc70--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b473afe2cc70--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b473afe2cc70--------------------------------) [Tyler Blume](https://medium.com/@tylerblume?source=post_page-----b473afe2cc70--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffd464a2f5769&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffixing-prophets-forecasting-issue-b473afe2cc70&user=Tyler+Blume&userId=fd464a2f5769&source=post_page-fd464a2f5769----b473afe2cc70---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b473afe2cc70--------------------------------) ·10 分钟阅读·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb473afe2cc70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffixing-prophets-forecasting-issue-b473afe2cc70&user=Tyler+Blume&userId=fd464a2f5769&source=-----b473afe2cc70---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb473afe2cc70&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffixing-prophets-forecasting-issue-b473afe2cc70&source=-----b473afe2cc70---------------------bookmark_footer-----------)![](../Images/8d63f95cc7d070338a256c75364f8771.png)

图片由[Hunter Haley](https://unsplash.com/@hnhmarketing?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，[Unsplash](https://unsplash.com/s/photos/nails-in-wood?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上可见

目前，Prophet 的预测准确性问题已不再是秘密。它在多个基准测试和预测竞赛中一再给出糟糕的结果。不过，它仍然是最常用的预测算法之一……

所以……现在是时候通过一些小的调整来解决困扰它的问题，并（希望）提高它的预测准确性。

[TSUtilities 项目](https://github.com/tblume1992/TSUtilities)

# Prophet 的趋势问题

奇怪的是，Prophet 的一个主要吸引力也是它的核心弱点之一。它确实呈现了一个引人注目的趋势，包含了变更点和线性段，这些都很容易分析。**但是**，有时，拟合这种趋势的方式既会过拟合又会欠拟合——在你的序列末尾进行简单的水平移位可能会导致趋势在预测范围内无限增长，同时也会在残差中留下**大量**有用的信号。

本文旨在解决第一个问题：**非约束性极端趋势**。

为了说明这个问题，我们将使用 M4 数据集。这些数据集都是开源的，并且可以在 M-competitions 的 [github](https://github.com/Mcompetitions/M4-methods/tree/master/Dataset) 上找到。
