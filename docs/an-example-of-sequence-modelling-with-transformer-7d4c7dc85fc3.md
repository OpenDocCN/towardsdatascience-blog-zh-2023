# 使用 Transformer 进行序列建模的示例

> 原文：[`towardsdatascience.com/an-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3?source=collection_archive---------9-----------------------#2023-02-10`](https://towardsdatascience.com/an-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3?source=collection_archive---------9-----------------------#2023-02-10)

## NLP 深度学习

## 有关 Transformer 的一些概念，以及如何使用 Transformer 构建序列模型的示例

[](https://medium.com/@jhwang1992m?source=post_page-----7d4c7dc85fc3--------------------------------)![Jiahui Wang](https://medium.com/@jhwang1992m?source=post_page-----7d4c7dc85fc3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7d4c7dc85fc3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7d4c7dc85fc3--------------------------------) [Jiahui Wang](https://medium.com/@jhwang1992m?source=post_page-----7d4c7dc85fc3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4037e6e33535&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3&user=Jiahui+Wang&userId=4037e6e33535&source=post_page-4037e6e33535----7d4c7dc85fc3---------------------post_header-----------) 发表在[《走向数据科学》](https://towardsdatascience.com/?source=post_page-----7d4c7dc85fc3--------------------------------) ·5 分钟阅读·2023 年 2 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7d4c7dc85fc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3&user=Jiahui+Wang&userId=4037e6e33535&source=-----7d4c7dc85fc3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7d4c7dc85fc3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fan-example-of-sequence-modelling-with-transformer-7d4c7dc85fc3&source=-----7d4c7dc85fc3---------------------bookmark_footer-----------)![](img/7516b578384db3e4826b2bf637a5ba5f.png)

由[Patrick Tomasso](https://unsplash.com/@impatrickt?utm_source=medium&utm_medium=referral)在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)上的照片

用户行为序列数据可用于预测用户的配置文件和偏好。例如，我们可以从用户的历史购买记录中了解用户的性别，或者我们可以根据用户的还款历史来预测用户是否会解决贷款。

在这篇文章中，我们将尝试通过对用户手机上的应用程序安装序列进行建模来预测用户的财务风险水平。在这里，我们将应用程序安装序列建模任务视为一个 NLP 问题。一组用户安装的应用程序形成一个语料库。每个应用程序类似于一个单词，而用户的每个应用程序安装序列类似于 NLP 问题中的一句话。为了解决这个应用程序安装序列建模任务，我们选择使用带有 transformer 的神经网络。

在这篇文章中，将涵盖以下内容：

+   Transformer 及其工作原理

+   带有 transformer 的神经网络在 Spark 上的开发和部署

# 1\. Transformer 及其工作原理
