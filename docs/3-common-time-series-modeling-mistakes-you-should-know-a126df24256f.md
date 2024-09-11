# 你应该知道的 3 个常见时间序列建模错误

> 原文：[`towardsdatascience.com/3-common-time-series-modeling-mistakes-you-should-know-a126df24256f?source=collection_archive---------4-----------------------#2023-07-19`](https://towardsdatascience.com/3-common-time-series-modeling-mistakes-you-should-know-a126df24256f?source=collection_archive---------4-----------------------#2023-07-19)

## 常见错误及避免方法，附实用示例

[](https://medium.com/@anthonybaum?source=post_page-----a126df24256f--------------------------------)![Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----a126df24256f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a126df24256f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a126df24256f--------------------------------) [Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----a126df24256f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad510de786e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-common-time-series-modeling-mistakes-you-should-know-a126df24256f&user=Anthony+Baum&userId=ad510de786e8&source=post_page-ad510de786e8----a126df24256f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a126df24256f--------------------------------) ·7 分钟阅读·2023 年 7 月 19 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa126df24256f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-common-time-series-modeling-mistakes-you-should-know-a126df24256f&user=Anthony+Baum&userId=ad510de786e8&source=-----a126df24256f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa126df24256f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2F3-common-time-series-modeling-mistakes-you-should-know-a126df24256f&source=-----a126df24256f---------------------bookmark_footer-----------)![](img/be529a863e258d3c61eb61b9b54f45cd.png)

图片由 [Diggity Marketing](https://unsplash.com/@diggitymarketing?utm_source=medium&utm_medium=referral) 提供，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我自己也遇到过很多次 — 运行某些模型训练代码后，当错误评分结果非常好时感到惊讶。过于惊人。深入检查特征工程代码时，发现有一个计算将未来数据纳入了训练数据中，修正特征后，这些均方误差又回到了现实中。现在那个白板在哪里呢……

时间序列问题有许多独特的陷阱。幸运的是，通过一些细心和一点练习，你会在将*from sklearn import*输入到你的笔记本之前就能考虑到这些陷阱。以下是需要注意的三件事，以及你可能遇到它们的一些场景。

# 前瞻性偏差

这几乎肯定是你在处理时间序列时遇到的第一个陷阱，而且是我在入门级投资组合中见到的最常见的问题（看着你，普通的股票市场预测项目）。好消息是，这通常是最容易避免的问题。

**问题：** 简而言之，前瞻性偏差是指当你的模型使用了现实中无法获得的未来数据进行训练时。
