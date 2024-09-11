# 发现异常值后该如何处理

> 原文：[`towardsdatascience.com/what-to-do-with-outliers-once-you-find-them-8e4d3d32f228?source=collection_archive---------16-----------------------#2023-02-15`](https://towardsdatascience.com/what-to-do-with-outliers-once-you-find-them-8e4d3d32f228?source=collection_archive---------16-----------------------#2023-02-15)

[](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)![Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8e4d3d32f228--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e4d3d32f228--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-to-do-with-outliers-once-you-find-them-8e4d3d32f228&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----8e4d3d32f228---------------------post_header-----------) 发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8e4d3d32f228--------------------------------) · 阅读时间 5 分钟·2023 年 2 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8e4d3d32f228&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-to-do-with-outliers-once-you-find-them-8e4d3d32f228&user=Bex+T.&userId=39db050c2ac2&source=-----8e4d3d32f228---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8e4d3d32f228&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-to-do-with-outliers-once-you-find-them-8e4d3d32f228&source=-----8e4d3d32f228---------------------bookmark_footer-----------)![](img/29afd747b095ce34bcc4b024ce5a9a8b.png)

图片由[Ralf Kunze](https://pixabay.com/users/realworkhard-23566/?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=199054)提供，来自[Pixabay](https://pixabay.com//?utm_source=link-attribution&utm_medium=referral&utm_campaign=image&utm_content=199054)

异常值检测只是故事的一部分。真正的挑战在于弄清楚如何处理这些异常值。仅仅将异常值忽略掉是很容易的，但需要考虑的细节和因素还有很多。

盲目移除它们可能会带来不必要的后果。找到某种类型的异常值可能暗示数据存在更严重的问题，而检测到另一种类型的异常值可能根本不是问题。因此，重要的是不要做出仓促的决定，因为这可能会导致数据质量下降或在后续的机器学习模型中产生偏差。

今天，我们将从两个角度来解析这个问题。首先，我们将根据异常值存在的原因来看适当的行动步骤，然后我们会讨论根据数据集中异常值的数量应该怎么做。让我们开始吧！

查看这一系列异常值检测的早期部分，我们更注重代码：

![Bex T.](img/3f2835593290cd528199731ac48cf262.png)

[Bex T.](https://ibexorigin.medium.com/?source=post_page-----8e4d3d32f228--------------------------------)

## BEXGBoost - 异常值检测

[查看列表](https://ibexorigin.medium.com/list/bexgboost-outlier-detection-16cfc06cd0f3?source=post_page-----8e4d3d32f228--------------------------------)3 个故事![](img/04febaa835c6275808dbb5c9cee9e16b.png)![](img/5ec34d780dee50a815538e11ee75c3f7.png)![](img/662f87df807ee91113997c66cccd58a0.png)

## 1\. 存在原因：错误
