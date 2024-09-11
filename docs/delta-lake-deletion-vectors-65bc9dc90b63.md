# Delta Lake: 删除向量

> 原文：[`towardsdatascience.com/delta-lake-deletion-vectors-65bc9dc90b63?source=collection_archive---------3-----------------------#2023-05-25`](https://towardsdatascience.com/delta-lake-deletion-vectors-65bc9dc90b63?source=collection_archive---------3-----------------------#2023-05-25)

## 删除向量如何与 DML 命令相关联，并且它们如何提高写入性能？

[](https://medium.com/@vitorf24?source=post_page-----65bc9dc90b63--------------------------------)![Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----65bc9dc90b63--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65bc9dc90b63--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----65bc9dc90b63--------------------------------) [Vitor Teixeira](https://medium.com/@vitorf24?source=post_page-----65bc9dc90b63--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6b05068b69d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-deletion-vectors-65bc9dc90b63&user=Vitor+Teixeira&userId=6b05068b69d8&source=post_page-6b05068b69d8----65bc9dc90b63---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65bc9dc90b63--------------------------------) ·9 分钟阅读·2023 年 5 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65bc9dc90b63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-deletion-vectors-65bc9dc90b63&user=Vitor+Teixeira&userId=6b05068b69d8&source=-----65bc9dc90b63---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65bc9dc90b63&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdelta-lake-deletion-vectors-65bc9dc90b63&source=-----65bc9dc90b63---------------------bookmark_footer-----------)![](img/a0f093c1a52476e936a00d6ef3306189.png)

由[Sam Pak](https://unsplash.com/@melocokr?utm_source=medium&utm_medium=referral)拍摄，照片来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

能够更新和删除记录是从传统数据仓库转向数据湖时丢失的一个功能。虽然数据湖在解决规模和成本问题上表现出色，但它们牺牲了更新和删除记录的能力。数据湖由许多文件构成，这些文件迅速变成数据沼泽，这就是问题所在，而湖仓架构为此提供了救赎。

湖仓架构是一种数据仓库和数据湖的混合体，结合了两者以解决它们的问题。其中一个问题是缺乏 DML 支持的 ACID 事务，这在数据仓库中非常受欢迎，而这正是 Delta Lake 的强项。然而，由于 Delta 的 ACID 保证，原地文件更改是不可能的。

在这篇文章中，我将讨论 Delta 如何支持 DML 命令，删除向量是如何工作的，以及它们为何作为性能改进而重要。

# Delta Lake — DML，底层实现

借助于[事务日志](https://www.databricks.com/blog/2019/08/21/diving-into-delta-lake-unpacking-the-transaction-log.html)，Delta Lake 支持像*更新*、*删除*和*合并*这样的 DML 命令。为了简单起见，我们将专注于*更新*和*删除*命令，因为它们…
