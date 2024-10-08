# 工程已永远改变

> 原文：[`towardsdatascience.com/engineering-has-changed-forever-4d2fcc4f3841`](https://towardsdatascience.com/engineering-has-changed-forever-4d2fcc4f3841)

## 最近 AI 的进展正在颠覆传统架构。未能选择正确架构的公司将被甩在后头。

[](https://medium.com/@frankw_usa?source=post_page-----4d2fcc4f3841--------------------------------)![Frank Wittkampf](https://medium.com/@frankw_usa?source=post_page-----4d2fcc4f3841--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4d2fcc4f3841--------------------------------)![数据科学前沿](https://towardsdatascience.com/?source=post_page-----4d2fcc4f3841--------------------------------) [Frank Wittkampf](https://medium.com/@frankw_usa?source=post_page-----4d2fcc4f3841--------------------------------)

·发布于[数据科学前沿](https://towardsdatascience.com/?source=post_page-----4d2fcc4f3841--------------------------------) ·阅读时间 5 分钟·2023 年 11 月 20 日

--

> “我们对良好软件架构的几乎所有认识都与使软件易于更改有关。”
> 
> — Mary Poppendieck，软件开发专家和作者

在过去六个月里，工程的基础经历了戏剧性的转变，这种变化如此深远，以至于许多组织刚刚开始理解和适应其影响。大多数公司开始采取小的渐进步骤，将一些 AI 融入其软件中，而不是深入探索其可能性。

在本文中，让我们关注 AI 的架构影响。广义上，这种转变包括从传统的基于规则的架构转向更加动态的 AI 中心模型，这深刻改变了 AI 在软件系统中的角色，并挑战了我们对软件架构的基本理解。

![](img/8dba09067582c2905abfbb7c7ce50885.png)

图表：向 AI 工程转变（图像由作者提供）

你可以将架构转变描述为四个阶段：

1.  经典软件服务架构

1.  AI 驱动的软件服务架构

1.  AI 软件服务架构

1.  AI 工程

每个阶段在实施和结果上都有显著差异。一些公司会从一个阶段跌入下一个阶段，而其他公司则会跳跃前进。无论如何，大多数公司尚未在选项之间做出明确选择。更别提这种选择可能是生存或被竞争对手超越的关键区别。

> 注：在本文中，当我提到 AI 时，我通常指的是生成性 AI，不过一些读者可能会将某些机器学习应用也归为此类。根据我的经验，一个例子是金融科技公司如何使用机器学习进行信用决策和支付时机。

## **1. 经典软件服务架构**

我们在这里大幅简化，但为了本文的目的，我们将各种软件服务架构归入“经典”类别。它们具有可预测性和精确性，通常遵循基于规则的面向服务的方法。该系统中的每个组件都执行特定的、预定的功能。这种架构通常是确定性的和模块化的，目的是保持可预测性和可扩展性。

大多数企业中的软件工程都属于这一类别。每家公司都希望通过更多的 AI 功能来提升他们的软件，通常会着眼于一步 ahead 来集成一些 AI，这使得他们进入了一个…

## **2\. AI 赋能的软件服务架构**

AI 赋能的软件服务架构将经典的软件架构与精选的 AI 增强功能相结合。虽然核心依然是面向服务的结构，但某些服务会通过 AI 能力进行增强，引入机器学习、预测分析或生成式 AI 等元素。这种集成保留了系统的基础模块化和可预测性，但增加了适应性，使其能够动态响应不断变化的数据和需求。该架构在保持传统软件服务可靠性的同时，兼具 AI 带来的灵活性和新功能。

使传统服务具备 AI 的例子包括：将非结构化数据转换为结构化数据（将消费者情感提取到数据库中）、简单的聊天机器人服务，或简单的 AI/ML 决策服务（我们是否应向此人提供信用）。然而，当你尝试深入挖掘 AI 的潜力时，你很快会发现，在旧的软件构造中管理 AI 会大大限制其潜力。在 AI 之上放置一个编排服务会将其许多用例限制为输入-输出转换服务。

## **3\. AI 软件服务架构**

AI 软件服务架构在传统软件框架中嵌入至少一个完全由 AI 驱动的服务。在这里，经典平台管理整体功能，但利用一个 AI 专用服务来处理复杂的、自适应的任务。这个 AI 服务能够自主处理文本或数据事件等输入，支持系统的高级决策。尽管架构的核心仍然根植于经典的软件方法论，但这种 AI 中心服务的集成增强了功能。它将传统软件的结构化编排与 AI 的动态问题解决能力结合起来。

以这种方式使用 AI 可以解锁更广泛的 AI 应用场景。例如，一个 AI 服务可以执行输入转换、反思、决策和行动的混合过程。例如，当新文件到达时，AI 会查看文件，将其与指令集进行比较，确定这是一个非结构化文件，读取首几行内容，确定需要手动检查，并将其转发给监督部门。

## **4\. AI 工程**

在真正的本土 AI 工程中，焦点完全转向 AI 作为核心软件，与传统和混合架构显著不同。在这种范式中，工程工作重点在于赋能 AI 本身，提升其能力和自主性。AI 不仅是系统中的一个元素，它**是**系统，负责平台类的职责。工程师致力于扩展 AI 的功能，而不是协调 AI。工程工作的例子包括：集成多模态输入、拓宽决策范围以及配备多样化工具。这种方法是培养 AI 无缝融入各种工作流程和实际应用，确保它演变成一个更复杂、自主的实体。与以往 AI 补充或增强传统结构的模型不同，这里 AI 是基础，凭借其无限潜力和适应性重新定义了软件工程的概念。

在这一架构类别中可以想象的例子包括自适应学习循环、端到端 AI 流程自动化、多模态决策场景反馈，以及其他无尽的例子，唯一的限制就是你的想象力。

架构阶段 3 和 4 有相似之处，但它们也有显著差异。让我们进一步澄清这些差异。在 AI 工程中……

+   … 工程师的关注点转向使 AI 得以实现

+   … AI 是大多数活动的协调者，任何必要的协调都是非常轻量的，主要目的是使用户能够与 AI 互动

+   … 软件服务是 AI 的工具/促进者，而不是 AI 作为软件服务的工具

一个真正的 AI 工程服务示例：“获取金融市场变化，反思我的投资组合，决定是否需要进行额外投资，然后自主执行推荐（例如，卖出股票 ABC）。”

## **反思**

目前许多公司仍在摸索 AI 对他们的意义，并实验如何提取价值。当公司开始构建真正的解决方案时，至关重要的是要反思哪种工程范式最符合他们的目标。逐步启用 AI 解决方案是大多数公司将要开始的地方，但这样做会错失 AI 真正的能力。

很快，一代新兴公司将会出现。这些公司和那些深度融入人工智能本土理念的老公司，它们的发展速度将远远超越任何传统企业。

所以，请明智选择。在这个充斥着人工智能的工程领域，你的下一步行动可能决定成功或被淘汰的命运。
