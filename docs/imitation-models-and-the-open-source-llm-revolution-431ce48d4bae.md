# 模仿模型与开源 LLM 革命

> 原文：[https://towardsdatascience.com/imitation-models-and-the-open-source-llm-revolution-431ce48d4bae?source=collection_archive---------2-----------------------#2023-09-27](https://towardsdatascience.com/imitation-models-and-the-open-source-llm-revolution-431ce48d4bae?source=collection_archive---------2-----------------------#2023-09-27)

## 像 ChatGPT 和 GPT-4 这样的专有 LLM 实际上是否容易被复制？

[](https://wolfecameron.medium.com/?source=post_page-----431ce48d4bae--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----431ce48d4bae--------------------------------)[](https://towardsdatascience.com/?source=post_page-----431ce48d4bae--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----431ce48d4bae--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----431ce48d4bae--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimitation-models-and-the-open-source-llm-revolution-431ce48d4bae&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----431ce48d4bae---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----431ce48d4bae--------------------------------) ·15 分钟阅读·2023年9月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F431ce48d4bae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimitation-models-and-the-open-source-llm-revolution-431ce48d4bae&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----431ce48d4bae---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F431ce48d4bae&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimitation-models-and-the-open-source-llm-revolution-431ce48d4bae&source=-----431ce48d4bae---------------------bookmark_footer-----------)![](../Images/f8709e8cc037ca4e582e6581004111d2.png)

（图片由 [Tanbir Mahmud](https://unsplash.com/@photo_tanbir?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/oyJKjAzAcbU?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

LLaMA套件 [2] 大型语言模型（LLMs）的提议引发了关于开源LLMs的出版物激增。在许多情况下，这些工作的目标是以低成本生产质量可与像 [ChatGPT](https://openai.com/blog/chatgpt) 和 [GPT-4](https://openai.com/research/gpt-4) 这样的专有模型相当的小型开源LLMs（用于研究目的）。这些模型采用了模仿策略，通过从更强大的LLM中获取的合成对话数据来微调基础LLM。尽管训练成本低，这些模型似乎与专有LLMs（如ChatGPT）表现相当。因此，深度学习研究社区很快接受了开源LLMs将主宰未来的观点 — *再现专有模型的开源变体既简单又经济高效*!

> “最强大的LLMs会是闭源的，还是会自由分发供任何人使用、修改和扩展？” *— 来自 [1]*

不幸的是，对这些模型的初步评估依赖于其他LLMs（例如，GPT-4）或人工众包工作者提供的评分，评估过程有些粗略。*模仿模型的表现是否真正与像ChatGPT这样的模型相匹配？* 为了更严格地回答这个问题，我们将…
