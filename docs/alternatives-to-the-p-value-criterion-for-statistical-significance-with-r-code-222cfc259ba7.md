# p值标准的替代方法（附R代码）

> 原文：[https://towardsdatascience.com/alternatives-to-the-p-value-criterion-for-statistical-significance-with-r-code-222cfc259ba7?source=collection_archive---------1-----------------------#2023-03-12](https://towardsdatascience.com/alternatives-to-the-p-value-criterion-for-statistical-significance-with-r-code-222cfc259ba7?source=collection_archive---------1-----------------------#2023-03-12)

## 改进统计决策的方法

[](https://medium.com/@jaekim8080?source=post_page-----222cfc259ba7--------------------------------)[![Jae Kim](../Images/34716958ecfe8c0540f5cf5c1640d587.png)](https://medium.com/@jaekim8080?source=post_page-----222cfc259ba7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----222cfc259ba7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----222cfc259ba7--------------------------------) [Jae Kim](https://medium.com/@jaekim8080?source=post_page-----222cfc259ba7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F3a7641c3f8c1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Falternatives-to-the-p-value-criterion-for-statistical-significance-with-r-code-222cfc259ba7&user=Jae+Kim&userId=3a7641c3f8c1&source=post_page-3a7641c3f8c1----222cfc259ba7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----222cfc259ba7--------------------------------) · 8 min read · Mar 12, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F222cfc259ba7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Falternatives-to-the-p-value-criterion-for-statistical-significance-with-r-code-222cfc259ba7&user=Jae+Kim&userId=3a7641c3f8c1&source=-----222cfc259ba7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F222cfc259ba7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Falternatives-to-the-p-value-criterion-for-statistical-significance-with-r-code-222cfc259ba7&source=-----222cfc259ba7---------------------bookmark_footer-----------)![](../Images/3c146903030390702939d348cf3d4753.png)

照片由 [Rommel Davila](https://unsplash.com/@rommeldav22?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在建立统计显著性时，p值标准几乎被普遍使用。这个标准是当p值小于显著性水平(*α*)时，拒绝原假设（H0），接受备择假设（H1）。用于这个决策阈值的传统值包括0.05、0.10和0.01。

按定义，p 值衡量样本信息与 H0 的兼容性：即 P(D|H0)，即在 H0 下数据 (D) 的概率或可能性。然而，正如美国统计协会的声明中明确指出的 ([Wasserstein 和 Lazar, 2016](https://www.tandfonline.com/doi/full/10.1080/00031305.2016.1154108))，p 值标准作为决策规则存在若干严重缺陷。主要缺陷包括

1.  p 值是样本大小的递减函数；

1.  该标准完全忽略了 P(D|H1)，即数据与 H1 的兼容性；

1.  常规的 *α* 值（如 0.05）是任意的，几乎没有科学依据。

其中一个后果是，当 p 值标准经常在 H0 被实际微不足道的边际违反时拒绝 H0，尤其是当样本量很大时……
