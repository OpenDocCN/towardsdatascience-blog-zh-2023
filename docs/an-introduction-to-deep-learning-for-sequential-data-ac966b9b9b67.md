# 《深度学习在序列数据中的应用介绍》

> 原文：[`towardsdatascience.com/an-introduction-to-deep-learning-for-sequential-data-ac966b9b9b67?source=collection_archive---------6-----------------------#2023-11-14`](https://towardsdatascience.com/an-introduction-to-deep-learning-for-sequential-data-ac966b9b9b67?source=collection_archive---------6-----------------------#2023-11-14)

## 突出时间序列和自然语言处理的相似性

[](https://donatoriccio.medium.com/?source=post_page-----ac966b9b9b67--------------------------------)![Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ac966b9b9b67--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ac966b9b9b67--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ac966b9b9b67--------------------------------) [Donato Riccio](https://donatoriccio.medium.com/?source=post_page-----ac966b9b9b67--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe384fc71d292&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-deep-learning-for-sequential-data-ac966b9b9b67&user=Donato+Riccio&userId=e384fc71d292&source=post_page-e384fc71d292----ac966b9b9b67---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ac966b9b9b67--------------------------------) ·8 分钟阅读·2023 年 11 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fac966b9b9b67&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-deep-learning-for-sequential-data-ac966b9b9b67&user=Donato+Riccio&userId=e384fc71d292&source=-----ac966b9b9b67---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fac966b9b9b67&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-introduction-to-deep-learning-for-sequential-data-ac966b9b9b67&source=-----ac966b9b9b67---------------------bookmark_footer-----------)![](img/9bd71d38282448e4edea2f653b4701a1.png)

当我加载时间序列数据集时，我想象中的自己。图片由作者提供。（AI 辅助）

时间序列和自然语言等序列数据需要能够捕捉顺序和上下文的模型。时间序列分析专注于基于时间模式进行预测，而自然语言处理则旨在从单词序列中提取语义意义。

虽然是不同的任务，但这两种数据类型都具有长期依赖关系，其中远程元素会影响预测。随着深度学习的进步，最初为一个领域开发的模型架构已被调整用于另一个领域。

## 序列数据

时间序列和自然语言都有顺序结构，其中观察值在序列中的位置非常重要。

![](img/9a269b83f4cb166a2c118aed8691e4ea.png)![](img/9f6877653538823f1d05aaebe893604c.png)

时间序列是一系列数值。（左）文本是一系列单词。（右）图片由作者提供。

时间序列是一组按时间顺序排列的观察值，这些观察值按固定时间间隔采样。一些例子包括：

+   每天的股票价格

+   每小时的服务器指标

+   每秒钟的温度读数
