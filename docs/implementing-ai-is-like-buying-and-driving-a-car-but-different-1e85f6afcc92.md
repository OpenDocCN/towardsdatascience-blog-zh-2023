# 实施人工智能就像买车和开车（但有所不同）

> 原文：[https://towardsdatascience.com/implementing-ai-is-like-buying-and-driving-a-car-but-different-1e85f6afcc92?source=collection_archive---------16-----------------------#2023-01-18](https://towardsdatascience.com/implementing-ai-is-like-buying-and-driving-a-car-but-different-1e85f6afcc92?source=collection_archive---------16-----------------------#2023-01-18)

## 一个帮助解释常见误区和陷阱给管理层的类比

[](https://medium.com/@kpeters_?source=post_page-----1e85f6afcc92--------------------------------)[![Koen Peters](../Images/1202fcfbec150807f1d46a4b9bad450a.png)](https://medium.com/@kpeters_?source=post_page-----1e85f6afcc92--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e85f6afcc92--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----1e85f6afcc92--------------------------------) [Koen Peters](https://medium.com/@kpeters_?source=post_page-----1e85f6afcc92--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F769bd197e7cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-ai-is-like-buying-and-driving-a-car-but-different-1e85f6afcc92&user=Koen+Peters&userId=769bd197e7cf&source=post_page-769bd197e7cf----1e85f6afcc92---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e85f6afcc92--------------------------------) ·8分钟阅读·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1e85f6afcc92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-ai-is-like-buying-and-driving-a-car-but-different-1e85f6afcc92&user=Koen+Peters&userId=769bd197e7cf&source=-----1e85f6afcc92---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1e85f6afcc92&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimplementing-ai-is-like-buying-and-driving-a-car-but-different-1e85f6afcc92&source=-----1e85f6afcc92---------------------bookmark_footer-----------)![](../Images/e1e7b4dc4dbe89dec747cae2e54d9295.png)

实施人工智能的常见误区和陷阱 — 作者：Babette Huisman

到现在，大多数公司已经开始涉足AI。然而，只有少数的举措似乎最终成功实施。根据Gartner的最新AI调查，54%的AI项目进入生产阶段[1]。根据我作为数据科学顾问的个人经验，进入生产阶段的项目要少得多。鉴于Gartner报告还提到40%的受调查公司部署了成千上万的模型，这似乎并没有很好地代表那些刚刚开始涉足AI的公司。这些公司在整个组织中对AI的知识不足。决策者是其中的一层。这种知识的缺乏导致了不良决策、解决方案未能达到预期以及对所需努力的低估。为了帮助决策者提出正确的问题，并最终做出良好的、知情的决策，我提出了一个类比，解释了实施AI的一些复杂性、误区和陷阱。通过在这里分享这个类比，我希望也能帮助其他人。让我们深入探讨一下吧！

> 实施AI就像买车和驾驶汽车（但有所不同）

## 陷阱：出色的销售推介

不论销售推介多么出色或汽车看起来多么华丽，你都应该事先检查几个方面：

+   你是否检查了汽车是否适合你的车道或车库？类似地，你也应该检查AI是否适合你要解决的问题。尽管这看起来是显而易见的，但利益相关者往往会为了使用特定技术而推动解决方案。是的，AI是过去十年的热门词汇，那些首先掌握其实施的人期望获得巨额收益。但如果你的解决方案不适合你的问题，这种期望将永远无法实现。

提示：在开始使用AI之前，你应该先尝试一些更简单的方法。比如增加更多的业务逻辑，更改或自动化部分工作流程等。

+   汽车在不同地形上的表现如何？它能否在草地、土路或越野路上行驶？类似地，你要了解你的AI有多么健壮和公平。它在数据的不同分布中表现是否一样？例如，不同的年龄组、特定类别如性别等。尽管你可能期望AI在各方面表现良好，但最近的历史告诉我们，这不一定总是如此[2]。这可能对你的业务和受AI结果影响的人造成严重后果。确定你的模型将用于什么，因此了解其要求是非常重要的。

+   汽车的马力能告诉你*一些*关于它行驶速度的信息。然而，没有足够的扭矩，你的车将无法加速并达到速度。对于AI来说，准确率能告诉你*一些*关于其性能的信息。然而，当100个案例中有1个代表欺诈或病人时，AI可能“预测”没有人是欺诈/病人，准确率达到99%。这样的模型实际上是无用的。因此，查看其他性能指标也是很重要的，以便对你的AI性能有一个坚实的理解。例如，“召回率”表示正确识别的实际欺诈/病人的百分比[3]。在这种情况下，召回率将为零，表明AI模型的表现不如预期。

提示：数据科学家受过训练，能够解释这些指标，并应能够为你提供有关AI性能的建议。

![](../Images/c8f42cb0a443c67f748033110a211a0d.png)

100个中需要被识别的一个 — 作者：巴贝特·赫伊斯曼

总结一下，如果你听说了一个很棒的解决方案，深入了解一下，看看它是否真的能解决你的问题。

## 陷阱：成本与收益

无论AI解决方案多么出色，如果没有对你尝试改进的过程的基线测量，你无法判断收益是否超过成本。

![](../Images/d5d3090ae7bf36053f623f5996748910.png)

车速表遇见KPI仪表 — 作者：巴贝特·赫伊斯曼

+   在你买车之前，你会想检查里程表（以公里/英里计算的行驶距离）和导航系统。你可能想跟踪你在特定时间段内行驶了多少公里，并且是否接近你的目标或目的地。如果你驾驶的次数非常少，那么拥有一辆车的成本可能不会超过其收益。项目失败往往是因为无法量化额外的收益与成本，使决策者只能猜测，有时甚至会放弃本来很好的项目。因此，在实施AI之前，你需要对过程有一个良好的理解，了解该过程当前的表现，并设定一个改进过程的目标。你将收集数据，定义、计算并跟踪过程KPI，以便在实施AI解决方案后，你可以监控过程的改进程度或预计达到某个目标的时间。这些指标反过来可以让你计算投资是否值得，并让决策者对（继续或停止）项目做出明智的决策。

能够沟通结果、成本和效益对于做出项目决策至关重要。

## 神话：AI是“设定即忘”

对于不熟悉AI的人来说，一个常见的误解是，一旦你实施了解决方案，你就完成了，它可以正常工作，不需要偶尔检查一下。

![](../Images/06882ffa0d2a82e12d82a7f448632739.png)

警报：当你的AI表现不如预期时 — 作者：巴贝特·赫伊斯曼

+   就像汽车一样，AI 也需要维护。然而，与汽车不同的是，AI 通常没有预装的仪表盘来显示警告灯，也不会发出奇怪的噪音。如果你希望获得通知（而且你应该这样做），你的数据团队需要建立自己的仪表盘，并在 AI 显示性能下降的迹象时进行监控或发出警报。你甚至可以实施类似于“定期车辆检查”的措施，让你的 AI 通过检查（或需要进行某些调整以便通过检查），以确保它能够持续在生产中运行。此外，AI 是一款软件，就像其他任何软件产品一样，你可能需要在有新功能和安全措施时进行更新。

让 AI 在生产环境中成功运行需要你数据团队的持续努力。

## 神话：AI 是即插即用的。

不幸的是，你不能只是获取一个 AI，启动它并期望它能正常工作。

![](../Images/76e59eb7672028caf56ce4911dec3576.png)

gaspump 与你自己的数据管道 — 作者：Babette Huisman

+   使用汽车时，你可以开车去加油站，加注正确类型的汽油，然后就可以出发了。（或者对于电动车，你可以直接插上电源）。AI 解决方案也需要燃料，即数据。然而，与汽车不同的是，公开可用的“加油站”并不多，你不能从中获得正确处理的数据。你必须自己收集/挖掘数据，然后建立自己的数据精炼工厂（ETL 过程、数据管道等）。获得可用数据所需的努力往往被低估了。大型公司有专门的团队专注于获取数据、清理数据、提高数据质量（最好是在数据收集阶段）并确保数据代表现实。此外，更重要的是，数据随着时间的推移而变化，你需要不断监控你的数据管道，并调整数据处理或 AI 引擎，以确保它持续接收可以操作的“燃料”。

获取正确类型和质量的数据也需要你数据团队的持续努力。

## 陷阱和神话：AI 替代人或使人变得冗余。

另一个常见的误解，如果处理不当，会对公司产生负面长期影响。

+   今天的汽车使驾驶变得更加轻松，但豪华车的车道辅助或自动驾驶功能并不意味着驾驶员不再需要掌握方向盘！（至少现在是这样）。同样，拥有AI解决方案并不意味着你不再需要员工。你应该在可能的情况下以不同的方式使用这些员工。他们的工作的一部分可以包括为公司增加更高价值的活动，例如制定未来目标、投资客户关系或解决AI无法处理的复杂问题。另一部分工作应包括检查AI完成的随机样本。这被称为“人类参与”（‘human in the loop’）[4]，不仅为AI提供了宝贵的反馈，从而使其更准确，也为员工提供了额外的“安全”层，能够发现监控仪表板可能遗漏的异常行为。此外，如果员工能够监督他们的AI同事并通过纠正其行为来不断改进AI，将增加信任和采用度。在这里，观察者间的可靠性是实现最佳结果的关键。

![](../Images/34f07ae776a2c99858aaa3399823faa0.png)

人类参与 — 作者：Babette Huisman

+   就像你仍然需要坐在方向盘后面一样，你还需要知道如何驾驶。如果因为某种原因你不能使用AI，并且公司里没有人能手动完成工作，这对公司将是有害的。你希望这些业务流程的专业知识留在公司里，最好的办法是让员工“监督”AI或解决上述更复杂的案例。此外，每当你的监控仪表板突出显示一个问题时，公司内拥有专家知识来确定和实施所需的修复是保持AI在生产中运行的关键。

一般来说，你不希望失去公司内的专家知识，因为这些知识可以帮助指导公司的活动。

考虑到这里描述的要点，你离成功实施AI并真正让其加速业务又近了一步。

![](../Images/03cae6c9b60304c8bf26cf1750b1ecd9.png)

终于可以加速的汽车 — 作者：Babette Huisman

我相信还有更多的汽车与AI的类比，如果你想到一个好的，请在下面的评论中告诉我！此外，我想感谢Babette Huisman提供的精彩插图（这些插图在本文中经她许可使用）以及Maikel Grobbe的建设性批评。他们都帮助提升了这篇文章的水平。

参考资料：

1.  [Gartner AI调查2022](https://www.gartner.com/en/newsroom/press-releases/2022-08-22-gartner-survey-reveals-80-percent-of-executives-think-automation-can-be-applied-to-any-business-decision)

1.  [有偏见或不稳定的AI](https://www.fiddler.ai/blog/the-never-ending-issues-around-ai-and-bias-whos-to-blame-when-ai-goes-wrong)

1.  [精确度和召回率](https://developers.google.com/machine-learning/crash-course/classification/precision-and-recall)

1.  [环中人](https://www.unite.ai/what-is-human-in-the-loop-hitl/)
