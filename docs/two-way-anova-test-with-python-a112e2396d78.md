# 双因素 ANOVA 测试，使用 Python

> 原文：[`towardsdatascience.com/two-way-anova-test-with-python-a112e2396d78?source=collection_archive---------5-----------------------#2023-01-05`](https://towardsdatascience.com/two-way-anova-test-with-python-a112e2396d78?source=collection_archive---------5-----------------------#2023-01-05)

## 完整的双因素 ANOVA 测试入门指南（附代码！）

[](https://chaodeyu.medium.com/?source=post_page-----a112e2396d78--------------------------------)![Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----a112e2396d78--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a112e2396d78--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a112e2396d78--------------------------------) [Chao De-Yu](https://chaodeyu.medium.com/?source=post_page-----a112e2396d78--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5b7be08f8f4c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-way-anova-test-with-python-a112e2396d78&user=Chao+De-Yu&userId=5b7be08f8f4c&source=post_page-5b7be08f8f4c----a112e2396d78---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a112e2396d78--------------------------------) ·6 分钟阅读·2023 年 1 月 5 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa112e2396d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-way-anova-test-with-python-a112e2396d78&user=Chao+De-Yu&userId=5b7be08f8f4c&source=-----a112e2396d78---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa112e2396d78&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftwo-way-anova-test-with-python-a112e2396d78&source=-----a112e2396d78---------------------bookmark_footer-----------)![](img/d896358a9cda32130ebea2395752b3f5.png)

图片由 [Sergey Pesterev](https://unsplash.com/@sickle) 提供，来自 [Unsplash](https://unsplash.com)

ANOVA 测试旨在检验三组或更多组之间是否存在统计学显著差异。常用的 ANOVA（方差分析）有两种类型，`**单因素 ANOVA 测试**` 和 `**双因素 ANOVA 测试**`。唯一的区别在于影响因变量的自变量数量。

# 双因素 ANOVA

双因素 ANOVA 是对 单因素 ANOVA 的扩展，旨在检验 `**两个不同的分类自变量或两个独立因素**` 对 `**一个连续因变量**` 的影响。

双因素方差分析不仅旨在检验每个独立因素的主要效应，还测试两个因素是否相互影响从而影响因变量，即两个独立因素之间是否存在交互作用。[2]

方差分析使用 F 检验，这是一种组内比较检验，用于统计显著性。它比较不同因素下每组均值的方差（因素 A、因素 B、因素 A 与因素 B 之间的交互作用）与因变量的总体方差。最后，根据 F 检验统计量做出结论。

# 平方和（SS）
