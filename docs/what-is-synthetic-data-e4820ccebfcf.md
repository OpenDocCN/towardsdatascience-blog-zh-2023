# 什么是合成数据？

> 原文：[https://towardsdatascience.com/what-is-synthetic-data-e4820ccebfcf?source=collection_archive---------3-----------------------#2023-06-30](https://towardsdatascience.com/what-is-synthetic-data-e4820ccebfcf?source=collection_archive---------3-----------------------#2023-06-30)

## 使数据有用

## 各种伪数据物种的实地指南：第一部分

[](https://kozyrkov.medium.com/?source=post_page-----e4820ccebfcf--------------------------------)[![Cassie Kozyrkov](../Images/ad18dd12979a4a3ec130bdf8b889af23.png)](https://kozyrkov.medium.com/?source=post_page-----e4820ccebfcf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e4820ccebfcf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e4820ccebfcf--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----e4820ccebfcf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2fccb851bb5e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-synthetic-data-e4820ccebfcf&user=Cassie+Kozyrkov&userId=2fccb851bb5e&source=post_page-2fccb851bb5e----e4820ccebfcf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e4820ccebfcf--------------------------------) ·6分钟阅读·2023年6月30日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe4820ccebfcf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-synthetic-data-e4820ccebfcf&source=-----e4820ccebfcf---------------------bookmark_footer-----------)

***合成数据***，直白点说，就是伪数据。也就是说，这些数据并非实际来自你感兴趣的***群体***。（“群体”是[数据科学](http://bit.ly/quaesita_datascim)中的一个术语，我在[这里](http://bit.ly/quaesita_popwrong)解释了它。）这些是你计划当作*如同*来自你希望它来自的地方/群体的数据。（实际上并非如此。）

> 合成数据，直白点说，就是伪数据。

*人工数据、合成数据、虚假数据* 和 [*模拟数据*](http://bit.ly/quaesita_sims) 是同义词，只是在不同的时期有稍微不同的流行程度，因此它们带有来自不同年代的诗意内涵。如今，时髦的人更喜欢 ***合成数据*** 这一流行词，或许是因为投资者需要相信有些新东西被发明出来，而不是重新发现。而这里确实有一些略微新的东西，但（在我看来）还不够新到让所有旧观念变得无关紧要。

让我们深入探讨吧！

![](../Images/9133b114590c5fbddf074fcc5d7a5e53.png)

一些合成数字！所有图像权利归作者所有。

*（注：此帖中的链接会带你到同一作者的解释页面。）*

# 无限可能性

如果你像我一样经历过 *高级概率与测度理论* 的研究生课程（我的治疗师和...
