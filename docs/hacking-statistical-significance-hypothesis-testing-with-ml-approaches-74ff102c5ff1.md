# 破解统计显著性：使用机器学习方法进行假设检验

> 原文：[https://towardsdatascience.com/hacking-statistical-significance-hypothesis-testing-with-ml-approaches-74ff102c5ff1?source=collection_archive---------15-----------------------#2023-01-10](https://towardsdatascience.com/hacking-statistical-significance-hypothesis-testing-with-ml-approaches-74ff102c5ff1?source=collection_archive---------15-----------------------#2023-01-10)

## 在任何上下文中测试统计显著性，无需假设

[](https://medium.com/@cerlymarco?source=post_page-----74ff102c5ff1--------------------------------)[![Marco Cerliani](../Images/48a07a024349bac3c8e397bf5a0372e2.png)](https://medium.com/@cerlymarco?source=post_page-----74ff102c5ff1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----74ff102c5ff1--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----74ff102c5ff1--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----74ff102c5ff1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc843902314c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhacking-statistical-significance-hypothesis-testing-with-ml-approaches-74ff102c5ff1&user=Marco+Cerliani&userId=c843902314c7&source=post_page-c843902314c7----74ff102c5ff1---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----74ff102c5ff1--------------------------------) ·7分钟阅读·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F74ff102c5ff1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhacking-statistical-significance-hypothesis-testing-with-ml-approaches-74ff102c5ff1&user=Marco+Cerliani&userId=c843902314c7&source=-----74ff102c5ff1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F74ff102c5ff1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhacking-statistical-significance-hypothesis-testing-with-ml-approaches-74ff102c5ff1&source=-----74ff102c5ff1---------------------bookmark_footer-----------)![](../Images/d51442429b9c1ef354277a898ba90539.png)

图片由[Christian Stahl](https://unsplash.com/@woodpecker65?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

数据分析在各个领域的重要性众所周知。从商业到学术，进行恰当的分析是取得前沿结果的关键。在这一方面，正确地操控和提取数据中的有意义见解至关重要。**数据分析师/科学家负责弥合理论假设与实际证据之间的差距**。

**对所有可能出现的问题提供分析性回答是一段昂贵而艰难的旅程**。将问题/需求转化为分析语言是执行的第一步。这种操作的优良性至关重要，因为它影响最终结果的正确性。在初步阶段，理解分析目标并指出最佳的数据源、框架和需要参与的人对于实现最佳结果至关重要。

大多数时候，**通过进行统计测试来分析性地回答问题**。许多统计测试的工作方式如下：

+   设定一个零假设，这是一种可以描述世界的默认选项。

+   提出一个替代方案，并且…
