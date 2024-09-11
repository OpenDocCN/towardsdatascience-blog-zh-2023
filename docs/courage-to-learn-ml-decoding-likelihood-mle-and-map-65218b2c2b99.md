# 勇敢学习机器学习：解读似然性、MLE 和 MAP

> 原文：[`towardsdatascience.com/courage-to-learn-ml-decoding-likelihood-mle-and-map-65218b2c2b99?source=collection_archive---------2-----------------------#2023-12-03`](https://towardsdatascience.com/courage-to-learn-ml-decoding-likelihood-mle-and-map-65218b2c2b99?source=collection_archive---------2-----------------------#2023-12-03)

## 还有一尾猫粮偏好

[](https://amyma101.medium.com/?source=post_page-----65218b2c2b99--------------------------------)![Amy Ma](https://amyma101.medium.com/?source=post_page-----65218b2c2b99--------------------------------)[](https://towardsdatascience.com/?source=post_page-----65218b2c2b99--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----65218b2c2b99--------------------------------) [Amy Ma](https://amyma101.medium.com/?source=post_page-----65218b2c2b99--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fd6d8df787b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-decoding-likelihood-mle-and-map-65218b2c2b99&user=Amy+Ma&userId=d6d8df787b&source=post_page-d6d8df787b----65218b2c2b99---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----65218b2c2b99--------------------------------) ·10 分钟阅读·2023 年 12 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F65218b2c2b99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-decoding-likelihood-mle-and-map-65218b2c2b99&user=Amy+Ma&userId=d6d8df787b&source=-----65218b2c2b99---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F65218b2c2b99&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcourage-to-learn-ml-decoding-likelihood-mle-and-map-65218b2c2b99&source=-----65218b2c2b99---------------------bookmark_footer-----------)![](img/93ae20104fe2612dcc2083619a7ca29f.png)

照片由[Anastasiia Rozumna](https://unsplash.com/@rozumna?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

欢迎来到‘[勇敢学习机器学习](https://towardsdatascience.com/tagged/courage-to-learn-ml)’。这个系列旨在简化复杂的机器学习概念，将其呈现为一种轻松且信息丰富的对话风格，类似于“[勇敢做自己](https://www.goodreads.com/book/show/43306206-the-courage-to-be-disliked)”的吸引风格，但重点在于机器学习。

在本系列的这一部分，我们的导师-学员组合将深入探讨统计概念，如 MLE 和 MAP。这次讨论将为我们重新审视 L1 和 L2 正则化的先前探索奠定基础。为了获得完整的理解，我建议在阅读第四部分‘[勇敢学习机器学习：揭开 L1 和 L2 正则化的面纱](https://yujing-ma45.medium.com/understanding-l1-l2-regularization-part-1-9c7affe6f920)’之前阅读这篇文章。

本文旨在以问答形式解决一些可能在你面前出现的基本问题。像往常一样，如果你有类似的问题，那么你来对地方了：

+   ‘似然性’究竟是什么？

+   似然性和概率之间的区别

+   为什么在机器学习的背景下，似然性如此重要？

+   什么是 MLE（最大似然估计）？

+   什么是 MAP（最大后验估计**）？**

+   MLE 和…之间的区别
