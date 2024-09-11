# 您的 LLM 应用程序准备好公开了吗？

> 原文：[`towardsdatascience.com/is-your-llm-application-ready-for-the-public-55fdcce99888?source=collection_archive---------17-----------------------#2023-06-20`](https://towardsdatascience.com/is-your-llm-application-ready-for-the-public-55fdcce99888?source=collection_archive---------17-----------------------#2023-06-20)

## 将基于 LLM 的应用程序投入生产时的关键关注点

[](https://medium.com/@itai_88363?source=post_page-----55fdcce99888--------------------------------)![Itai Bar Sinai](https://medium.com/@itai_88363?source=post_page-----55fdcce99888--------------------------------)[](https://towardsdatascience.com/?source=post_page-----55fdcce99888--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----55fdcce99888--------------------------------) [Itai Bar Sinai](https://medium.com/@itai_88363?source=post_page-----55fdcce99888--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdae6c2441260&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-your-llm-application-ready-for-the-public-55fdcce99888&user=Itai+Bar+Sinai&userId=dae6c2441260&source=post_page-dae6c2441260----55fdcce99888---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----55fdcce99888--------------------------------) ·6 分钟阅读·2023 年 6 月 20 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F55fdcce99888&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-your-llm-application-ready-for-the-public-55fdcce99888&user=Itai+Bar+Sinai&userId=dae6c2441260&source=-----55fdcce99888---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F55fdcce99888&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-your-llm-application-ready-for-the-public-55fdcce99888&source=-----55fdcce99888---------------------bookmark_footer-----------)

大型语言模型（LLMs）正成为现代自然语言处理（NLP）应用的基本组成部分，并且在许多方面取代了各种更为专业的工具，例如命名实体识别模型、问答模型和文本分类器。因此，很难想象一个不以某种方式使用 LLM 的 NLP 产品。虽然 LLM 带来了诸如个性化和创意对话生成等诸多好处，但在将这些模型集成到服务最终用户的软件产品中时，了解其陷阱以及如何应对这些问题是很重要的。事实证明，监控能够很好地解决这些挑战，并且是任何与 LLM 合作的企业工具箱中的一个重要部分。

# 数据、隐私和提示注入

![](img/f33bf41296052bcbdf1e05d77807782d.png)

图片由 TheDigitalArtist 提供，来源于 [Pixabay](https://pixabay.com/photos/computer-business-gdpr-legislation-3233754/)

# 数据与隐私

隐私和数据使用是现代消费者的主要关注点之一，尤其是在剑桥分析等著名数据共享丑闻的影响下，消费者越来越不愿使用那些可能危及个人隐私的服务和产品。虽然 LLM（大型语言模型）为用户提供了极高的个性化，但了解它们所带来的风险也很重要。与所有机器学习模型一样，LLM 容易受到针对性的攻击，这些攻击旨在揭示训练数据，由于其生成性质，LLM 尤其容易受到风险，甚至在进行自由形式生成时可能会意外泄露数据。例如，在[2020 年博客文章](https://ai.googleblog.com/2020/12/privacy-considerations-in-large.html)中，谷歌大脑的研究科学家尼古拉斯·卡尔尼讨论了如何通过提示 LLM（如 GPT）来泄露个人身份信息，如姓名、地址和电子邮件，这些信息包含在模型的训练数据中。这表明，针对客户数据对 LLM 进行微调的企业可能会引发类似的隐私风险。同样，来自微软研究人员的[论文](https://ppml-workshop.github.io/ppml21/pdfs/ppml21-final58.pdf)也证实了这些说法，并建议了一些具体的缓解策略，这些策略利用差分隐私技术来训练 LLM，同时减少数据泄露的担忧。不幸的是，由于许多企业使用的 LLM API 不允许他们控制微调过程，因此无法利用这些技术。这些公司的解决方案在于插入一个监控步骤，在将结果返回给最终用户之前验证和限制模型的输出。通过这种方式，企业可以在隐私违规实际发生之前识别并标记潜在的训练数据泄露实例。例如，监控工具可以应用诸如命名实体识别和正则表达式过滤等技术来识别模型生成的姓名、地址、电子邮件及其他敏感信息，防止这些信息落入不良分子之手。这对于在隐私受限领域如医疗或金融工作的组织尤其重要，这些领域涉及 HIPAA 和 FTC/FDIC 等严格法规。即使是仅仅在国际上运营的企业也面临违反复杂的地域特定法规，如欧盟的 GDPR。

# 提示注入

提示注入（Prompt injection）指的是设计 LLM 提示的过程，通常是恶意的，通过某种方式“欺骗”或混淆系统，从而产生有害的输出。例如，最近的一篇文章展示了精心设计的提示注入攻击如何使得 OpenAI 的 GPT-4 模型被颠覆，从而提供事实错误的信息甚至推广阴谋论。可以想象到更多邪恶的场景，例如用户提示 LLM 提供如何制造炸弹的建议、如何最佳自杀的细节，或生成可用于感染其他计算机的代码。提示注入攻击的脆弱性是不幸的副作用，由于 LLM 的训练方式，很难在前端做任何事情来防止所有可能的提示注入攻击。即使是最强大和最新的 LLM，如 OpenAI 的 ChatGPT——专门为安全而调整——也证明了对提示注入的脆弱性。

由于提示注入可能表现出的方式千差万别，因此几乎不可能防范所有可能性。因此，监控 LLM 生成的输出至关重要，因为它提供了一种识别和标记虚假信息以及明显有害生成的机制。监控可以使用简单的 NLP 启发式方法或额外的 ML 分类器来标记包含有害内容的模型响应，并在返回给用户之前拦截它们。类似地，对提示本身的监控可以在提示被传递给模型之前捕捉到一些有害的提示。

# 幻觉

幻觉（hallucination）指的是 LLM 偶尔“编造”那些实际上不符合现实的输出的倾向。提示注入和幻觉可以表现为同一枚硬币的两面，尽管提示注入中的虚假生成是用户的故意行为，而幻觉则是 LLM 训练目标的无意副作用。由于 LLM 被训练为在每个时间步骤中预测序列中下一个最可能的词，因此它们能够生成高度逼真的文本。因此，幻觉是最可能的情况并不总是真的简单结果。

![](img/4ad9a20e80f4114ca5a3042e78f5b19a.png)

图片来源：Matheus Bertelli via [Pexels](https://www.pexels.com/photo/woman-laptop-working-internet-16094040/)

最新一代的大型语言模型（LLM），例如 GPT-3 和 GPT-4，采用了一种叫做**人类反馈强化学习**（RLHF）的算法进行优化，以匹配人类对什么构成良好响应的主观判断。虽然这使得 LLM 能够达到更高的对话流畅度，但有时也会导致它们在回应时表现得过于自信。例如，询问 ChatGPT 一个问题时，它可能会自信地给出一个初看似乎合理的回答，但经过进一步检查，结果却是客观上错误的。赋予 LLM 提供不确定性量化的能力仍然是一个活跃的研究问题，并且不太可能很快得到解决。因此，LLM 基于的产品的开发者应该考虑监控和分析输出，以试图检测幻觉，并提供比 LLM 模型开箱即用的更细致的回应。这在 LLM 的输出可能指导某些下游过程的情况下尤为重要。例如，如果一个 LLM 聊天机器人在帮助用户提供产品推荐并协助在零售商网站上下订单时，应该实施监控程序，以确保模型不会建议购买在该零售商网站上实际上并未销售的产品。

# **不受控制的成本**

由于 LLM 通过 API 正变得越来越商品化，企业在将这些模型集成到其产品中时，制定防止成本无限增加的计划非常重要。如果没有保护措施，用户生成数千个 API 调用并发出数千个令牌的提示（例如，用户将一份极长的文档粘贴到输入中，并要求 LLM 分析它）是很容易的。由于 LLM API 通常根据调用次数和令牌计数（包括提示和模型的回应）进行计量，因此成本迅速失控并不困难。因此，企业在制定定价结构时需要小心，以抵消这些成本。此外，企业应该有监控程序，以了解使用激增如何影响成本，并通过施加使用上限或采取其他补救措施来缓解这些激增。

# **结论**

每个将大型语言模型（LLMs）应用于其产品的企业，都应确保将监控功能整合到其系统中，以避免并解决 LLMs 的众多陷阱。此外，使用的监控解决方案应专门针对 LLMs 应用，允许用户识别潜在的隐私侵犯，防止和/或修复提示注入，标记幻觉，并诊断不断上升的成本。最佳的监控解决方案将解决所有这些问题，并为企业提供一个框架，以确保其基于 LLM 的应用准备好对公众发布。通过[预约演示](https://www.monalabs.io/request-demo)来查看 Mona 的全面监控能力，确保你的 LLM 应用经过充分优化并按预期运行。
