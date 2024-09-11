# EDA 使用 Polars: Pandas 用户的逐步指南（第一部分）

> 原文：[https://towardsdatascience.com/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008?source=collection_archive---------4-----------------------#2023-07-06](https://towardsdatascience.com/eda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008?source=collection_archive---------4-----------------------#2023-07-06)

## 使用 Polars 提升您的数据分析

[](https://medium.com/@antonsruberts?source=post_page-----b2ec500a1008--------------------------------)[![Antons Tocilins-Ruberts](../Images/363a4f32aa793cca7a67dea68e76e3cf.png)](https://medium.com/@antonsruberts?source=post_page-----b2ec500a1008--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b2ec500a1008--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b2ec500a1008--------------------------------) [Antons Tocilins-Ruberts](https://medium.com/@antonsruberts?source=post_page-----b2ec500a1008--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F9dee50b0374b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=post_page-9dee50b0374b----b2ec500a1008---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b2ec500a1008--------------------------------) ·12 分钟阅读·2023 年 7 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb2ec500a1008&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008&user=Antons+Tocilins-Ruberts&userId=9dee50b0374b&source=-----b2ec500a1008---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb2ec500a1008&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Feda-with-polars-step-by-step-guide-for-pandas-users-part-1-b2ec500a1008&source=-----b2ec500a1008---------------------bookmark_footer-----------)![](../Images/1ec62693f9e625f8eb7847708504fcaa.png)

图片由 [Mitul Grover](https://unsplash.com/@mitulgrover?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

每隔一段时间，总会有一些工具显著改变数据分析的方式。我相信 Polars 就是其中之一，所以在这一系列帖子中，我将深入探讨这个库，将其与一个更知名、成熟的库——Pandas 进行比较，并展示使用示例数据集的分析工作流程。

# 什么是 Polars?

Polars 是一个用 Rust 编写的极其快速的 DataFrame 库。幸运的是，对于我们（数据科学家/分析师）来说，它有一个文档非常完善的 Python 封装，提供了一整套功能来处理数据和构建数据管道。以下是我在切换到 Polars 后看到的主要优点：

+   更快的预处理操作

+   能够处理超出 RAM 容量的数据集

+   由于需要正确构建数据管道，代码质量更高

你可以在这个 [用户指南](https://pola-rs.github.io/polars-book/user-guide/#philosophy) 中查看完整的优点集合，并在这个 H20 [基准测试](https://h2oai.github.io/db-benchmark/) 中查看速度对比。

# 从 Pandas 切换

初看起来，Pandas 和 Polars 似乎非常相似，例如像…这样的函数
