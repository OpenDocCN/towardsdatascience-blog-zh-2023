# 使用 Python 生成合成数据

> 原文：[`towardsdatascience.com/generating-synthetic-data-with-python-ea15fd0555ee?source=collection_archive---------3-----------------------#2023-07-31`](https://towardsdatascience.com/generating-synthetic-data-with-python-ea15fd0555ee?source=collection_archive---------3-----------------------#2023-07-31)

## 创建合成数据的综合指南

[](https://iffatm.medium.com/?source=post_page-----ea15fd0555ee--------------------------------)![Iffat Malik](https://iffatm.medium.com/?source=post_page-----ea15fd0555ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea15fd0555ee--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea15fd0555ee--------------------------------) [Iffat Malik](https://iffatm.medium.com/?source=post_page-----ea15fd0555ee--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88491120e677&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-synthetic-data-with-python-ea15fd0555ee&user=Iffat+Malik&userId=88491120e677&source=post_page-88491120e677----ea15fd0555ee---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea15fd0555ee--------------------------------) ·14 分钟阅读·2023 年 7 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea15fd0555ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-synthetic-data-with-python-ea15fd0555ee&user=Iffat+Malik&userId=88491120e677&source=-----ea15fd0555ee---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea15fd0555ee&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-synthetic-data-with-python-ea15fd0555ee&source=-----ea15fd0555ee---------------------bookmark_footer-----------)![](img/a17fd93d1fcd31d058aef3ac14afca95.png)

图片由作者提供

我们不断听到数据在推动增长、创新和竞争力方面的关键作用。它已成为所有行业成功的基石。本质上，数据已经成为我们每一个努力的基础，无论是编写技术博客、教育内容、测试产品或调试软件，还是探索 AI/ML 训练模型和算法的复杂性，数据都处于这些任务的核心。

获取完美符合各种需求和兴趣的精确数据可能是一项艰巨的任务。在互联网中搜索你所需的确切数据可能既令人沮丧又耗时。即使你设法找到合适的数据，清理和处理这些数据的过程也可能需要宝贵的时间、资源和费用。此外，隐私问题、数据敏感性、版权和监管限制常常成为重要障碍。例如，包含敏感信息的数据集，如医疗数据、财务记录数据，或者从版权网站获取演示数据集等。

在这种情况下，合成数据来拯救局面！在本文中，我们将探讨什么是合成数据，以及如何使用 2 个不同的库在 Python 中生成它。
