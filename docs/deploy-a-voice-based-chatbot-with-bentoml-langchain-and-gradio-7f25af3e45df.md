# 了解如何使用 Langchain 和 BentoML 构建和部署语音聊天机器人

> 原文：[`towardsdatascience.com/deploy-a-voice-based-chatbot-with-bentoml-langchain-and-gradio-7f25af3e45df?source=collection_archive---------4-----------------------#2023-05-02`](https://towardsdatascience.com/deploy-a-voice-based-chatbot-with-bentoml-langchain-and-gradio-7f25af3e45df?source=collection_archive---------4-----------------------#2023-05-02)

## BentoML 对于机器学习工程师来说就像乐高

[](https://ahmedbesbes.medium.com/?source=post_page-----7f25af3e45df--------------------------------)![Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----7f25af3e45df--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7f25af3e45df--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f25af3e45df--------------------------------) [Ahmed Besbes](https://ahmedbesbes.medium.com/?source=post_page-----7f25af3e45df--------------------------------)

·

[点击这里](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fadc8ea174c69&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-a-voice-based-chatbot-with-bentoml-langchain-and-gradio-7f25af3e45df&user=Ahmed+Besbes&userId=adc8ea174c69&source=post_page-adc8ea174c69----7f25af3e45df---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7f25af3e45df--------------------------------) · 11 分钟阅读 · 2023 年 5 月 2 日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7f25af3e45df&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeploy-a-voice-based-chatbot-with-bentoml-langchain-and-gradio-7f25af3e45df&source=-----7f25af3e45df---------------------bookmark_footer-----------)![](img/62bdc77c059993e827235577879e894c.png)

图片由 [Jason Leung](https://unsplash.com/@ninjason?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将引导你完成构建一个基于语音的 ChatGPT 克隆的过程，该克隆依赖于 OpenAI API，并使用 Wikipedia 作为额外的数据源。

为了构建和部署这个应用程序，我们将使用 [BentoML](https://github.com/bentoml/BentoML)：一个用于模型服务和部署的 Python 框架。

BentoML 不仅帮助你构建连接到第三方专有 API 的服务。它还通过将这些服务与其他开源模型结合起来，增强了这些服务，形成了复杂而强大的推理图。

实际上，我们将要构建的应用程序将包含由来自 HuggingFace [hub](https://huggingface.co/)的不同模型处理的**语音转文本**和**文本转语音**任务，以及由[**LangChain**](https://langchain.readthedocs.io/)管理的 LLM 任务。

在本地测试项目后，我们将其推送到 BentoCloud，这是一个简化版本控制、跟踪和将 ML 服务部署到云端的过程的平台。

> ***到文章结束时，你应该全面了解使用 BentoML 构建和部署多模型服务的知识。你还将学习一些使模型工业化变得更容易的具体功能。***
