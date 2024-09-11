# 基于LLM的聊天机器人应用的架构：单体式与微服务架构模式

> 原文：[https://towardsdatascience.com/anatomy-of-llm-based-chatbot-applications-monolithic-vs-microservice-architectural-patterns-77796216903e?source=collection_archive---------6-----------------------#2023-05-08](https://towardsdatascience.com/anatomy-of-llm-based-chatbot-applications-monolithic-vs-microservice-architectural-patterns-77796216903e?source=collection_archive---------6-----------------------#2023-05-08)

## 使用Streamlit、Huggingface和FastAPI构建单体式和微服务聊天机器人应用的实用指南

[](https://stephen-leo.medium.com/?source=post_page-----77796216903e--------------------------------)[![Marie Stephen Leo](../Images/c5669d884da5ff5c965f98904257d379.png)](https://stephen-leo.medium.com/?source=post_page-----77796216903e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----77796216903e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----77796216903e--------------------------------) [Marie Stephen Leo](https://stephen-leo.medium.com/?source=post_page-----77796216903e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F954c0bee6530&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanatomy-of-llm-based-chatbot-applications-monolithic-vs-microservice-architectural-patterns-77796216903e&user=Marie+Stephen+Leo&userId=954c0bee6530&source=post_page-954c0bee6530----77796216903e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----77796216903e--------------------------------) ·9分钟阅读·2023年5月8日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F77796216903e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanatomy-of-llm-based-chatbot-applications-monolithic-vs-microservice-architectural-patterns-77796216903e&user=Marie+Stephen+Leo&userId=954c0bee6530&source=-----77796216903e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F77796216903e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fanatomy-of-llm-based-chatbot-applications-monolithic-vs-microservice-architectural-patterns-77796216903e&source=-----77796216903e---------------------bookmark_footer-----------)![](../Images/5b4d2660c860f9c8391e33823a5f1824.png)

图片由作者使用Midjourney V5.1生成，提示词为：“等轴测高现实主义视角的笔记本电脑，屏幕上显示一个明亮的、多彩的魔方，内部有光线照射，明亮、温暖、愉快的照明。8k, hdr, unreal engine”

随着OpenAI的ChatGPT的出现，聊天机器人正迅速流行起来！每个企业都在寻找将ChatGPT融入其面向客户和内部应用的方式。此外，由于开源聊天机器人发展如此迅速，以至于[谷歌工程师似乎也得出结论认为它们与OpenAI“没有护城河”](https://www.semianalysis.com/p/google-we-have-no-moat-and-neither)，现在正是进入AI行业的最佳时机！

作为构建此类应用的数据科学家，其中一个关键决策是选择单体架构还是微服务架构。这两种架构各有优缺点；最终选择取决于业务的需求，例如可扩展性和与现有系统的集成难易程度。在这篇博客文章中，我们将通过使用Streamlit、Huggingface和FastAPI的实时代码示例来探讨这两种架构之间的差异！

首先，创建一个新的conda环境并安装必要的库。

```py
# Create and activate a conda environment
conda create -n hf_llm_chatbot python=3.9
conda activate hf_llm_chatbot

# Install the…
```
