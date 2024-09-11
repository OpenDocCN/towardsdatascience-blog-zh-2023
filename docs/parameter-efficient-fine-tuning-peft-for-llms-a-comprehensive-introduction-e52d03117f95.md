# 对大语言模型（LLMs）进行参数高效微调（PEFT）：全面介绍

> 原文：[https://towardsdatascience.com/parameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95?source=collection_archive---------1-----------------------#2023-08-22](https://towardsdatascience.com/parameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95?source=collection_archive---------1-----------------------#2023-08-22)

## 一项关于Hugging Face、谷歌的Vertex AI以及最终OpenAI使用的PEFT方法的概念调查

[](https://medium.com/@smsmith714?source=post_page-----e52d03117f95--------------------------------)[![Sean Smith](../Images/611395d113b10ec4bbfaf781301139c7.png)](https://medium.com/@smsmith714?source=post_page-----e52d03117f95--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e52d03117f95--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e52d03117f95--------------------------------) [Sean Smith](https://medium.com/@smsmith714?source=post_page-----e52d03117f95--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6957f6523097&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95&user=Sean+Smith&userId=6957f6523097&source=post_page-6957f6523097----e52d03117f95---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----e52d03117f95--------------------------------) ·19分钟阅读·2023年8月22日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe52d03117f95&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fparameter-efficient-fine-tuning-peft-for-llms-a-comprehensive-introduction-e52d03117f95&source=-----e52d03117f95---------------------bookmark_footer-----------)![](../Images/425a0569c2f36a07f176e6742754e25f.png)

由DALL-E创建的图像。《大岛上的一个星期日下午》，但每个人都是类人。

大型语言模型（LLMs）的名字中含有“大型”二字。这些模型通常拥有从70亿到700亿不等的参数。要完全加载一个700亿参数的模型需要280 GB的GPU内存！训练这个模型时，你需要在数百万或数十亿个文档中更新数十亿个标记。更新这些参数所需的计算量是巨大的。这些模型的自监督训练非常昂贵，[公司花费高达1亿美元](https://www.wired.com/story/openai-ceo-sam-altman-the-age-of-giant-ai-models-is-already-over/)。

对于我们其他人来说，如何将我们的数据适应这些模型是一个重要的问题。面对有限的数据集（相比之下）和不足的计算能力，我们如何在更低成本的情况下创建可以超越主要玩家的模型？

这就是参数高效微调（PEFT）研究领域发挥作用的地方。通过各种技术，我们将很快详细探讨这些技术，我们可以增强这些模型的小部分，使其更好地适应我们要完成的任务。

阅读完这篇文章后，你将对Hugging Face中应用的每种PEFT技术有一个概念上的把握，并且……
