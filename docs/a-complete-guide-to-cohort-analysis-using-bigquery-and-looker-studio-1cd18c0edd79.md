# 使用 BigQuery 和 Looker Studio 的 cohort 分析完整指南

> 原文：[https://towardsdatascience.com/a-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79?source=collection_archive---------3-----------------------#2023-03-09](https://towardsdatascience.com/a-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79?source=collection_archive---------3-----------------------#2023-03-09)

## 逐步揭开 cohort 分析的神秘面纱

[](https://medium.com/@damien.azzopardi?source=post_page-----1cd18c0edd79--------------------------------)[![Damien Azzopardi](../Images/4cceb06ac7792c0afcee94cfe9d8385f.png)](https://medium.com/@damien.azzopardi?source=post_page-----1cd18c0edd79--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1cd18c0edd79--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1cd18c0edd79--------------------------------) [Damien Azzopardi](https://medium.com/@damien.azzopardi?source=post_page-----1cd18c0edd79--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd87b6d78019b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79&user=Damien+Azzopardi&userId=d87b6d78019b&source=post_page-d87b6d78019b----1cd18c0edd79---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1cd18c0edd79--------------------------------) ·5 min read·2023年3月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1cd18c0edd79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79&user=Damien+Azzopardi&userId=d87b6d78019b&source=-----1cd18c0edd79---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1cd18c0edd79&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-complete-guide-to-cohort-analysis-using-bigquery-and-looker-studio-1cd18c0edd79&source=-----1cd18c0edd79---------------------bookmark_footer-----------)![](../Images/92801613a39771c131cf360df1915772.png)

图片来源于 [Hunter Harritt](https://unsplash.com/ja/@hharritt?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/Ype9sdOPdYc?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

承认吧，cohort 分析乍看之下可能让人感到畏惧。然而，它是一个强大的工具，能提供有价值的见解，有时也是可视化数据的唯一正确方式。掌握这些工具将使你在数据分析的旅程中获得**明显的优势**。

但首先，我们所说的阶段分析是什么意思呢？

> 阶段分析是一种研究和比较不同人群随时间变化的方式。

这些群体是通过共同的特征来定义的，比如他们加入服务的日期或首次购买的日期。

阶段分析通常用于分析客户终止与产品或服务关系的比率，这一概念通常被称为“流失”。对于基于订阅的业务，流失代表了在给定期间内取消订阅的客户百分比。

客户流失是一个重要的业务指标，它可能会显著影响收入和增长。虽然高流失率可能表明客户的不满，但相反，低流失率可以证明客户的忠诚和满意。
