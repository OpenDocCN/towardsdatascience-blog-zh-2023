# 数据工程书籍

> 原文：[`towardsdatascience.com/data-engineering-books-f373005d53fc`](https://towardsdatascience.com/data-engineering-books-f373005d53fc)

## 逐步学习数据工程的读者文摘

[](https://mshakhomirov.medium.com/?source=post_page-----f373005d53fc--------------------------------)![💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----f373005d53fc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f373005d53fc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f373005d53fc--------------------------------) [💡Mike Shakhomirov](https://mshakhomirov.medium.com/?source=post_page-----f373005d53fc--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f373005d53fc--------------------------------) ·阅读时间 8 分钟·2023 年 11 月 12 日

--

![](img/df3db3d06f1e303478767f804be4672f.png)

照片由 [Tamas Pap](https://unsplash.com/@tamasp?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这个故事中，我想谈谈那些可能对学习数据工程（DE）的人感兴趣的数据工程书籍和资源。我发现市场上很少有书籍能全面系统地解释数据工程这一概念。其中一些书籍在如何使用特定工具和数据平台架构方面很出色，而一些书籍则是我最喜欢的睡前读物：读着非常容易入睡，且极为无聊。还有些书籍适合战略决策，还有些虽然看起来有些过时但仍然有用。希望你会觉得有趣。

*免责声明：此帖可能包含附属链接，这意味着如果你决定通过我的链接购买商品，我会获得佣金，但对你没有任何费用。*

## **1\.** [**使用 Python 进行数据工程**](https://amzn.to/3QSaHp4)

**使用 Python 处理大量数据集以设计数据模型并自动化数据管道**

Paul Crickard, 2020

这是一本适合那些希望学习开源 Apache 工具用于数据工程的书。它涵盖了数据工程的所有关键主题，如数据建模，并提供了大量最常见数据转换的示例。正如书中描述的，它涉及 Python 和数据建模，因此读者将重点关注 ETL 技术，以使用 Python 工具提取、清洗和丰富数据集。它详细解释了 Apache Kafka 和 Apache Spark，但也涵盖了处理文件格式、数据转换和清洗的基础知识。这本书提供了对数据管道部署以及数据环境工作的非常有用的观点。

我与先进 ETL 技术的故事，以补充这本书：

[](/python-for-data-engineers-f3d5db59b6dd?source=post_page-----f373005d53fc--------------------------------) ## 数据工程师的 Python

### 面向初学者的高级 ETL 技术

towardsdatascience.com

## 2\. [**数据工程基础**](https://amzn.to/465BUsB)

作者：乔·瑞斯，马特·豪斯利

发布日期：2022 年 6 月

出版社：O’Reilly Media, Inc.

总体而言，这是一本非常好的书，我相信它是我目前正在编写的书籍的最接近匹配。它涵盖了基础知识，确实非常棒。然而，它没有解释如何成为数据工程师。根据这本书，没有捷径，也没有简单的方式进入这个角色。读者需要投入 2 到 3 年的时间来学习这个特定领域。

> 我喜欢这本书的是它提供了对技术和架构的独立视角。

在这里我们不会看到任何市场营销内容。它非常专注于**第二章**中的数据工程生命周期，并解释了从项目需求收集、管道设计到上线的最佳实践。

这本书完全关于 SQL 和 Python，以及如何使用它们解决实际的数据工程任务。**第四章**介绍了选择合适数据工程技术的框架，非常类似于我在之前的故事中提到的：

[](/data-platform-architecture-types-f255ac6e0b7?source=post_page-----f373005d53fc--------------------------------) ## 数据平台架构类型

### 它在多大程度上满足了你的业务需求？选择的困境。

towardsdatascience.com

总的来说，这是我最喜欢的书籍之一。它不仅涵盖了数据生成、ETL、汇总和清洗的复杂性，还有战略方面的重点，这可能对数据工程经理有用。

## 3\. [数据仓库工具包：维度建模的权威指南](https://amzn.to/473r4om)

建模，第 3 版

作者：拉尔夫·金博尔，玛吉·罗斯

发布日期：2013 年

出版社：Wiley

我记得我在开始使用 Snowflake 时几年前就买了这本书。

> 这本书在 2013 年发布，至今仍适用于许多数据建模场景。

我喜欢这本书的一个特别之处是案例研究。它提供了来自不同行业（如零售、市场营销等）的 20 多个非常有用的场景。它帮助我在高级水平上理解了维度建模和数据仓库设计。基本上，它解释了你需要了解的有关事实表和维度表的一切，以及如何在数据仓库解决方案中运行 ETL。

> 即使现在读起来也非常有趣，可以见证数据仓库平台的发展。

## 4\. [数据网格](https://amzn.to/49A6KwH)

作者：扎马克·德赫加尼

发布日期：2022 年

出版社：Wiley

关于数据网格原则的简介。数据网格和去中心化数据管理无疑是数据工程领域的主要趋势之一。

> **数据网格**定义了当我们有不同的数据领域（公司部门）及其团队和共享数据资源的状态。

我之前在我的一篇关于现代数据工程的文章中写过这个话题。

[](/modern-data-engineering-e202776fb9a9?source=post_page-----f373005d53fc--------------------------------) ## 现代数据工程

### 平台特定工具和高级技术

towardsdatascience.com

这本书适合那些希望了解数据网格设计、策略和架构的读者。书中以逻辑一致的方式解释了数据所有权模型，以超越传统的数据仓库方法，向去中心化和分布式数据平台迈进。

## 5\. [数据管道速查手册：用于分析的数据移动和处理 第 1 版](https://amzn.to/47bbA1G)

作者：詹姆斯·登斯莫尔

格式：Kindle 版

发布于 2021 年 2 月

出版社：O’Reilly Media, Inc.

这是我最喜欢的数据管道书籍之一。某些[Python 和 SQL 代码片段](https://github.com/jamesdensmore/datapipelinesbook)在我的职业生涯中曾非常有用。书中的 Github 代码库演示了如何从外部数据源提取数据并将其转换为数据集。

这本书介绍了一种“构建与购买”方法，这正是数据工程师需要做的。实际上，目前市场上有许多托管的 ETL 解决方案，例如 Stitch、Fivetran 等。书中涵盖了数据管道设计原则，并解释了如何创建强健的数据处理以实现成功的分析。书中从架构角度解释了数据管道设计的许多关键点，同时还涉及现代数据基础设施、数据管道监控和警报等方面。我记得我写过一篇关于数据管道设计模式的文章，提供了类似的观点，并专注于选择合适工具的策略。

[](/data-pipeline-design-patterns-100afa4b93e3?source=post_page-----f373005d53fc--------------------------------) ## 数据管道设计模式

### 选择合适的架构和示例

towardsdatascience.com

## 6\. [现代数据平台架构：大规模企业 Hadoop 指南](https://amzn.to/3QzAOj9)

作者：扬·库尼克、伊恩·巴斯、保罗·威尔金森、拉斯·乔治

发布于 2019 年

出版社：O’Reilly Media, Inc.

这本书在解释 Hadoop 技术方面非常出色。尽管这种技术在中小企业层面并不太流行，但它认为企业级应用仍然可行。读起来很有趣，重点关注实际用例，创建云端和本地的大数据基础设施。我相信它将对负责创建企业级云管道并确保高水平安全性和可用性经验丰富的数据工程师有帮助。

这不是我经常阅读的书籍，但仍然有用，因为它提供了一个被认为早已过时的概述。知道 Hadoop 仍然存在真是太好了。

## 7\. [Spark: 权威指南：简化的大数据处理 第一版](https://amzn.to/46bCSUq)

作者：Bill Chambers, Matei Zaharia

发布于 2018 年

出版社：O’Reilly Media, Inc.

这是我在大数据管道中的 ETL 方面的最爱之一。我们都喜欢 Spark，因为它的前所未有的可扩展性和成本效益。这是一本很好的书，适合想要学习数据湖中可扩展数据处理的初学者和中级用户。它涵盖了一些基本的数据工程概念和使用 Apache Spark 的数据湖数据处理。Apache Spark 被用于许多云产品中，例如 AWS Glue。这使得这本书成为有志数据工程师的绝佳选择。

## 8\. [流处理系统: 大规模数据处理的“什么、哪里、何时以及如何” 第一版](https://amzn.to/3QVXDPi)

作者：Tyler Akidau, Slava Chernyak, Reuven Lax

发布于 2018 年

出版社：O’Reilly Media, Inc.

一本关于最流行的数据管道设计模式之一——流处理的好书。它解释了流数据处理管道及其核心原理。对于数据工程师来说，理解数据管道设计模式的本质并正确应用它们，如批处理数据处理、流处理 ETL 等，非常重要。由于流处理，应用程序可以对新的数据事件作出即时响应。

> *流处理是企业数据的“必备”解决方案。*

这本书在选择正确的数据处理方法和创建接近实时分析管道方面帮助了我很多。通常情况下，流处理并非必需，最终可能会成为一个昂贵的解决方案。

## 9\. [数据讲故事: 商业专业人士的数据可视化指南 第一版](https://amzn.to/3MGiOma)

作者：Cole Nussbaumer Knaflic

发布于 2015 年

出版社：Wiley

一本关于数据可视化技术和商业智能（BI）的好书。尽管 BI 是数据工程的重要部分（反之亦然），但这本书不是职业指南。它解释了数据工程如何补充商业智能。它展示了如何以信息丰富、引人入胜的方式传达数据洞察。这对我的仪表板设计帮助很大。我要把它添加到我的书架上。

## 10\. [流畅的 Python: 清晰、简洁且高效的编程 第二版](https://amzn.to/3QWGZPT)

作者：Luciano Ramalho

发布于 2022 年

出版社：O’Reilly Media, Inc.

另一本我非常珍视的实用 Python 书。Python 是数据工程的重要组成部分，这使得这本书极具实用性。这本书分为五个部分，涵盖了数据工程师在数据管道中可能需要使用的几乎所有内容，即上下文管理器、装饰器、生成器和异步编程。

[](/python-for-data-engineers-f3d5db59b6dd?source=post_page-----f373005d53fc--------------------------------) ## 数据工程师的 Python

### 初学者的高级 ETL 技术

towardsdatascience.com

## 11\. [每个数据工程师都应该知道的 97 件事：专家的集体智慧](https://amzn.to/475joC0)

作者：托比亚斯·梅西

发布于 2021 年

出版社：O’Reilly Media, Inc.

这是一本很棒的书，它确认了现在数据工程师的需求很高。这本书汇集了数据工程师的经验。许多作者为在大数据和人工智能领域取得显著成功的公司设计了数据管道和 ETL 过程。很高兴看到人们仍然愿意分享他们的知识，并解释他们是如何解决具有挑战性的 ETL 问题的。这本书包含 97 个用例，几乎每个数据工程师都可以用于数据处理和数据管道设计。**我喜欢每天读一本。**

## 结论

**如果你是一个学习者或有志于获取新数据技能的数据爱好者，那么在云端有很多免费的机会。** 我强烈建议你在一个云平台供应商那里注册账户，开始学习市场上可用的数据工程工具。许多平台提供免费层服务，探索最新的数据工程进展不应该花费任何费用。只要在使用免费层工具时注意账单就可以了。本文中提供的书籍概述将支持你的学习曲线。大多数书籍假设读者能够熟练使用 JSON、SQL、REST API，并了解 Python 编程基础。这与我之前在一篇文章中写到的数据工程技能集相符。我希望你会觉得这很有用。

[](/how-to-become-a-data-engineer-c0319cb226c2?source=post_page-----f373005d53fc--------------------------------) ## 如何成为数据工程师

### 2024 年的初学者捷径

towardsdatascience.com

## 推荐阅读：

1.  [`medium.com/towards-data-science/python-for-data-engineers-f3d5db59b6dd`](https://medium.com/towards-data-science/python-for-data-engineers-f3d5db59b6dd)

1.  `towardsdatascience.com/data-platform-architecture-types-f255ac6e0b7`

1.  `towardsdatascience.com/modern-data-engineering-e202776fb9a9`

1.  `towardsdatascience.com/data-pipeline-design-patterns-100afa4b93e3`

1.  `towardsdatascience.com/how-to-become-a-data-engineer-c0319cb226c2`
