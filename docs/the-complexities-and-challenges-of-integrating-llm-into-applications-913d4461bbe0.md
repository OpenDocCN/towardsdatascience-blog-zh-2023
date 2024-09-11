# 将大型语言模型（LLMs）集成到应用中的复杂性与挑战

> 原文：[`towardsdatascience.com/the-complexities-and-challenges-of-integrating-llm-into-applications-913d4461bbe0?source=collection_archive---------0-----------------------#2023-07-08`](https://towardsdatascience.com/the-complexities-and-challenges-of-integrating-llm-into-applications-913d4461bbe0?source=collection_archive---------0-----------------------#2023-07-08)

## 计划将某些 LLM 服务集成到您的代码中吗？这里有一些在进行集成时可能遇到的常见挑战

[](https://medium.com/@shahar.davidson?source=post_page-----913d4461bbe0--------------------------------)![Shahar Davidson](https://medium.com/@shahar.davidson?source=post_page-----913d4461bbe0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----913d4461bbe0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----913d4461bbe0--------------------------------) [Shahar Davidson](https://medium.com/@shahar.davidson?source=post_page-----913d4461bbe0--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffa5cf0bcd8ab&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complexities-and-challenges-of-integrating-llm-into-applications-913d4461bbe0&user=Shahar+Davidson&userId=fa5cf0bcd8ab&source=post_page-fa5cf0bcd8ab----913d4461bbe0---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----913d4461bbe0--------------------------------) ·7 分钟阅读·2023 年 7 月 8 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F913d4461bbe0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complexities-and-challenges-of-integrating-llm-into-applications-913d4461bbe0&user=Shahar+Davidson&userId=fa5cf0bcd8ab&source=-----913d4461bbe0---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F913d4461bbe0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-complexities-and-challenges-of-integrating-llm-into-applications-913d4461bbe0&source=-----913d4461bbe0---------------------bookmark_footer-----------)![](img/0d86770db058ef29cfeef1d0c5f18184.png)

图片由 [Christina @ wocintechchat.com](https://unsplash.com/es/@wocintechchat?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在 OpenAI 的 ChatGPT 和 GPT API 发布之前，大型语言模型（LLMs）已经存在。但由于 OpenAI 的努力，GPT 现在对开发者和非开发者都易于获取。这次发布无疑在 AI 的近期复兴中发挥了重要作用。

OpenAI 的 GPT API 在推出仅六个月内就被迅速接受，确实令人惊叹。几乎所有 SaaS 服务都以某种方式整合了它，以提高用户的生产力。

然而，只有那些完成了此类 API 设计和集成工作的人，才能真正理解其中的复杂性和新挑战。

在过去几个月里，我实现了几个利用 OpenAI GPT API 的功能。在这个过程中，我遇到了几个似乎对任何使用 GPT API 或其他 LLM API 的人都很常见的挑战。通过在这里列出这些挑战，我希望能帮助工程团队正确准备和设计他们的 LLM 功能。

让我们来看一些典型的障碍。

## **上下文记忆与上下文限制**

这可能是所有挑战中最常见的。LLM 输入的上下文是有限的。就在最近，OpenAI 发布了对 16K tokens 的上下文支持，在 GPT-4 中，上下文限制可以达到 32K，这相当于几页（例如，如果你希望 LLM 处理一个包含几页的大文档）。但在许多情况下，你需要更多，尤其是当处理许多文档，每个文档有几十页时（比如想象一下一个需要处理数十份法律文档以提取答案的法律技术公司）。

有不同的[技术](https://www.pinecone.io/learn/series/langchain/langchain-conversational-memory)来克服这个挑战，还有[其他技术](https://huggingface.co/papers/2306.07174)正在出现，但这意味着你必须自己实现这些技术中的一种或多种。又一项需要实现、测试和维护的工作负担。

## **数据增强**

你的基于 LLM 的功能可能需要某种专有数据作为输入。无论你是将用户数据作为上下文的一部分，还是使用你存储的其他收集数据或文档，你都需要一个简单的机制来抽象从你拥有的各种数据源中获取数据的调用。

## **模板化**

你提交给 LLM 的提示将包含硬编码的文本和来自其他数据源的数据。这意味着你将创建一个静态模板，并在运行时动态填充数据，这些数据应作为提示的一部分。换句话说，你将为你的提示创建模板，并且可能有多个。

这意味着你应该使用某种模板框架，因为你可能不希望你的代码看起来像一堆字符串拼接。

这不是一个大挑战，但另一个需要考虑的任务。

## **测试与微调**

使 LLM 达到令人满意的准确度需要大量的测试（有时这仅仅是通过大量的试错进行提示工程）和基于用户反馈的微调。

当然，还有作为 CI 部分运行的测试，以确保所有集成都正常工作，但这不是实际的挑战。

当我说测试时，我是指在沙盒中反复运行提示，以微调结果的准确性。

对于测试，你会需要一种方法，使测试工程师可以更改模板，用所需的数据丰富它们，并执行提示以测试我们是否得到了想要的结果。你如何设置这样的测试框架？

此外，我们需要通过获取用户对 LLM 输出的反馈来不断微调 LLM 模型。我们如何设置这样的过程？

## 缓存

LLM 模型，如 OpenAI 的 GPT，有一个参数可以控制答案的随机性，从而让 AI 更具创造力。然而，如果你处理大规模的请求，你将会产生高昂的 API 调用费用，可能会遇到速率限制，并且你的应用性能可能会下降。如果 LLM 的一些输入在不同调用中重复出现，你可能需要考虑缓存答案。例如，你处理了 100K 次调用你的 LLM 基础功能。如果所有这些调用都触发对 LLM 提供商的 API 调用，那么成本将非常高。然而，如果输入重复出现（这可能在你使用模板并填入特定用户字段时发生），那么你有很高的机会可以保存一些预处理过的 LLM 输出，并从缓存中提供服务。

这里的挑战是为此构建一个缓存机制。这并不难实现；它只是增加了另一层需要维护和正确处理的部件。

## 安全性和合规性

安全性和隐私可能是这个过程最具挑战性的方面——我们如何确保创建的过程不会导致数据泄露，以及我们如何确保没有[个人身份信息](https://www.technology.pitt.edu/help-desk/how-to-documents/guide-identifying-personally-identifiable-information-pii)被暴露？

此外，你还需要审计所有操作，以确保所有操作都可以检查，以确保没有数据泄露或隐私政策的违反。

这是任何依赖第三方服务的软件公司常见的挑战，也需要在这里解决。

## 可观察性

与任何你使用的外部 API 一样，你必须监控其性能。是否有错误？处理时间多长？我们是否超过或即将超过 API 的速率限制或阈值？

此外，你还需要记录所有调用，不仅仅是为了安全审计目的，还为了通过评分输出来帮助你微调 LLM 工作流或提示。

## 工作流管理

假设我们开发了一款律师用来提高生产力的法律技术软件。在我们的例子中，我们有一个基于 LLM 的功能，它从 CRM 系统中提取客户的详细信息和案件的一般描述，并基于法律先例为律师的查询提供答案。

让我们看看完成这些任务需要做些什么：

1.  根据给定的客户 ID 查找所有客户的详细信息。

1.  查找当前正在处理的案件的所有详细信息。

1.  使用 LLM 根据律师的查询从当前正在处理的案件中提取相关信息。

1.  将上述所有信息组合到一个预定义的问题模板中。

1.  用大量法律案件丰富上下文。（回忆一下上下文记忆挑战）

1.  让 LLM 找到最匹配当前案件、客户和律师查询的法律先例。

现在，想象一下你有 2 个或更多这样的功能，最后试着想象在实现这些工作流之后你的代码会是什么样的。我敢打赌，仅仅想到这里的工作就让你在椅子上感到不舒服。

为了使你的代码具有可维护性和可读性，你需要实现各种抽象层，并且如果你预见到未来会有更多工作流，可能需要考虑采用或实现某种工作流管理框架。

最后，这个例子引出了下一个挑战：

## 强耦合代码

现在你已经意识到上述所有挑战及其复杂性，你可能会开始看到一些需要完成的任务不应由开发人员负责。

具体来说，所有与构建工作流、测试、微调、监控结果和外部 API 使用相关的任务可以由更专注于这些任务且专长不在于软件开发的人来完成。我们称这个角色为*LLM 工程师*。

没有理由将 LLM 工作流、测试、微调等工作放在软件开发人员的职责范围内——软件开发人员擅长构建软件。同时，LLM 工程师应该擅长构建和微调 LLM 工作流，而不是开发软件。

但目前的框架中，LLM 工作流管理与代码库耦合在一起。任何构建这些工作流的人都需要具备软件开发人员和 LLM 工程师的专业知识。

有一些方法可以进行解耦，例如创建一个处理所有工作流的专用微服务，但这又是另一个需要解决的挑战。

这些只是一些挑战。

我没有深入探讨如何解决所有这些问题，因为我自己还在努力弄清楚。不过，我可以说，[**LangChain**](https://python.langchain.com/docs/get_started/introduction.html)似乎是唯一一个在某种程度上接近解决这些问题的框架，虽然距离完全解决还远，但方向似乎是对的。随着时间的推移，我相信 LLM 提供商会改进他们的产品，并提供能够在一定程度上应对这些挑战的平台。

如果你有任何额外的挑战要分享，请在评论中告诉我们，以便其他读者受益。此外，如果你知道任何可以帮助应对这些挑战的工具，也请在评论中分享。

希望你觉得这篇文章有启发！如果有，请通过鼓掌和关注我来表达你的支持，以便获取更多关于团队建设、软件工程和技术的文章。
