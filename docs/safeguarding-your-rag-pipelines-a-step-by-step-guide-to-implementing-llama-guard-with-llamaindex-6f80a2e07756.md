# 保护您的 RAG 管道：实施 Llama Guard 和 LlamaIndex 的逐步指南

> 原文：[`towardsdatascience.com/safeguarding-your-rag-pipelines-a-step-by-step-guide-to-implementing-llama-guard-with-llamaindex-6f80a2e07756?source=collection_archive---------2-----------------------#2023-12-27`](https://towardsdatascience.com/safeguarding-your-rag-pipelines-a-step-by-step-guide-to-implementing-llama-guard-with-llamaindex-6f80a2e07756?source=collection_archive---------2-----------------------#2023-12-27)

## 如何将 Llama Guard 添加到您的 RAG 管道中，以调节 LLM 输入和输出，并防止提示注入

[](https://medium.com/@wenqiglantz?source=post_page-----6f80a2e07756--------------------------------)![Wenqi Glantz](https://medium.com/@wenqiglantz?source=post_page-----6f80a2e07756--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6f80a2e07756--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6f80a2e07756--------------------------------) [Wenqi Glantz](https://medium.com/@wenqiglantz?source=post_page-----6f80a2e07756--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fce7cd5b8b74a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsafeguarding-your-rag-pipelines-a-step-by-step-guide-to-implementing-llama-guard-with-llamaindex-6f80a2e07756&user=Wenqi+Glantz&userId=ce7cd5b8b74a&source=post_page-ce7cd5b8b74a----6f80a2e07756---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6f80a2e07756--------------------------------) ·15 分钟阅读·2023 年 12 月 27 日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6f80a2e07756&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fsafeguarding-your-rag-pipelines-a-step-by-step-guide-to-implementing-llama-guard-with-llamaindex-6f80a2e07756&source=-----6f80a2e07756---------------------bookmark_footer-----------)![](img/50c09645ace0257d68c3faa039b9ec07.png)

图像由作者使用 DALL-E 3 生成

LLM 安全是我们都知道值得充分关注的领域。渴望采用生成性 AI 的组织，无论大小，都面临着确保 LLM 应用安全的巨大挑战。如何对抗提示注入、处理不安全的输出以及防止敏感信息泄露，都是每个 AI 架构师和工程师需要解决的紧迫问题。企业级生产环境的 LLM 应用如果没有坚实的解决方案来解决 LLM 安全问题，将无法在实际环境中生存。

Llama Guard，由 Meta 于 2023 年 12 月 7 日开源，提供了一种有效的解决方案来应对 LLM 输入输出漏洞和对抗提示注入。Llama Guard 属于[紫色 Llama](https://about.fb.com/news/2023/12/purple-llama-safe-responsible-ai-development/)项目，“提供开放的信任和安全工具及评估，旨在为开发者负责任地部署生成性 AI 模型提供公平竞争环境。”[1]

我们一个月前探讨了[OWASP LLM 应用的十大漏洞](https://medium.com/gitconnected/security-driven-development-with-owasp-top-10-for-llm-applications-588406f40d4c?sk=dde699f26d74e8bcfb1ea2c4488b62e5)。有了 Llama Guard，我们现在有了一个相当合理的解决方案来开始解决这些十大漏洞中的一些，具体包括：
