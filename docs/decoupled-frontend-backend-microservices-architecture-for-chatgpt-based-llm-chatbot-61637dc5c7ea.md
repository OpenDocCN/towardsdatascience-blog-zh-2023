# 解耦前端 — 基于 ChatGPT 的 LLM 聊天机器人后端微服务架构

> 原文：[https://towardsdatascience.com/decoupled-frontend-backend-microservices-architecture-for-chatgpt-based-llm-chatbot-61637dc5c7ea?source=collection_archive---------7-----------------------#2023-05-24](https://towardsdatascience.com/decoupled-frontend-backend-microservices-architecture-for-chatgpt-based-llm-chatbot-61637dc5c7ea?source=collection_archive---------7-----------------------#2023-05-24)

## **使用 Streamlit、FastAPI 和 OpenAI API 构建无头 ChatGPT 应用程序的实用指南**

[](https://stephen-leo.medium.com/?source=post_page-----61637dc5c7ea--------------------------------)[![Marie Stephen Leo](../Images/c5669d884da5ff5c965f98904257d379.png)](https://stephen-leo.medium.com/?source=post_page-----61637dc5c7ea--------------------------------)[](https://towardsdatascience.com/?source=post_page-----61637dc5c7ea--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----61637dc5c7ea--------------------------------) [Marie Stephen Leo](https://stephen-leo.medium.com/?source=post_page-----61637dc5c7ea--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F954c0bee6530&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoupled-frontend-backend-microservices-architecture-for-chatgpt-based-llm-chatbot-61637dc5c7ea&user=Marie+Stephen+Leo&userId=954c0bee6530&source=post_page-954c0bee6530----61637dc5c7ea---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----61637dc5c7ea--------------------------------) · 8 分钟阅读 · 2023 年 5 月 24 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F61637dc5c7ea&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdecoupled-frontend-backend-microservices-architecture-for-chatgpt-based-llm-chatbot-61637dc5c7ea&source=-----61637dc5c7ea---------------------bookmark_footer-----------)![](../Images/64f56820c6449617c8f28c080da1275e.png)

作者使用 Midjourney V5.1 生成的图像，提示词：“解耦前端后端软件应用程序”

在[我的上一篇文章](https://medium.com/towards-data-science/anatomy-of-llm-based-chatbot-applications-monolithic-vs-microservice-architectural-patterns-77796216903e)中，我讨论了基于LLM的聊天机器人应用中单体和微服务架构模式的区别。选择微服务架构模式的一个重要优势是可以将前端代码与数据科学逻辑分离，这样数据科学家可以专注于数据科学逻辑，而不用担心前端代码。在本文中，我将向您展示如何使用Streamlit、FastAPI和OpenAI API构建一个微服务聊天机器人应用。我们将前端和后端代码解耦，以便轻松地将一个前端框架如React、Swift、Dash、Gradio等替换为另一个。

首先，创建一个新的conda环境并安装必要的库。

```py
# Create and activate a conda environment
conda create -n openai_chatbot python=3.10
conda activate openai_chatbot

# Install the necessary libraries
pip install ipykernel streamlit "fastapi[all]" openai
```

# **后端：数据科学逻辑**
