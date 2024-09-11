# GPTQ 或 bitsandbytes：LLMs 的量化方法选择 — Llama 2 示例

> 原文：[https://towardsdatascience.com/gptq-or-bitsandbytes-which-quantization-method-to-use-for-llms-examples-with-llama-2-f79bc03046dc?source=collection_archive---------2-----------------------#2023-08-25](https://towardsdatascience.com/gptq-or-bitsandbytes-which-quantization-method-to-use-for-llms-examples-with-llama-2-f79bc03046dc?source=collection_archive---------2-----------------------#2023-08-25)

## 大型语言模型量化：实现经济实惠的微调和计算机推理

[](https://medium.com/@bnjmn_marie?source=post_page-----f79bc03046dc--------------------------------)[![Benjamin Marie](../Images/3ea1ad230cb1e67610418a8e36a5e5dd.png)](https://medium.com/@bnjmn_marie?source=post_page-----f79bc03046dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f79bc03046dc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f79bc03046dc--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----f79bc03046dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgptq-or-bitsandbytes-which-quantization-method-to-use-for-llms-examples-with-llama-2-f79bc03046dc&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----f79bc03046dc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f79bc03046dc--------------------------------) ·7 分钟阅读·2023年8月25日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff79bc03046dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgptq-or-bitsandbytes-which-quantization-method-to-use-for-llms-examples-with-llama-2-f79bc03046dc&user=Benjamin+Marie&userId=ad2a414578b3&source=-----f79bc03046dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff79bc03046dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgptq-or-bitsandbytes-which-quantization-method-to-use-for-llms-examples-with-llama-2-f79bc03046dc&source=-----f79bc03046dc---------------------bookmark_footer-----------)![](../Images/ff5993f4f2ee78297a0c1cd107099ea9.png)

作者插图 — 制作自 [Pixabay](https://pixabay.com/vectors/llama-alpaca-animal-mammal-zoo-297668/)

随着大型语言模型（LLM）参数的增多，提出了新的技术来减少其内存使用。

减少内存中模型大小的最有效方法之一是**量化**。你可以将量化视为对大型语言模型（LLM）的一种压缩技术。实际上，量化的主要目标是降低LLM权重的精度，通常从16位降到8位、4位，甚至3位，同时尽量减少性能下降。

目前有两种流行的LLM量化方法：GPTQ和bitsandbytes。

在这篇文章中，我讨论了这两种方法之间的主要区别。它们各自有自己的优缺点，使其适用于不同的使用场景。我展示了使用Llama 2的内存使用情况和推理速度的对比。我还根据以往工作的实验结果讨论了它们的性能。

*注意：如果你想了解更多关于量化的信息，我推荐阅读* [*Maxime Labonne*](https://medium.com/u/dc89da634938?source=post_page-----f79bc03046dc--------------------------------)*的这篇优秀介绍：*
