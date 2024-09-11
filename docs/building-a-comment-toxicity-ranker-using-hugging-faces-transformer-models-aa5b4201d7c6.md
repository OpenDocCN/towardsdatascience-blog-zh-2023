# 使用 Hugging Face 的 Transformer 模型构建评论毒性排名器

> 原文：[`towardsdatascience.com/building-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6?source=collection_archive---------5-----------------------#2023-08-06`](https://towardsdatascience.com/building-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6?source=collection_archive---------5-----------------------#2023-08-06)

## 跟进 NLP 和 LLM（第一部分）

[](https://medium.com/@jacky.kaub?source=post_page-----aa5b4201d7c6--------------------------------)![Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----aa5b4201d7c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa5b4201d7c6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa5b4201d7c6--------------------------------) [Jacky Kaub](https://medium.com/@jacky.kaub?source=post_page-----aa5b4201d7c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7ccb7065ef90&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6&user=Jacky+Kaub&userId=7ccb7065ef90&source=post_page-7ccb7065ef90----aa5b4201d7c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa5b4201d7c6--------------------------------) · 18 分钟阅读 · 2023 年 8 月 6 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faa5b4201d7c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6&user=Jacky+Kaub&userId=7ccb7065ef90&source=-----aa5b4201d7c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faa5b4201d7c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-a-comment-toxicity-ranker-using-hugging-faces-transformer-models-aa5b4201d7c6&source=-----aa5b4201d7c6---------------------bookmark_footer-----------)![](img/138fd036a0427c78ad23c3b834c93d74.png)

照片由[Brett Jordan](https://unsplash.com/@brett_jordan?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

作为数据科学家，我从未有机会深入探索自然语言处理的最新进展。随着夏季的到来和大型语言模型在年初的新热潮，我决定是时候深入研究这个领域并着手一些小项目了。毕竟，没有比实践更好的学习方式了。

在我的旅程开始时，我发现很难找到那种一步步引导读者，帮助深入理解新的 NLP 模型的内容。这也是我决定开始这一系列新文章的原因。

## 使用 HuggingFace 的 Transformer 模型构建评论毒性排序器

在这篇文章中，我们将深入探讨如何构建一个评论毒性排序器。这个项目的灵感来源于去年在 Kaggle 上举办的[“Jigsaw 评论毒性严重性评级”竞赛](https://www.kaggle.com/competitions/jigsaw-toxic-severity-rating)。

比赛的目标是构建一个模型，能够确定哪条评论（在两个输入评论中）最具毒性。
