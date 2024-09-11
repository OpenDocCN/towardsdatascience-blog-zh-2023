# 在 Power BI 中与 sklearn 机器学习模型进行交互

> 原文：[https://towardsdatascience.com/deploying-sklearn-models-in-power-bi-d982f2d21ec?source=collection_archive---------13-----------------------#2023-05-24](https://towardsdatascience.com/deploying-sklearn-models-in-power-bi-d982f2d21ec?source=collection_archive---------13-----------------------#2023-05-24)

![](../Images/74ee0509e142ebc1ab282778e7d62b1f.png)[](https://newmarrk.medium.com/?source=post_page-----d982f2d21ec--------------------------------)[![Mark Graus](../Images/5b8c4d77254d891b9fb0a3e96616525a.png)](https://newmarrk.medium.com/?source=post_page-----d982f2d21ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d982f2d21ec--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d982f2d21ec--------------------------------) [Mark Graus](https://newmarrk.medium.com/?source=post_page-----d982f2d21ec--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F44f3ee2b4e9b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-sklearn-models-in-power-bi-d982f2d21ec&user=Mark+Graus&userId=44f3ee2b4e9b&source=post_page-44f3ee2b4e9b----d982f2d21ec---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d982f2d21ec--------------------------------) ·9 分钟阅读·2023年5月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd982f2d21ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-sklearn-models-in-power-bi-d982f2d21ec&user=Mark+Graus&userId=44f3ee2b4e9b&source=-----d982f2d21ec---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd982f2d21ec&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploying-sklearn-models-in-power-bi-d982f2d21ec&source=-----d982f2d21ec---------------------bookmark_footer-----------)

在某些情况下，我们希望拥有一个可以进行交互的监督学习模型。虽然任何数据科学家都可以很容易地在 Jupyter notebook 中构建一个 SKLearn 模型并进行试验，但当你希望其他利益相关者与模型进行互动时，你将需要创建一个前端。这可以通过一个简单的 Flask webapp 实现，为用户提供一个网页界面，用于将数据输入到 sklearn 模型或管道中以查看预测输出。然而，本文将重点介绍如何在 Power BI 中使用 Python 可视化与模型进行交互。

本文将分为两个主要部分：

+   构建 SKLearn 模型 / 构建管道

+   构建 Power BI 界面

代码非常简单，你可以从这篇文章中复制粘贴你需要的部分，但它也可以在[我的Github](https://github.com/marrk/sklearn-in-powerbi)上找到。要使用它，你需要做两件事。运行Python Notebook中的代码以序列化管道，并在Power BI文件中修改该管道的路径。

# 1\. 构建模型

在这个例子中，我们将使用Titanic数据集来构建一个简单的预测模型。该模型将是一个分类模型，使用一个分类特征（‘sex’）和一个数值特征（‘age’）作为预测变量。为了演示这种方法，我们…
