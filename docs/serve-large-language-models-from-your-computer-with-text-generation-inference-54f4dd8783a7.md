# 从你的计算机上使用文本生成推理来服务大型语言模型

> 原文：[`towardsdatascience.com/serve-large-language-models-from-your-computer-with-text-generation-inference-54f4dd8783a7?source=collection_archive---------7-----------------------#2023-07-13`](https://towardsdatascience.com/serve-large-language-models-from-your-computer-with-text-generation-inference-54f4dd8783a7?source=collection_archive---------7-----------------------#2023-07-13)

## 使用 Falcon-7B 的指令版本的示例

[](https://medium.com/@bnjmn_marie?source=post_page-----54f4dd8783a7--------------------------------)![Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----54f4dd8783a7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----54f4dd8783a7--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f4dd8783a7--------------------------------) [Benjamin Marie](https://medium.com/@bnjmn_marie?source=post_page-----54f4dd8783a7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fad2a414578b3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fserve-large-language-models-from-your-computer-with-text-generation-inference-54f4dd8783a7&user=Benjamin+Marie&userId=ad2a414578b3&source=post_page-ad2a414578b3----54f4dd8783a7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----54f4dd8783a7--------------------------------) ·6 分钟阅读·2023 年 7 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F54f4dd8783a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fserve-large-language-models-from-your-computer-with-text-generation-inference-54f4dd8783a7&user=Benjamin+Marie&userId=ad2a414578b3&source=-----54f4dd8783a7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F54f4dd8783a7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fserve-large-language-models-from-your-computer-with-text-generation-inference-54f4dd8783a7&source=-----54f4dd8783a7---------------------bookmark_footer-----------)![](img/41e6b63543c42e19cc0b1e4ee23f95bf.png)

照片由 [Nana Dua](https://unsplash.com/@nanadua11?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

由于量化方法如 QLoRa 和[GPTQ](https://github.com/IST-DASLab/gptq)，现在可以在消费者硬件上本地运行非常大的语言模型（LLM）。

考虑到加载 LLM 所需的时间，我们可能还希望将 LLM 保存在内存中，以便即时查询并获得结果。如果你使用的是标准推理管道，你必须每次都重新加载模型。如果模型非常大，你可能需要等待几分钟才能生成输出。

有多种框架可以在服务器上托管 LLMs（本地或远程）。在我的博客上，我已经介绍了由 NVIDIA 开发的[极为优化的 Triton 推理服务器](https://medium.com/towards-data-science/deploy-your-local-gpt-server-with-triton-a825d528aa5d)，它能够服务多个 LLMs 并在 GPU 之间平衡负载。但如果你只有一张 GPU，并且想要在你的计算机上托管模型，使用 Triton 推理可能显得不太合适。

在这篇文章中，我介绍了一种名为 Text Generation Inference 的替代方案。一个更为简洁的框架，实现了运行和服务 LLMs 所需的所有基本功能，适用于消费级硬件。

阅读完这篇文章后，你将会在你的计算机上本地部署一个聊天模型/LLM，并且……
