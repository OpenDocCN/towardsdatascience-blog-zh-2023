# 当你的p值为0.052时，你的决定应该是什么？

> 原文：[https://towardsdatascience.com/what-should-your-decision-be-when-your-p-value-0-052-1d50f9d4d2c5?source=collection_archive---------5-----------------------#2023-04-13](https://towardsdatascience.com/what-should-your-decision-be-when-your-p-value-0-052-1d50f9d4d2c5?source=collection_archive---------5-----------------------#2023-04-13)

## 选择显著性水平的指南

[](https://medium.com/@jaekim8080?source=post_page-----1d50f9d4d2c5--------------------------------)[![Jae Kim](../Images/34716958ecfe8c0540f5cf5c1640d587.png)](https://medium.com/@jaekim8080?source=post_page-----1d50f9d4d2c5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1d50f9d4d2c5--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1d50f9d4d2c5--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----1d50f9d4d2c5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-should-your-decision-be-when-your-p-value-0-052-1d50f9d4d2c5&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----1d50f9d4d2c5---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----1d50f9d4d2c5--------------------------------) ·7分钟阅读·2023年4月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1d50f9d4d2c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-should-your-decision-be-when-your-p-value-0-052-1d50f9d4d2c5&user=Jae+Kim&userId=3a7641c3f8c1&source=-----1d50f9d4d2c5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1d50f9d4d2c5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-should-your-decision-be-when-your-p-value-0-052-1d50f9d4d2c5&source=-----1d50f9d4d2c5---------------------bookmark_footer-----------)![](../Images/6753d3eef3966669e4b03886365e0784.png)

图片由[Burst](https://unsplash.com/@burst?utm_source=medium&utm_medium=referral)提供，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在假设检验中，“p值 < α”几乎普遍作为统计显著性的标准和决策规则，其中α是显著性水平。例如，在测试中

H0: θ = 0; H1: θ ≠ 0，

在这里，θ 是表示效应的感兴趣参数，如果 p 值 < α，我们在 α 显著性水平下拒绝无效应的 H0。通常设置 α = 0.05，而 0.10 和 0.01 也被广泛采用。众所周知，这些传统值是任意的，缺乏科学依据。

当 p 值接近 0.05 时，你该怎么办？例如，

+   如果 p 值 = 0.052，你应该保持 α = 0.05 并接受 H0 吗？或者，你应该设置 α = 0.10 并拒绝它？

+   同样，如果 p 值 = 0.047，你应该保持 α = 0.05 并拒绝 H0 吗？或者，你应该设置 α = 0.01 并接受 H0？

统计学教科书在这一点上提供的指导很少，而这种情况在统计分析和机器学习方法的实际应用中经常遇到。
