# 常见的 AB 测试错误。第二卷

> 原文：[https://towardsdatascience.com/common-ab-testing-mistakes-vol-2-3f3040a65e8b?source=collection_archive---------15-----------------------#2023-04-13](https://towardsdatascience.com/common-ab-testing-mistakes-vol-2-3f3040a65e8b?source=collection_archive---------15-----------------------#2023-04-13)

## 让我们从错误中学习吧！

[](https://markeltsefon.medium.com/?source=post_page-----3f3040a65e8b--------------------------------)[![Mark Eltsefon](../Images/5ab4cccd496f73cd155bbb253f85ec4d.png)](https://markeltsefon.medium.com/?source=post_page-----3f3040a65e8b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3f3040a65e8b--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3f3040a65e8b--------------------------------) [Mark Eltsefon](https://markeltsefon.medium.com/?source=post_page-----3f3040a65e8b--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88f461f6049a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcommon-ab-testing-mistakes-vol-2-3f3040a65e8b&user=Mark+Eltsefon&userId=88f461f6049a&source=post_page-88f461f6049a----3f3040a65e8b---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3f3040a65e8b--------------------------------) · 4 分钟阅读 · 2023年4月13日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3f3040a65e8b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcommon-ab-testing-mistakes-vol-2-3f3040a65e8b&source=-----3f3040a65e8b---------------------bookmark_footer-----------)

一年前，我发表了一篇关于 AB 测试中常见错误的 [文章](https://medium.com/towards-data-science/common-mistakes-during-a-b-testing-bdb9eefdc7f0)。许多人对实验挑战及其克服方法非常感兴趣。因此，我决定发表一篇关于人们常犯的下三个错误的文章。

避免这些常见错误，我们可以确保实验的可靠性、有效性和信息性，*从而最终实现更好的决策和更成功的结果*。

![](../Images/5fcc613a06f202706f62e105f24a3f48.png)

图片由 [Santa Barbara](https://unsplash.com/@barbaris778?utm_source=medium&utm_medium=referral) 提供，[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 将所需样本量乘以假设数量

有一个众所周知的公式来计算样本量。

它考虑了指标的方差、显著性水平、检验的效能和 MDE（最小可检测效应）。

然而，在进行多重假设检验时，人们常常犯一个错误，就是简单地用组数替换数字“2”

这是一种正确的方法吗？不完全正确。假设数量的增加会导致 I 型错误率的膨胀，因此我们需要控制 I 型错误以及显著性水平。为了控制它，通常使用 Bonferroni 校正。主要思想是将 I 型错误除以假设的数量…
