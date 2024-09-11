# 利用猎鹰 40B 模型，这是最强大的开源 LLM

> 原文：[`towardsdatascience.com/harnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10?source=collection_archive---------0-----------------------#2023-06-09`](https://towardsdatascience.com/harnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10?source=collection_archive---------0-----------------------#2023-06-09)

## 掌握开源语言模型：深入探讨猎鹰-40B

[](https://medium.com/@luisroque?source=post_page-----f70010bc8a10--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----f70010bc8a10--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f70010bc8a10--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f70010bc8a10--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----f70010bc8a10--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----f70010bc8a10---------------------post_header-----------) 发布在[向数据科学进发](https://towardsdatascience.com/?source=post_page-----f70010bc8a10--------------------------------) ·12 min read·Jun 9, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff70010bc8a10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----f70010bc8a10---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff70010bc8a10&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fharnessing-the-falcon-40b-model-the-most-powerful-open-source-llm-f70010bc8a10&source=-----f70010bc8a10---------------------bookmark_footer-----------)

# **介绍**

AI 行业的重点已转向构建更强大、更大规模的语言模型，这些模型可以理解和生成类似人类的文本。像 OpenAI 的 GPT-3 这样的模型引领了这一方向，展示了卓越的能力。OpenAI 的座右铭曾经是使这些模型开源。不幸的是，他们决定走另一条道路，新的模型如 ChatGPT（或 GPT-3.5）和 GPT-4 现在是闭源的。专有性质和有限的访问权限推动了许多研究人员和开发者寻找开源替代品并为之做出贡献。

这就是**Falcon-40B**的意义所在。上周末，技术创新研究所（TII）宣布 Falcon-40B 现已对商业和研究使用免收版税。因此，它打破了专有模型的障碍，给开发者和研究人员提供了一个最先进的语言模型，他们可以根据自己的特定需求使用和修改它。

此外，Falcon-40B 模型现在是[OpenLLM 排行榜](https://huggingface.co/spaces/HuggingFaceH4/open_llm_leaderboard)上表现最好的模型，超越了 LLaMA、StableLM、RedPajama 和 MPT 等模型。这个排行榜旨在追踪、排名和评估各种 LLM 和聊天机器人的表现，提供一个清晰、客观的衡量指标。
