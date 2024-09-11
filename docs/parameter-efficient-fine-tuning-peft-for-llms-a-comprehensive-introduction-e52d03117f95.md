# 对大语言模型（LLMs）进行参数高效微调（PEFT）：全面介绍

> 原文：[`towardsdatascience.com/parameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95?source=collection_archive---------1-----------------------#2023-08-22`](https://towardsdatascience.com/parameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95?source=collection_archive---------1-----------------------#2023-08-22)

## 一项关于 Hugging Face、谷歌的 Vertex AI 以及最终 OpenAI 使用的 PEFT 方法的概念调查

[](https://medium.com/@smsmith714?source=post_page-----e52d03117f95--------------------------------)![Sean Smith](https://medium.com/@smsmith714?source=post_page-----e52d03117f95--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e52d03117f95--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e52d03117f95--------------------------------) [Sean Smith](https://medium.com/@smsmith714?source=post_page-----e52d03117f95--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6957f6523097&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95&user=Sean+Smith&userId=6957f6523097&source=post_page-6957f6523097----e52d03117f95---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----e52d03117f95--------------------------------) ·19 分钟阅读·2023 年 8 月 22 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe52d03117f95&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95&source=-----e52d03117f95---------------------bookmark_footer-----------)![](img/425a0569c2f36a07f176e6742754e25f.png)

由 DALL-E 创建的图像。《大岛上的一个星期日下午》，但每个人都是类人。

大型语言模型（LLMs）的名字中含有“大型”二字。这些模型通常拥有从 70 亿到 700 亿不等的参数。要完全加载一个 700 亿参数的模型需要 280 GB 的 GPU 内存！训练这个模型时，你需要在数百万或数十亿个文档中更新数十亿个标记。更新这些参数所需的计算量是巨大的。这些模型的自监督训练非常昂贵，[公司花费高达 1 亿美元](https://www.wired.com/story/openai-ceo-sam-altman-the-age-of-giant-ai-models-is-already-over/)。

对于我们其他人来说，如何将我们的数据适应这些模型是一个重要的问题。面对有限的数据集（相比之下）和不足的计算能力，我们如何在更低成本的情况下创建可以超越主要玩家的模型？

这就是参数高效微调（PEFT）研究领域发挥作用的地方。通过各种技术，我们将很快详细探讨这些技术，我们可以增强这些模型的小部分，使其更好地适应我们要完成的任务。

阅读完这篇文章后，你将对 Hugging Face 中应用的每种 PEFT 技术有一个概念上的把握，并且……
