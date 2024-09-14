# 为什么企业 AI 中的信任与安全（相对来说）很简单

> 原文：[`towardsdatascience.com/why-trust-and-safety-in-enterprise-ai-is-relatively-easy-32de240788d0`](https://towardsdatascience.com/why-trust-and-safety-in-enterprise-ai-is-relatively-easy-32de240788d0)

## 数据驱动的领导力与职业生涯

## 为什么传统 AI 比生成式 AI 具有更高的可靠性优势

[](https://kozyrkov.medium.com/?source=post_page-----32de240788d0--------------------------------)![Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----32de240788d0--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32de240788d0--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----32de240788d0--------------------------------) [Cassie Kozyrkov](https://kozyrkov.medium.com/?source=post_page-----32de240788d0--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32de240788d0--------------------------------) ·阅读时间 5 分钟·2023 年 5 月 31 日

--

在[本系列的第一部分](https://bit.ly/quaesita_rawmat1)中，我说了一句我认为自己永远不会说的话：当我们处理典型的企业规模 AI 系统时，信任和安全是容易的。

什么？！亵渎！

听我说。

是的，确实，[这实际上很难](http://bit.ly/mfml_part3)。但这种难度与新一波[生成式 AI](http://bit.ly/quaesita_uxrevolution)带来的信任与安全难题相比显得微不足道。原因如下。

![](img/3b5bd26b1d7aac816b3a847fca189110.png)

所有图片版权归作者所有。

想象一下，你是一个没有基于 AI 的[票价系统](https://d3.harvard.edu/platform-rctom/submission/machine-learning-and-ai-at-delta-air-lines/)的航空公司首席执行官。你的幕僚长气喘吁吁地跑进你的办公室，告诉你组织中的一些数据专家团队距离全面推出 AI 定价系统只有几个小时，但他们被听到说，*“我不知道这个 AI 系统的好坏。不知道它能赚多少或亏多少……但它看起来有用，所以我们就启动吧。”*

头会滚。这样的系统影响力和潜在商业影响过于巨大，无法容忍这种程度的草率。你很可能会解雇所有与这个完全失控的情况有关的人员，你这样做是对的。毕竟，作为首席执行官，航空公司成功的最终责任落在你身上，鉴于他们几乎把你的企业置于不适当的风险之中，清除这群小丑将是显而易见的。整个情况简直是犯罪般的愚蠢。你的公司没有他们会更好。

> 不管你怎么说大型组织，但它们有一件事做得不错，那就是避免一切轻率地动摇现状的事情。

通常，这样的问题在到达 CEO 的桌面之前就已经被解决了。不管你对大型组织有何评价，但它们通常擅长避免任何轻率的风险。它们内在地倾向于谨慎而非冒险，这就是为什么企业级人工智能系统通常只有在(1) *可以证明*解决一个***具体***问题非常有效，或(2) 具有*可以证明*的低潜在危害（因为风险低，错误不会非常尴尬/痛苦，或因为应用的重要性较低）时，才会被推出。

> 人工智能系统存在的直接性是一个极其强大的简化工具。

*从航空公司的角度来看：*

(1) 一个人工智能定价系统，该系统经过仔细推出、逐步扩展，并经过统计测试，确保至少能带来*x%*的正面收入影响。

(2) 一个人工智能发音系统，使登机口工作人员可以听到关于如何宣布乘客姓名的数据驱动的最佳猜测，以帮助工作人员在不确定发音时使用。这样的系统几乎不具有关键任务性质，并且它的好处在于能够向世界展示你在做人工智能，而风险很小。此外，当训练有素的人员审批所有输出时，系统的无害性更容易实现，因此你会希望你的登机口工作人员使用他们的判断。

![](img/d692a2d1caced9e9901d3d3de980a5cf.png)

“你想让我怎么发音？”（当人们需要发音我的姓氏 Kozyrkov 时，我常常会收到这种困惑的表情。我告诉他们只需说“咖啡壶”，没有人会注意到。）所有图片版权属于作者。

要点是，除非信任和安全问题已经因应用本身的特性而被最小化，否则企业级人工智能系统在没有*证明*其潜在收益值得冒险的情况下是不可能获得实际应用的……而在系统的价值不明确时，这种*证明*是无法获得的。

为什么这使事情变得简单？因为这意味着每个关键任务的传统企业级人工智能系统（第(1)类）往往具有：

+   一个相对直接的用例声明

+   对系统期望的“[良好行为](http://bit.ly/mfml_039)”的愿景

+   一个明确的、单一的目标

+   可衡量的性能

+   明确定义的[测试标准](http://bit.ly/mfml_047)

+   对可能出现的问题及因此需要的[安全网](https://bit.ly/quaesita_policy)有相对清晰的认识

这里也有很多挑战，比如如何确保这样的系统与所有现有的企业系统兼容（[请参见我的 YouTube 课程及更多内容](http://bit.ly/mfml_part3)），但系统存在的直接性是一个极其强大的简化工具。

这里的关键见解是，企业级解决方案的经济学往往倾向于规模。旨在大规模部署的系统通常有一个明确的目的，否则聪明的领导者会直接把它们送到垃圾压缩机。这就是为什么过去十年中大多数企业级 AI 系统都是为了在规模上做一件非常具体的事情而设计的。

> 过去十年中大多数企业级 AI 系统都是为了在规模上做一件非常具体的事情而设计的。

这对信任和安全来说是一个巨大的优势。非常大！当然，确保你的系统在面对“[长尾](http://bit.ly/mfml_086)”（即异常用户）时能保护用户仍然需要大量的通用可靠性工作，但保护一个多样化的用户群体免受单一目的、单一功能系统的影响，比保护相同的用户群体免受*多*功能系统的影响要容易得多。从大多数企业的角度来看，生成式 AI 系统从根本上是多功能和多用途的。

这就是关键的见解，所以让我们重复一下：

> 保护一个多样化的用户群体免受单一目的系统的影响，比保护相同的用户群体免受多功能系统的影响要容易得多。

如果你想更好地理解这个见解，请继续阅读这个系列的[第三部分](https://bit.ly/quaesita_rawmat3)。

另一方面，如果你觉得最后一个见解对你来说显而易见，那么可以跳过第三部分，直接阅读[第四部分](https://bit.ly/quaesita_rawmat4)，我将解释为什么生成式 AI 不具备这些简化特征以及这对 AI 监管意味着什么。

# 感谢阅读！怎么样，来一门课程？

如果你在这里玩得开心，并且你在寻找一个既能让 AI 初学者和专家都感到愉快的、有趣的领导力导向课程，[这是我为你准备的一点小东西](https://bit.ly/funaicourse)：

![](img/300b5280620ea948fc3dbffb708084d4.png)

课程链接：[`bit.ly/funaicourse`](https://bit.ly/funaicourse)

[](https://kozyrkov.medium.com/membership?source=post_page-----32de240788d0--------------------------------) [## 加入 Medium

### 阅读 Cassie Kozyrkov 的每一篇故事（以及 Medium 上的成千上万其他作家的文章）。你的会员费将直接支持……

kozyrkov.medium.com](https://kozyrkov.medium.com/membership?source=post_page-----32de240788d0--------------------------------)

*P.S. 你是否尝试过在 Medium 上多次点击鼓掌按钮看看会发生什么？* ❤️

# 喜欢作者？与 Cassie Kozyrkov 联系

让我们成为朋友吧！你可以在[Twitter](https://twitter.com/quaesita)、[YouTube](https://www.youtube.com/channel/UCbOX--VOebPe-MMRkatFRxw)、[Substack](http://decision.substack.com)和[LinkedIn](https://www.linkedin.com/in/kozyrkov/)找到我。有兴趣让我在你的活动上发言吗？请使用[这个表单](http://bit.ly/makecassietalk)与我联系。
