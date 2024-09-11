# 特征工程死了吗?

> 原文：[`towardsdatascience.com/is-feature-engineering-dead-203e6a9e5751?source=collection_archive---------20-----------------------#2023-01-10`](https://towardsdatascience.com/is-feature-engineering-dead-203e6a9e5751?source=collection_archive---------20-----------------------#2023-01-10)

## **评估特征工程在现代数据科学中的作用**

[](https://medium.com/@DataScienceRebalanced?source=post_page-----203e6a9e5751--------------------------------)![Leah Berg and Ray McLendon](https://medium.com/@DataScienceRebalanced?source=post_page-----203e6a9e5751--------------------------------)[](https://towardsdatascience.com/?source=post_page-----203e6a9e5751--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----203e6a9e5751--------------------------------) [Leah Berg and Ray McLendon](https://medium.com/@DataScienceRebalanced?source=post_page-----203e6a9e5751--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F52338acfb4b9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-feature-engineering-dead-203e6a9e5751&user=Leah+Berg+and+Ray+McLendon&userId=52338acfb4b9&source=post_page-52338acfb4b9----203e6a9e5751---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----203e6a9e5751--------------------------------) ·6 min read·2023 年 1 月 10 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F203e6a9e5751&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-feature-engineering-dead-203e6a9e5751&user=Leah+Berg+and+Ray+McLendon&userId=52338acfb4b9&source=-----203e6a9e5751---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F203e6a9e5751&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-feature-engineering-dead-203e6a9e5751&source=-----203e6a9e5751---------------------bookmark_footer-----------)![](img/57a93fc8ab47955efe632b13f0f11810.png)

照片由[Markus Spiske](https://unsplash.com/@markusspiske?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

深度学习是目前数据科学领域最热门的话题之一 — 你好，[ChatGPT](https://openai.com/blog/chatgpt/)。该领域的前沿数据科学家们了解到，深度学习与许多其他机器学习和人工智能领域有着相当大的不同。特征工程，即创建和转换模型输入的过程，是这些关键区别之一。该领域的领导者，如谷歌的软件工程师弗朗索瓦·肖莱特（Francois Chollet），表示“深度学习去除了对特征工程的需求”（1）。

那么这是否意味着特征工程已经过时了？我认为不是，理由如下。

# **特征工程 — 机器学习与深度学习的区别**

在我们深入探讨之前，我们首先需要了解在机器学习和深度学习中特征是如何生成的。在传统机器学习中，你通常不会直接使用原始数据作为模型的输入。相反，你会对其进行调整和转换，以提高模型性能。

例如，如果你需要创建一个对文档进行分类的模型，你可能会想将文档的文本作为特征，但你可能不会将原始文本直接输入模型。相反，你可能会将文本表示为...
