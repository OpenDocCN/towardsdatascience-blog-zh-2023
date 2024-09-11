# 什么是时间序列单位根？

> 原文：[https://towardsdatascience.com/what-is-a-time-series-unit-root-dba24fa099f4?source=collection_archive---------2-----------------------#2023-10-11](https://towardsdatascience.com/what-is-a-time-series-unit-root-dba24fa099f4?source=collection_archive---------2-----------------------#2023-10-11)

## 回答时间序列分析中一个非常重要的问题

[](https://medium.com/@egorhowell?source=post_page-----dba24fa099f4--------------------------------)[![Egor Howell](../Images/1f796e828f1625440467d01dcc3e40cd.png)](https://medium.com/@egorhowell?source=post_page-----dba24fa099f4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----dba24fa099f4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----dba24fa099f4--------------------------------) [Egor Howell](https://medium.com/@egorhowell?source=post_page-----dba24fa099f4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1cac491223b2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-time-series-unit-root-dba24fa099f4&user=Egor+Howell&userId=1cac491223b2&source=post_page-1cac491223b2----dba24fa099f4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----dba24fa099f4--------------------------------) · 7分钟阅读 · 2023年10月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fdba24fa099f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-time-series-unit-root-dba24fa099f4&user=Egor+Howell&userId=1cac491223b2&source=-----dba24fa099f4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fdba24fa099f4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-time-series-unit-root-dba24fa099f4&source=-----dba24fa099f4---------------------bookmark_footer-----------)![](../Images/c59558f7b3568c61d07e2091c6e32d09.png)

图片由 [Icons8 Team](https://unsplash.com/@icons8?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 背景

[**单位根**](https://en.wikipedia.org/wiki/Unit_root) 在时间序列和预测文献中时常出现（请原谅这个双关语）。

[维基百科](https://en.wikipedia.org/wiki/Unit_root) 对单位根的定义为：

> “在概率论和统计学中，**单位根**是一些随机过程（如随机游走）的一个特征，它可能会在涉及时间序列模型的统计推断中造成问题。如果`1`是该过程特征方程的根，则线性随机过程具有单位根。这种过程是非平稳的，但不总是具有趋势。”

是的，对我来说也没有太多意义……

然而，上述说法过于复杂了，实际上单位根并不是一个过于难以理解的概念。

# 什么是平稳性？

## 概述

要理解单位根，我们首先必须清楚地了解[**平稳性**](https://www.youtube.com/watch?v=621MSxpYv60&t=4s)。我在之前的博客和YouTube视频中已经覆盖了这个话题，但我们将快速回顾一下关键点。
