# 非参数检验入门（第1部分：秩检验和符号检验）

> 原文：[https://towardsdatascience.com/non-parametric-tests-for-beginners-part-1-rank-and-sign-tests-629704f27f2f?source=collection_archive---------12-----------------------#2023-06-01](https://towardsdatascience.com/non-parametric-tests-for-beginners-part-1-rank-and-sign-tests-629704f27f2f?source=collection_archive---------12-----------------------#2023-06-01)

## 结合示例和 R 代码

[](https://medium.com/@jaekim8080?source=post_page-----629704f27f2f--------------------------------)[![Jae Kim](../Images/34716958ecfe8c0540f5cf5c1640d587.png)](https://medium.com/@jaekim8080?source=post_page-----629704f27f2f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----629704f27f2f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----629704f27f2f--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----629704f27f2f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnon-parametric-tests-for-beginners-part-1-rank-and-sign-tests-629704f27f2f&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----629704f27f2f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----629704f27f2f--------------------------------) · 9分钟阅读 · 2023年6月1日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F629704f27f2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnon-parametric-tests-for-beginners-part-1-rank-and-sign-tests-629704f27f2f&user=Jae+Kim&userId=3a7641c3f8c1&source=-----629704f27f2f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F629704f27f2f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnon-parametric-tests-for-beginners-part-1-rank-and-sign-tests-629704f27f2f&source=-----629704f27f2f---------------------bookmark_footer-----------)![](../Images/91888e55e896cb2b8b8478a2f184525d.png)

图片由 [Joshua Earle](https://unsplash.com/@joshuaearle?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

非参数检验是推断统计学的一个重要分支。然而，许多数据科学家和分析师对其并不广泛使用，也不完全理解。它是传统参数检验（如 t 检验）的自然替代方案，具有多种优点，并且在现代应用（如 A/B 测试）中具有极大的潜力。

非参数检验是基于数据点的秩次或符号构建的，或使用如自助法（bootstrap）这样的重新抽样方法。在这篇文章中，将讨论基于秩次和符号的检验及其示例和R代码。自助法将在系列的第二部分中讨论。我要感谢Venkat Raman，他最近的[LinkedIn帖子](https://www.linkedin.com/feed/update/urn:li:activity:7068311379614593024/)激发了这篇文章的写作。

# 1\. 参数检验与非参数检验

推断统计或假设检验涉及以下关键要素：

1.  虚无假设和备择假设（H0和H1）

1.  检验统计量

1.  在H0下检验统计量的抽样分布

1.  决策规则（p值或临界值，在给定显著性水平下）

## 参数检验
