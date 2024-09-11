# 不要让你的第一个 AI 项目在推出时陷入困境

> 原文：[https://towardsdatascience.com/dont-let-your-first-ai-project-get-stuck-going-out-the-door-9b2217a0affa?source=collection_archive---------11-----------------------#2023-05-09](https://towardsdatascience.com/dont-let-your-first-ai-project-get-stuck-going-out-the-door-9b2217a0affa?source=collection_archive---------11-----------------------#2023-05-09)

## 融合 AI 和 DevOps，启动你组织的 AI 之旅

[](https://medium.com/@emilypotyraj?source=post_page-----9b2217a0affa--------------------------------)[![Emily Potyraj](../Images/90b05cc4955d3925f87441ed6a4b7418.png)](https://medium.com/@emilypotyraj?source=post_page-----9b2217a0affa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----9b2217a0affa--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----9b2217a0affa--------------------------------) [Emily Potyraj](https://medium.com/@emilypotyraj?source=post_page-----9b2217a0affa--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff882220c17d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-let-your-first-ai-project-get-stuck-going-out-the-door-9b2217a0affa&user=Emily+Potyraj&userId=ff882220c17d&source=post_page-ff882220c17d----9b2217a0affa---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----9b2217a0affa--------------------------------) · 7 分钟阅读 · 2023年5月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F9b2217a0affa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-let-your-first-ai-project-get-stuck-going-out-the-door-9b2217a0affa&user=Emily+Potyraj&userId=ff882220c17d&source=-----9b2217a0affa---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F9b2217a0affa&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdont-let-your-first-ai-project-get-stuck-going-out-the-door-9b2217a0affa&source=-----9b2217a0affa---------------------bookmark_footer-----------)![](../Images/2e00659dc66ca5361e960184c66e2c21.png)

不要让你的 AI 项目在推出时陷入困境！*(所有图片均由作者和 DALL-E 提供。)*

人工智能（AI）项目有可能彻底改变组织，但成功将 AI 项目投入生产需要强大的 DevOps 基础。这篇文章是为那些刚刚开始 AI 策略或第一个 AI 项目的团队设计的。如果你只是为了乐趣而尝试 AI 模型，那很好！但如果你的目标是最终让模型可供客户（内部或外部）使用，这篇文章就是为你准备的。

基于他人的组织学习，我将分享如何为AI项目构建一个稳固的DevOps框架的建议，以及为什么这对你团队的成功至关重要。

# 1\. 在开始之前进入DevOps思维模式。

如果你的团队对AI不熟悉，你有机会避免一些其他团队犯过的错误！在首次AI项目中一个常见的错误是仅专注于*构建*模型。

作为AI解决方案架构师，我见证了无数团队决定投资AI，构建模型，然后几个月甚至几年都没有推出一个模型！通常，团队没有明确的路径来将模型发布出去。看到一个模型接近影响业务价值却在最后一刻止步，真的让人感到沮丧。

除非你决心为了研究做AI，否则你的目标不是“拥有一个模型。”你的目标是“让人们使用这个模型。”DevOps是开发工作和用户使用产品之间的界限。不要让优秀的实验因为没有转化为优秀的产品而白白浪费。

# 2\. 提前与团队设定期望。

认识到，将模型服务于最终用户需要不同的技能和工具，与训练模型不同。促进你的DevOps团队和AI团队之间的合作。或者，如果你的AI团队将自行执行DevOps任务，提前讨论支持生产环境中的模型将涉及哪些内容。

![](../Images/d708aa0a11c243afef3803f2d2b92975.png)

# 3\. 接受AI项目的迭代特性。

对于新进入该领域的团队来说，AI部署的一个方面可能会让人感到意外，那就是AI是一个不断发展的过程。要做好支持持续改进和迭代的准备。

即使你的模型一开始具有令人印象深刻的准确性，但它遇到的输入和条件会随着时间的推移而变化。这个过程变成了一个训练、整合新数据和重新训练的循环，以不断提升AI的能力。

这里有一些示例，展示了为什么持续再训练可能是至关重要的：

1.  预测产品和服务需求的AI模型必须不断适应不断变化的市场条件和客户偏好。

1.  检测欺诈交易的AI模型需要与不断变化的欺诈手法和金融数据模式保持同步。

1.  分析社交媒体趋势的AI模型需要考虑到病毒内容和用户行为迅速变化的环境。

所以，如果你希望模型保持相关性和有效性，可能会有长期的DevOps工作来支持迭代和重新部署。

# 10个关键问题，助力AI项目增长。

在你为团队制定最佳实践时，考虑以下目标和问题。

**注意：** 你可能会发现第一个AI项目的答案与后续项目不同。现在，大多数问题的答案可能是“Jesse每周手动检查”。最好现在就有一个明确的计划——即使看起来技术含量不高——也比以后遇到意外要好。

## 1\. 促进合作和知识共享

你如何促进AI和DevOps团队之间的跨功能协作？哪些平台或工具将促进有效的知识共享？

+   计划一个联合启动会议和定期的团队检查点。如果你有一个DevOps团队，他们可以了解即将发生的事情，并与模型构建者感受到团队的归属感。如果你的模型构建者将执行DevOps工作，这将明确地集中时间进行生产计划。

+   为讨论或工作跟踪指定一个空间。（Slack？JIRA？）

+   在各个小组之间广泛分享关于模型开发进展的激动人心的更新。

## 2\. 确定AI特定流程

部署和管理AI模型时，你需要什么独特的流程？你的DevOps团队如何学习并适应这些流程？

+   开始起草将模型从开发转移到生产的部署管道。

+   讨论你对监控、维护和更新生产中AI模型的目标。

+   识别可能需要时间或资源来学习AI DevOps工具或实践的人。

## 3\. 实施强有力的安全和合规措施

应该遵循哪些安全最佳实践来保护你的AI模型和数据？你将如何确保遵守相关法规和行业标准？

+   考虑这个新用例是否与数据的现有使用预期一致（例如，你的客户是否同意以这种方式使用他们的数据？）。

+   考虑是否需要起草有关数据和模型使用的新文档，可能是内部使用的或面向客户的文档。考虑是否需要涉及你的法律团队。

+   识别适用的法规（例如GDPR、HIPAA），并建立流程以保持合规。

+   进行定期的合规审查和审计。

## 4\. 明智地分配资源

你将如何监控和分配资源，例如GPU使用情况？你有硬件预算吗？

在开始时，你可能会关注*在哪里*训练模型和*如何*部署它。随着进展，你可能会探索更高级的基础设施规划，例如优化资源分配或将管道容器化以实现可移植性和可扩展性。

## 5\. 监控模型性能

我们将如何跟踪AI模型在生产中的性能？哪些指标和监控工具将帮助识别潜在问题或性能退化？

+   确定模型的关键性能指标。除了准确性，其他指标可能也很重要，具体取决于应用程序和用例（例如，精确度、召回率）。还要考虑是否有模型的延迟要求。

+   制定一个持续审查关键性能指标和检测“模型漂移”的计划。

## 6\. 计划模型再训练

我们如何安排和管理AI模型的定期再训练，以确保它们跟上最新的信息？我们可以实施哪些具体方法或工具来简化再训练过程并开始自动化？

+   实施一个纳入新数据的流程。

+   根据数据和模型性能的预期变化速率制定再训练计划。考虑如何在部署前验证再训练的模型。

## 7\. 高效管理数据管道

我们将如何处理数据摄取和预处理？我们需要设置什么数据验证？哪些工具和流程能确保数据在整个管道中的质量和一致性？

+   考虑实施数据质量检查，以识别错误或缺失值。考虑检查异常值、离群值或数据漂移。

+   对于数据准备工作流，考虑你的团队是否会倾向于使用你正在使用的机器学习工具中的原生功能（例如 [PyTorch](https://pytorch.org)），而不是添加额外的管道管理工具（例如 [Airflow](https://airflow.apache.org)）。

注意：虽然深入了解机器学习运维（[MLOps](https://services.google.com/fh/files/misc/practitioners_guide_to_mlops_whitepaper.pdf)）很有诱惑，但**早期过度自动化是很重要的**。

> 仔细思考你要解决的问题和解决问题所需的基本方法，而不是过于分心于炫目的工具或平台。 — Mihail Eric, [MLOps Is a Mess But That’s to be Expected](https://www.mihaileric.com/posts/mlops-is-a-mess/)

从简单开始，根据需要让你的MLOps流程自然增长。

## 8\. 确保模型的可重复性

你将如何保证在训练和部署AI模型时结果的一致性？哪些版本控制和追踪系统会有所帮助？

+   尽可能记录和版本控制AI项目的各个方面，包括数据、代码和配置。

+   考虑使用像Docker这样的容器化工具，以确保开发和生产环境的一致性。

## 9\. 最终制定模型更新策略

一开始，你的重点可能是让你的AI模型按照项目目标运行。在未来，你的团队可能会探索结合架构改进、新特性或调整模型参数以提升其能力。为了支持这些未来的发展，你可能最终会制定一个包含强大测试过程和必要时能够回滚更改的模型更新策略。

## 10\. 不断完善和优化流程

你将如何不断改进你的AI DevOps流程？哪些指标有助于优化？

+   鼓励所有相关小组的团队成员之间开放沟通。

+   定期审查流程并结合反馈以优化工作流。

## **总结**

这是一长串需要考虑的问题！如果有疑问，尽量**避免创建以下情况：**

+   😞 AI模型经过一次重大努力后被发布，但几乎不可能发布模型的新版本。

+   😞 消耗新的训练数据需要巨大的努力。

+   😞 没有人知道模型在6个月后是否仍然准确。

## 你能做到的！

启动你的第一个 AI 项目可能是一个复杂且具有挑战性的过程。通过遵循这些指南并专注于整合 AI 和 DevOps，你可以为成功的 AI 之旅奠定基础。记住保持沟通畅通，适应变化的需求，并随着组织的演变自然地发展你的 AI 和 DevOps 实践。
