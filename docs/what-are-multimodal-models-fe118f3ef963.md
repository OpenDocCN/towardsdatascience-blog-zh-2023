# 多模态模型是什么？

> 原文：[https://towardsdatascience.com/what-are-multimodal-models-fe118f3ef963?source=collection_archive---------2-----------------------#2023-10-16](https://towardsdatascience.com/what-are-multimodal-models-fe118f3ef963?source=collection_archive---------2-----------------------#2023-10-16)

## 赋予大型语言模型（LLMs）视觉能力！

[](https://medium.com/@omermx?source=post_page-----fe118f3ef963--------------------------------)[![Omer Mahmood](../Images/0c87da4134bea397c77bc4ba6640e34b.png)](https://medium.com/@omermx?source=post_page-----fe118f3ef963--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fe118f3ef963--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fe118f3ef963--------------------------------) [Omer Mahmood](https://medium.com/@omermx?source=post_page-----fe118f3ef963--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F62cd989987f6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-multimodal-models-fe118f3ef963&user=Omer+Mahmood&userId=62cd989987f6&source=post_page-62cd989987f6----fe118f3ef963---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----fe118f3ef963--------------------------------) ·6分钟阅读·2023年10月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffe118f3ef963&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-multimodal-models-fe118f3ef963&user=Omer+Mahmood&userId=62cd989987f6&source=-----fe118f3ef963---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffe118f3ef963&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-are-multimodal-models-fe118f3ef963&source=-----fe118f3ef963---------------------bookmark_footer-----------)![](../Images/4b7fb0fc89f35adbb6b9d33055d2f0d0.png)

[Mecari 文本与图像嵌入演示](https://atlas.nomic.ai/map/vertex-mercari)的截图，运行在 Nomic 的 Atlas 上。

## 这篇文章适合谁？

+   **读者群体 [🟢⚪️⚪️]:** 人工智能初学者，熟悉流行概念、模型及其应用

+   **水平 [🟢🟢️⚪️]:** 中级话题

+   **复杂性 [🟢⚪️⚪️]:** 易于理解，没有数学公式或复杂理论

# ❓为什么这很重要

基础的大型语言模型（LLMs），在巨大的数据集上进行预训练，非常高效地处理通用的多任务，通过零-shot、few-shot 或迁移学习进行提示。

确实，像[PaLM2](https://ai.google/discover/palm2/)和[GPT4](https://openai.com/research/gpt-4)这样的模型已经彻底改变了我们与计算机**使用文本作为输入**的交互方式，但…

+   如果有一种方法可以扩展这些模型的智能，通过使它们能够使用不同的输入模态，如照片、音频和视频，**换句话说，让它们变得多模态！**

+   这可以大大改善我们在网上搜索东西的方式，甚至理解我们周围的世界，例如在现实世界应用中，如医学和病理学。

+   有解决方案！多模态深度学习模型可以结合来自不同类型输入的嵌入，使得例如一个 LLM 能够“看到”什么……
