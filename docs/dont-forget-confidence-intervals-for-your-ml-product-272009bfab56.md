# 不要忘记为你的 ML 产品设置置信区间

> 原文：[`towardsdatascience.com/dont-forget-confidence-intervals-for-your-ml-product-272009bfab56?source=collection_archive---------5-----------------------#2023-10-10`](https://towardsdatascience.com/dont-forget-confidence-intervals-for-your-ml-product-272009bfab56?source=collection_archive---------5-----------------------#2023-10-10)

## 机器学习从来不会 100%准确。因此，ML 模型只有在用户理解预测的不确定性时才会有帮助。

[](https://medium.com/@benjamin.thuerer?source=post_page-----272009bfab56--------------------------------)![Benjamin Thürer](https://medium.com/@benjamin.thuerer?source=post_page-----272009bfab56--------------------------------)[](https://towardsdatascience.com/?source=post_page-----272009bfab56--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----272009bfab56--------------------------------) [Benjamin Thürer](https://medium.com/@benjamin.thuerer?source=post_page-----272009bfab56--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcd27eb9661fd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-forget-confidence-intervals-for-your-ml-product-272009bfab56&user=Benjamin+Th%C3%BCrer&userId=cd27eb9661fd&source=post_page-cd27eb9661fd----272009bfab56---------------------post_header-----------) 发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----272009bfab56--------------------------------) ·7 min read·2023 年 10 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F272009bfab56&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-forget-confidence-intervals-for-your-ml-product-272009bfab56&user=Benjamin+Th%C3%BCrer&userId=cd27eb9661fd&source=-----272009bfab56---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F272009bfab56&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-forget-confidence-intervals-for-your-ml-product-272009bfab56&source=-----272009bfab56---------------------bookmark_footer-----------)

几乎每天我们都会发现新的机器学习产品、服务或数据集的发布。这是人工智能的时代，但很少有这些产品告知用户对结果应有多少信心。然而，[研究](https://doi.org/10.1145/3351095.3372852)表明，良好的决策需要了解何时信任 AI，何时不信任。否则，用户需要频繁尝试模型，以了解何时信任模型，何时不信任，并且找出所提供的产品是否对他们有用。

用户采用这种尝试和错误的原则是因为每个模型（无论是基于机器学习还是统计学）都是建立在数据及其不确定性之上的。模型的基础数据并不代表模型所应预测的实际真实情况。否则，如果那个真实情况可用，你根本不需要模型。因此，结果模型只能提供一个估计值，而不是一个真实值。

> 简而言之，机器学习和统计模型的正确性是不确定的，不能总是被信任。

# 示例：预测跨县移动
