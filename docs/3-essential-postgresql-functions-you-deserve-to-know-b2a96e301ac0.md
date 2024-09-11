# 你值得知道的3个基本 PostgreSQL 函数

> 原文：[https://towardsdatascience.com/3-essential-postgresql-functions-you-deserve-to-know-b2a96e301ac0?source=collection_archive---------8-----------------------#2023-05-22](https://towardsdatascience.com/3-essential-postgresql-functions-you-deserve-to-know-b2a96e301ac0?source=collection_archive---------8-----------------------#2023-05-22)

## PostgreSQL

## 三个鲜为人知的函数，将提升你的数据处理能力

[](https://polmarin.medium.com/?source=post_page-----b2a96e301ac0--------------------------------)[![Pol Marin](../Images/a4f69a96717d453db9791f27b8f85e86.png)](https://polmarin.medium.com/?source=post_page-----b2a96e301ac0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2a96e301ac0--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b2a96e301ac0--------------------------------) [Pol Marin](https://polmarin.medium.com/?source=post_page-----b2a96e301ac0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1fa43cc443e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-essential-postgresql-functions-you-deserve-to-know-b2a96e301ac0&user=Pol+Marin&userId=1fa43cc443e7&source=post_page-1fa43cc443e7----b2a96e301ac0---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2a96e301ac0--------------------------------) ·6分钟阅读·2023年5月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb2a96e301ac0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-essential-postgresql-functions-you-deserve-to-know-b2a96e301ac0&user=Pol+Marin&userId=1fa43cc443e7&source=-----b2a96e301ac0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb2a96e301ac0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-essential-postgresql-functions-you-deserve-to-know-b2a96e301ac0&source=-----b2a96e301ac0---------------------bookmark_footer-----------)![](../Images/5f7f6b01e046f826aefa04bd3846f590.png)

图片由 [Sunder Muthukumaran](https://unsplash.com/ja/@sunder_2k25?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

Postgres 是最著名的关系数据库管理系统 (RDBMS) 之一，原因显而易见。

我不会详细讨论使用它的所有优点，但我会用我在互联网上找到的一个非常清晰的结论来说明：

> “*PostgreSQL 因其作为关系数据的强大、功能丰富的选择而获得了很高的声誉。重视稳定性、功能性和标准符合性，PostgreSQL 满足了许多项目的所有要求。*”[1]

我每天在工作中使用它，发现了一些在网上不容易找到的实用功能。

以前，我会从 Postgres 中提取所有必要的数据，然后用 Python 和 Pandas 进行处理。但现在不再需要了，尤其是我即将分享的这三个功能。

在介绍最酷的功能之前，让我先分享一些更常见的功能。这样可以让我们处于同一水平线上。

我将把这篇文章的内容分为三类：基本功能、替代功能（相对于基本功能）和酷炫功能。
