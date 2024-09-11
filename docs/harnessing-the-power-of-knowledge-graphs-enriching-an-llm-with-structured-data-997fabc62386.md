# 利用知识图谱的力量：用结构化数据丰富大型语言模型（LLM）

> 原文：[https://towardsdatascience.com/harnessing-the-power-of-knowledge-graphs-enriching-an-llm-with-structured-data-997fabc62386?source=collection_archive---------2-----------------------#2023-07-10](https://towardsdatascience.com/harnessing-the-power-of-knowledge-graphs-enriching-an-llm-with-structured-data-997fabc62386?source=collection_archive---------2-----------------------#2023-07-10)

![](../Images/e32186b4eaa2e36dee4c8c50e5110b8f.png)

## 创建知识图谱的逐步指南，并探索其提升大型语言模型（LLM）潜力的方式

[](https://stevehedden.medium.com/?source=post_page-----997fabc62386--------------------------------)[![Steve Hedden](../Images/af7bec4a191ab857eccd885dd89e88b4.png)](https://stevehedden.medium.com/?source=post_page-----997fabc62386--------------------------------)[](https://towardsdatascience.com/?source=post_page-----997fabc62386--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----997fabc62386--------------------------------) [Steve Hedden](https://stevehedden.medium.com/?source=post_page-----997fabc62386--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2c634ce75a74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-the-power-of-knowledge-graphs-enriching-an-llm-with-structured-data-997fabc62386&user=Steve+Hedden&userId=2c634ce75a74&source=post_page-2c634ce75a74----997fabc62386---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----997fabc62386--------------------------------) · 20 min 阅读 · 2023年7月10日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F997fabc62386&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-the-power-of-knowledge-graphs-enriching-an-llm-with-structured-data-997fabc62386&user=Steve+Hedden&userId=2c634ce75a74&source=-----997fabc62386---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F997fabc62386&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-the-power-of-knowledge-graphs-enriching-an-llm-with-structured-data-997fabc62386&source=-----997fabc62386---------------------bookmark_footer-----------)

*有关代码的详细内容，请参见笔记本* [*此处。*](https://github.com/SteveHedden/kg_llm/blob/main/SDKG.ipynb)

近年来，[大型语言模型](https://snorkel.ai/large-language-models-llms/)（LLMs）已经变得无处不在。也许最著名的LLM是ChatGPT，它由OpenAI于2022年11月发布。ChatGPT能够[生成创意](https://www.linkedin.com/pulse/generate-100-content-ideas-chat-gpt-mfon-akpan/)，[提供个性化推荐](https://bootcamp.uxdesign.cc/how-to-use-chatgpt-for-personalized-recommendations-840e01dcad89)，[理解复杂主题](https://medium.com/101-innovation-hacks/using-chatgpt-to-explain-complex-concepts-2ea6aba97cf3)，[充当写作助手](https://chatgptwriter.ai/)，或者[帮助你建立一个预测奥斯卡奖的模型。](https://medium.com/design-bootcamp/using-chatgpt-to-predict-the-oscars-c6d8cdb6b3a0) Meta宣布了他们自己的LLM，叫做[LLaMA](https://ai.meta.com/blog/large-language-model-llama-meta-ai/)，Google则有[LaMDA](https://blog.google/technology/ai/lamda/)，甚至还有一个开源替代品，[BLOOM。](https://huggingface.co/bigscience/bloom)

LLMs在自然语言处理（NLP）任务中表现出色，例如上述任务，因为LLMs历来关注于[非结构化数据](https://en.wikipedia.org/wiki/Unstructured_data)—即没有预定义结构的数据，通常以文本为主。我问ChatGPT，“为什么LLMs历来关注于非结构化数据？”回答是：

> “LLMs历来关注于非结构化数据，这主要是由于其丰富性、可获得性以及所带来的挑战。非结构化数据提供了大量的训练语言模型的资源，使它们能够学习模式、上下文和语义。LLMs在处理…
