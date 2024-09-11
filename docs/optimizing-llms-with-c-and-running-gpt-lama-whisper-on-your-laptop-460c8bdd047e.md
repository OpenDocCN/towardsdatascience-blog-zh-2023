# 使用 C 优化 LLMs，并在你的笔记本电脑上运行 GPT、Llama 和 Whisper

> 原文：[`towardsdatascience.com/optimizing-llms-with-c-and-running-gpt-lama-whisper-on-your-laptop-460c8bdd047e?source=collection_archive---------3-----------------------#2023-09-23`](https://towardsdatascience.com/optimizing-llms-with-c-and-running-gpt-lama-whisper-on-your-laptop-460c8bdd047e?source=collection_archive---------3-----------------------#2023-09-23)

## 在这第一篇文章中，我们将深入了解`ggml`，这是由 Georgi Gerganov 创建的精彩张量库。它是如何工作的？张量创建过程是怎样的？我们能否从一些简单的例子开始？

[](https://stefanobosisio1.medium.com/?source=post_page-----460c8bdd047e--------------------------------)![Stefano Bosisio](https://stefanobosisio1.medium.com/?source=post_page-----460c8bdd047e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----460c8bdd047e--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----460c8bdd047e--------------------------------) [Stefano Bosisio](https://stefanobosisio1.medium.com/?source=post_page-----460c8bdd047e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff7141087b94&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-llms-with-c-and-running-gpt-lama-whisper-on-your-laptop-460c8bdd047e&user=Stefano+Bosisio&userId=ff7141087b94&source=post_page-ff7141087b94----460c8bdd047e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----460c8bdd047e--------------------------------) ·15 分钟阅读·2023 年 9 月 23 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F460c8bdd047e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-llms-with-c-and-running-gpt-lama-whisper-on-your-laptop-460c8bdd047e&user=Stefano+Bosisio&userId=ff7141087b94&source=-----460c8bdd047e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F460c8bdd047e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimizing-llms-with-c-and-running-gpt-lama-whisper-on-your-laptop-460c8bdd047e&source=-----460c8bdd047e---------------------bookmark_footer-----------)![](img/3de95b94179dd7cec4a2cc9abeefedba.png)

图片由[Aryo Yarahmadi](https://unsplash.com/@aryo_yarahmadi)提供，来源于[Unsplash](https://unsplash.com/photos/ylMP3TetKoQ)

## 目录

1.  实现一个简单的数学函数

    1.1 上下文的定义

    1.2 初始化张量

    1.3 前向计算与计算图

    1.4 编译与运行

1.  本部分的最终说明

1.  支持我的写作

大型语言模型（LLMs）现在到处都在炒作。报纸用大量文字描述一个即将到来的新世界，确保“AI 终于来了”。虽然 LLMs 对我们的生活带来了实际影响，但我们必须保持冷静，批判性地分析整个情况。LLMs 的炒作让我想起了几年前“数据科学家”职位的炒作。回到 2014 年，当我开始攻读博士学位时，我看到数据科学家职位的数量稳步增加，到 2018 年左右达到了峰值。那时，新闻再次炒作，写着：“数据科学家：百万美元职业”或“21 世纪最性感的职业”——这些标题是否听起来很熟悉……
