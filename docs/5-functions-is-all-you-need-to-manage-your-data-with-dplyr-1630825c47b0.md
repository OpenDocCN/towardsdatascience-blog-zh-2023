# 5 个函数足以管理你的数据，使用 dplyr

> 原文：[https://towardsdatascience.com/5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0?source=collection_archive---------10-----------------------#2023-03-10](https://towardsdatascience.com/5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0?source=collection_archive---------10-----------------------#2023-03-10)

## 如何高效地准备数据以供使用

[](https://sonery.medium.com/?source=post_page-----1630825c47b0--------------------------------)[![Soner Yıldırım](../Images/c589572e9d1ee176cd4f5a0008173f1b.png)](https://sonery.medium.com/?source=post_page-----1630825c47b0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1630825c47b0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1630825c47b0--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----1630825c47b0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----1630825c47b0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1630825c47b0--------------------------------) · 8 分钟阅读 · 2023年3月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1630825c47b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----1630825c47b0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1630825c47b0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F5-functions-is-all-you-need-to-manage-your-data-with-dplyr-1630825c47b0&source=-----1630825c47b0---------------------bookmark_footer-----------)![](../Images/37b7af736531d661d732e23977fce5dd.png)

图片由 [Kelly Sikkema](https://unsplash.com/@kellysikkema?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/-1_RZL8BGBM?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有实际处理过真实数据的经验的人知道，数据整理占据了更大的比重。

我所说的数据整理涵盖了将数据准备好以供其他利益相关者或下游过程使用的操作。

无论你是数据分析师、数据科学家还是数据工程师，你都需要在日常工作中执行一个或多个以下操作：

+   过滤（例如：获取德克萨斯州的销售数据）

+   排序（例如：我想查看上周的前10名畅销产品）

+   更新（例如：更改这些产品的类别）

+   汇总（例如：我想查看每个类别的平均收入）

数据科学生态系统中存在数据分析和操作工具，提供高效的方式来执行这些操作，以便能够在生态系统中保持其存在。

在这篇文章中，我们将学习如何使用数据科学中的一个主要工具：dplyr，来处理这些任务。

这是一个用于R编程语言的包，被描述为“数据操作的语法”。
