# 通用模型的非理性有效性

> 原文：[https://towardsdatascience.com/the-unreasonable-effectiveness-of-general-models-b4e822eaeb27?source=collection_archive---------13-----------------------#2023-01-17](https://towardsdatascience.com/the-unreasonable-effectiveness-of-general-models-b4e822eaeb27?source=collection_archive---------13-----------------------#2023-01-17)

## 在一个极其困难的问题上测试通用模型

[](https://medium.com/@mazzanti.sam?source=post_page-----b4e822eaeb27--------------------------------)[![Samuele Mazzanti](../Images/432477d6418a3f79bf25dec42755d364.png)](https://medium.com/@mazzanti.sam?source=post_page-----b4e822eaeb27--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b4e822eaeb27--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b4e822eaeb27--------------------------------) [Samuele Mazzanti](https://medium.com/@mazzanti.sam?source=post_page-----b4e822eaeb27--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe16f3bb86e03&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-unreasonable-effectiveness-of-general-models-b4e822eaeb27&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=post_page-e16f3bb86e03----b4e822eaeb27---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b4e822eaeb27--------------------------------) ·8 min read·2023年1月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb4e822eaeb27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-unreasonable-effectiveness-of-general-models-b4e822eaeb27&user=Samuele+Mazzanti&userId=e16f3bb86e03&source=-----b4e822eaeb27---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb4e822eaeb27&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-unreasonable-effectiveness-of-general-models-b4e822eaeb27&source=-----b4e822eaeb27---------------------bookmark_footer-----------)![](../Images/e0cce5f765f3d90678879ef2406c1b7d.png)

[图片由作者提供，使用 Excalidraw 制作]

在 [上一篇文章](https://medium.com/towards-data-science/what-is-better-one-general-model-or-many-specialized-models-9500d9f8751d) 中，我尝试揭穿那种有些模糊的观点，即一堆模型（每个模型专注于数据集的一个子集）应该比单一模型表现更好。

为此，我取了数据集的一部分（例如，仅包括美国客户）并在该组上训练了一个模型，也就是一个**专业模型**。然后，我在整个数据集（即所有客户，不论国籍）上训练了第二个模型，也就是**通用模型**。最后，我在一个仅由该组观察结果构成的保留集上比较了这两个模型的性能。

我在多个数据集和同一数据集的多个组上重复了这个过程，总共进行了600次比较。**结果有一个明显的赢家：通用模型**。

然而，我的实验并没有说服一些人，他们认为我的方法过于简单化。例如，这是一条在我关于这篇文章的Linkedin帖子下最受欢迎的评论之一：

![](../Images/4fdb1b7f061a8941066769f1d1c26b1b.png)

[来自我[Linkedin帖子](https://www.linkedin.com/feed/update/urn:li:activity:7015963227549282305/)评论区的截图]
