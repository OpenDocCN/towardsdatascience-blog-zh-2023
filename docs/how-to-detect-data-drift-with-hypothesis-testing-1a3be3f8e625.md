# 如何通过假设检验检测数据漂移

> 原文：[`towardsdatascience.com/how-to-detect-data-drift-with-hypothesis-testing-1a3be3f8e625?source=collection_archive---------2-----------------------#2023-05-17`](https://towardsdatascience.com/how-to-detect-data-drift-with-hypothesis-testing-1a3be3f8e625?source=collection_archive---------2-----------------------#2023-05-17)

## MLOps

## 提示：忘记 p 值

[](https://michaloleszak.medium.com/?source=post_page-----1a3be3f8e625--------------------------------)![Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----1a3be3f8e625--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1a3be3f8e625--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a3be3f8e625--------------------------------) [Michał Oleszak](https://michaloleszak.medium.com/?source=post_page-----1a3be3f8e625--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc58320fab2a8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-detect-data-drift-with-hypothesis-testing-1a3be3f8e625&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=post_page-c58320fab2a8----1a3be3f8e625---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1a3be3f8e625--------------------------------) · 18 分钟阅读 · 2023 年 5 月 17 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1a3be3f8e625&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-detect-data-drift-with-hypothesis-testing-1a3be3f8e625&user=Micha%C5%82+Oleszak&userId=c58320fab2a8&source=-----1a3be3f8e625---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1a3be3f8e625&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-detect-data-drift-with-hypothesis-testing-1a3be3f8e625&source=-----1a3be3f8e625---------------------bookmark_footer-----------)![](img/11cc327d0c01a9d68a28e474497e8023.png)

数据漂移是任何使用机器学习模型进行实时预测的人的关注点。世界在变化，随着消费者的口味或人口统计特征的转变，模型开始接收与训练时不同的特征值，这可能导致意外的输出。检测特征漂移似乎很简单：我们只需要决定训练和服务中该特征的分布是否相同。虽然确实有统计检验方法，但你确定自己使用得当吗？

# 单变量漂移检测

监控机器学习模型的部署后性能是其生命周期中的一个关键部分。随着世界的变化和数据的漂移，[许多模型往往会随着时间的推移而表现出性能下降](https://www.nature.com/articles/s41598-022-15245-z)。保持警惕的最佳方法是实时计算性能指标，或者在缺乏真实标签的情况下[估算它们](https://medium.com/towards-artificial-intelligence/estimating-model-performance-without-ground-truth-453b850dad9a)。

观察到性能下降的一个可能原因是数据漂移。[数据漂移](https://medium.com/towards-data-science/dont-let-your-model-s-quality-drift-away-53d2f7899c09)是指模型输入的分布在训练数据和生产数据之间发生变化。检测和分析数据漂移的性质有助于改善性能下降的模型……
