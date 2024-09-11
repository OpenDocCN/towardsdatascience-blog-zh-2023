# 当AutoML遇见大型语言模型

> 原文：[https://towardsdatascience.com/when-automl-meets-large-language-model-756e6bb9baa7?source=collection_archive---------4-----------------------#2023-10-05](https://towardsdatascience.com/when-automl-meets-large-language-model-756e6bb9baa7?source=collection_archive---------4-----------------------#2023-10-05)

## 利用大型语言模型（LLMs）的力量来指导超参数搜索

[](https://shuaiguo.medium.com/?source=post_page-----756e6bb9baa7--------------------------------)[![Shuai Guo](../Images/d673c066f8006079be5bf92757e73a59.png)](https://shuaiguo.medium.com/?source=post_page-----756e6bb9baa7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----756e6bb9baa7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----756e6bb9baa7--------------------------------) [Shuai Guo](https://shuaiguo.medium.com/?source=post_page-----756e6bb9baa7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7b08bf52bf9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-automl-meets-large-language-model-756e6bb9baa7&user=Shuai+Guo&userId=7b08bf52bf9c&source=post_page-7b08bf52bf9c----756e6bb9baa7---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----756e6bb9baa7--------------------------------) ·23分钟阅读·2023年10月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F756e6bb9baa7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-automl-meets-large-language-model-756e6bb9baa7&user=Shuai+Guo&userId=7b08bf52bf9c&source=-----756e6bb9baa7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F756e6bb9baa7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-automl-meets-large-language-model-756e6bb9baa7&source=-----756e6bb9baa7---------------------bookmark_footer-----------)![](../Images/cf26341128d5b5fbb82f432835e76c2d.png)

图片由 [John Schnobrich](https://unsplash.com/@johnschno?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 提供

*自动化机器学习*，简称AutoML，旨在自动化机器学习流程中的各个步骤。其中一个关键方面是**超参数调优**。超参数是指控制机器学习算法结构和行为的参数（例如神经网络模型中的层数），这些参数的值是在数据学习之前设置的，而不是从数据中学习的，这与其他机器学习参数（如神经网络层的权重和偏差）不同。

当前在AutoML中用于超参数调优的实践 heavily 依赖于复杂的算法（如 [贝叶斯优化](https://medium.com/towards-data-science/an-introduction-to-surrogate-optimization-intuition-illustration-case-study-and-the-code-5d9364aed51b)）来自动识别能产生最佳模型性能的超参数组合。

尽管从纯算法角度处理超参数调优问题是有效的，但还有一个关键的信息可以补充算法搜索策略：**人类专业知识**。

资深数据科学家凭借多年的经验和对ML算法的深入理解，通常能够直观地知道从哪里开始搜索，哪些搜索空间的区域可能更有前景，或者何时缩小或扩大可能性。
