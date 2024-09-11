# 在数据科学中追求工具无关性：SQL Case When 和 Pandas Where

> 原文：[`towardsdatascience.com/towards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa?source=collection_archive---------13-----------------------#2023-07-17`](https://towardsdatascience.com/towards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa?source=collection_archive---------13-----------------------#2023-07-17)

## 通过示例进行解释

[](https://sonery.medium.com/?source=post_page-----f6a3d3cbcafa--------------------------------)![Soner Yıldırım](https://sonery.medium.com/?source=post_page-----f6a3d3cbcafa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f6a3d3cbcafa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6a3d3cbcafa--------------------------------) [Soner Yıldırım](https://sonery.medium.com/?source=post_page-----f6a3d3cbcafa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2cf6b549448&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=post_page-2cf6b549448----f6a3d3cbcafa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f6a3d3cbcafa--------------------------------) ·6 分钟阅读·2023 年 7 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff6a3d3cbcafa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa&user=Soner+Y%C4%B1ld%C4%B1r%C4%B1m&userId=2cf6b549448&source=-----f6a3d3cbcafa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff6a3d3cbcafa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftowards-being-tool-agnostic-in-data-science-sql-case-when-and-pandas-where-f6a3d3cbcafa&source=-----f6a3d3cbcafa---------------------bookmark_footer-----------)![](img/886913bc04bb14136595417bda17365c.png)

图片由 [Monika Simeonova](https://unsplash.com/@monnysim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/photos/yUdawywuYm0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

您的客户数据存储在 SQL 数据库中。您被分配了一个任务，涉及从一些表中检索数据，进行数据清理和处理，然后将结果写入另一个表中。

不幸的是，你不知道如何用 SQL 完成这些操作。别担心！你在使用 Pandas 进行数据清洗和处理方面很出色。因此，你提出了一个解决方案，即：

+   从 SQL 表中检索所有数据

+   将数据下载为 CSV 文件

+   将 CSV 文件读取到 Pandas DataFrames 中

+   执行所需的数据清洗和处理操作

+   将结果写入另一个 CSV 文件

+   将 CSV 文件中的数据上传到 SQL 表中

这个计划不错吧？

如果你真的执行这个计划，我相信你的经理会和你谈话，这可能是愉快的，也可能是不愉快的，取决于你经理的性格。无论如何，我不认为你会在谈话后继续执行这个精彩的计划。

我知道在数据科学中，通常有许多不同的方法来完成任务。你应该总是追求最有效的方法……
