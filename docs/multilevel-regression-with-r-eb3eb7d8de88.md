# 使用 R 进行多层次回归

> 原文：[`towardsdatascience.com/multilevel-regression-with-r-eb3eb7d8de88?source=collection_archive---------4-----------------------#2023-05-15`](https://towardsdatascience.com/multilevel-regression-with-r-eb3eb7d8de88?source=collection_archive---------4-----------------------#2023-05-15)

## 通过这个简单的解释和例子来理解层次线性模型

[](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb3eb7d8de88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb3eb7d8de88--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4429d99b1245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultilevel-regression-with-r-eb3eb7d8de88&user=Gustavo+Santos&userId=4429d99b1245&source=post_page-4429d99b1245----eb3eb7d8de88---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb3eb7d8de88--------------------------------) ·9 分钟阅读·2023 年 5 月 15 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Feb3eb7d8de88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultilevel-regression-with-r-eb3eb7d8de88&user=Gustavo+Santos&userId=4429d99b1245&source=-----eb3eb7d8de88---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Feb3eb7d8de88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmultilevel-regression-with-r-eb3eb7d8de88&source=-----eb3eb7d8de88---------------------bookmark_footer-----------)![](img/df6a3d8f1bd84381780b225b2e4fde4c.png)

图片由 [Lidya Nada](https://unsplash.com/@lidyanada?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/BnzqQwerUOY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 引言

回归模型已经存在很长时间了，远在机器学习成为热门领域之前。统计学家们早在 1900 年代之前就开始使用这些模型来理解变量之间的关系，当时**弗朗西斯·高尔顿**（1885 年）首次提出了这一概念。

幸运的是，自那时以来，理论得到了极大的发展，计算机和技术也有了很大的进步，以至于我们可以说，现在创建回归模型是一件容易的（如果不是最容易的）事情。

然而，不要被它实现的简单性所欺骗。它确实简单，但并非那么简单。可用的回归模型不止一种，不仅仅是普通最小二乘法（OLS）。

我们在这篇文章中讨论的是层次线性模型（HLM），或者简单来说，就是**多层次回归**。

# 准备工作

在我们深入内容之前，让我们先熟悉一下在这个示例中将使用的数据集以及我们在编码过程中需要的库。

加载以下库。请记住，如果你没有安装其中任何一个库，只需使用…
