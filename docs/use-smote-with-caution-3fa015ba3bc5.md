# SMOTE 和其他选择：处理类别不平衡数据的全面指南

> 原文：[`towardsdatascience.com/use-smote-with-caution-3fa015ba3bc5?source=collection_archive---------12-----------------------#2023-01-03`](https://towardsdatascience.com/use-smote-with-caution-3fa015ba3bc5?source=collection_archive---------12-----------------------#2023-01-03)

## 何时使用或不使用合成数据来解决类别不平衡的指南

[](https://ransakaravihara.medium.com/?source=post_page-----3fa015ba3bc5--------------------------------)![Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----3fa015ba3bc5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3fa015ba3bc5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----3fa015ba3bc5--------------------------------) [Ransaka Ravihara](https://ransakaravihara.medium.com/?source=post_page-----3fa015ba3bc5--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F61b4d96de932&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-smote-with-caution-3fa015ba3bc5&user=Ransaka+Ravihara&userId=61b4d96de932&source=post_page-61b4d96de932----3fa015ba3bc5---------------------post_header-----------) 发表在[《走进数据科学》](https://towardsdatascience.com/?source=post_page-----3fa015ba3bc5--------------------------------) ·10 min 读·2023 年 1 月 3 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3fa015ba3bc5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-smote-with-caution-3fa015ba3bc5&user=Ransaka+Ravihara&userId=61b4d96de932&source=-----3fa015ba3bc5---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3fa015ba3bc5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fuse-smote-with-caution-3fa015ba3bc5&source=-----3fa015ba3bc5---------------------bookmark_footer-----------)![](img/7bbfd0409f31269de20f9ae943cb9fd2.png)

[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上[은하](https://unsplash.com/@b0nn13_4nd_clyd3?utm_source=medium&utm_medium=referral)的照片

如果你是机器学习从业者，你可能会更频繁地遇到类别不平衡的问题。类别不平衡发生在数据集中存在不同的类别分布时。举个例子，假设我们在处理一个客户流失问题。在这个特定场景下，我们的小类和大类是客户流失和客户留在当前服务提供商那里。但是，如果你进一步探索这个问题，你会发现流失类别的客户较少，因为客户流失是一个不常见的事件，这对业务有利但对模型不利。因此，如果我们将这个数据集输入到模型中，它将比小类场景更准确地学习大类模式（非流失场景）。这就是我们问题的开始。

# 如何处理机器学习中的类别不平衡

最明显的答案是，由于模型在训练过程中与小类的交互较少，我们可以通过将更多的小类数据添加到模型中来改善这一点。但怎么做呢？我们有几种方法：

1.  收集更多关于小类的数据——这在理论上是简单的…
