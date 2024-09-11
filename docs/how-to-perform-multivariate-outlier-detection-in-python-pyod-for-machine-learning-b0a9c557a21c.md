# 如何在 Python PyOD 中执行多变量异常值检测以进行机器学习

> 原文：[https://towardsdatascience.com/how-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c?source=collection_archive---------5-----------------------#2023-02-07](https://towardsdatascience.com/how-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c?source=collection_archive---------5-----------------------#2023-02-07)

## 异常值检测系列，第 3 部分

[](https://ibexorigin.medium.com/?source=post_page-----b0a9c557a21c--------------------------------)[![Bex T.](../Images/516496f32596e8ad56bf07f178a643c6.png)](https://ibexorigin.medium.com/?source=post_page-----b0a9c557a21c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b0a9c557a21c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b0a9c557a21c--------------------------------) [Bex T.](https://ibexorigin.medium.com/?source=post_page-----b0a9c557a21c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F39db050c2ac2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c&user=Bex+T.&userId=39db050c2ac2&source=post_page-39db050c2ac2----b0a9c557a21c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b0a9c557a21c--------------------------------) ·9 分钟阅读·2023年2月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb0a9c557a21c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c&user=Bex+T.&userId=39db050c2ac2&source=-----b0a9c557a21c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb0a9c557a21c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-perform-multivariate-outlier-detection-in-python-pyod-for-machine-learning-b0a9c557a21c&source=-----b0a9c557a21c---------------------bookmark_footer-----------)![](../Images/f1e3a4d96c11ab2134a5310a7737a0d8.png)

图片由 [Takashi Miyazaki](https://unsplash.com/@miyatankun?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/backgrounds/art/abstract?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## 动机

以下是一个*非常*可疑的箱线图：

![](../Images/567acd70936349dc6af5c43bb04d9a66.png)

图片由作者提供。

这描绘了钻石质量与价格之间的关系。六个质量类别按降序排列，因此最好的钻石在*理想*类别中，而最低质量的钻石在*公平*类别中。

现在，这里是奇怪的部分。首先，所有类别都有许多离群点，用小黑点标记在须条上方。

第二，即使理想钻石被认为是最好的，它们的中位价格仍低于其他任何类别（中位数以盒子内的线表示）。

钻石质量与价格之间这种奇怪的关系让我们提出一个问题：那些离群点*真的*是离群点吗？

今天，我们将回答这个确切的问题，换句话说，我们将学习如何区分多变量离群点以及如何检测它们。

这是我们离群点检测系列的第三部分。查看前两部分……
