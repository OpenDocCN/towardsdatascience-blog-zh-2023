# Excel 中的决策树回归器

> 原文：[`towardsdatascience.com/decision-tree-regressor-in-excel-2d29d16df1db?source=collection_archive---------6-----------------------#2023-03-23`](https://towardsdatascience.com/decision-tree-regressor-in-excel-2d29d16df1db?source=collection_archive---------6-----------------------#2023-03-23)

![](img/704b63133df778fd0325d550992bcd91.png)

图片来源：[Kevin Young](https://unsplash.com/@kevinjyoung?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 机器学习初学者的逐步指南

[](https://medium.com/@angela.shi?source=post_page-----2d29d16df1db--------------------------------)![Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----2d29d16df1db--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d29d16df1db--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d29d16df1db--------------------------------) [Angela and Kezhan Shi](https://medium.com/@angela.shi?source=post_page-----2d29d16df1db--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2bf03e38122e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-tree-regressor-in-excel-2d29d16df1db&user=Angela+and+Kezhan+Shi&userId=2bf03e38122e&source=post_page-2bf03e38122e----2d29d16df1db---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d29d16df1db--------------------------------) ·6 分钟阅读·2023 年 3 月 23 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2d29d16df1db&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecision-tree-regressor-in-excel-2d29d16df1db&source=-----2d29d16df1db---------------------bookmark_footer-----------)

我正在撰写一系列关于使用 Excel 实现机器学习算法的文章，Excel 是理解这些算法工作原理的绝佳工具，无需编程。

在这篇文章中，我们将一步一步实现决策树回归算法。

我将使用 Google 表格来演示实现过程。如果你想访问这个表格以及我开发的其他表格——例如，使用梯度下降的线性回归、逻辑回归、带有反向传播的神经网络、KNN、k-means 等——请考虑在 Ko-fi 上支持我。你可以通过以下链接找到所有这些资源：[`ko-fi.com/s/4ddca6dff1`](https://ko-fi.com/s/4ddca6dff1)

# 一个简单的数据集上的简单决策树

让我们使用一个只有一个连续特征的简单数据集。

![](img/9df88702c0faaf46171e4d283c42ce9e.png)

Excel 中的决策树回归简单数据集——图像由作者提供

我们可以直观地猜测，对于第一次分裂，有两个可能的值，一个在 5.5 左右，另一个在 12 左右。现在的问题是，我们选择哪一个？
