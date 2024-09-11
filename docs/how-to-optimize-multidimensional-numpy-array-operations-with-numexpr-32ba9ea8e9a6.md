# 如何使用 Numexpr 优化多维 Numpy 数组操作

> 原文：[https://towardsdatascience.com/how-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6?source=collection_archive---------5-----------------------#2023-10-22](https://towardsdatascience.com/how-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6?source=collection_archive---------5-----------------------#2023-10-22)

## [快速计算](https://medium.com/@qtalen/list/fast-computing-2a37a7e82be5)

## Numpy 性能优化的实际案例研究

[](https://qtalen.medium.com/?source=post_page-----32ba9ea8e9a6--------------------------------)[![Peng Qian](../Images/9ce9aeb381ec6b017c1ee5d4714937e2.png)](https://qtalen.medium.com/?source=post_page-----32ba9ea8e9a6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32ba9ea8e9a6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----32ba9ea8e9a6--------------------------------) [Peng Qian](https://qtalen.medium.com/?source=post_page-----32ba9ea8e9a6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8e2fe735546d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6&user=Peng+Qian&userId=8e2fe735546d&source=post_page-8e2fe735546d----32ba9ea8e9a6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32ba9ea8e9a6--------------------------------) ·5 min read·2023年10月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F32ba9ea8e9a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6&user=Peng+Qian&userId=8e2fe735546d&source=-----32ba9ea8e9a6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F32ba9ea8e9a6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-optimize-multidimensional-numpy-array-operations-with-numexpr-32ba9ea8e9a6&source=-----32ba9ea8e9a6---------------------bookmark_footer-----------)![](../Images/96fef5f72810913ebac709c8f00de6a6.png)

如何使用 Numexpr 优化多维 Numpy 数组操作。图片来源：作者创作，[Canva](https://www.canva.com/)。

这是一篇相对简短的文章。在文中，我将通过一个实际场景示例，讲解如何在多维 Numpy 数组中使用 [Numexpr 表达式](https://numexpr.readthedocs.io/en/latest/user_guide.html?ref=dataleadsfuture.com#supported-functions) 实现显著的性能提升。

解释如何在多维Numpy数组中使用Numexpr以及如何使用Numexpr表达式的文章不多，所以我希望这篇文章能对你有所帮助。

# 介绍

最近，在回顾我以前的一些工作时，我偶然发现了这段代码：

```py
def predict(X, w, b):
    z = np.dot(X, w)
    y_hat = sigmoid(z)
    y_pred = np.zeros((y_hat.shape[0], 1))

    for i in range(y_hat.shape[0]):
        if y_hat[i, 0] < 0.5:
            y_pred[i, 0] = 0
        else:
            y_pred[i, 0] = 1
    return y_pred
```

这段代码将机器学习中的逻辑回归模型中的预测结果从概率转换为分类结果0或1。

但天哪，谁会用`for loop`来遍历Numpy ndarray？
