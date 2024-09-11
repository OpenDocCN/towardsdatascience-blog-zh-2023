# 荷兰的电动车：使用 Python 进行探索性数据分析

> 原文：[https://towardsdatascience.com/electric-cars-in-the-netherlands-exploratory-data-analysis-with-python-d01477949984?source=collection_archive---------13-----------------------#2023-02-10](https://towardsdatascience.com/electric-cars-in-the-netherlands-exploratory-data-analysis-with-python-d01477949984?source=collection_archive---------13-----------------------#2023-02-10)

## 使用 Python、Pandas 和 Bokeh 进行数据分析和可视化

[](https://dmitryelj.medium.com/?source=post_page-----d01477949984--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----d01477949984--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d01477949984--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d01477949984--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----d01477949984--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felectric-cars-in-the-netherlands-exploratory-data-analysis-with-python-d01477949984&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----d01477949984---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d01477949984--------------------------------) ·16分钟阅读·2023年2月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd01477949984&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felectric-cars-in-the-netherlands-exploratory-data-analysis-with-python-d01477949984&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----d01477949984---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd01477949984&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felectric-cars-in-the-netherlands-exploratory-data-analysis-with-python-d01477949984&source=-----d01477949984---------------------bookmark_footer-----------)![](../Images/d82a9da311e424b14898ce93dffcbc64.png)

Smart EQ Car，图片来源 [https://en.wikipedia.org/wiki/Smart_electric_drive](https://en.wikipedia.org/wiki/Smart_electric_drive)

第一辆电动车是什么时候注册的？（剧透：比大多数人想象的要早得多。）哪款车更贵，电动Porsche还是Jaguar？探索性数据分析（EDA）不仅是构建每个数据管道的重要部分，而且是一个相当有趣的过程。在本文中，我将使用荷兰RDW（荷兰车辆管理局）公共数据集来查找有关电动车的信息。我们将使用Python、Pandas和Bokeh来提取和展示哪些数据。

让我们开始吧。

# 数据加载

**RDW**（“Rijks Dienst Wegverkeer”，[https://www.rdw.nl](https://www.rdw.nl/)）是一个荷兰组织，负责荷兰机动车辆和驾驶执照的审批和注册。作为一个公共政府机构，它的数据对所有人开放。对我们来说最有趣的是“Gekentekende voertuigen”（“有牌照的车辆”）数据集。该数据集在公共领域许可下免费提供，并可从 [opendata.rdw.nl](https://opendata.rdw.nl/Voertuigen/Open-Data-RDW-Gekentekende_voertuigen/m9d7-ebf2) 下载。文件大小约为10 GB，包含自1952年以来在荷兰注册的所有车辆信息。处理这样的文件…
