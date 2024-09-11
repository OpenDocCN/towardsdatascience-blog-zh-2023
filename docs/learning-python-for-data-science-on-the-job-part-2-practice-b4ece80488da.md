# 数据科学实战中的 Python 学习，第二部分：实践

> 原文：[`towardsdatascience.com/learning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da?source=collection_archive---------3-----------------------#2023-01-15`](https://towardsdatascience.com/learning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da?source=collection_archive---------3-----------------------#2023-01-15)

## 线性回归在 Excel 和 Python 中的并行案例研究

[](https://nrlewis929.medium.com/?source=post_page-----b4ece80488da--------------------------------)![Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----b4ece80488da--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b4ece80488da--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----b4ece80488da--------------------------------) [Nicholas Lewis](https://nrlewis929.medium.com/?source=post_page-----b4ece80488da--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa4cd35f7b702&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da&user=Nicholas+Lewis&userId=a4cd35f7b702&source=post_page-a4cd35f7b702----b4ece80488da---------------------post_header-----------) 发布于 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----b4ece80488da--------------------------------) ·10 分钟阅读·2023 年 1 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb4ece80488da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da&user=Nicholas+Lewis&userId=a4cd35f7b702&source=-----b4ece80488da---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb4ece80488da&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flearning-python-for-data-science-on-the-job-part-2-practice-b4ece80488da&source=-----b4ece80488da---------------------bookmark_footer-----------)![](img/7fe25f9ef78a62d00f5ba0f71a4fd54b.png)

图片由 [Hitesh Choudhary](https://unsplash.com/@hiteshchoudhary?utm_source=medium&utm_medium=referral) 提供，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

欢迎来到这一系列文章的第二部分，这些文章讨论了如何在工作中或没有正式教育背景的情况下学习 Python 和数据科学。[第一部分](https://medium.com/towards-data-science/learning-python-for-data-science-on-the-job-part-1-philosophy-6e2aedc4e041)谈到了我在过去 10 年中在工作和正式教育环境中的学习经验。如果你对学习哲学感兴趣，或者想了解一些激励自己开始学习的想法，可以去看看。如果你像我一样通过实际操作具体示例来学习效果最好，那就继续阅读吧！

## 问题定义

所有相关的文件都可以在我的[Github](https://github.com/nrlewis929/excel_python_side-by-side)上找到。然而，我鼓励你完全从头开始，按照这里提供的代码块和截图进行操作。

在这个案例研究中，我们将进行一个简单的线性回归分析。我们有两类输入数据，基于这些输入，我们希望训练一个线性模型来预测输出，基于实际观察到的数据。在`data.csv`文件中，这些输入被称为`x1`和`x2`，观察到的数据被称为`y`。模型的形式为 Ax1 + Bx2 + C = y。你可能会注意到 x2 = x1²。这是故意的，随着你在数据科学领域的进步，你可能会想要记住这个小技巧……
