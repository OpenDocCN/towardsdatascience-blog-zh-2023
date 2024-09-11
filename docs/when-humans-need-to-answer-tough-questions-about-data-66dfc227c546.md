# 人类在需要回答数据相关的棘手问题时

> 原文：[https://towardsdatascience.com/when-humans-need-to-answer-tough-questions-about-data-66dfc227c546?source=collection_archive---------9-----------------------#2023-12-07](https://towardsdatascience.com/when-humans-need-to-answer-tough-questions-about-data-66dfc227c546?source=collection_archive---------9-----------------------#2023-12-07)

[](https://towardsdatascience.medium.com/?source=post_page-----66dfc227c546--------------------------------)[![TDS Editors](../Images/4b2d1beaf4f6dcf024ffa6535de3b794.png)](https://towardsdatascience.medium.com/?source=post_page-----66dfc227c546--------------------------------)[](https://towardsdatascience.com/?source=post_page-----66dfc227c546--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----66dfc227c546--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----66dfc227c546--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-humans-need-to-answer-tough-questions-about-data-66dfc227c546&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----66dfc227c546---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----66dfc227c546--------------------------------) · 作为 [通讯](https://towardsdatascience.com/newsletter?source=post_page-----66dfc227c546--------------------------------) 发送 · 3分钟阅读 · 2023年12月7日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F66dfc227c546&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-humans-need-to-answer-tough-questions-about-data-66dfc227c546&user=TDS+Editors&userId=7e12c71dfa81&source=-----66dfc227c546---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F66dfc227c546&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-humans-need-to-answer-tough-questions-about-data-66dfc227c546&source=-----66dfc227c546---------------------bookmark_footer-----------)

数据科学和机器学习专业人员知道如何从数据中寻找答案：这可能是他们工作的核心支柱。当我们探讨一些涉及数据的棘手问题时，例如内置的偏见以及它可能被利用来达到可疑目的的方式，情况就变得更加复杂。

随着我们进入年末最后阶段，我们邀请读者探讨一些引发关键讨论的大问题，这些问题近年来已经引起广泛关注，并且几乎可以肯定会继续塑造2024年及以后的领域。

我们本周的亮点涵盖了广泛的主题，从数据支持知识的本质到其在医疗等特定领域的应用；我们希望这些内容能够激发更多的反思，并吸引新的参与者参与到这些重要的讨论中。

+   [**偏差、毒性与破解大型语言模型（LLMs）**](/bias-toxicity-and-jailbreaking-large-language-models-llms-37cd71a3048f) LLMs 的快速崛起和演变使得从业者难以暂停并评估其固有风险。[Rachel Draelos, MD, PhD](https://medium.com/u/209c0f742bcf?source=post_page-----66dfc227c546--------------------------------) 对近期研究的详细概述提供了对这些模型在大规模上延续甚至加剧偏差和毒性的能力的及时观察。

+   [**哲学与数据科学——深入思考数据**](/philosophy-and-data-science-thinking-deeply-about-data-222cc9fbdcc5) 认识论中的主要概念——如演绎推理和归纳推理、怀疑主义和实用主义——如何融入数据科学家的工作中？[Jarom Hulet](https://medium.com/u/88982a88b4e5?source=post_page-----66dfc227c546--------------------------------) 的最新文章探讨了这两个领域之间（有时意外的）重叠。

+   [**数据科学中的认知偏差：类别规模偏差**](/cognitive-biases-in-data-science-the-category-size-bias-8dbd851608c3) 在关于认知偏差的新系列中，[Maham Haroon](https://medium.com/u/398c9514a58b?source=post_page-----66dfc227c546--------------------------------) 详细阐述了我们的大脑在分析和从数据中提取洞见时，如何引导我们走向错误的多种方式。第一篇文章聚焦于类别规模偏差，并解释了它如何渗透到数据科学家在日常工作中所做的假设中。

![](../Images/c67edaa61d47e29f7dc616cc36011318.png)

图片由 [Bozhin Karaivanov](https://unsplash.com/@bkaraivanov?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   [**AI 在医疗保健中应扮演什么角色？**](/what-role-should-ai-play-in-healthcare-7acaf9131278) 我们迄今为止所讨论的偏差可能对模型、业务和底线造成严重破坏。然而，[Stephanie Kirmer](https://medium.com/u/a8dc77209ef3?source=post_page-----66dfc227c546--------------------------------) 强调，这些偏差在医疗领域尤为严重，因为该领域常涉及生死攸关的情况，“失败的风险极为灾难性。”

+   [**变压器的挽歌？**](/a-requiem-for-the-transformer-297e6f14e189)

    在一个快速变化的领域中，我们容易将一个六年前的概念视为核心和永恒。自2017年以来，Transformer模型在主流AI工具的采纳中发挥了重要作用；但正如[萨尔瓦托雷·拉伊利](https://medium.com/u/f1a08d9452cd?source=post_page-----66dfc227c546--------------------------------)所指出的，它们也可能有保质期，也许现在是时候思考下一步是什么。

大问题很重要，但中等规模和紧凑型问题也很有用！不要错过我们最近关于职业变动、数据工程和其他及时话题的一些精彩内容：

+   我们如何[让模型忘记信息](/learn-to-unlearn-machines-6b0843dfc40f)我们不再希望它们保留？[叶夫根尼亚·苏霍多尔斯卡娅](https://medium.com/u/ab8927d88a52?source=post_page-----66dfc227c546--------------------------------)提出了一种数据驱动的方法，用于生成语言模型的“遗忘”。

+   思考在新的一年里换个角色吗？[Thu Vu](https://medium.com/u/4336ed7a3103?source=post_page-----66dfc227c546--------------------------------)最近分享了一个详细的路线图，适合那些[有兴趣转向数据分析](/how-to-transition-to-data-analytics-128a3dca54d5)的人。

+   想要了解[图神经网络领域的前沿研究](/co-operative-graph-neural-networks-34c59bf6805e)，可以查看[迈克尔·布朗斯坦](https://medium.com/u/7b1129ddd572?source=post_page-----66dfc227c546--------------------------------)的最新文章（与合著者本·芬克尔施泰因、伊斯梅尔·杰兰和邢越·黄一起）。

+   如果你是一名害怕回填过程的数据工程师，[高小旭](https://medium.com/u/2adc5a07e772?source=post_page-----66dfc227c546--------------------------------)的指南[概述了高效的实现方法](/demystify-data-backfilling-cf1713d7f7a3)，将帮助你简化这个工作流程。

+   对scikit-learn不熟悉？[Yoann Mocquin](https://medium.com/u/173731d06320?source=post_page-----66dfc227c546--------------------------------)最近推出了一系列[适合初学者的sklearn教程](/sklearn-tutorial-module-1-f31b3964a3b4)，带我们了解这个流行的机器学习库的不同模块和功能。

+   LLMs的SQL会是什么样的？[玛利亚·曼苏罗娃](https://medium.com/u/15a29a4fc6ad?source=post_page-----66dfc227c546--------------------------------)详细介绍了[语言模型查询语言（LMQL）](/lmql-sql-for-language-models-d7486d88c541)的概述，允许用户在一个提示中结合多个调用，控制输出并降低成本。

感谢你支持我们作者的工作！如果你喜欢TDS上的文章，可以考虑[成为Medium的朋友会员](https://blog.medium.com/become-a-friend-of-medium-dd2fa7bf16c3)：这是一个全新的会员级别，为你喜爱的作者提供更多奖励。

直到下一个Variable，

TDS 编辑部
