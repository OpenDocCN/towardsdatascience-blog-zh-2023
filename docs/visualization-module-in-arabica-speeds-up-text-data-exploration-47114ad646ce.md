# Arabica 中的可视化模块加速了文本数据探索

> 原文：[`towardsdatascience.com/visualization-module-in-arabica-speeds-up-text-data-exploration-47114ad646ce?source=collection_archive---------18-----------------------#2023-01-09`](https://towardsdatascience.com/visualization-module-in-arabica-speeds-up-text-data-exploration-47114ad646ce?source=collection_archive---------18-----------------------#2023-01-09)

## Arabica 现在提供 unigram、bigram 和 trigram 词云、热图以及折线图，以进一步加速时间序列文本数据分析

[](https://petrkorab.medium.com/?source=post_page-----47114ad646ce--------------------------------)![Petr Korab](https://petrkorab.medium.com/?source=post_page-----47114ad646ce--------------------------------)[](https://towardsdatascience.com/?source=post_page-----47114ad646ce--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----47114ad646ce--------------------------------) [Petr Korab](https://petrkorab.medium.com/?source=post_page-----47114ad646ce--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13a053cbaad9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualization-module-in-arabica-speeds-up-text-data-exploration-47114ad646ce&user=Petr+Korab&userId=13a053cbaad9&source=post_page-13a053cbaad9----47114ad646ce---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----47114ad646ce--------------------------------) ·6 分钟阅读·2023 年 1 月 9 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F47114ad646ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualization-module-in-arabica-speeds-up-text-data-exploration-47114ad646ce&user=Petr+Korab&userId=13a053cbaad9&source=-----47114ad646ce---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F47114ad646ce&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvisualization-module-in-arabica-speeds-up-text-data-exploration-47114ad646ce&source=-----47114ad646ce---------------------bookmark_footer-----------)![](img/77cc4c006de9eb9ca534fe5764e9f6a4.png)

图 1\. Bigram 词云，图片由作者提供。

# 介绍

Arabica 是一个用于探索性文本数据分析的 Python 库，重点关注时间序列视角的文本。它反映了许多文本数据集作为时间上重复观察的经验现实。**时间序列文本数据**包括新闻文章标题、研究文章摘要和元数据、产品评论、社交网络通信等。[**Arabica**](https://pypi.org/project/arabica/)通过提供这些方法来简化对这些数据集的探索性分析（EDA）：

+   **arabica_freq:** 描述性 n-gram 分析和时间序列 n-gram 分析，用于基于 n-gram 的文本数据集 EDA

+   **cappuccino:** 用于数据的可视化探索。

本文介绍了**Cappuccino**，Arabica 用于时间序列文本数据的探索性分析的可视化模块。请阅读[文档](https://arabica.readthedocs.io/en/latest/index.html)以及教程这里以获得对 Arabica 的一般介绍。

***EDIT Jan 2023****: Arabica 已更新。查看[***文档***](https://arabica.readthedocs.io/en/latest/index.html) *以获取完整的参数列表。*
