# **检索增强生成（RAG）的内幕与外延**

> 原文：[`towardsdatascience.com/the-ins-and-outs-of-retrieval-augmented-generation-rag-56f470ccda4?source=collection_archive---------7-----------------------#2023-10-12`](https://towardsdatascience.com/the-ins-and-outs-of-retrieval-augmented-generation-rag-56f470ccda4?source=collection_archive---------7-----------------------#2023-10-12)

[](https://towardsdatascience.medium.com/?source=post_page-----56f470ccda4--------------------------------)![TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----56f470ccda4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----56f470ccda4--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----56f470ccda4--------------------------------) [TDS 编辑](https://towardsdatascience.medium.com/?source=post_page-----56f470ccda4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ins-and-outs-of-retrieval-augmented-generation-rag-56f470ccda4&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----56f470ccda4---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----56f470ccda4--------------------------------) · 发送为 通讯 · 阅读时间约 3 分钟 · 2023 年 10 月 12 日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F56f470ccda4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ins-and-outs-of-retrieval-augmented-generation-rag-56f470ccda4&user=TDS+Editors&userId=7e12c71dfa81&source=-----56f470ccda4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F56f470ccda4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-ins-and-outs-of-retrieval-augmented-generation-rag-56f470ccda4&source=-----56f470ccda4---------------------bookmark_footer-----------)

当大型语言模型首次出现时，那种兴奋感是显而易见的：除了它们的新奇性，它们还承诺彻底改变众多领域和工作方向。

几乎在 ChatGPT 发布一年后，我们对 LLMs 的局限性以及将它们集成到现实世界产品中所面临的挑战有了更清晰的认识。到现在为止，我们也提出了一些强大的策略来补充和增强 LLMs 的潜力；其中，检索增强生成（RAG）无疑是最突出的。它赋予实践者将预训练模型与外部的最新信息源连接起来，从而生成更准确、更有用的输出。

本周，我们汇集了一系列强大的文章，解释了使用 RAG 的复杂性和实际考虑因素。无论你是深入机器学习领域，还是从数据科学家或产品经理的角度来看待这个话题，对这种方法有更深的了解都可以帮助你为未来 AI 工具的发展做好准备。让我们直接进入正题！

+   **使用检索增强生成（RAG）将您自己的数据添加到 LLM 中**对于初学者友好的入门介绍，[Beatriz Stollnitz](https://medium.com/u/1c8863892480?source=post_page-----56f470ccda4--------------------------------)最近的深度分析是一个很好的资源，可以访问并收藏以供将来参考。它介绍了 RAG 的理论基础，然后过渡到一个基本的动手实现，展示了如何创建一个聊天机器人，帮助客户找到有关公司销售产品的信息。

+   **提升检索增强生成系统性能的 10 种方法**如果你已经开始在项目中尝试 RAG，你可能会观察到，设置它是一回事，但让它持续有效并产生预期结果则是另一回事：“RAG 很容易原型化，但很难投入生产。” [Matt Ambrogi](https://medium.com/u/1e23ad8f92c5?source=post_page-----56f470ccda4--------------------------------)的指南提供了关于弥合框架潜力与更具实际效益之间差距的务实见解。

![](img/e2b8c46b2d8c415caaf95e37761e6759.png)

[Frank Zhang](https://unsplash.com/@terasproductions?utm_source=medium&utm_medium=referral)拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   **RAG 与细调 — 哪种是提升 LLM 应用的最佳工具？** 在构建更好的 AI 产品时，RAG 还有不少替代方案。[Heiko Hotz](https://medium.com/u/993c21f1b30f?source=post_page-----56f470ccda4--------------------------------) 提供了 RAG 和模型细调的细致且全面的对比，后者是提升通用 LLM 性能的另一种重要策略。最终，正如 Heiko  eloquently 表达的那样，“没有一刀切的解决方案；成功在于将优化方法与任务的具体要求对齐。”

关于从反事实见解到动态定价的其他优秀读物，我们希望你能探索一些我们近期的其他亮点：

+   如果你想测试 ChatGPT API 的威力，[Mariya Mansurova](https://medium.com/u/15a29a4fc6ad?source=post_page-----56f470ccda4--------------------------------) 分享了一份 使用 ChatGPT API 进行主题建模的入门指南。

+   想提升编程技能吗？[Marcin Kozak](https://medium.com/u/4762f0cff9b2?source=post_page-----56f470ccda4--------------------------------) 的动手教程 处理 Python 中的 NaN (非数字) 值 及如何正确使用它们。

+   [Reza Bagheri](https://medium.com/u/da2d000eaa4d?source=post_page-----56f470ccda4--------------------------------) 带来了一次标志性的深度分析，这次 详细介绍了维度的数学基础（以及由此带来的著名“诅咒”）。

+   要了解反事实及其在数据分析中的位置，不要错过 [Maham Haroon](https://medium.com/u/398c9514a58b?source=post_page-----56f470ccda4--------------------------------) 的清晰易懂的讲解。

+   为什么如此多的企业即使没有明确的商业目标也要 赶上生成式 AI 的潮流？[Stephanie Kirmer](https://medium.com/u/a8dc77209ef3?source=post_page-----56f470ccda4--------------------------------) 探讨了这一新兴难题。

+   在探索使用强化学习方法进行动态定价的潜力后，[Massimiliano Costacurta](https://medium.com/u/233cb43234c3?source=post_page-----56f470ccda4--------------------------------) 权衡了 为多臂老虎机解决方案添加上下文的好处。

+   在一个有趣的项目演示中，[Caroline Arnold](https://medium.com/u/9367198e7a3c?source=post_page-----56f470ccda4--------------------------------)展示了如何利用预训练模型和再分析数据来创建一个自定义的 AI 天气预报应用。

感谢你对我们作者工作的支持！如果你喜欢在 TDS 上阅读的文章，可以考虑[成为 Medium 会员](https://bit.ly/tds-membership)——这将解锁我们的所有存档（以及 Medium 上的其他每一篇文章）。
