# **洞察力揭秘**：GPT 从图表和表格中提取意义

> 原文：[https://towardsdatascience.com/illuminating-insights-gpt-extracts-meaning-from-charts-and-tables-a0b71c991d34?source=collection_archive---------1-----------------------#2023-12-24](https://towardsdatascience.com/illuminating-insights-gpt-extracts-meaning-from-charts-and-tables-a0b71c991d34?source=collection_archive---------1-----------------------#2023-12-24)

## 使用 GPT 视觉来解释和汇总图像数据。

[](https://medium.com/@ilia.teimouri?source=post_page-----a0b71c991d34--------------------------------)[![Ilia Teimouri PhD](../Images/0eb948c4d3f81c116cd16fa4d5016629.png)](https://medium.com/@ilia.teimouri?source=post_page-----a0b71c991d34--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a0b71c991d34--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a0b71c991d34--------------------------------) [Ilia Teimouri PhD](https://medium.com/@ilia.teimouri?source=post_page-----a0b71c991d34--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbf9b9036159&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Filluminating-insights-gpt-extracts-meaning-from-charts-and-tables-a0b71c991d34&user=Ilia+Teimouri+PhD&userId=bf9b9036159&source=post_page-bf9b9036159----a0b71c991d34---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a0b71c991d34--------------------------------) · 7 分钟阅读 · 2023年12月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa0b71c991d34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Filluminating-insights-gpt-extracts-meaning-from-charts-and-tables-a0b71c991d34&user=Ilia+Teimouri+PhD&userId=bf9b9036159&source=-----a0b71c991d34---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa0b71c991d34&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Filluminating-insights-gpt-extracts-meaning-from-charts-and-tables-a0b71c991d34&source=-----a0b71c991d34---------------------bookmark_footer-----------)![](../Images/6f4c9df91feb7c242dca20cf31c52358.png)

照片由 [David Travis](https://unsplash.com/@dtravisphd) 拍摄，发布在 [Unsplash](https://unsplash.com)。

许多领域专家认为，将图像等视觉输入与文本和语音结合到大型语言模型（LLMs）中，是人工智能研究的一个重要新方向。通过增强这些模型以处理不仅仅是语言的数据模式，有可能显著扩展其应用范围，并提升其在现有自然语言处理任务中的整体智能和性能。

多模态人工智能的前景涵盖了更具互动性的用户体验，例如能够观察周围环境并参考周围物体的对话代理，以及能够将命令流畅转化为物理动作的机器人，这些机器人结合了语言和视觉的知识。通过将历史上分开的人工智能领域整合到统一的模型架构中，多模态性可能会加速依赖于多种技能的任务的进展，如视觉问答或图像描述。学习算法、数据类型和模型设计之间的协同作用可能会导致快速的技术进步。

许多公司已经以各种形式接受了多模态技术：[OpenAI](https://chat.openai.com)、[Anthropic](http://claude.ai)、Google（[Bard](https://bard.google.com) 和 [Gemini](https://deepmind.google/technologies/gemini/#introduction)）允许你上传自己的图像或…
