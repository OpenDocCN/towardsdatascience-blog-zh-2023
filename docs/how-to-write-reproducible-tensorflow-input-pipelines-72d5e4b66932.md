# 如何编写可重复的 TensorFlow 输入管道

> 原文：[https://towardsdatascience.com/how-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932?source=collection_archive---------19-----------------------#2023-01-09](https://towardsdatascience.com/how-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932?source=collection_archive---------19-----------------------#2023-01-09)

## 通过使用种子修复输入顺序

[](https://pascaljanetzky.medium.com/?source=post_page-----72d5e4b66932--------------------------------)[![Pascal Janetzky](../Images/43d68509b63c5f9b3fc9cef3cbfc1a88.png)](https://pascaljanetzky.medium.com/?source=post_page-----72d5e4b66932--------------------------------)[](https://towardsdatascience.com/?source=post_page-----72d5e4b66932--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----72d5e4b66932--------------------------------) [Pascal Janetzky](https://pascaljanetzky.medium.com/?source=post_page-----72d5e4b66932--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F672b95fdf976&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932&user=Pascal+Janetzky&userId=672b95fdf976&source=post_page-672b95fdf976----72d5e4b66932---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----72d5e4b66932--------------------------------) ·6分钟阅读·2023年1月9日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F72d5e4b66932&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932&user=Pascal+Janetzky&userId=672b95fdf976&source=-----72d5e4b66932---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F72d5e4b66932&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-write-reproducible-tensorflow-input-pipelines-72d5e4b66932&source=-----72d5e4b66932---------------------bookmark_footer-----------)

在准备机器学习实验时，输入管道在数据准备中扮演了关键角色。尽管它们通常比较简单——机器学习框架使这一过程相对容易——，但它们缺乏可重复性。

![](../Images/9f8ae83ad000df4f9014a188dab704b4.png)

图片来源：[Quinten de Graaf](https://unsplash.com/@quinten149?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是默认情况：输入数据中的随机性，例如在每个周期后应用洗牌，已被证明在神经网络训练中发挥着重要作用。因此，默认情况下“启用”随机性是显而易见的选择。然而，当我们希望更深入地理解我们的训练时，我们希望尽可能固定更多的影响因素/参数。输入顺序就是其中之一。

固定输入顺序是否意味着我们必须完全放弃洗牌或其他随机化操作？不，幸运的是，不能这样做。我来解释一下原因。在处理随机性时，我们可以做出一个选择：设置随机性操作的种子。机器学习框架从这个种子推导出任何其他操作及其顺序，通常是一个整数，比如42、111或1337。

在这里，考虑从列表中随机选择一个数字。当这个操作没有种子时，我们每次进行抽取时都会得到不同的数字。然而，如果我们…
