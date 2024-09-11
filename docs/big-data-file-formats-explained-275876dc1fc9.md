# 大数据文件格式解析

> 原文：[`towardsdatascience.com/big-data-file-formats-explained-275876dc1fc9?source=collection_archive---------0-----------------------#2023-02-28`](https://towardsdatascience.com/big-data-file-formats-explained-275876dc1fc9?source=collection_archive---------0-----------------------#2023-02-28)

## Parquet 与 ORC 与 AVRO 与 JSON。应该选择哪个，如何使用它们？

[](https://mshakhomirov.medium.com/?source=post_page-----275876dc1fc9--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----275876dc1fc9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----275876dc1fc9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----275876dc1fc9--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----275876dc1fc9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe06a48b3dd48&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbig-data-file-formats-explained-275876dc1fc9&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=post_page-e06a48b3dd48----275876dc1fc9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----275876dc1fc9--------------------------------) ·9 分钟阅读·2023 年 2 月 28 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F275876dc1fc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbig-data-file-formats-explained-275876dc1fc9&user=%F0%9F%92%A1Mike+Shakhomirov&userId=e06a48b3dd48&source=-----275876dc1fc9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F275876dc1fc9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbig-data-file-formats-explained-275876dc1fc9&source=-----275876dc1fc9---------------------bookmark_footer-----------)![](img/ce5abf8987056d730cd51c36eda44998.png)

图片由 [James Lee](https://unsplash.com/@picsbyjameslee?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我是数据仓库（DWH）解决方案的忠实粉丝，尤其是带有 ELT 设计（提取-加载-转换）数据管道的方案。然而，在某些时候，我面临了*处理*原始事件数据的需求，并且**需要选择文件格式**来存储数据文件。

> *这是一种典型场景，机器学习工程师需要创建行为数据集来训练模型，并生成更好的产品推荐或预测客户流失。*

为我们的机器学习管道选择合适的文件格式至关重要，因为它可能会显著改变数据的输入/输出时间，并且肯定会对我们的模型训练性能产生更广泛的影响。

[## 提升你的数据工程技能：通过这个机器学习管道](https://pub.towardsai.net/supercharge-your-data-engineering-skills-with-this-machine-learning-pipeline-b69d159780b7?source=post_page-----275876dc1fc9--------------------------------)

### 数据建模、Python、DAG、Big Data 文件格式、成本……它涵盖了所有内容

[pub.towardsai.net](https://pub.towardsai.net/supercharge-your-data-engineering-skills-with-this-machine-learning-pipeline-b69d159780b7?source=post_page-----275876dc1fc9--------------------------------)

另一个需要考虑的因素是数据的大小，因为我们已经为文件存储支付了过多的费用。

这个故事旨在考虑这些重要的问题和其他选项，以找到数据管道的**最佳**大数据文件格式。
