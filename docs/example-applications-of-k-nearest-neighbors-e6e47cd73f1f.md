# K-最近邻的示例应用

> 原文：[`towardsdatascience.com/example-applications-of-k-nearest-neighbors-e6e47cd73f1f?source=collection_archive---------2-----------------------#2023-08-04`](https://towardsdatascience.com/example-applications-of-k-nearest-neighbors-e6e47cd73f1f?source=collection_archive---------2-----------------------#2023-08-04)

## 为什么简单算法比你想象的更实用

[](https://medium.com/@anthonybaum?source=post_page-----e6e47cd73f1f--------------------------------)![Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----e6e47cd73f1f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e6e47cd73f1f--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e6e47cd73f1f--------------------------------) [Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----e6e47cd73f1f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad510de786e8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexample-applications-of-k-nearest-neighbors-e6e47cd73f1f&user=Anthony+Baum&userId=ad510de786e8&source=post_page-ad510de786e8----e6e47cd73f1f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e6e47cd73f1f--------------------------------) ·6 分钟阅读·2023 年 8 月 4 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe6e47cd73f1f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexample-applications-of-k-nearest-neighbors-e6e47cd73f1f&user=Anthony+Baum&userId=ad510de786e8&source=-----e6e47cd73f1f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe6e47cd73f1f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fexample-applications-of-k-nearest-neighbors-e6e47cd73f1f&source=-----e6e47cd73f1f---------------------bookmark_footer-----------)![](img/14d5744cbfae5ab01b4a57bc54341e0a.png)

照片由 [Brooke Cagle](https://unsplash.com/@brookecagle?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

我第一个机器学习算法是 K-最近邻（KNN）模型。这对于初学者来说很有意义——直观、易于理解，甚至可以在不使用专门包的情况下实现。

因为对初学者来说它很有意义，当向不熟悉机器学习的人解释时也非常有意义。我无法用言语表达让一群持怀疑态度的人更容易接受 KNN 方法，而不是一个黑箱随机森林的感受。

这是一种被低估的建模方法，它在转向更复杂的算法之前，作为一个优秀的基准模型，并且对于许多使用案例，你可能会发现更复杂的算法的时间和成本实际上不值得。

为了激发你的建模灵感，这里有三个 KNN 的示例应用，你可能会发现，在现实世界中获得的结果远比你预期的要好。

# 市场营销组合建模（MMM）

我从事市场营销工作，我在 MMM 系统中的工作通常涉及识别可以改善活动表现和/或扩展活动以覆盖更多人群的营销渠道。从高层次来看，这就是…
