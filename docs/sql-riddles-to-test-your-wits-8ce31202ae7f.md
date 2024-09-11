# SQL 谜题测试你的智慧

> 原文：[`towardsdatascience.com/sql-riddles-to-test-your-wits-8ce31202ae7f?source=collection_archive---------4-----------------------#2023-02-22`](https://towardsdatascience.com/sql-riddles-to-test-your-wits-8ce31202ae7f?source=collection_archive---------4-----------------------#2023-02-22)

## 时间戳、依赖过滤器和表现不佳的左连接

[](https://mgsosna.medium.com/?source=post_page-----8ce31202ae7f--------------------------------)![Matt Sosna](https://mgsosna.medium.com/?source=post_page-----8ce31202ae7f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8ce31202ae7f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ce31202ae7f--------------------------------) [Matt Sosna](https://mgsosna.medium.com/?source=post_page-----8ce31202ae7f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff17fb22b897&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-riddles-to-test-your-wits-8ce31202ae7f&user=Matt+Sosna&userId=f17fb22b897&source=post_page-f17fb22b897----8ce31202ae7f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8ce31202ae7f--------------------------------) · 8 分钟阅读 · 2023 年 2 月 22 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8ce31202ae7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-riddles-to-test-your-wits-8ce31202ae7f&user=Matt+Sosna&userId=f17fb22b897&source=-----8ce31202ae7f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8ce31202ae7f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsql-riddles-to-test-your-wits-8ce31202ae7f&source=-----8ce31202ae7f---------------------bookmark_footer-----------)![](img/e1fc7bf3a6b50e65020f31e0892a018c.png)

图片由 [Saffu](https://unsplash.com/@saffu?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

SQL 是一种看似简单的语言。在其众多方言中，用户可以使用类似英语的语法查询数据库。**你所见即所得……直到你发现不是。**

我时不时会遇到一些查询，返回的结果与我预期的完全不同，这让我学到了语言中的一些小细节。我在这篇文章中汇编了三个最近让我困惑的问题，并将它们安排成谜题以增加趣味。尝试在阅读每节的结尾前找出答案！

我还包括了快速的[**公共表表达式 (CTEs)**](https://learnsql.com/blog/what-is-common-table-expression/)以生成每个示例中的表格，因此你无需尝试查询公司生产表！但为了真正熟悉 SQL，我实际上建议你创建自己的数据库和表来练习。查看[这篇文章](https://medium.com/towards-data-science/intermediate-sql-for-everyone-fe35c541147a)了解方法。

请注意所有查询都是在 Postgres 中进行的——你在不同方言中可能会得到不同的结果。最后，必须提及的是，每个查询中的实际数据和主题仅仅是示例。🙂
