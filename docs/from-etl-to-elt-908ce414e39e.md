# 从 ETL 过渡到 ELT

> 原文：[`towardsdatascience.com/from-etl-to-elt-908ce414e39e?source=collection_archive---------8-----------------------#2023-12-06`](https://towardsdatascience.com/from-etl-to-elt-908ce414e39e?source=collection_archive---------8-----------------------#2023-12-06)

## 云计算和分析工程如何推动从 ETL 到 ELT 的过渡

[](https://gmyrianthous.medium.com/?source=post_page-----908ce414e39e--------------------------------)![Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----908ce414e39e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----908ce414e39e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----908ce414e39e--------------------------------) [Giorgos Myrianthous](https://gmyrianthous.medium.com/?source=post_page-----908ce414e39e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F76c21e75463a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-etl-to-elt-908ce414e39e&user=Giorgos+Myrianthous&userId=76c21e75463a&source=post_page-76c21e75463a----908ce414e39e---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----908ce414e39e--------------------------------) ·7 min read·2023 年 12 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F908ce414e39e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-etl-to-elt-908ce414e39e&user=Giorgos+Myrianthous&userId=76c21e75463a&source=-----908ce414e39e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F908ce414e39e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-etl-to-elt-908ce414e39e&source=-----908ce414e39e---------------------bookmark_footer-----------)![](img/0ac0bf0fce07c34df7fbef6780fafd48.png)

图像通过 [DALL-E](https://labs.openai.com/s/R6GmBJ0EmCXudKsPuIGJBBjC) 生成

ETL（提取-转换-加载）和 ELT（提取-加载-转换）是数据工程领域中常用的两个术语，特别是在数据摄取和转换的背景下。

尽管这些术语经常被互换使用，但它们指代的概念略有不同，对数据管道的设计有不同的影响。

在这篇文章中，我们将澄清 ETL 和 ELT 过程的定义，概述它们之间的差异，并讨论它们对工程师和数据团队的优势和劣势。

最重要的是，我将描述现代数据团队组成的近期变化如何影响了 ETL 与 ELT 之战的格局。

# 独立理解提取、加载和转换

比较 ETL 和 ELT 时，主要的关注点显然是提取、加载和转换步骤在数据管道中执行的顺序。

现在，让我们忽略这个执行顺序，专注于实际术语，讨论每个单独步骤应该是什么…
