# 程序仿真提示框架的定义：Prompt Engineering Evolution

> 原文：[https://towardsdatascience.com/prompt-engineering-evolution-defining-the-new-program-simulation-prompt-framework-d8a1ee096904?source=collection_archive---------2-----------------------#2023-09-29](https://towardsdatascience.com/prompt-engineering-evolution-defining-the-new-program-simulation-prompt-framework-d8a1ee096904?source=collection_archive---------2-----------------------#2023-09-29)

## 制定不同类型程序仿真提示的路线图

[](https://medium.com/@hominum_universalis?source=post_page-----d8a1ee096904--------------------------------)[![Giuseppe Scalamogna](../Images/ff7b3bec7c26e5684fba26211b6f027a.png)](https://medium.com/@hominum_universalis?source=post_page-----d8a1ee096904--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d8a1ee096904--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d8a1ee096904--------------------------------) [Giuseppe Scalamogna](https://medium.com/@hominum_universalis?source=post_page-----d8a1ee096904--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe039aa8b7221&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-evolution-defining-the-new-program-simulation-prompt-framework-d8a1ee096904&user=Giuseppe+Scalamogna&userId=e039aa8b7221&source=post_page-e039aa8b7221----d8a1ee096904---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----d8a1ee096904--------------------------------) ·8 分钟阅读·2023 年 9 月 29 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd8a1ee096904&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-evolution-defining-the-new-program-simulation-prompt-framework-d8a1ee096904&user=Giuseppe+Scalamogna&userId=e039aa8b7221&source=-----d8a1ee096904---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd8a1ee096904&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fprompt-engineering-evolution-defining-the-new-program-simulation-prompt-framework-d8a1ee096904&source=-----d8a1ee096904---------------------bookmark_footer-----------)![](../Images/1d11500b6dcaa48488da1f1520727f47.png)

来源：图像由作者生成，使用MidJourney

## **介绍**

在我最近的文章中，[*新的ChatGPT提示工程技术：程序模拟*](https://medium.com/towards-data-science/new-chatgpt-prompt-engineering-technique-program-simulation-56f49746aa7b)，我探讨了一种新的提示工程技术类别，旨在使ChatGPT-4表现得像一个程序。在工作的过程中，特别让我印象深刻的是ChatGPT-4在程序规范范围内自我配置功能的能力。在原始的程序模拟提示中，我们严格定义了一组功能，并期望ChatGPT-4保持程序状态的一致性。结果令人印象深刻，许多读者分享了他们成功将这种方法应用于各种用例的经验。

但是，如果我们稍微放宽一下限制会发生什么？如果我们给ChatGPT-4更多的自由来定义功能和程序的行为呢？这种方法不可避免地会牺牲一些可预测性和一致性。然而，增加的灵活性可能会为我们提供更多选项，并且可能适用于更广泛的应用领域。我提出了一个初步框架，涵盖了这个技术类别，如下图所示：

![](../Images/2775575dee28fc7c0c8fa4e69c38bf26.png)

来源：作者提供的图像

让我们花一点时间来审视这个图表。我已经确定了两个在程序模拟提示制作中广泛适用的关键维度：

1.  决定定义程序模拟的功能数量和类型。

1.  决定程序的行为和配置的自主程度。

在第一篇文章中，我们制定了一个属于“结构化预配置”类别（紫点）的提示。今天，我们将探索“非结构化自配置”方法（蓝点）。这个图表的有用之处在于，它提供了一个简洁的概念路线图，用于制定程序模拟提示。它还提供了易于应用的维度，便于在应用技术时进行实验、调整和优化。

## 非结构化自配置程序模拟提示

话不多说，让我们开始检查“非结构化自配置程序模拟”方法。我设计了一个提示，其目的是创建插图儿童故事，如下所示：

“*表现得像一个自我组装的程序，其目的是创建插图儿童故事。你在确定程序的功能、特性和用户界面时拥有完全的灵活性。对于插图功能，程序将生成可以与文本到图像模型一起使用的提示来生成图像。你的目标是在收到此提示后，将其余聊天运行成一个完全功能的程序，准备好接受用户输入。*”

正如你所见，这个提示看起来非常简单。在提示越来越长、复杂，并且特定到难以适应不同情况的时代，这可能具有吸引力。我们赋予了GPT-4完全的自由来定义功能、配置和程序行为。唯一的具体指示是指导输出为可以用于文本到图像生成的插图提示。另一个重要的成分是我设定了一个目标，让聊天模型努力实现。最后需要注意的是，我使用了“自组装”而不是“自配置”的术语。你可以尝试两者，但“自配置”倾向于促使ChatGPT模拟实际程序/用户交互。

**“表现得像”与“行动得像”**

还值得强调提示中的另一个不同用词。你们都遇到过在提示中使用“表现得像某种专家”的指导。在我的测试中，“行动得像”倾向于引导聊天模型朝着角色驱动的响应。“表现得像”提供了更多的灵活性，特别是当目标是让模型更像一个程序或系统时。同时，它也可以在以角色为中心的上下文中使用。

如果一切按计划进行，结果输出应该是这样的（注意：你们都会看到略有不同的内容。）

![](../Images/a4c5441abf5ffc2fdb65aec490affab1.png)

这看起来和感觉起来都像一个程序。功能直观且合适。菜单甚至包括了“设置”和“帮助与教程”。让我们探索一下这些，因为我承认它们是意料之外的。

![](../Images/3032a02b8c444a211cde5fffc9b74105.png)

所展示的“设置”非常有用。我将做一些选择以简化故事，并将语言和词汇水平设置为“初学者”。

由于我们有兴趣考察模型自动自配置程序的能力，我将把设置更改合并成一行文本，看看效果如何。

![](../Images/197a2b04b02acd7bd62ff8f3f99d8b1f.png)

设置更新已确认。随后出现的菜单选项完全是自由形式的，但适合我们在“程序”中的上下文。

现在让我们检查“帮助与教程”

![](../Images/5fb637e8fbaa485fbd1a83807e698203.png)

接下来我们深入探讨“插图提示与生成”。

![](../Images/2cc099639d6f732a1b02757e3674223c.png)

再次，非常有用，令人印象深刻，因为我们在程序定义中没有定义这些。

我将返回主菜单并启动创建新故事。

![](../Images/a33046c18e4c460cd49d64edbf856638.png)

这是一个简洁而简单的小故事，共3页，针对初学者词汇水平（正如我们在设置中指定的那样）。所呈现的功能再次适合我们在程序中的位置。我们可以生成插图，修改故事或退出到主菜单。

让我们继续处理我们的插图提示。

![](../Images/4c228f73c567fb65e53a1fcd28058a27.png)

我没有包括其他插图提示生成的文本，但它们类似于你在第一页看到的那一个。让我们将插图提示按原样提供给 MidJourney 以生成一些图像。

*“一只可爱的棕色泰迪熊，圆圆的眼睛坐在一个小蓝房子的窗台上，位于一个宁静的城镇。”*

![](../Images/c13b87ac79c970f079b815a117d155dc.png)

来源：作者提供的图片，并使用 MidJourney 生成

非常好。这个步骤是手动的，我们面临的额外挑战是确保所有三页上的插图一致。这可以通过 MidJourney 实现，但需要上传其中一张图像作为基准，以生成其他图像。也许 DALL·E 3 将包括允许无缝完成此操作的功能。至少，OpenAI 宣布的功能表明我们可以直接在 ChatGPT 中生成图像。

让我们“保存并退出”，看看在我们的 ChatGPT 对话中会发生什么：

![](../Images/b7de9e3d2b1e293e0f63988f1bc88b4d.png)

现在，让我们尝试“加载已保存的故事”。

![](../Images/7b04a2e41f9bdd4f621da08122a890d5.png)

“迷失的泰迪”已被“保存”，当我指示它“打开”时，它回忆起整个故事和所有插图提示。最后，它提供了这个自组装的功能菜单：

![](../Images/3dbed01a0ffcde1150ea4c51bcd711d5.png)

好的。我们到此为止。如果你愿意，可以继续生成自己的故事，但请记住，由于提示的设计，结果行为对每个人来说都会有所不同。

让我们继续一些总体结论和观察。

## 结论和观察

非结构化自配置程序模拟技术展示了强大的能力，这源自一个简单的提示，该提示提供了明确而简洁的目标，但 otherwise 给予模型广泛的自由裁量权。

这会有什么用处呢？也许你不知道如何定义程序模拟要执行的功能。或者你已经定义了一些功能，但不确定是否还有其他有用的功能。这种方法非常适合原型设计和实验，最终制定出“结构化预配置程序模拟”提示。

由于程序模拟自然融合了诸如思维链、基于指令、逐步和角色扮演等技术元素，它是一个非常强大的技术类别，你应该尝试并保持它，因为它与聊天模型的广泛使用场景相契合。

**超越生成聊天模型，迈向生成操作系统**

随着我对程序模拟方法的深入探索，我确实更好地理解了为什么OpenAI的萨姆·奥特曼表示，提示工程的意义可能会随着时间的推移而减弱。生成模型可能会发展到超越生成文本和图像的程度，本能地知道如何执行一组特定的任务以达到期望的结果。我最新的探索让我觉得我们离这个现实比我们想象的要近。

让我们考虑一下生成性人工智能可能会走向何方，为了做到这一点，我认为把生成模型视为人类的一种方式是有帮助的。以这种思维方式，让我们考虑一下人们是如何在特定能力领域或知识领域中获得熟练的。

1.  这个人通过使用特定领域的知识和技术，在监督和非监督环境中进行培训（无论是自我培训还是外部培训）。

1.  这个人的能力会相对于相关领域的能力进行测试。根据需要提供改进和额外的培训。

1.  这个人被要求（或自己要求自己）执行一个任务或完成一个目标。

这听起来很像训练生成模型所做的事情。然而，在执行阶段或“询问”阶段确实出现了一个关键的区别。通常，熟练的个人不需要详细的指令。

我相信未来，当与生成模型互动时，“询问”的机制将更接近我们与熟练人类的互动。对于任何给定的任务，模型将展现出深刻的理解或推断目标和期望结果的能力。鉴于这一发展趋势，看到多模态能力的出现（例如DALL·E 3与ChatGPT的集成，以及ChatGPT新宣布的视觉、思考和听觉能力）应该不奇怪。我们可能最终会看到一种元代理的出现，它实质上为我们的设备（无论是手机、电脑、机器人还是其他智能设备）提供操作系统。有人可能会担心这会导致计算的大量普及带来的低效和环境影响。但如果历史可以作为指示，这些方法产生了人们想要的工具和解决方案，创新机制将启动，市场也会相应地提供。

感谢阅读，希望你在你的提示冒险中发现程序模拟是一种有用的方法！我正在进行额外的探索，所以一定要关注我，并在新文章发布时获得通知。

除非另有说明，本文中的所有图片均由作者提供。
