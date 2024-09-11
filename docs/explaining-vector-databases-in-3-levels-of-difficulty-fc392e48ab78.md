# 以三种难度级别解释向量数据库

> 原文：[`towardsdatascience.com/explaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78?source=collection_archive---------0-----------------------#2023-07-04`](https://towardsdatascience.com/explaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78?source=collection_archive---------0-----------------------#2023-07-04)

## 从新手到专家：揭开向量数据库的神秘面纱

[](https://medium.com/@iamleonie?source=post_page-----fc392e48ab78--------------------------------)![Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----fc392e48ab78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fc392e48ab78--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc392e48ab78--------------------------------) [Leonie Monigatti](https://medium.com/@iamleonie?source=post_page-----fc392e48ab78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a38da70d8dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexplaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78&user=Leonie+Monigatti&userId=3a38da70d8dc&source=post_page-3a38da70d8dc----fc392e48ab78---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fc392e48ab78--------------------------------) ·8 分钟阅读·2023 年 7 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffc392e48ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexplaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78&user=Leonie+Monigatti&userId=3a38da70d8dc&source=-----fc392e48ab78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffc392e48ab78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexplaining-vector-databases-in-3-levels-of-difficulty-fc392e48ab78&source=-----fc392e48ab78---------------------bookmark_footer-----------)![](img/328d368e5d803b2e02d900d2bda2437e.png)

向量空间（图像由作者手绘）

向量数据库最近受到很多关注，许多向量数据库初创公司筹集了数百万美元的资金。

你很可能已经听说过它们，但直到现在才真正关注——至少，我猜这就是你现在在这里的原因……

如果你只想要简短的回答，那我们直接进入正题：

# 定义：什么是向量数据库？

向量数据库是一种存储和管理非结构化数据（如文本、图像或音频）的数据库，它将这些数据转换为向量嵌入（高维向量），以便快速查找和检索相似对象。

如果这个定义让你更困惑，那我们一步一步来。本文的灵感来源于[WIRED 的“5 Levels”视频系列](https://www.wired.com/video/series/5-levels)，并将向量数据库的概念分解为以下三个难度级别：

+   像我五岁一样解释

+   向数字原住民和科技爱好者解释向量数据库

+   向工程师和数据专业人士解释向量数据库
