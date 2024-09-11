# 2个最佳SQL技巧来查找表中的重复值

> 原文：[https://towardsdatascience.com/2-best-sql-tricks-to-find-duplicate-values-in-a-table-1197618dcc74?source=collection_archive---------3-----------------------#2023-06-27](https://towardsdatascience.com/2-best-sql-tricks-to-find-duplicate-values-in-a-table-1197618dcc74?source=collection_archive---------3-----------------------#2023-06-27)

## 数据科学

## 删除重复记录以节省时间和金钱

[](https://medium.com/@17.rsuraj?source=post_page-----1197618dcc74--------------------------------)[![Suraj Gurav](../Images/f5dca32861f8c1c428e66fbe2174c04b.png)](https://medium.com/@17.rsuraj?source=post_page-----1197618dcc74--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1197618dcc74--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1197618dcc74--------------------------------) [Suraj Gurav](https://medium.com/@17.rsuraj?source=post_page-----1197618dcc74--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fdda183cca2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-best-sql-tricks-to-find-duplicate-values-in-a-table-1197618dcc74&user=Suraj+Gurav&userId=1fdda183cca2&source=post_page-1fdda183cca2----1197618dcc74---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1197618dcc74--------------------------------) ·7分钟阅读·2023年6月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1197618dcc74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-best-sql-tricks-to-find-duplicate-values-in-a-table-1197618dcc74&user=Suraj+Gurav&userId=1fdda183cca2&source=-----1197618dcc74---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1197618dcc74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F2-best-sql-tricks-to-find-duplicate-values-in-a-table-1197618dcc74&source=-----1197618dcc74---------------------bookmark_footer-----------)![](../Images/ff6a7b3150944efd4023ee12e71b1667.png)

由 [kofookoo.de](https://unsplash.com/@kofookoo?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来源于 [Unsplash](https://unsplash.com/photos/6EgxRnKU5BI?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**重复记录无处不在！**

这是每个数据库用户面临的最常见问题之一。

这些重复记录出现在数据库中是由于多种原因，例如数据整合、系统故障、在更新数据库时的人为错误，以及缺乏数据验证检查。

重复记录使数据不一致。这样的数据库会让你或你的公司花费更多的钱、时间和资源来维护和处理。重复记录不必要地占用额外的存储空间，并且会减慢查询执行速度。

因此，在进行分析之前，你必须至少从你查询的表中删除这些重复记录。

这就是为什么在工作面试中，公司也常常会询问处理重复记录的问题。

在本文中，你将探讨识别重复记录的两种最佳、节省时间的 SQL 方法。我在过程中提供了有趣的例子，以便清楚地解释这些概念。

同样，**别忘了在阅读完毕后查看一些惊人的 SQL 资源**。
