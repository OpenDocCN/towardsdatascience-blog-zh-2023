# GPT 与超越：大型语言模型的技术基础

> 原文：[https://towardsdatascience.com/gpt-and-beyond-the-technical-foundations-of-llms-2e2c89fc6c7a?source=collection_archive---------4-----------------------#2023-08-03](https://towardsdatascience.com/gpt-and-beyond-the-technical-foundations-of-llms-2e2c89fc6c7a?source=collection_archive---------4-----------------------#2023-08-03)

[](https://towardsdatascience.medium.com/?source=post_page-----2e2c89fc6c7a--------------------------------)[![TDS 编辑](../Images/4b2d1beaf4f6dcf024ffa6535de3b794.png)](https://towardsdatascience.medium.com/?source=post_page-----2e2c89fc6c7a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2e2c89fc6c7a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2e2c89fc6c7a--------------------------------) [TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----2e2c89fc6c7a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-and-beyond-the-technical-foundations-of-llms-2e2c89fc6c7a&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----2e2c89fc6c7a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2e2c89fc6c7a--------------------------------) ·发送至 [新闻通讯](/newsletter?source=post_page-----2e2c89fc6c7a--------------------------------) ·3分钟阅读·2023年8月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2e2c89fc6c7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-and-beyond-the-technical-foundations-of-llms-2e2c89fc6c7a&user=TDS+Editors&userId=7e12c71dfa81&source=-----2e2c89fc6c7a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2e2c89fc6c7a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgpt-and-beyond-the-technical-foundations-of-llms-2e2c89fc6c7a&source=-----2e2c89fc6c7a---------------------bookmark_footer-----------)

仅仅几个月时间，大型语言模型就从专业研究人员的领域进入了全球数据和机器学习团队的日常工作流。在 TDS，我们见证了随着这一过渡，焦点也转向了实际应用和动手解决方案。

直接进入调整模式对于在行业中工作的数据专业人员来说是非常合理的——毕竟时间是宝贵的。然而，总是很有必要建立对我们使用和工作的技术的深入理解，这正是我们每周亮点所关注的。

我们推荐的阅读内容既探讨了大型语言模型（LLMs）的理论基础——特别是GPT家族——也关注了它们出现所引发的高层次问题。即使你只是这些模型的普通用户，我们认为你也会喜欢这些深思熟虑的探索。

+   transformers架构是最初使GPT模型成为可能的突破性创新。[Beatriz Stollnitz](https://medium.com/u/1c8863892480?source=post_page-----2e2c89fc6c7a--------------------------------)明确表示，“理解它们如何工作的细节是每个AI从业者的重要技能，”并且[**你将从她详细的解释中获得清晰的理解**](/the-transformer-architecture-of-gpt-models-b8695b48728b) transformers的强大之处。

+   [Lily Hughes-Robinson](https://medium.com/u/5389e25ca1bb?source=post_page-----2e2c89fc6c7a--------------------------------)提供了[**一种不同的学习transformers的方法**](/learning-transformers-code-first-part-2-gpt-up-close-and-personal-1635b52ae0d7)**:** 这种方法专注于源代码，以便你可以从基础开始直观地构建知识。

+   在LLMs的性能中，规模的重要性有多大？[Gadi Singer](https://medium.com/u/51de1f48d0b?source=post_page-----2e2c89fc6c7a--------------------------------)详细探讨了这个问题，并[**审视了最新一代紧凑型生成AI模型**](/survival-of-the-fittest-compact-generative-ai-models-are-the-future-for-cost-effective-ai-at-scale-6bbdc138f618)**。** 这些竞争者旨在以较低的成本和更大的可扩展性潜力与GPT-4在准确性上竞争。

![](../Images/2c84298995d84249504a2861bc494d4e.png)

[K8](https://unsplash.com/@_k8_?utm_source=medium&utm_medium=referral)拍摄，来自[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   在围绕ChatGPT及类似工具的激烈辩论中，也许没有比关于LLMs所谓智能的问题更具争议的了。[Lan Chu](https://medium.com/u/3916743f0e10?source=post_page-----2e2c89fc6c7a--------------------------------)直接应对了这一话题，并且[**带来了一个清新而务实的视角**](/is-chatgpt-actually-intelligent-42d07462fe59)。 （剧透：不，AI并不具备意识；是的，情况很复杂。）

+   “那么，我们如何超越将像ChatGPT这样的LLMs视为神秘黑箱的看法？物理学或许能提供答案。”[Tim Lou, PhD](https://medium.com/u/8d41b438feef?source=post_page-----2e2c89fc6c7a--------------------------------)的最新文章提出了一个发人深省的观点：即[**使语言模型运转的方程类似于物理学定律**](/understanding-large-language-models-the-physics-of-chat-gpt-and-bert-ea512bcc6a64)，它们像物理学定律一样支配粒子和力量。

我们在最近几周发布了*如此*多精彩的文章，涵盖了其他话题；这里只列出了我们绝对必须强调的几篇。

+   谁说夏季阅读一定要轻松无聊？我们的八月版 [汇集了一系列令人印象深刻的文章](/august-edition-summer-reads-for-data-scientists-52d5ad64835b)，这些文章引人入胜，启发思考，并且耐高温。

+   [你的营销策略中可能缺少的成分就是机器学习](/leveraging-machine-learning-for-effective-marketing-strategy-development-99b1b887f2f5)，[Elena K.](https://medium.com/u/1a63363b910a?source=post_page-----2e2c89fc6c7a--------------------------------)说，她的首篇TDS故事充满了可操作的技巧和窍门。

+   如果你对另一个商业主题感兴趣，你很幸运：[Matteo Courthoud](https://medium.com/u/666130fb420f?source=post_page-----2e2c89fc6c7a--------------------------------)带来了一个新的贡献，专注于流失率和收入的互动。

+   回到与LLM工作更实际的一面，[Felipe de Pontes Adachi](https://medium.com/u/a038269245d5?source=post_page-----2e2c89fc6c7a--------------------------------) [概述了监控其行为的七种策略](/7-ways-to-monitor-large-language-model-behavior-25c267d58f06)，以确保性能一致。

+   [Anna Via](https://medium.com/u/c1a8933ed8b?source=post_page-----2e2c89fc6c7a--------------------------------)的新帖子鼓励行业数据从业者在启动以ML为中心的项目之前，退后一步，[询问是否真的需要机器学习模型](/to-use-or-not-to-use-machine-learning-d28185382c14)来解决当前的问题。

感谢你对我们作者的支持！如果你喜欢在TDS上阅读的文章，考虑 [成为 Medium 会员](https://bit.ly/tds-membership) —— 这将解锁我们的整个档案（还有Medium上的所有其他文章）。

我们希望你们中的许多人也在 [计划参加 Medium Day](https://blog.medium.com/youre-invited-to-medium-day-526193e251b2#:~:text=On%20August%2012th%2C%202023%2C%20we,share%20their%20lives%20on%20Medium.)，于8月12日庆祝社区以及让它特别的故事 —— [注册（免费）现已开放](https://hopin.com/events/medium-day-2023/registration)。

直到下一个Variable，

TDS编辑团队
