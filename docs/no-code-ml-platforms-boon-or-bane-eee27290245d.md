# 无代码机器学习平台：福音还是祸根？

> 原文：[`towardsdatascience.com/no-code-ml-platforms-boon-or-bane-eee27290245d`](https://towardsdatascience.com/no-code-ml-platforms-boon-or-bane-eee27290245d)

## 深入了解无代码平台如何促进加速的机器学习应用

[](https://blog.jojomoolayil.com/?source=post_page-----eee27290245d--------------------------------)![Jojo John Moolayil](https://blog.jojomoolayil.com/?source=post_page-----eee27290245d--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eee27290245d--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eee27290245d--------------------------------) [Jojo John Moolayil](https://blog.jojomoolayil.com/?source=post_page-----eee27290245d--------------------------------)

·发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----eee27290245d--------------------------------) ·9 分钟阅读·2023 年 1 月 13 日

--

![](img/3f5b8b0b8314103cd745481ac0ae3a49.png)

[Scott Graham](https://unsplash.com/@homajob?utm_source=medium&utm_medium=referral)的照片，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

近年来，我们看到一些大型企业和蓬勃发展的初创公司推出了多种无代码机器学习和数据科学平台。如今，大多数领先的云服务提供商至少提供一种无代码/低代码机器学习平台。微软的[Azure ML Studio](https://studio.azureml.net/)、亚马逊的[Sagemaker Canvas](https://aws.amazon.com/sagemaker/canvas/)和谷歌的[AutoML](https://cloud.google.com/automl?utm_source=google&utm_medium=cpc&utm_campaign=na-CA-all-en-dr-bkws-all-all-trial-e-dr-1011347&utm_content=text-ad-none-any-DEV_c-CRE_626205913379-ADGP_NA+%7C+CA+%7C+en+%7C+Desk+%7C+SEM+%7C+BKWS+%7E+EXA+%7E+AutoML%28FINAL%29-KWID_43700073441701560-kwd-1721531454223&utm_term=KW_google+cloud+auml-ST_google+cloud+auml&gclid=CjwKCAiAkfucBhBBEiwAFjbkryI1K09-P4GLdSn0hEsFN8fQqfOvK5qPDb24shTT8oner0NUGh0hbxoCodYQAvD_BwE&gclsrc=aw.ds)是其中的一些例子。如果你更深入地观察它们，会发现它们的共同使命是民主化 AI/ML/DS。很长一段时间，我坚信无代码/低代码不会有效地民主化机器学习。然而，最近我改变了看法，原因可能不是你所猜测的。让我解释一下。

# 简短回顾

回到 2015 年，当我探索 Azure ML 工作室时，我确实感到很惊讶。那个时候的平台已经成熟，提供了丰富的功能来解决机器学习问题。数据导入、探索性数据分析、模型构建、超参数调优和部署的整个过程都可以通过拖放工具完成。这是我在这个类别中使用的第一个工具之一，我感到了一种完整感。这个工具让我实现了当时测试的目标——将一个模型部署到生产环境中，而不需要写一行代码（尽管是一个用于测试的简单模型）。然后，到 2016 年底，我确信这一类别的服务有巨大的市场，而且无代码工具很快会在机器学习问题上得到广泛采用。

然而，随着时间的推移，我几乎没有注意到这些工具在我主要参与的社区中的采用情况。这些工具确实有一些很炫的演示，但在大多数情况下，它们对我而言意义不大。渐渐地，我开始倾向于认为这些工具对于使人工智能普及来说是多余的。我的理由很简单；对于商业上重要的并最终部署到生产环境中的严肃机器学习用例而言，从不适合使用将控制权锁定在基于 UI 的工具中的工具构建。此外，对于严肃的机器学习用例，数据工程和数据处理是一个庞大的工作量。工程的庞大体量和复杂性永远无法适合过于简化的无代码工具。对我而言，无代码/低代码平台突然变成了一个被美化的工具，仅仅用于做出优秀的营销。

# 回到今天

最近，我开始从不同的角度看待这些工具。我觉得我可能对这些工具的看法存在偏见。这个可能性很大，因为我大多与已经熟悉某种形式的编码的数据显示科学家或在该领域经验丰富的专业人士互动。此外，我大多数时候在一个与软件工程师紧密合作的环境中工作，这些工程师帮助将研究原型转化为生产管道。因此，我们必须建立一个研究工作流程实践，以确保将研究原型和生产工件之间的转化努力降到最低。因此，我们通常选择了大数据工具支持的 Python 生态系统，这些工具运行在成熟的云平台上。在这种情况下，排除无代码解决方案是非常自然的。

为了用更广泛的视角和不同的用户群体理解现状，我开始联系我现有网络之外的人，以了解他们技术栈的变化以及无代码工具的采用情况。总体来说，在接触了相当多样化的受众后，我有了一些学习体会，这些体会最终改变了我的看法。

# 从新的视角看待问题

首先，我重新审视了组织在科学实践中的结构。尽管机器学习领域已经成熟，但在很多组织中仍然很少有科学职能。大多数组织在起步时都很艰难，通常团队人数也很不足。尽管这些组织中的科学问题潜力可能很大，但从一开始就很难明确大方向。从机器学习问题中发现价值并实现其业务影响的过程是缓慢且迭代的，需要有面对重大失败的心理准备。没有一种完美的科学路径能够帮助人们从识别问题到生成业务价值，像过于简化的从 A 点到 B 点的过程一样。这个过程通常是艰难且迭代的。这让我思考——不同成熟度的组织采用了什么工具？

事实上，并非所有组织都能负担得起或愿意从一开始就大规模投资于昂贵的科学技能。这个过程通常是一个未定义的路径。下图展示了从问题发现到解决以产品为驱动的科学解决方案的简化路径。*[当然，每一步都有其自己的迭代，但你可以看到更大的图景。]*

![](img/ef9c22a61c918fb535858c35c1ee24e5.png)

[作者提供的图像] - 机器学习使用案例的产品化路径示意图。

灰色区域表示给定里程碑的迭代频率。自然地，我们会有大量的想法在实现基本原型之前被淘汰，而这些原型在正式投入到严肃的原型之前还会进一步修剪，最后收窄为用于最终产品的关键优化版本。

长时间以来，我一直从不同的视角看待这些产品，并且不合理地批评了无代码平台的价值。我关键的问题是——*这个解决方案对严肃的业务有多大价值？* 在某些地方，这看起来对重要的用例来说有些多余。但后来我意识到，我是在从一个不缺乏机器学习技能和工程资源的工作环境的角度来进行比较。然而，并非所有地方都是这样。大多数组织没有足够的资源和团队来大规模支持科学用例验证，也可能没有成熟的科学职能来支持这一点。

下图展示了无代码平台在业务问题生命周期中的有效性的思考过程。

![](img/f02b647b8c841b7c3de2430b6703c753.png)

[作者提供的图像] — 无代码工具在问题生命周期阶段的有效性示意图

我的偏见是由于对问题更成熟阶段的倾斜。然而，这只是一个具体而狭窄的视角。每个组织根据其科学成熟度的位置，将拥有不同的工具。如果我们对大多数组织的解决问题过程进行概括，我们需要理解并非所有的想法都会变成生产产品。想法、原型、MVP 和最终产品的比例看起来像是多米诺骨牌倒退的顺序。因此，需要用不同的工具以不同的方式支持问题的每个生命周期阶段。下表将更深入地探讨上述问题生命周期阶段。

![](img/19d98db51ae0f248d748348fa7e4813c.png)

[图片来自作者]

如上所示，如果我们将问题的生命周期拆解成更小的里程碑，我们可以看到不同阶段对技能和资源的不同需求。专门的科学团队绝不是节省资源的团队，他们的成本通常与工程团队相当或更高。因此，小型组织中通常没有太多这样的团队。那么，那些可能没有专门科学团队的人员如何在不进行重大妥协的情况下更快地完成这个过程呢？

这时我开始看到无代码平台的新价值。

# 重新考虑无代码平台

在解决方案旅程中使用一刀切的解决方案是否合理？当然不！随着问题的进展会发生什么变化？在理想的世界中，为了使数据科学和机器学习变得普及，确实需要一个生态系统来促进在迭代频率非常高且失败率高的领域中更快地移动。为了支持创意阶段，我们已经拥有了最好的工具，比如白板、PPT、文档、写作等。对于基础和严肃的原型，我们是否有能够更快推进的工具？有人认为 Python 已经被充分普及，可以促进这一过程。这可能只是部分正确；不是所有分析师都精通 Python 和 SQL。因此，存在可以填补这一空白的东西。

这就是为什么我强烈认为无代码解决方案可以蓬勃发展的原因。

# 无代码解决方案提供了什么？

本质上，无代码机器学习平台显著降低了普通人接受数据科学的门槛。通过用模块化构建块整洁地抽象出关键复杂科学组件，这些平台支持从构思到实验和验证的过程，并留有额外的自定义空间。这些工具提供了稳健的默认设置，确保大多数任务可以在用户几乎不需自定义输入的情况下顺利进行。因此，这些工具通过简化数据工程和模型构建任务的过程，加快了验证创意的进程。此外，这些工具还简化了结果（成果）的消费过程，并支持更广泛的 go/no-go 决策，适用于大规模实验。对于首次接触机器学习的小型组织或新团队，这些工具以实惠且有效的价格点提供了巨大的价值，可以自信地加快初步步伐。

# 无代码机器学习平台不提供什么？

无代码工具绝不是大型严肃解决方案的替代品。它不是一个永久的工具集，无法应对从原型到生产的整个过程。当业务问题经过充分验证并开始扩展时，无代码工具的价值将开始减小，提示需要更精细的控制。无代码工具缺乏使大规模生产问题运行的复杂性。

# 那么它适合什么场景呢？

机器学习和数据科学用例的迭代和实验性特征确实使其成为一个资源密集型的倡议。不断增长的科技企业和/或最近刚刚采用机器学习进行业务的企业需要时间来验证创意，然后再做进一步投入。我们目前拥有的工具集可能并不是新团队开始数据科学工作的最友好和易于上手的手段。虽然它肯定是一个稳健的工具，但对于初学者来说可能不太理想。这正是民主化 AI/ML 工具开始发挥关键作用的地方。一个组织能否以仅一名员工和没有前期成本的低数据科学投资开始新的旅程？创意能否在没有严重工程努力和有限科学成熟度的情况下得到验证？一个有前途的创意能否逐步扩展，直到团队对大规模投资有信心？所有这些问题的明确答案并不总是容易获得，尤其是在现有的 Python 机器学习宇宙中；需要有更多的工具来提供支持。对于那些需要快速验证并有效地迭代到成熟的规模问题，无代码机器学习解决方案恰到好处。

当我们民主化 AI 和 ML 工具时，我们开始为生态系统提供合适的工具，以像抚养新生儿直到上幼儿园一样培养创意。一旦进入幼儿园，或许是时候寻求更好的工具了。但在此之前，无代码平台是你最好的朋友。

# **总结思考**

一般来说，高质量的生产材料不建议通过过于简化的工具进行交付。但科学用例的迭代和实验性特征使得从一开始就进行资源密集型工程并不合适。问题的不同阶段以及组织的科学成熟度需要不同的工具来导航科学之旅。无代码/低代码解决方案提供了一个很好的起点，并有效降低了组织探索该领域是否对其业务有价值的门槛。当组织认真起来时，才有可能需要迁移到提供更多细化控制的工具和服务。在那之前，无代码工具将是您团队探索的好伙伴。

*你好，感谢阅读！如果你想获取我即将发布的博客更新，请在* [*Twitter*](https://twitter.com/jojo62000) *上关注我，以便第一时间收到新帖通知。再次感谢！*
