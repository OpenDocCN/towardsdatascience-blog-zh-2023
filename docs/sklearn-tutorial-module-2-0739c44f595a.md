# Sklearn 教程：模块 2

> 原文：[`towardsdatascience.com/sklearn-tutorial-module-2-0739c44f595a?source=collection_archive---------5-----------------------#2023-11-25`](https://towardsdatascience.com/sklearn-tutorial-module-2-0739c44f595a?source=collection_archive---------5-----------------------#2023-11-25)

## 我参加了官方的 sklearn MOOC 教程。以下是我的一些收获。

[](https://mocquin.medium.com/?source=post_page-----0739c44f595a--------------------------------)![Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----0739c44f595a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----0739c44f595a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----0739c44f595a--------------------------------) [Yoann Mocquin](https://mocquin.medium.com/?source=post_page-----0739c44f595a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F173731d06320&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-2-0739c44f595a&user=Yoann+Mocquin&userId=173731d06320&source=post_page-173731d06320----0739c44f595a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----0739c44f595a--------------------------------) ·14 分钟阅读·2023 年 11 月 25 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F0739c44f595a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-2-0739c44f595a&user=Yoann+Mocquin&userId=173731d06320&source=-----0739c44f595a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F0739c44f595a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsklearn-tutorial-module-2-0739c44f595a&source=-----0739c44f595a---------------------bookmark_footer-----------)

在多年使用 Python 科学栈（NumPy、Matplotlib、SciPy、Pandas 和 Seaborn）之后，我逐渐意识到下一步应该是 scikit-learn，或称“sklearn”。

![](img/12ce198b8d409170b76046f45bd4f12c.png)

图片来源：[Nick Morrison](https://unsplash.com/@nickmorrison?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

本模块重点介绍模型评分的概念，包括测试评分和训练评分。这些评分随后用于定义过拟合和欠拟合，以及偏差和方差的概念。

我们还将学习如何根据模型的复杂性和输入样本的数量来检查模型的性能。

*所有图片均由作者提供。*

如果你错过了，我强烈推荐你先看看本系列的第一篇文章——这样会更容易跟上：

[](/sklearn-tutorial-module-1-f31b3964a3b4?source=post_page-----0739c44f595a--------------------------------) ## Sklearn 教程：模块 1

### 我参加了官方的 sklearn MOOC 教程。以下是我的收获。

[towardsdatascience.com

# **分数：训练分数和测试分数**

我想讨论的第一个概念是**训练分数和测试分数**。**分数是用来数值化模型性能的一种方式**。为了计算这种性能，我们使用一个分数……
