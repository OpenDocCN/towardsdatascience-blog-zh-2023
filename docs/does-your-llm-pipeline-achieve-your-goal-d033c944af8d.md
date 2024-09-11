# 你的LLM管道达到了你的目标吗？

> 原文：[https://towardsdatascience.com/does-your-llm-pipeline-achieve-your-goal-d033c944af8d?source=collection_archive---------13-----------------------#2023-07-20](https://towardsdatascience.com/does-your-llm-pipeline-achieve-your-goal-d033c944af8d?source=collection_archive---------13-----------------------#2023-07-20)

## *探索在LLM管道中最重要的评估内容及其测量方法。*

[](https://medium.com/@robertdegraaf78?source=post_page-----d033c944af8d--------------------------------)[![Robert de Graaf](../Images/ec3fbe8876fd9110a0455f9d1d6fe548.png)](https://medium.com/@robertdegraaf78?source=post_page-----d033c944af8d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d033c944af8d--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d033c944af8d--------------------------------) [Robert de Graaf](https://medium.com/@robertdegraaf78?source=post_page-----d033c944af8d--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2c24006124e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-your-llm-pipeline-achieve-your-goal-d033c944af8d&user=Robert+de+Graaf&userId=2c24006124e3&source=post_page-2c24006124e3----d033c944af8d---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d033c944af8d--------------------------------) ·8分钟阅读·2023年7月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd033c944af8d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-your-llm-pipeline-achieve-your-goal-d033c944af8d&user=Robert+de+Graaf&userId=2c24006124e3&source=-----d033c944af8d---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd033c944af8d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdoes-your-llm-pipeline-achieve-your-goal-d033c944af8d&source=-----d033c944af8d---------------------bookmark_footer-----------)![](../Images/f033bb7083e947a54ac9c1084bd68a11.png)

AI照片由 [Piret Ilver](https://unsplash.com/es/@saltsup?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/98MbUldcDJY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

有效实施LLM管道的一个关键要素是评估管道的效果。也就是说，你需要评估最终输出，这不仅仅是LLM本身或提示的产物，还包括LLM、提示以及设置（如温度或最小和最大token）的相互作用。

考虑访问GPT API的示例代码（自动生成）：

```py
import os
import openai

openai.api_key = os.getenv("OPENAI_API_KEY")

response = openai.ChatCompletion.create(
  model="gpt-3.5-turbo",
  messages=[],
  temperature=1,
  max_tokens=256,
  top_p=1,
  frequency_penalty=0,
  presence_penalty=0
)
```

函数中有七个参数用于创建‘response’，每个参数都会影响最终输出。能够选择这些输出的最佳组合取决于能够评估和区分由这些参数的不同值产生的输出。

这是一个与LLM评估不同的问题，LLM评估通常出现在论文或LLM制造商的网站上。虽然你可能正在使用一个能够通过律师资格考试或类似测试的LLM……
