# 从因果树到森林

> 原文：[https://towardsdatascience.com/from-causal-trees-to-forests-43c4536f1481?source=collection_archive---------3-----------------------#2023-02-20](https://towardsdatascience.com/from-causal-trees-to-forests-43c4536f1481?source=collection_archive---------3-----------------------#2023-02-20)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## *如何使用随机森林进行政策目标定位*

[](https://medium.com/@matteo.courthoud?source=post_page-----43c4536f1481--------------------------------)[![Matteo Courthoud](../Images/d873eab35a0cf9fc696658c0bee16b33.png)](https://medium.com/@matteo.courthoud?source=post_page-----43c4536f1481--------------------------------)[](https://towardsdatascience.com/?source=post_page-----43c4536f1481--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----43c4536f1481--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----43c4536f1481--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F666130fb420f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-causal-trees-to-forests-43c4536f1481&user=Matteo+Courthoud&userId=666130fb420f&source=post_page-666130fb420f----43c4536f1481---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----43c4536f1481--------------------------------) ·13分钟阅读·2023年2月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F43c4536f1481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-causal-trees-to-forests-43c4536f1481&user=Matteo+Courthoud&userId=666130fb420f&source=-----43c4536f1481---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F43c4536f1481&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffrom-causal-trees-to-forests-43c4536f1481&source=-----43c4536f1481---------------------bookmark_footer-----------)![](../Images/2977138a8f4b861a47fb69dc54222ec5.png)

封面，图片由作者提供

在我的[上一篇博客文章](https://medium.com/towards-data-science/understanding-causal-trees-920177462149)中，我们已经看到如何使用**因果树**来估计政策的异质性处理效应。如果你还没读过那篇文章，建议你先阅读，因为我们会以那篇文章的内容为基础继续讲解。

为什么要考虑异质性处理效应 (HTE)？首先，估计异质性处理效应可以帮助我们选择哪些用户（患者、用户、客户等）来提供治疗（药物、广告、产品等），这取决于他们预期的结果（疾病、公司收入、客户满意度等）。换句话说，估计 HTE 使我们能够进行**定向**。实际上，正如我们在文章后面将看到的那样，某种治疗在整体上可能无效甚至适得其反，但对某些用户却能带来积极的效果。反之亦然：一种药物在整体上可能有效，但如果我们能识别出其有副作用的用户，药物的效果可能会有所改善。

在这篇文章中，我们将探索因果树的扩展：因果森林。正如随机森林通过平均多个自助法树来扩展回归树一样，因果森林扩展了因果树。主要的区别来自于推断…
