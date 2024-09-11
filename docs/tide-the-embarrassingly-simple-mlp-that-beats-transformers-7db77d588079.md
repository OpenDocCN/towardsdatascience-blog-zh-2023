# TiDE：那个“令人尴尬”简单的MLP，击败了Transformers

> 原文：[https://towardsdatascience.com/tide-the-embarrassingly-simple-mlp-that-beats-transformers-7db77d588079?source=collection_archive---------1-----------------------#2023-12-08](https://towardsdatascience.com/tide-the-embarrassingly-simple-mlp-that-beats-transformers-7db77d588079?source=collection_archive---------1-----------------------#2023-12-08)

## 对TiDE的深度探索，包括使用Darts的实现，以及与DeepAR和TFT（一个Transformer架构）的实际应用案例比较

[](https://medium.com/@rjguedes?source=post_page-----7db77d588079--------------------------------)[![Rafael Guedes](../Images/b3d000b3bce0113d2b2727e84db04870.png)](https://medium.com/@rjguedes?source=post_page-----7db77d588079--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7db77d588079--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7db77d588079--------------------------------) [Rafael Guedes](https://medium.com/@rjguedes?source=post_page-----7db77d588079--------------------------------)

·

[查看](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2789d1da9c75&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftide-the-embarrassingly-simple-mlp-that-beats-transformers-7db77d588079&user=Rafael+Guedes&userId=2789d1da9c75&source=post_page-2789d1da9c75----7db77d588079---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7db77d588079--------------------------------) · 9分钟阅读 · 2023年12月8日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7db77d588079&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftide-the-embarrassingly-simple-mlp-that-beats-transformers-7db77d588079&user=Rafael+Guedes&userId=2789d1da9c75&source=-----7db77d588079---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7db77d588079&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftide-the-embarrassingly-simple-mlp-that-beats-transformers-7db77d588079&source=-----7db77d588079---------------------bookmark_footer-----------)

随着各行业的不断发展，准确预测的重要性已经成为无可谈判的资产，无论你是从事电子商务、医疗保健、零售还是农业。能够预见未来并相应地进行规划以克服未来的挑战，这种能力可以使你在竞争中脱颖而出，并在利润紧张、客户要求比以往任何时候都高的经济环境中蓬勃发展。

变压器架构在过去几年里一直是人工智能领域的热门话题，特别是由于它们在自然语言处理（NLP）中的成功，其中chatGPT作为一个引起所有人关注的成功案例，无论你是否对人工智能感兴趣。但是，NLP并不是变压器在超越最先进解决方案方面的唯一领域，计算机视觉中也有类似表现，例如Stable Diffusion及其变体。

但是，变压器（Transformers）能否在时间序列预测中超越最先进的模型？尽管已经做出了许多努力来开发用于时间序列预测的变压器，但似乎对于长期预测，简单的线性模型可以超越几种基于变压器的方法。
