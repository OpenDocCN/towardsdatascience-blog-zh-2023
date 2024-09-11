# 什么是 dbt（数据构建工具），你什么时候应该使用它？

> 原文：[https://towardsdatascience.com/what-is-dbt-data-build-tool-and-when-should-you-use-it-9dc1566d960d?source=collection_archive---------2-----------------------#2023-04-30](https://towardsdatascience.com/what-is-dbt-data-build-tool-and-when-should-you-use-it-9dc1566d960d?source=collection_archive---------2-----------------------#2023-04-30)

## 探索 dbt 的隐藏优点和缺点

[](https://khuyentran1476.medium.com/?source=post_page-----9dc1566d960d--------------------------------)[![Khuyen Tran](../Images/98aa66025ad29b618e875c75f1c400a5.png)](https://khuyentran1476.medium.com/?source=post_page-----9dc1566d960d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9dc1566d960d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9dc1566d960d--------------------------------) [Khuyen Tran](https://khuyentran1476.medium.com/?source=post_page-----9dc1566d960d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F84a02493194a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-dbt-data-build-tool-and-when-should-you-use-it-9dc1566d960d&user=Khuyen+Tran&userId=84a02493194a&source=post_page-84a02493194a----9dc1566d960d---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9dc1566d960d--------------------------------) ·8 min read·2023年4月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9dc1566d960d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-dbt-data-build-tool-and-when-should-you-use-it-9dc1566d960d&user=Khuyen+Tran&userId=84a02493194a&source=-----9dc1566d960d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9dc1566d960d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-dbt-data-build-tool-and-when-should-you-use-it-9dc1566d960d&source=-----9dc1566d960d---------------------bookmark_footer-----------)![](../Images/8fd7187361494d3070374a447154da1a.png)

作者提供的图片

# 动机

如果你的组织希望创建一个数据驱动的产品，你应该考虑拥有高效的数据管道，以便：

1.  **保持竞争力：** 通过高效的数据管道快速访问数据及其分析，加速决策过程，使你领先于竞争对手。

1.  **降低成本：** 通过高效的数据管道，可以显著减少收集和转换数据所需的时间和精力，从而降低成本，使员工能够专注于需要人工智能的更高级任务。

![](../Images/1da42b35f889b90b992755664495c754.png)

作者提供的图片

近年来，处理数据管道时越来越受欢迎的工具是 `dbt`（数据构建工具）。
