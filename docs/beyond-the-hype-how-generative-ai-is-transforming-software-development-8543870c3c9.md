# 超越炒作：生成式 AI 如何改变软件开发

> 原文：[`towardsdatascience.com/beyond-the-hype-how-generative-ai-is-transforming-software-development-8543870c3c9?source=collection_archive---------5-----------------------#2023-05-30`](https://towardsdatascience.com/beyond-the-hype-how-generative-ai-is-transforming-software-development-8543870c3c9?source=collection_archive---------5-----------------------#2023-05-30)

[](https://medium.com/@laumannfelix?source=post_page-----8543870c3c9--------------------------------)![Felix Laumann](https://medium.com/@laumannfelix?source=post_page-----8543870c3c9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8543870c3c9--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8543870c3c9--------------------------------) [Felix Laumann](https://medium.com/@laumannfelix?source=post_page-----8543870c3c9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F57f955c90c95&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-hype-how-generative-ai-is-transforming-software-development-8543870c3c9&user=Felix+Laumann&userId=57f955c90c95&source=post_page-57f955c90c95----8543870c3c9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8543870c3c9--------------------------------) ·7 min read·2023 年 5 月 30 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8543870c3c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-hype-how-generative-ai-is-transforming-software-development-8543870c3c9&user=Felix+Laumann&userId=57f955c90c95&source=-----8543870c3c9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8543870c3c9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbeyond-the-hype-how-generative-ai-is-transforming-software-development-8543870c3c9&source=-----8543870c3c9---------------------bookmark_footer-----------)![](img/ce4f72ea9b361c01b4aa2737ca3a62af.png)

图片由 [Google DeepMind](https://unsplash.com/@deepmind) 提供，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **介绍**

人工智能（AI）自 1950 年代诞生以来，已经取得了长足的进步。从基于规则的专家系统到深度学习，AI 已经发展成为一种强大的技术，彻底改变了医疗、金融、交通和制造等各个行业。AI 最新的热门话题是生成式 AI。是的，它是一个热门话题，但无疑具有颠覆我们个人和职业生活各个方面的潜力。在这篇博客文章中，我们将探讨一个许多技术行业人士正在思考的方面：我们如何构建软件。

一类生成式 AI 算法的子群体，即一类能够基于用户输入数据生成文本的模型，被称为大型语言模型（LLMs），在过去几个月引起了显著的关注。LLMs 在其最基本的描述中，是多层深度神经网络，经过大量——非常大量——的数据集训练，从中模型学习潜在的模式，并生成遵循这些模式的新数据。向 LLM 写入并请求回应称为“提示”，了解如何编写有效的提示是一项需求旺盛的技能，通常被称为提示工程。

为了调整大型语言模型（LLMs）以产生类似人类的回应，一种称为“人类反馈强化学习”（RLHF）的机器学习训练框架已被证明特别有效。这需要大量的人反复提示早期版本的 LLM，阅读其回应，并告诉模型是否通过了他们的“类人”试金石。这些“提示者”通常被告知要注意模型可能带来的性别或种族偏见对用户的无意侮辱。

LLMs 正在改变我们创作的方式，赋予每个人编写诗歌、文章、营销文案等所需的工具。应用场景非常广泛，但在这篇博客文章中，我们将探讨生成式 AI 对未来软件开发的影响。提示是生成式 AI 的一个关键概念，所以在深入探讨之前，让我们定义一下它。

**提示：** 提示作为用户或程序提供给 LLM 的输入或查询，旨在引出模型的针对性回应。这些提示形式多样，如自然语言句子、问题、代码片段或命令。它们可以是文本和代码的组合，允许多样化的交互。此外，提示可以嵌套或链式使用，从而利用一个提示的输出作为另一个提示的输入。这种能力促进了用户与 LLM 之间，甚至多个 LLM 之间的复杂和动态交流。

![](img/d0efe08e8228cbb73a7d75fd436e6be7.png)

图片由 [Christina](https://unsplash.com/@wocintechchat) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **LLMs 如何改变我们构建软件的方式**

尽管 LLM 最初是为语言生成任务开发的，但它们已经证明是多功能的，可以应用于生成文本之外的各种任务。我们将从探索一个对任何构建软件的人都特别有趣的能力开始：代码补全。

LLM 可以通过根据已编写的代码或根据描述软件某一部分功能的提示预测下一行代码来协助开发人员。例如，像“编写一个 Python 函数，将 NumPy 数组转换为字典，其中键是数组第一维范围的字符串”这样的提示将返回如下代码。

```py
import numpy as np

def array_to_dict(arr):
    """
    Convert a NumPy array into a dictionary where the keys are the strings of the range over the first dimension of the array.

    :param arr: The NumPy array to convert to a dictionary.
    :return: The resulting dictionary.
    """
    dict_ = {}
    for i in range(arr.shape[0]):
        key = str(i)
        dict_[key] = arr[i]
    return dict_
```

大多数软件开发人员习惯于“结对编程”的概念，其中一个开发人员记住整体软件结构，而另一个开发人员编写代码。像 GitHub 的 Copilot 这样的专用 LLM 上构建的产品可以作为编程伙伴的一个合适替代品——帮助加速软件开发过程。然而，在这种程度的协助下，用户必须了解如何提示 LLM 以实现他们想要的输出。这听起来可能很简单，但实际上需要对相关编程语言中的数据结构、函数和许多其他概念有一定的了解。

在[软件革命](https://blog.samaltman.com/the-software-revolution)的下一次飞跃将发生在开发人员不再需要将软件的总体功能拆分成较小的部分，这些部分可以通过 LLM 理解的提示进行提炼，比如上面的数组到字典的示例。当 LLM 能够理解主要功能本身并通过*对话体验*提供可行的实现建议时，生成式 AI 将从根本上改变软件开发的方式。

**从本质上讲，这将使不仅是开发人员能够创建软件，而是任何能够准确描述所需软件产品功能的人都能做到这一点。**

当前，LLM 不能返回创建高级软件产品所需的整个代码库。以一个例子来说，我想建立一个 Android 移动应用程序，该应用程序根据我拍摄的水果篮的照片来建议平滑的食谱。

*提交给 ChatGPT 的准确提示： “编写一个 Android 移动应用程序的代码，该应用程序根据我拍摄的水果篮的照片来建议平滑的食谱。”*

今天，一个大型语言模型（LLM）会返回一个关于如何构建该应用程序的大纲——可能包含示例代码——但不会返回完全功能的软件。

尽管返回整个代码库对构建此类应用程序的人将大有帮助，但这仍不足以启动并使其在任何拥有 Android 手机的人都可用，因为代码很可能需要托管在 Google Firebase 中并提交至 Play Store 进行验证（尽管最后一步可以通过 Firebase 实现）。

尽管如此，当前 LLM 的能力离实现这些功能并不远。一个普遍的误解是 LLM 是这一执行链中的瓶颈，例如，从编写移动应用程序代码到在 Play Store 上运营和可下载。实际上，当前的瓶颈是 LLM 的*安全和可靠集成*到第三方系统中。以上述移动应用程序示例为例，需要至少两个集成：LLM 到 GitHub 以存储代码，LLM 到 Firebase 以配置应用程序并提交至 Play Store 发布。

如果 LLM 能够提炼高级软件产品描述并使产品具备可操作性，那么开发过程将变成一种类似于产品经理和全栈软件工程师之间的对话体验。用过于简单的话说，开发工作的创意负责人，产品经理，会用自然语言提供产品的核心功能，而她的对手，即“全栈软件工程师转型的 LLM”则会提出澄清问题并技术性地执行产品经理的描述。

# **LLM 与第三方系统的安全和可靠集成**

让 LLM 直接与第三方系统交互的安全性和可靠性对使用 LLM 的好处至关重要。我们将描述集成 LLM 与第三方系统安全可靠的三个重要要求，但这个列表并不是详尽无遗的。

1.  **模型验证：** 在将 LLM 集成到第三方系统之前，彻底验证模型的准确性非常重要。这里的准确性指的是 LLM 在用户给定提示时的输出。LLM 的输出会在第三方系统中引发某种动作。必须在测试环境中验证这些动作是否在相同基本含义的提示下，随着提示的变化而一致。

1.  **透明性和可解释性：** LLM 是复杂的，通常很难理解某个响应是如何产生的。如果它们的决策过程，即它们如何得出任何给定的响应，能够通过自然语言解释进行透明化，那么在对话软件创建过程中，人类合作伙伴将能更好地了解如何提示 LLM，以产生期望的结果。

1.  **版本控制：** LLMs 可能会随着时间的推移而更新或替换，因此拥有明确的版本控制策略非常重要，因为对同一提示的响应可能会因版本不同而有所变化。因此，之前模型版本的存储库、任何模型更新后的全面测试和监控阶段，以及应对新模型更新中的意外问题或故障的应急计划都是至关重要的。

# **结论**

总之，大型语言模型（LLMs）具备出色的能力，适合协助处理来自大量结构化信息池并需要高度关注细节的任务。通过自动化许多重复且耗时的任务，LLMs 使其用户（例如产品经理）能够专注于需要创造力和解决问题技能的更高层次任务。虽然 LLMs 可以自动化许多常规任务，但它们并不具备人类所拥有的相同程度的创造力、直觉和同理心。

对于 LLMs 在许多商业应用中的成功，安全且可靠地集成到行业特定的第三方系统中非常重要。LLMs 的用户需要能够执行某些他们通常在没有专门的软件开发人员支持的情况下无法执行的操作。无代码工具在许多行业和职位中已经在兴起，但它们的功能需要成熟，以构建超越特定和狭窄功能的复杂软件（例如可以通过 Levity 构建的功能）。

总结来说，LLMs 在许多方面正在改变软件开发，而我们今天所看到的可能只是一个开始。从自动化常规任务到启用新的创造力和创新形式，LLMs 有望在未来的软件开发中发挥重要作用。随着生成式 AI 的持续兴起，我们必须超越炒作，看到技术发展的一个重要时刻正在形成。生成式 AI 改变了我们与软件的互动方式，未来它可能会改变软件的开发方式——使任何拥有产品愿景的人都能够通过对话的方式实现它。
