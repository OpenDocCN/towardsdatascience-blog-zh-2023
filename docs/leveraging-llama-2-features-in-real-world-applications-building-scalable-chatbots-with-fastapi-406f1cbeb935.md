# 在实际应用中利用 Llama 2 特性：使用 FastAPI、Celery、Redis 和 Docker 构建可扩展的聊天机器人

> 原文：[`towardsdatascience.com/leveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935?source=collection_archive---------0-----------------------#2023-07-24`](https://towardsdatascience.com/leveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935?source=collection_archive---------0-----------------------#2023-07-24)

## 深入探讨：开放源代码与封闭源代码 LLM，揭开 Llama 2 的独特特性，掌握提示工程的艺术，以及使用 FastAPI、Celery、Redis 和 Docker 设计稳健的解决方案

[](https://medium.com/@luisroque?source=post_page-----406f1cbeb935--------------------------------)![Luís Roque](https://medium.com/@luisroque?source=post_page-----406f1cbeb935--------------------------------)[](https://towardsdatascience.com/?source=post_page-----406f1cbeb935--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----406f1cbeb935--------------------------------) [Luís Roque](https://medium.com/@luisroque?source=post_page-----406f1cbeb935--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F2195f049db86&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=post_page-2195f049db86----406f1cbeb935---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----406f1cbeb935--------------------------------) ·14 分钟阅读·2023 年 7 月 24 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F406f1cbeb935&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935&user=Lu%C3%ADs+Roque&userId=2195f049db86&source=-----406f1cbeb935---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F406f1cbeb935&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fleveraging-llama-2-features-in-real-world-applications-building-scalable-chatbots-with-fastapi-406f1cbeb935&source=-----406f1cbeb935---------------------bookmark_footer-----------)

# 介绍

出人意料的是，Meta 几天前将其大型语言模型（LLM）Llama 2 开源了，这一决定可能会重新塑造当前的 AI 开发格局。它为主要的企业如 OpenAI 和 Google 提供了一个替代方案，这些公司选择保持对其 AI 模型的严格控制，限制了可及性并阻碍了更广泛的创新。希望 Meta 的决定能激发开源社区的集体反应，抵消限制访问领域进展的趋势。Llama 2 的新许可证更进一步，允许商业使用，赋予开发者和企业在现有及新产品中利用该模型的机会。

Llama2 系列包括了预训练和微调的 LLMs，涵盖了 Llama2 和 Llama2-Chat，参数规模高达 70B。这些模型在各种基准测试中表现优于开源模型[1]。它们在与一些封闭源模型的对比中也保持了优势，提供了一个...
