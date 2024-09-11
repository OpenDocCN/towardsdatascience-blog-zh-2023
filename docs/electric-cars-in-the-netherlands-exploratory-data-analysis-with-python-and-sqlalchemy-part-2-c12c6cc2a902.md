# 荷兰的电动汽车：使用 Python 和 SQLAlchemy 的探索性数据分析（第 2 部分）

> 原文：[https://towardsdatascience.com/electric-cars-in-the-netherlands-exploratory-data-analysis-with-python-and-sqlalchemy-part-2-c12c6cc2a902?source=collection_archive---------9-----------------------#2023-03-10](https://towardsdatascience.com/electric-cars-in-the-netherlands-exploratory-data-analysis-with-python-and-sqlalchemy-part-2-c12c6cc2a902?source=collection_archive---------9-----------------------#2023-03-10)

## 使用 Python、SQLAlchemy 和 Bokeh 进行数据分析和可视化

[](https://dmitryelj.medium.com/?source=post_page-----c12c6cc2a902--------------------------------)[![Dmitrii Eliuseev](../Images/7c48f0c016930ead59ddb785eaf3e0e6.png)](https://dmitryelj.medium.com/?source=post_page-----c12c6cc2a902--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c12c6cc2a902--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c12c6cc2a902--------------------------------) [Dmitrii Eliuseev](https://dmitryelj.medium.com/?source=post_page-----c12c6cc2a902--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F65c1f6ba75db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felectric-cars-in-the-netherlands-exploratory-data-analysis-with-python-and-sqlalchemy-part-2-c12c6cc2a902&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=post_page-65c1f6ba75db----c12c6cc2a902---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c12c6cc2a902--------------------------------) · 17 分钟阅读 · 2023 年 3 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc12c6cc2a902&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felectric-cars-in-the-netherlands-exploratory-data-analysis-with-python-and-sqlalchemy-part-2-c12c6cc2a902&user=Dmitrii+Eliuseev&userId=65c1f6ba75db&source=-----c12c6cc2a902---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc12c6cc2a902&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Felectric-cars-in-the-netherlands-exploratory-data-analysis-with-python-and-sqlalchemy-part-2-c12c6cc2a902&source=-----c12c6cc2a902---------------------bookmark_footer-----------)![](../Images/eb70cab9e1b8d3ded4a77f3599684a30.png)

Smart EQ 汽车，图片来源 [https://en.wikipedia.org/wiki/Smart_electric_drive](https://en.wikipedia.org/wiki/Smart_electric_drive)

什么时候注册了第一辆电动车？（剧透：它的注册时间比大多数人想象的要早得多。）哪辆车更贵，电动保时捷还是捷豹？探索性数据分析（EDA）不仅是构建每个数据管道的重要部分，而且是一个非常有趣的过程。在[第一部分](/electric-cars-in-the-netherlands-exploratory-data-analysis-with-python-d01477949984)中，我使用 Python 和 Pandas 分析了 RDW（荷兰车辆管理局）数据集，其中一个挑战是数据集的巨大（约 10 GB）体积。作为解决方案，我指定了需要在 Pandas 中加载的列列表。这样做有效，但如果数据集更大，内存仍然不够以容纳所有数据，或者数据集放置在远程数据库中怎么办？在本文中，我将展示如何使用 SQLAlchemy 进行类似的分析。这将允许使用 SQL 进行“重型”数据处理，而不需要将所有数据加载到 Pandas 中。

开始吧。

## 正在加载数据

**RDW**（“Rijks Dienst Wegverkeer”，[https://www.rdw.nl](https://www.rdw.nl/)）是一个荷兰组织，负责机动车辆和驾驶执照的审批与登记。
