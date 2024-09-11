# 大型语言模型中的解码策略

> 原文：[https://towardsdatascience.com/decoding-strategies-in-large-language-models-9733a8f70539?source=collection_archive---------0-----------------------#2023-06-04](https://towardsdatascience.com/decoding-strategies-in-large-language-models-9733a8f70539?source=collection_archive---------0-----------------------#2023-06-04)

## 从束搜索到核采样的文本生成指南

[](https://medium.com/@mlabonne?source=post_page-----9733a8f70539--------------------------------)[![Maxime Labonne](../Images/a7efdd305e3cc77d5509bbb1076d57d8.png)](https://medium.com/@mlabonne?source=post_page-----9733a8f70539--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9733a8f70539--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9733a8f70539--------------------------------) [Maxime Labonne](https://medium.com/@mlabonne?source=post_page-----9733a8f70539--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdc89da634938&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-strategies-in-large-language-models-9733a8f70539&user=Maxime+Labonne&userId=dc89da634938&source=post_page-dc89da634938----9733a8f70539---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9733a8f70539--------------------------------) ·15分钟阅读·2023年6月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9733a8f70539&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-strategies-in-large-language-models-9733a8f70539&user=Maxime+Labonne&userId=dc89da634938&source=-----9733a8f70539---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9733a8f70539&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoding-strategies-in-large-language-models-9733a8f70539&source=-----9733a8f70539---------------------bookmark_footer-----------)![](../Images/8e3084bd4e009d7fb0c6ec5d7aef4aa6.png)

图片由作者提供。

在大型语言模型（LLMs）迷人的世界中，模型架构、数据处理和优化受到广泛关注。然而，像束搜索这样的解码策略在文本生成中扮演着关键角色，但常被忽视。本文将深入探讨LLMs如何生成文本，通过剖析贪婪搜索和束搜索的机制，以及使用top-k和核采样的采样技术。

到本文结束时，你将不仅彻底理解这些解码策略，还会熟悉如何处理温度、num_beams、top_k和top_p等重要超参数。

本文的代码可以在[GitHub](https://github.com/mlabonne/llm-course/blob/main/Decoding_Strategies_in_Large_Language%C2%A0Models.ipynb)和[Google Colab](https://colab.research.google.com/drive/19CJlOS5lI29g-B3dziNn93Enez1yiHk2?usp=sharing)中找到，供参考和进一步探索。

# 📚 背景

为了开始，让我们用一个例子来讲解。我们将把文本“I have a dream”输入到GPT-2模型中，并要求它生成接下来的五个标记（词或子词）。

```py
from transformers import GPT2LMHeadModel, GPT2Tokenizer
import torch

device = 'cuda' if torch.cuda.is_available() else 'cpu'
model = GPT2LMHeadModel.from_pretrained('gpt2').to(device)
tokenizer =…
```
