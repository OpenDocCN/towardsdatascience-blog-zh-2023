# 逻辑回归是回归模型还是分类器？让我们结束这场辩论

> 原文：[`towardsdatascience.com/is-logistic-regression-a-regressor-or-a-classifier-lets-end-the-debate-a01b024f7f65?source=collection_archive---------4-----------------------#2023-03-14`](https://towardsdatascience.com/is-logistic-regression-a-regressor-or-a-classifier-lets-end-the-debate-a01b024f7f65?source=collection_archive---------4-----------------------#2023-03-14)

## 从两个不同的角度出发，并进行 3 轮讨论

[](https://medium.com/@angela.shi?source=post_page-----a01b024f7f65--------------------------------)![Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----a01b024f7f65--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a01b024f7f65--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a01b024f7f65--------------------------------) [Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----a01b024f7f65--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-logistic-regression-a-regressor-or-a-classifier-lets-end-the-debate-a01b024f7f65&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----a01b024f7f65---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a01b024f7f65--------------------------------) ·11 min read·2023 年 3 月 14 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa01b024f7f65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-logistic-regression-a-regressor-or-a-classifier-lets-end-the-debate-a01b024f7f65&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=-----a01b024f7f65---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa01b024f7f65&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-logistic-regression-a-regressor-or-a-classifier-lets-end-the-debate-a01b024f7f65&source=-----a01b024f7f65---------------------bookmark_footer-----------)

尽管我们可以找到许多讨论这个问题的帖子和文章，我还是决定再添加一篇。这是一个讨论一些潜在理论和框架的机会。

有些人试图通过给出明确的答案来捍卫他们的观点：逻辑回归是回归模型，还是逻辑回归是分类器。我不会捍卫任何观点，因为对我来说，这场争论有些荒谬。

+   对于那些认为逻辑回归是分类器的人，请你们知道，在“逻辑回归”这个名字中，有“回归”二字，所以给这个模型取这个名字的人认为它是回归模型是有原因的。是的，我知道，可能会有误称，但你们仍然必须承认有其理由。

+   对于那些认为逻辑回归只是回归模型的人，你们也知道它被应用于分类任务。因此，其他人希望称它为分类器。

答案是这两种观点都有可能。就像生活中的许多事情一样，我们对看似相同的主题会给出不同的，有时甚至矛盾的陈述，这是因为我们从不同的角度来看待它。因此，有趣的不是答案本身，而是我们如何得出这个答案。

这个问题也类似于关于番茄是水果还是……的古老辩论。
