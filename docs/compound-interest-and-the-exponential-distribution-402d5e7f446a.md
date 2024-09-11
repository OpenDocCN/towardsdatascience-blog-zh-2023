# 复利与指数分布

> 原文：[`towardsdatascience.com/compound-interest-and-the-exponential-distribution-402d5e7f446a?source=collection_archive---------17-----------------------#2023-05-24`](https://towardsdatascience.com/compound-interest-and-the-exponential-distribution-402d5e7f446a?source=collection_archive---------17-----------------------#2023-05-24)

## 你的抵押贷款是无记忆的，指数分布也是如此

[](https://medium.com/@rohitpandey576?source=post_page-----402d5e7f446a--------------------------------)![Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----402d5e7f446a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----402d5e7f446a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----402d5e7f446a--------------------------------) [Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----402d5e7f446a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa743c5fec8cd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcompound-interest-and-the-exponential-distribution-402d5e7f446a&user=Rohit+Pandey&userId=a743c5fec8cd&source=post_page-a743c5fec8cd----402d5e7f446a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----402d5e7f446a--------------------------------) ·8 min read·2023 年 5 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F402d5e7f446a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcompound-interest-and-the-exponential-distribution-402d5e7f446a&user=Rohit+Pandey&userId=a743c5fec8cd&source=-----402d5e7f446a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F402d5e7f446a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcompound-interest-and-the-exponential-distribution-402d5e7f446a&source=-----402d5e7f446a---------------------bookmark_footer-----------)![](img/d0e7c22335bb7bb0318b28dd13bbbf7b.png)

图片由 midjourney 生成，使用开源许可证。

# 欧拉数、指数分布与复利

在时间点上会发生许多有趣的事件。例如，公交车到达车站、高速公路上的事故、足球比赛中的进球。这些模型化这种时间点事件的过程被称为“点过程”。这些过程中的一个重要考虑因素是从一个事件到下一个事件所需的时间。例如，假设你刚刚错过了一辆公交车，你还需要等多久才能等到下一辆？这个时间是一个随机变量，随机变量的选择决定了点过程。对于这个随机变量的一个选择是那些并不特别随机的东西（确定性数字只是随机数字的一种特殊情况）。例如，公交车按时到达，每 10 分钟一次。这听起来可能是最简单的点过程，但还有更简单的情况。当事件之间的时间服从指数分布时（这个过程称为泊松过程），它就会出现。之所以叫做指数分布，是有充分理由的。它与欧拉数*e*和复利有关。在本文中，我们将探讨这一联系。
