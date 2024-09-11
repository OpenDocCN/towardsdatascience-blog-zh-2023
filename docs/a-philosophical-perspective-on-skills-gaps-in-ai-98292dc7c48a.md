# 人工智能技能缺口的（哲学）视角

> 原文：[https://towardsdatascience.com/a-philosophical-perspective-on-skills-gaps-in-ai-98292dc7c48a?source=collection_archive---------8-----------------------#2023-09-29](https://towardsdatascience.com/a-philosophical-perspective-on-skills-gaps-in-ai-98292dc7c48a?source=collection_archive---------8-----------------------#2023-09-29)

## 什么将初级机器学习从业者与高级解决方案架构师区分开来，特别是在快速发展的行业中？

[](https://medium.com/@lsci?source=post_page-----98292dc7c48a--------------------------------)[![Mathieu Lemay](../Images/39db2877c94829bef1d6642daf3ccecb.png)](https://medium.com/@lsci?source=post_page-----98292dc7c48a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----98292dc7c48a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----98292dc7c48a--------------------------------) [Mathieu Lemay](https://medium.com/@lsci?source=post_page-----98292dc7c48a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff84a70d8f74&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-philosophical-perspective-on-skills-gaps-in-ai-98292dc7c48a&user=Mathieu+Lemay&userId=f84a70d8f74&source=post_page-f84a70d8f74----98292dc7c48a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----98292dc7c48a--------------------------------) ·8分钟阅读·2023年9月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F98292dc7c48a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-philosophical-perspective-on-skills-gaps-in-ai-98292dc7c48a&user=Mathieu+Lemay&userId=f84a70d8f74&source=-----98292dc7c48a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F98292dc7c48a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-philosophical-perspective-on-skills-gaps-in-ai-98292dc7c48a&source=-----98292dc7c48a---------------------bookmark_footer-----------)![](../Images/9ffa03986811ef1e329620874fa78278.png)

在机器学习中，挫折是自然的，但也是可以避免的。图片来自 [Tim Gouw](https://www.pexels.com/photo/man-in-white-shirt-using-macbook-pro-52608/) [Pexels.com](https://pexels.com)。

*尽管有很多人工智能课程可供选择，我们常常发现许多申请者缺乏看似至关重要的能力。本文是一种通过轶事探索为什么会出现这种情况。*

+   [背景：思维模型与T型技能](#1a3b)

+   [机器学习中的不同之处](#5683)

+   [知识缺失的感知成本](#fb2d)

+   [推荐](http://4792)

最近，我们的一些项目中有客户询问关于将项目交接给尚不存在的内部团队的问题。“我们如何训练我们的团队以拥有你们所构建的解决方案？”“我们如何确保我们的团队在 AI 变化中保持未来适应性？”这些问题的大部分变体得到了推荐或变更管理计划的回答，但一个关键的主题依然存在：“我们如何在 AI 领域招聘合适的团队成员？”

对于像我们这样的企业 AI 咨询公司来说，这个问题尤其关键。在一个高度动态的行业中，识别、招聘、培训（和货币化）员工的能力需要付出大量的努力。更危险的是缺乏成功项目交付所需的附加技能，如需求管理、客户沟通和项目跟踪。

我们常见的员工和客户的最大障碍是缺乏非常具体且明确的技能，通常只涉及两到三个缺失的专业领域——却足以使他们整个项目陷入停滞。本文探讨了这种现象的表现形式。

# 背景：心智模型与 T 型技能

为了说明技能缺失如何导致项目突然中断以及如何应对这些问题，有两个简单的框架可以帮助解决这些问题。它们是：

+   心智模型，它们是对系统、概念或模式的抽象；以及

+   T 型技能，是指个体在多个领域拥有一般技能的同时，在其中一个或少数几个领域中高度专业化的能力。

## 心智模型

心智模型是独立的表征，旨在帮助与构造、概念和系统进行交互——机器学习领域中这些表征并不短缺。简单来说，它们有助于将思想进行分隔。

从研究论文“*心智模型：理论与方法的跨学科综合*”中，[Natalie Jones 和她的团队](https://www.jstor.org/stable/26268859#)进一步阐述了这一点：

> 心智模型是个人对外部现实的内部表征，帮助人们与周围的世界互动。它们由个体基于其独特的生活经历、感知和对世界的理解来构建。心智模型用于推理和决策，可以成为个体行为的基础。它们提供了过滤和存储新信息的机制。

这是你知道自己是否在使用合适的心智模型的方式：

+   它是有限的。

+   虽然是有限的，但在其表征目标上也是完整的。

+   它允许黑箱构造以在正确的抽象层次上简化思想。

然而，心智模型（以及重要的心智模型叠加）也有其局限性。再次引用 [N. Jones](https://www.jstor.org/stable/26268859#)：

> 然而，人们准确代表世界的能力始终有限且对每个人独特。心理模型因此被视为现实的不完整表现。它们也被视为不一致的表现，因为它们依赖于上下文，并可能根据使用情况而变化。本质上，心理模型必须是高度动态的，以适应不断变化的环境，并通过学习随时间演变。将认知表现视为复杂系统的动态、不准确模型，承认了人们在构思这些复杂系统方面的局限性。

因此，我们能够概念化、理解、分析、调查、原型设计、构建和部署任何级别的基于机器学习的自动化的能力，需要在多个知识领域中保持概念上的流动性，包括项目的技术和管理方面。

## T型技能

*在现代社会，“T型技能”这个术语有些不准确；真正有用的是在多个领域具有深度专长的多面手。*

具有T型技能的个人通常会在多个相关知识领域拥有广泛的知识，同时在某一特定主题或职能上具有深入的专长。

随着机器学习工程（即*科学机器学习原则的现实世界、风险意识应用*）的兴起，对同时具备多学科能力的需求显而易见。

描述T型个人的另一种方式是，某人在项目或职责范围内，能够成功处理多个必要的职能，并在其中一些方面是专家。他们在所有工作中都有危险，但在某些方面极具威胁。

这些个人通常表现为对他们整个工作范围有整体把握。尽管他们可能在某些任务方面不是专家，但他们至少知道如何将这些不熟悉的任务分解成具有明确输入和输出边界的工作项，因此他们对整个项目有可见性和能力。

尽管他们以前从未接触过鸭子，但他们知道鸭子应该是什么样子，鸭子应该怎么叫，这足以让他们不被阻碍。

# 机器学习中的不同之处

与云迁移项目或SaaS相比，机器学习通常具有许多按序堆叠的概念（与并发或树状结构相对），以及一系列针对生产级机器学习部署的额外考虑因素。部署依赖于模型类型，模型类型依赖于数据科学，而探索性数据分析依赖于项目需求。

在论文 *“机器学习软件应用程序在软件开发生命周期阶段的质量保证挑战”* ([Alamin2021](https://arxiv.org/abs/2105.01195)) 中，作者解释了传统软件开发与机器学习软件应用程序之间的明显差异。来自论文的内容：

> 在传统软件开发中，我们首先收集需求。然后设计、开发、测试、部署和维护应用程序。对于机器学习系统，我们仍需确定应用程序的目标，但不是设计算法，而是让机器学习模型从数据中学习所需的逻辑 [1]。这些观察引发了一个问题，即机器学习模型是否以及如何在不破坏软件开发生命周期（SDLC）的情况下被采纳。理想情况下，机器学习工作流/管道和SDLC阶段应该协同进行，以确保适当的质量保证。然而，如我们上面所提到的，由于机器学习模型设计和传统软件应用开发之间的固有差异，这些期望可能是不切实际的。

([Lwakatare2019](https://link.springer.com/chapter/10.1007/978-3-030-19034-7_14)) 在她的论文 *“机器学习系统的软件工程挑战分类：一项实证研究”* 中进一步阐述了：

> […] 尽管在学术界，许多关注点集中在学习算法的理论突破上，但实证研究表明，它们仅占操作性机器学习系统的一小部分 [20]。
> 
> 因此，在机器学习系统的开发和维护过程中遇到了一些挑战 [6]。为了解决这个问题，新兴证据强调了在机器学习系统开发中需要考虑和扩展已建立的软件工程（SE）原则、方法和工具 [11,19]。

因此，我们可以将AI项目描述为需要稍多的思维模型来完成类似规模的项目。但AI软件通常需要更多地顺序堆叠这些原则，而云和SaaS似乎在理念上更具并发性，从而导致它们之间的关键性相互依赖减少。

# 知识缺失的感知成本

让我们以一个简单的项目模型为例，其中需要在9个假设领域中进行一系列活动。这些领域可以代表项目管理、需求工程、数据科学、机器学习、云计算和MLOps技能的混合。虽然非常简化，但我们遇到的大多数项目都有类似的顺序变化的专业技能。

![](../Images/43d448cd4312b5197fb6ee18b0c45846.png)

作者插图。

然而，随着新技术的出现，有时你会被要求使用新技术。有时这些是小的变化（例如，用DVC替换Git LFS），有时则是更大、更复杂的变化（如从单体虚拟机方法转向Kubernetes）。理想情况下，你和你的团队应该完全胜任所有这些任务；实际上，随着行业变化的速度，你可能对大多数这些任务都比较熟悉，有些可能是新的或尚未完全掌握的。

在这种情况下，存在一个知识缺失的单一点。

![](../Images/8ef79cca3c6a6a1ec76e6cc743bec814.png)

作者插图。

我认为这是一种非常正常的操作状态。客户想尝试一个新的库；有人建议使用不同的数据库。这种情况时有发生。需要更改的模块可以在心理上进行热插拔，除了阅读一些API或学习不同的项目管理方法外，没有其他重大后果。

在MLOps中，问题在于通常两个或更多的问题领域会导致在项目中感知到的知识缺失更大。

![](../Images/b3b1d7bf3de4ecdd67657b9a8a4af62c.png)

作者插图。

虽然只有两个问题领域，但感知到的效能缺失影响了你大多数的指定活动。

**一 handful 的缺失概念将表现为在项目中无法交付或无法有效工作的令人沮丧的能力缺失。**

# 推荐

为了明确，我们整个团队（包括我自己）不断主动学习新的概念和技术，以确保没有意外的主题或知识领域让我们完全盲目。我们通常遵循以下步骤来发现这些领域：

+   **识别**。一旦你听到将要使用的新技术或方法，记下它的名称、描述和背景。

+   **隔离**。虽然很难识别缺失知识中的“未知未知”，我们会通过简单的问题来解决它们：这个概念的背景是什么？它与我已知的有什么相似之处？它与我已知的有什么不同之处？

+   **从小开始**。[“Hello World”](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program) 示例仍然是确保你有效使用特定工具的有效方法。

不要将你的技能总量作为进展的衡量标准，你应该关注你技能的组合以及你如何将它们结合起来以交付项目价值和成功。

# 其他你可能喜欢的文章

+   [我们如何赢得第一个政府AI项目](https://medium.com/towards-data-science/how-we-won-our-first-government-ai-project-8c67e58c22f0)

+   [解读MLOps的业务考量](https://medium.com/towards-data-science/interpreting-the-business-considerations-of-mlops-f32613c4bcb4)

+   [PyTorch 与 TensorFlow 在基于 Transformer 的 NLP 应用中的比较](/pytorch-vs-tensorflow-for-transformer-based-nlp-applications-b851bdbf229a)

+   [MLOps 批处理：在 GPU 上运行 Airflow](/mlops-for-batch-processing-running-airflow-on-gpus-dc94367869c6)

+   [数据集偏见：制度化歧视还是充分透明？](/dataset-biases-institutionalized-discrimination-or-adequate-transparency-ae4119e2a65c)

*如果你对这篇文章或我们的 AI 咨询有其他问题，请随时通过* [***LinkedIn***](https://www.linkedin.com/in/mnlemay/)*或通过* [***电子邮件***](mailto:matt@lemay.ai)*联系我。*

-Matt.
