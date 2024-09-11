# Building Better ML Systems — Chapter 1: Every Project Must Start with a Plan

> 原文：[https://towardsdatascience.com/building-better-ml-systems-chapter-1-every-project-must-start-with-a-plan-907a36774a32?source=collection_archive---------1-----------------------#2023-04-20](https://towardsdatascience.com/building-better-ml-systems-chapter-1-every-project-must-start-with-a-plan-907a36774a32?source=collection_archive---------1-----------------------#2023-04-20)

## *关于机器学习项目生命周期、设计文档、商业价值和需求。关于从小处开始和快速失败。*

[](https://olga-chernytska.medium.com/?source=post_page-----907a36774a32--------------------------------)[![Olga Chernytska](../Images/3a1a1b5f3c92d3b86283911cd90a9259.png)](https://olga-chernytska.medium.com/?source=post_page-----907a36774a32--------------------------------)[](https://towardsdatascience.com/?source=post_page-----907a36774a32--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----907a36774a32--------------------------------) [Olga Chernytska](https://olga-chernytska.medium.com/?source=post_page-----907a36774a32--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fcc932e019245&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-better-ml-systems-chapter-1-every-project-must-start-with-a-plan-907a36774a32&user=Olga+Chernytska&userId=cc932e019245&source=post_page-cc932e019245----907a36774a32---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----907a36774a32--------------------------------) ·9 min read·Apr 20, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F907a36774a32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-better-ml-systems-chapter-1-every-project-must-start-with-a-plan-907a36774a32&user=Olga+Chernytska&userId=cc932e019245&source=-----907a36774a32---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F907a36774a32&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fbuilding-better-ml-systems-chapter-1-every-project-must-start-with-a-plan-907a36774a32&source=-----907a36774a32---------------------bookmark_footer-----------)![](../Images/cf1720a8905e6822c85bdf2a687103d4.png)

图片由 [charlesdeluvio](https://unsplash.com/@charlesdeluvio?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

很多数据科学家和机器学习工程师在大学毕业后，对他们的日常工作有一种错误的认知——他们期望其工作与他们的学习相似：

*尝试最新的前沿算法在固定的相对干净的数据集上，并选择在准确性方面表现最好的算法（期望）。*

你不需要：

+   思考商业价值和永无止境的需求列表。

+   (最有可能) 收集、标记和清理数据集。在某些情况下，训练/验证/测试划分已经为你完成了。

+   彻底评估你的模型，检查偏见，并进行A/B测试。

+   将模型部署到成千上万（或百万）用户，并确保它99.9%的时间保持运行。

+   监控模型，捕捉任何准确率下降，并在需要时重新训练模型。

+   在部署上一个版本后立即收集新数据，并开始着手开发一个新的、更好的模型。

是的，你不需要在研究/学习项目期间考虑这些。但在实际项目中，这变得至关重要。

研究和实际项目之间的主要区别是：

*在现实生活中，许多用户以各种可想象和不可想象的方式使用你的模型，并期望它始终快速、准确且公平地工作，没有偏见。用户的行为不断变化，可能会发生疫情和战争，而你的公司则试图通过提供用户想要的东西来赚取利润，并通过以其他人从未尝试或成功过的方式应用机器学习来建立竞争优势（现实）。*

在本系列中，你将了解到，构建更好的机器学习系统需要将其视为一个系统——对每个组件及其关系给予足够的关注。

本教程将对数据科学家、机器学习工程师、团队和技术负责人（或那些希望成为技术负责人的人）有所帮助。虽然本系列不会全面覆盖所有内容，但它将帮助你打下机器学习系统设计的坚实基础，弥补任何不足，并让你探索不太熟悉的话题。在过程中，我会提供许多优秀文章、论文和书籍的链接。

事不宜迟，让我们开始吧！

**每个项目都必须以计划开始。**

下面是机器学习项目生命周期。让自己放松。首先，你需要理解任务并确定需要做什么。接着，你收集、标记和清理数据。然后，你进入建模阶段。之后，你评估模型并选择最佳模型。最后，你部署模型并监控其性能。

![](../Images/6029065023464ad4b652daae10789640.png)

*图1\. 机器学习项目生命周期。图源作者。*

这就是结局吗？不，这仅仅是开始。

在监控过程中，你可能会发现模型在某些用户子集上的表现不好，或者准确率随着时间的推移而下降，因此你重新开始：理解问题 -> 获取数据 -> 建模和评估 -> 部署。

或者在模型评估过程中，你可能会发现模型还不够好，无法部署，因此你将重新开始：了解哪些地方不工作以及如何改进 -> 收集更多数据 -> 进行更多建模 -> 评估（希望）这次获得更好的结果。

（如果这是你第一次了解机器学习项目生命周期，我建议你查看Anton Morgunov的文章：[机器学习项目的生命周期：有哪些阶段？](https://neptune.ai/blog/life-cycle-of-a-machine-learning-project)）

因此，有两件重要的事情需要理解：

1.  **构建机器学习系统是一个迭代过程，直到模型从生产中移除为止，这一过程将永无止境。**（无休止的工作）

1.  图 1 提供了机器学习系统开发的简化版本，但实际上，你并不会顺利地从一个阶段过渡到另一个阶段。在每个阶段，可能会出现问题（通常会出现），这可能会让你退回一个或多个步骤，甚至让你重新开始。（欢迎来到现实世界）

![](../Images/b1bdb2551b35fd9c49b891fc9477fe33.png)

*图 2\.* ***现实*** *机器学习项目的生命周期。图像由作者提供。*

那些有工程背景的人可能会想：机器学习项目和传统软件开发有什么区别？测试、构建和发布在哪里？谢谢你的提问。

事实上，机器学习项目是软件工程项目的一个子集。**所以你在软件工程中想到的所有最佳实践在机器学习项目中都非常受欢迎**。话虽如此，让我向你介绍一个真正现实的软件项目生命周期，其中包含机器学习组件：

![](../Images/bb0faf3c993fdeab6ffb7d605dfd63ce.png)

*图 3\. 真实* ***现实*** *的带有机器学习组件的软件项目生命周期。图像由作者提供。*

**为了控制这种混乱，每个项目都必须从计划开始。**

（阅读 [MLOps: Machine Learning Life Cycle](https://www.ml4devs.com/articles/mlops-machine-learning-life-cycle/) 由Satish Chandra Gupta撰写，以了解更多关于机器学习软件开发生命周期的内容。）

在花费数千美元进行数据标注和数周进行机器学习模型开发之前，你需要做四件事。我们称之为“编程前”阶段。因此，现在关闭你的PyCharm吧，你只需要一个Google文档、大脑和Zoom。

## 1\. **估算机器学习项目的商业价值**。

任何商业公司的目标都是赚更多的钱或提供更好的客户体验……为了赚更多的钱。牢记这个简单的原理，向你的老板、高层管理人员和利益相关者证明当前的机器学习项目是一个值得投资的项目。

理想情况下，您需要提供一些关于机器学习模型如何增加公司收入、用户参与度或减少请求处理时间等方面的大致数据。在这里可以发挥创造力，关闭完美主义，并且不要犹豫向财务和市场部门的同事寻求帮助。

（请记住，后续这些指标将用于评估项目，因此在承诺交付内容时务必要实际可行。）

## **2\. 收集需求。**

一旦没有人对机器学习模型的必要性表示怀疑，就开始收集需求。

每个领域都是特定的，每个项目都是独特的，因此没有一份详尽的需求清单可供参考。因此，请相信您的经验，并与同事合作。

这里有一个有用的提示：列出一些通用问题（我将在下面分享我的），然后直接提问。开始对话，随着讨论的进行，更多与项目相关的问题自然会浮现。

+   我们有多少数据？我们将如何标记它？

+   模型的延迟应该是多少？

+   模型将部署在云端还是本地？实例规格是什么？

+   是否有关于数据隐私和模型可解释性的要求？

**如果一个任务可以通过机器学习解决，这并不意味着一定要这样做。** 在这一点上，我建议您重新考虑是否采用纯软件工程方法或基本的规则驱动方法可能更适合。以下是可以帮助您做出决策的一些文章：

- [*何时使用机器学习*](https://docs.aws.amazon.com/machine-learning/latest/dg/when-to-use-machine-learning.html)，亚马逊

- [*四种情况下不适合使用机器学习*](https://qymatix.de/en/situations-without-machine-learning/)，作者Svenja Szillat

没有数据就没有机器学习。这似乎是显而易见的，但不幸的是，在我的职业生涯中，我见过太多公司犯同样的错误：他们想要人工智能，但他们的数据集很小，缺少重要的特征或者数据不干净。Monica Rogati 在文章“[AI 需求层次](https://medium.com/hackernoon/the-ai-hierarchy-of-needs-18f111fcc007)”中提到，AI应被视为需求金字塔的顶端，而数据收集、存储和清理则是其基础。

![](../Images/01bbf3e22453e5626bca686e095fe8b4.png)

*图 4\. AI 需求层次。修改自* [*Monica Rogati 在“AI 需求层次”中的图像*](https://medium.com/hackernoon/the-ai-hierarchy-of-needs-18f111fcc007)*。*

## **3\. 从小开始，快速失败。**

即使您的目标是创建一个每天为数百万用户提供服务的机器学习系统，从一个小得多的项目开始也是明智的选择：

+   PoC（概念验证）。从数据存储中手动检索数据，在Jupyter Notebook中快速迭代几种算法，最后，证明（或拒绝）假设：您可以在您拥有的数据上训练一个具有令人满意准确度的机器学习模型。在PoC阶段，您还将了解到部署和扩展模型所需的内容。

+   MVP（最小可行产品）。假设 PoC 阶段成功，现在你正在创建一个仅包含主要功能并发布给用户的产品。在机器学习项目中，这意味着**将模型推出给一部分用户并评估它是否带来了预期的商业价值**。

一旦你意识到一个想法行不通——可以心无旁骛地放弃它并转向下一个。这在你还没有花费多年工作或数十万美元时要容易得多。将失败的成本保持在较低水平是项目成功的关键因素。

（要深入了解这个话题，请阅读 [POC 与 MVP：选择构建优秀产品](https://www.codica.com/blog/poc-vs-mvp/) 作者：Dmitry Chekalin。）

## **4. 编写设计文档。**

软件工程中的设计文档是对软件系统架构的描述——其整体结构、各个组件及其相互之间的互动。它可以采取任意形式和结构，可以是正式的或非正式的，高级别的或详细的（这由团队决定）。在软件开发的实施阶段，设计文档作为开发人员遵循的蓝图。

这是软件工程中的最佳实践，正如我之前提到的，所有的软件工程最佳实践在 ML 项目中都受到高度欢迎。

我个人喜爱设计文档的原因如下：

+   **编写触发思考过程。** 编写设计文档就像在高层次上实现项目——你实际上不编写代码，但仍然对数据、算法和基础设施做出决策。你考虑所有场景并评估权衡，这意味着你将通过避免死胡同在未来节省时间和金钱。

+   **设计文档简化了团队内部的同步和协作。** 文档在团队成员之间共享，以便他们可以审阅，熟悉系统设计，并在需要时发起讨论。没有人被忽视，每个人都被鼓励参与。

如果你准备开始编写设计文档，以下是 [Eugene Yan 提出的机器学习系统模板](https://github.com/eugeneyan/ml-design-docs)。可以随意修改并根据项目需求进行调整。

如果你想了解更多关于设计文档的概念，可以查看这些文章：

- [如何为机器学习系统编写设计文档](https://eugeneyan.com/writing/ml-design-docs/) 作者：Eugene Yan

- [Google 的设计文档](https://www.industrialempathy.com/posts/design-docs-at-google/) 作者：Malte Ubl

## **结论**

在这一章中，我们了解到每个项目都必须以计划开始，因为机器学习系统过于复杂，无法以临时的方式实施。我们回顾了机器学习项目的生命周期，讨论了为什么以及如何估算项目的商业价值，如何收集需求，然后冷静地重新评估是否真的需要机器学习。我们学习了如何通过“PoC”和“MVP”等概念从小处开始并快速失败。最后，我们谈到了在规划阶段设计文档的重要性。

在接下来的文章中，你将了解数据收集和标注、模型开发、实验跟踪、在线和离线评估、部署、监控、再训练以及更多内容——所有这些将帮助你构建更好的机器学习系统。

下一章已经发布：

[## 构建更好的机器学习系统。第2章：驯服数据混乱](https://towardsdatascience.com/building-better-ml-systems-chapter-2-taming-data-chaos-841d5a04b39?source=post_page-----907a36774a32--------------------------------)

### 关于数据中心AI、训练数据、数据标注和清洗、合成数据以及一点数据工程和…

[towardsdatascience.com](/building-better-ml-systems-chapter-2-taming-data-chaos-841d5a04b39?source=post_page-----907a36774a32--------------------------------)
