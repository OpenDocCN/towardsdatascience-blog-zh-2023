# 评估新语言模型的三种基本方法

> 原文：[https://towardsdatascience.com/the-three-essential-methods-to-evaluate-a-new-language-model-aa5c526bacfd?source=collection_archive---------5-----------------------#2023-07-03](https://towardsdatascience.com/the-three-essential-methods-to-evaluate-a-new-language-model-aa5c526bacfd?source=collection_archive---------5-----------------------#2023-07-03)

## 如何检查最新、最热门的大型语言模型（LLM）是否符合你的需求

[](https://heiko-hotz.medium.com/?source=post_page-----aa5c526bacfd--------------------------------)[![Heiko Hotz](../Images/d08394d46d41d5cd9e76557a463be95e.png)](https://heiko-hotz.medium.com/?source=post_page-----aa5c526bacfd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----aa5c526bacfd--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----aa5c526bacfd--------------------------------) [Heiko Hotz](https://heiko-hotz.medium.com/?source=post_page-----aa5c526bacfd--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F993c21f1b30f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-three-essential-methods-to-evaluate-a-new-language-model-aa5c526bacfd&user=Heiko+Hotz&userId=993c21f1b30f&source=post_page-993c21f1b30f----aa5c526bacfd---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----aa5c526bacfd--------------------------------) ·6 min 阅读·2023年7月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Faa5c526bacfd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-three-essential-methods-to-evaluate-a-new-language-model-aa5c526bacfd&user=Heiko+Hotz&userId=993c21f1b30f&source=-----aa5c526bacfd---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Faa5c526bacfd&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-three-essential-methods-to-evaluate-a-new-language-model-aa5c526bacfd&source=-----aa5c526bacfd---------------------bookmark_footer-----------)![](../Images/d44df1d02e0e695eedca24b1c6e2d15b.png)

作者提供的图像（使用 Stable Diffusion）

# 这是什么？

每周都会有新的LLM发布，如果你像我一样，你可能会问自己：这个模型是否终于适合我想要使用LLM的所有用例？在本教程中，我将分享我用来评估新LLM的技术。我将介绍我经常使用的三种技术——这些技术都不是新的（实际上，我会引用我之前写的博客文章），但通过将它们结合在一起，每当有新的LLM发布时，我能节省大量时间。我将演示在新的[OpenChat](https://huggingface.co/openchat/openchat_8192)模型上的测试示例。

# 为什么这很重要？

当涉及到新的LLM时，理解它们的能力和局限性非常重要。不幸的是，弄清楚如何部署模型并系统地进行测试可能会有点麻烦。这个过程通常是手动的，可能会消耗大量时间。然而，通过标准化的方法，我们可以更快地迭代，并快速判断一个模型是否值得投入更多时间，或者我们是否应该弃用它。所以，让我们开始吧。

# **入门指南**
