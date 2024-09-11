# 深度学习在时间序列预测和分类中的进展：2023 年冬季版

> 原文：[`towardsdatascience.com/advances-in-deep-learning-for-time-series-forecasting-and-classification-winter-2023-edition-6617c203c1d1?source=collection_archive---------1-----------------------#2023-01-10`](https://towardsdatascience.com/advances-in-deep-learning-for-time-series-forecasting-and-classification-winter-2023-edition-6617c203c1d1?source=collection_archive---------1-----------------------#2023-01-10)

## 变压器在时间序列预测中的衰退以及时间序列嵌入方法的崛起。还有异常检测、分类和优化(t)干预的进展。

[](https://igodfried.medium.com/?source=post_page-----6617c203c1d1--------------------------------)![Isaac Godfried](https://igodfried.medium.com/?source=post_page-----6617c203c1d1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6617c203c1d1--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6617c203c1d1--------------------------------) [Isaac Godfried](https://igodfried.medium.com/?source=post_page-----6617c203c1d1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bebc59e793b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvances-in-deep-learning-for-time-series-forecasting-and-classification-winter-2023-edition-6617c203c1d1&user=Isaac+Godfried&userId=5bebc59e793b&source=post_page-5bebc59e793b----6617c203c1d1---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6617c203c1d1--------------------------------) ·16 分钟阅读·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6617c203c1d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvances-in-deep-learning-for-time-series-forecasting-and-classification-winter-2023-edition-6617c203c1d1&user=Isaac+Godfried&userId=5bebc59e793b&source=-----6617c203c1d1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6617c203c1d1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fadvances-in-deep-learning-for-time-series-forecasting-and-classification-winter-2023-edition-6617c203c1d1&source=-----6617c203c1d1---------------------bookmark_footer-----------)![](img/7af7bc73ed4069181cec96d796eced4a.png)

照片来自我自己（大沙丘国家公园日落时分）

*注意，你可以在[2024 年更新版的文章在 DDS 中找到](https://medium.com/deep-data-science/advances-in-deep-learning-for-time-series-forecasting-classification-winter-2024-a3fd31b875b0)。

自从我上次写关于时间序列深度学习现状的更新已经有一段时间了。几次会议已经过去，整个领域在多个方面都有了进展。在这里，我将尝试涵盖过去一年或多年来一些更有前景以及关键的论文，以及对[流预测](https://github.com/AIStream-Peelout/flow-forecast)框架[FF]的更新。

## [流预测框架](https://github.com/AIStream-Peelout/flow-forecast) 更新：

+   在过去的一年里，我们在 FF 的架构和文档方面取得了重大进展。最近，我们推出了对时间序列分类和监督异常检测的全面支持。此外，我们还增加了更多的 [教程笔记本](https://github.com/AIStream-Peelout/flow_tutorials)，并将我们的单元测试覆盖率扩展到了 77%以上。

+   我们还添加了一个普通的 GRU 模型，你可以用来处理时间序列……
