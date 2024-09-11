# 使用 Pytest 进行 PySpark 代码的单元测试

> 原文：[https://towardsdatascience.com/unit-testing-pyspark-code-using-pytest-b5ab2fd54415?source=collection_archive---------15-----------------------#2023-01-16](https://towardsdatascience.com/unit-testing-pyspark-code-using-pytest-b5ab2fd54415?source=collection_archive---------15-----------------------#2023-01-16)

[](https://julianwest155.medium.com/?source=post_page-----b5ab2fd54415--------------------------------)[![Julian West](../Images/c7740cd407ed17e84af2c49a37dbbe92.png)](https://julianwest155.medium.com/?source=post_page-----b5ab2fd54415--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b5ab2fd54415--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b5ab2fd54415--------------------------------) [Julian West](https://julianwest155.medium.com/?source=post_page-----b5ab2fd54415--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F257ac04bf615&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funit-testing-pyspark-code-using-pytest-b5ab2fd54415&user=Julian+West&userId=257ac04bf615&source=post_page-257ac04bf615----b5ab2fd54415---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b5ab2fd54415--------------------------------) ·10 分钟阅读·2023年1月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb5ab2fd54415&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funit-testing-pyspark-code-using-pytest-b5ab2fd54415&user=Julian+West&userId=257ac04bf615&source=-----b5ab2fd54415---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb5ab2fd54415&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funit-testing-pyspark-code-using-pytest-b5ab2fd54415&source=-----b5ab2fd54415---------------------bookmark_footer-----------)![](../Images/314b8ebf1cb2596498d48e1f09861a50.png)

图片由 [Jez Timms](https://unsplash.com/@jeztimms?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/_Ch_onWf38o?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**我非常喜欢单元测试。**

**阅读了两本书——** [**《程序员修炼之道》**](https://engineeringfordatascience.com/book-notes/pragmatic-programmer/) **和** [**《重构》**](https://engineeringfordatascience.com/book-notes/refactoring/) **——完全改变了我对单元测试的看法。**

> “测试不是为了发现错误。
> 
> 我们认为，测试的主要好处发生在你考虑和编写测试时，而不是在你运行它们时。”
> 
> *—— 《程序员修炼之道》，大卫·托马斯与安德鲁·亨特*

我不是将测试视为完成数据管道后的琐事，而是看作提升代码设计、减少耦合、加快迭代速度以及建立工作信任的强大工具。

然而，为数据应用编写良好的测试可能很困难。

与输入相对明确的传统软件应用不同，生产中的数据应用依赖于大量且不断变化的输入数据。

在测试环境中准确地表示这些数据，并详细覆盖所有边界情况，可能非常具有挑战性。有些人认为对数据管道进行单元测试没有多大意义，而是专注于数据验证技术。
