# 使用维基百科生成知识图谱

> 原文：[`towardsdatascience.com/generating-knowledge-graphs-with-wikipedia-ec17030a40f6?source=collection_archive---------3-----------------------#2023-01-01`](https://towardsdatascience.com/generating-knowledge-graphs-with-wikipedia-ec17030a40f6?source=collection_archive---------3-----------------------#2023-01-01)

## Python 快速生成知识图谱指南

[](https://jyesr.medium.com/?source=post_page-----ec17030a40f6--------------------------------)![Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----ec17030a40f6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ec17030a40f6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec17030a40f6--------------------------------) [Jye Sawtell-Rickson](https://jyesr.medium.com/?source=post_page-----ec17030a40f6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F74d976cb1305&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-knowledge-graphs-with-wikipedia-ec17030a40f6&user=Jye+Sawtell-Rickson&userId=74d976cb1305&source=post_page-74d976cb1305----ec17030a40f6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ec17030a40f6--------------------------------) · 5 分钟阅读 · 2023 年 1 月 1 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fec17030a40f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-knowledge-graphs-with-wikipedia-ec17030a40f6&user=Jye+Sawtell-Rickson&userId=74d976cb1305&source=-----ec17030a40f6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fec17030a40f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgenerating-knowledge-graphs-with-wikipedia-ec17030a40f6&source=-----ec17030a40f6---------------------bookmark_footer-----------)![](img/946ca13307226697062b4dccf607ae8d.png)

图片由 [DeepMind](https://unsplash.com/@deepmind?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/LIlsk-UFVxk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)。

知识图谱使我们能够理解不同知识点之间的关系，从而对一个领域或主题有一个广泛的理解。这些图谱帮助我们辨别单个知识点如何汇聚成更大的全貌。显然，构建和可视化知识图谱可以成为许多领域的有效方法。

在本文中，我们描述了利用最大的公开可用知识图谱——Wikipedia——生成新知识图谱的过程。我们将通过 Python 完全自动化生成过程，从而为任何感兴趣的领域创建一个可扩展的生成知识图谱的方法。

> 如果你想跟着操作，可以在[Google Colab 上找到完整的笔记本](https://colab.research.google.com/drive/1iKsJtRY-7gX_pGAHT2Cu3e75b3LztU63?usp=sharing)。

# 方法

我们的方法如下：

+   🔌 使用 Wikipedia API 下载与术语相关的信息

+   🔁 迭代多个术语以构建知识库

+   🔝 根据术语的‘重要性’进行排名

+   🌐 使用 networkx 库可视化知识图谱
