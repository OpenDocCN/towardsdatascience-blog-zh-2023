# 实用提示工程

> 原文：[https://towardsdatascience.com/practical-prompt-engineering-74e96130abc4?source=collection_archive---------0-----------------------#2023-07-30](https://towardsdatascience.com/practical-prompt-engineering-74e96130abc4?source=collection_archive---------0-----------------------#2023-07-30)

## 成功提示LLM的技巧与窍门…

[](https://wolfecameron.medium.com/?source=post_page-----74e96130abc4--------------------------------)[![Cameron R. Wolfe, Ph.D.](../Images/52bb88d7cf1105501be2fae5ccbe7a03.png)](https://wolfecameron.medium.com/?source=post_page-----74e96130abc4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----74e96130abc4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----74e96130abc4--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----74e96130abc4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F28aa6026c553&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-prompt-engineering-74e96130abc4&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=post_page-28aa6026c553----74e96130abc4---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----74e96130abc4--------------------------------) ·15分钟阅读·2023年7月30日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F74e96130abc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-prompt-engineering-74e96130abc4&user=Cameron+R.+Wolfe%2C+Ph.D.&userId=28aa6026c553&source=-----74e96130abc4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F74e96130abc4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fpractical-prompt-engineering-74e96130abc4&source=-----74e96130abc4---------------------bookmark_footer-----------)![](../Images/e25abf0c0a3c6577a9a7741aa923ce0d.png)

（照片由[Jan Kahánek](https://unsplash.com/@honza_kahanek?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/photos/g3O5ZtRk2E4?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

由于其文本到文本的格式，大语言模型（LLMs）能够通过单一模型解决各种任务。这种能力最初通过零样本和少样本学习在 [GPT-2](https://cameronrwolfe.substack.com/p/language-models-gpt-and-gpt-2) 和 [GPT-3](https://cameronrwolfe.substack.com/i/88082618/language-models-are-few-shot-learners) [5, 6] 等模型中得到了展示。然而，当经过微调以符合人类的偏好和指令时，LLMs 变得更加吸引人，能够支持流行的生成应用程序，如 [编码助手](https://cameronrwolfe.substack.com/i/93578656/evaluating-large-language-models-trained-on-code)、[信息寻求对话代理](https://cameronrwolfe.substack.com/i/93578656/training-language-models-to-follow-instructions-with-human-feedback) 和 [基于聊天的搜索体验](http://microsoft.com/en-us/bing?form=MW00X7&ef_id=_k_Cj0KCQjwgLOiBhC7ARIsAIeetVB3LkqQ31NslKZ7qj1J1Sx3PYJltfeBZs6bYulrUtPSrChf8KLmmZMaAkoKEALw_wcB_k_&OCID=AIDcmmf8m4fdss_SEM__k_Cj0KCQjwgLOiBhC7ARIsAIeetVB3LkqQ31NslKZ7qj1J1Sx3PYJltfeBZs6bYulrUtPSrChf8KLmmZMaAkoKEALw_wcB_k_&gclid=Cj0KCQjwgLOiBhC7ARIsAIeetVB3LkqQ31NslKZ7qj1J1Sx3PYJltfeBZs6bYulrUtPSrChf8KLmmZMaAkoKEALw_wcB&ch=)。

由于大语言模型（LLMs）使得各种应用成为可能，它们在研究界和大众文化中迅速崭露头角。在这一过程中，我们也见证了一个新的、互补的领域的发展：*提示工程*。从高层次来看，LLMs 通过 *i)* 将文本（即提示）作为输入和 *ii)* 产生文本输出，从中我们可以提取出有用的信息（例如分类、总结、翻译等）。这种方法的灵活性是有益的。然而，同时，我们必须确定如何正确构造输入提示，以便 LLM 有最佳机会生成期望的输出。

提示工程是一门经验科学，研究不同提示策略如何用于优化 LLM...
