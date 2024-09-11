# 哪种量化方法适合你？（GPTQ vs. GGUF vs. AWQ）

> 原文：[https://towardsdatascience.com/which-quantization-method-is-right-for-you-gptq-vs-gguf-vs-awq-c4cd9d77d5be?source=collection_archive---------2-----------------------#2023-11-14](https://towardsdatascience.com/which-quantization-method-is-right-for-you-gptq-vs-gguf-vs-awq-c4cd9d77d5be?source=collection_archive---------2-----------------------#2023-11-14)

## 探索预量化的大型语言模型

[](https://medium.com/@maartengrootendorst?source=post_page-----c4cd9d77d5be--------------------------------)[![Maarten Grootendorst](../Images/58e24b9cf7e10ff1cd5ffd75a32d1a26.png)](https://medium.com/@maartengrootendorst?source=post_page-----c4cd9d77d5be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c4cd9d77d5be--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c4cd9d77d5be--------------------------------) [Maarten Grootendorst](https://medium.com/@maartengrootendorst?source=post_page-----c4cd9d77d5be--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F22405c3b2875&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhich-quantization-method-is-right-for-you-gptq-vs-gguf-vs-awq-c4cd9d77d5be&user=Maarten+Grootendorst&userId=22405c3b2875&source=post_page-22405c3b2875----c4cd9d77d5be---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c4cd9d77d5be--------------------------------) ·11分钟阅读·2023年11月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc4cd9d77d5be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhich-quantization-method-is-right-for-you-gptq-vs-gguf-vs-awq-c4cd9d77d5be&user=Maarten+Grootendorst&userId=22405c3b2875&source=-----c4cd9d77d5be---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc4cd9d77d5be&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhich-quantization-method-is-right-for-you-gptq-vs-gguf-vs-awq-c4cd9d77d5be&source=-----c4cd9d77d5be---------------------bookmark_footer-----------)![](../Images/c45770a07b8d0685c7ab123f15862789.png)

在过去的一年里，我们见证了大型语言模型（LLMs）的“狂野西部”。新技术和模型发布的速度令人震惊！因此，我们拥有许多不同的标准和处理LLM的方法。

在这篇文章中，我们将**深入探讨**一个话题，即通过几种（量化）标准来加载你的本地LLM。由于分片、量化以及不同的保存和压缩策略，使得选择适合你的方法变得不那么简单。

在示例过程中，我们将使用[Zephyr 7B](https://huggingface.co/HuggingFaceH4/zephyr-7b-beta)，这是Mistral 7B的一个精调版本，经过[Direct Preference Optimization](https://arxiv.org/abs/2305.18290)（DPO）训练。

🔥 **提示**：在加载每个LLM示例后，建议重新启动笔记本，以防止出现OutOfMemory错误。加载多个LLM需要大量的RAM/VRAM。你可以通过删除模型并重置缓存来重置内存，如下所示：

```py
# Delete any models previously created
del model, tokenizer, pipe

# Empty VRAM cache
import torch
torch.cuda.empty_cache()
```

你还可以参考[**Google Colab Notebook**](https://colab.research.google.com/drive/1rt318Ew-5dDw21YZx2zK2vnxbsuDAchH?usp=sharing)以确保一切按预期工作。
