# 用简单的、甚至不是线性的时间序列模型赢得胜利

> 原文：[https://towardsdatascience.com/winning-with-simple-not-even-linear-time-series-models-6ece77be22bc?source=collection_archive---------15-----------------------#2023-05-10](https://towardsdatascience.com/winning-with-simple-not-even-linear-time-series-models-6ece77be22bc?source=collection_archive---------15-----------------------#2023-05-10)

## 如果你的数据集较小，以下想法可能会有用

[](https://sarem-seitz.medium.com/?source=post_page-----6ece77be22bc--------------------------------)[![Sarem Seitz](../Images/f833f915a0eb061f47524a67685ba76c.png)](https://sarem-seitz.medium.com/?source=post_page-----6ece77be22bc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6ece77be22bc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6ece77be22bc--------------------------------) [Sarem Seitz](https://sarem-seitz.medium.com/?source=post_page-----6ece77be22bc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8f6d033b1a40&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwinning-with-simple-not-even-linear-time-series-models-6ece77be22bc&user=Sarem+Seitz&userId=8f6d033b1a40&source=post_page-8f6d033b1a40----6ece77be22bc---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6ece77be22bc--------------------------------) · 9分钟阅读 · 2023年5月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6ece77be22bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwinning-with-simple-not-even-linear-time-series-models-6ece77be22bc&user=Sarem+Seitz&userId=8f6d033b1a40&source=-----6ece77be22bc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6ece77be22bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwinning-with-simple-not-even-linear-time-series-models-6ece77be22bc&source=-----6ece77be22bc---------------------bookmark_footer-----------)![](../Images/df6c32f54e340c91d39bddc987d1cca5.png)

图片由 [Thomas Bormans](https://unsplash.com/@thomasbormans?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来自 [Unsplash](https://unsplash.com/de/s/fotos/time?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

**免责声明：** 标题深受 [这场](https://www.youtube.com/watch?v=68ABAU_V8qI&pp=ygUad2lubmluZyB3aXRoIHNpbXBsZSBtb2RlbHM%3D&ref=sarem-seitz.com) 出色演讲的启发。

顾名思义，今天我们要考虑几乎简单的模型。虽然当前趋势指向复杂模型，即使对于时间序列模型，我仍然坚信简单性。特别是当你的数据集较小时，后续的思想可能会很有用。

公平地说，这篇文章对刚开始进行时间序列分析的人来说可能最有价值。其他人应该先查看目录，然后自己决定是否继续阅读。

就我个人而言，我仍然对即使是最简单的时间序列模型可以推进多远感到很感兴趣。接下来的段落展示了一些我在这个话题上逐渐积累的想法和见解。

# 纯**i**.i.d. 噪声模型

我们从最简单的（概率）方法来建模（单变量）时间序列开始。即，我们希望研究普通**i**独立的，**i**同分布的**d**随机性：

这意味着我们所有的观察在任何时间点都遵循相同的分布（**identically**...
