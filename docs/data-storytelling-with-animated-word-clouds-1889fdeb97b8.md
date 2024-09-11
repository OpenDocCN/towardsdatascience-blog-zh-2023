# 使用动态词云讲述数据故事

> 原文：[https://towardsdatascience.com/data-storytelling-with-animated-word-clouds-1889fdeb97b8?source=collection_archive---------8-----------------------#2023-11-22](https://towardsdatascience.com/data-storytelling-with-animated-word-clouds-1889fdeb97b8?source=collection_archive---------8-----------------------#2023-11-22)

## 动态词云将经典词云转变为一种动态可视化效果。了解如何在Python中讲述数据故事。

[](https://petrkorab.medium.com/?source=post_page-----1889fdeb97b8--------------------------------)[![Petr Korab](../Images/9f3afb4b8985584981220e30f18e3b69.png)](https://petrkorab.medium.com/?source=post_page-----1889fdeb97b8--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1889fdeb97b8--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1889fdeb97b8--------------------------------) [Petr Korab](https://petrkorab.medium.com/?source=post_page-----1889fdeb97b8--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F13a053cbaad9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-storytelling-with-animated-word-clouds-1889fdeb97b8&user=Petr+Korab&userId=13a053cbaad9&source=post_page-13a053cbaad9----1889fdeb97b8---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1889fdeb97b8--------------------------------) · 5分钟阅读 · 2023年11月22日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1889fdeb97b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-storytelling-with-animated-word-clouds-1889fdeb97b8&user=Petr+Korab&userId=13a053cbaad9&source=-----1889fdeb97b8---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1889fdeb97b8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdata-storytelling-with-animated-word-clouds-1889fdeb97b8&source=-----1889fdeb97b8---------------------bookmark_footer-----------)

# 介绍

动态词云以**视频文件中的一系列图像**展示n-gram（连续的文本样本项序列）的绝对频率。它对在源文本中出现频率较高的词汇给予更大的重视。n-gram显示的越大和越粗体，表示它在文本中出现的频率越高。它建立在经典词云的直观逻辑基础上，并为可视化增加了时间视角。

由于现在收集了大量文本数据作为多个时间段的文本观察，如何可视化数据随时间的变化成为了一个特殊的挑战。与其为多个不同时间段制作汇总表或图表，不如准备一个 MP4 视频来讲述故事，吸引观众，并为演示增添“哇”效应。

本文将描述如何在 Python 中从文本数据生成动画词云。以下是[AnimatedWordCloud library](https://pypi.org/project/AnimatedWordCloud/)的一些独特功能：
