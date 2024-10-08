# 书评：《设计机器学习系统》

> 原文：[`towardsdatascience.com/book-review-designing-machine-learning-systems-48d3c23d155`](https://towardsdatascience.com/book-review-designing-machine-learning-systems-48d3c23d155)

## 一本关于**Chip Huyen**所著《设计机器学习系统》的书评与总结

[](https://thedatageneralist.medium.com/?source=post_page-----48d3c23d155--------------------------------)![Steven Finkelstein](https://thedatageneralist.medium.com/?source=post_page-----48d3c23d155--------------------------------)[](https://towardsdatascience.com/?source=post_page-----48d3c23d155--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----48d3c23d155--------------------------------) [Steven Finkelstein](https://thedatageneralist.medium.com/?source=post_page-----48d3c23d155--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----48d3c23d155--------------------------------) ·阅读时间 5 分钟·2023 年 2 月 20 日

--

![](img/a2eadf72cdcfcf0c4cb26bc877f2ad0b.png)

来源：照片由[Thought Catalog](https://unsplash.com/@thoughtcatalog?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/tech-book?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

数据科学专业人士成长的重要部分是从数据准备和模型训练发展到掌握整个机器学习开发周期。完整的周期包括数据摄取、数据清洗和准备、特征工程、模型训练、模型评估、模型部署、模型服务和模型维护。这种更广泛的所有权模型可能要求数据科学专业人士提高他们对整个开发周期的概念理解和应用理解。也许在这一学习旅程中最重要的时刻是理解模型训练通常是过程中的最简单步骤。[Chip Huyen 所著《设计机器学习系统》](https://learning.oreilly.com/library/view/designing-machine-learning/9781098107956/)通过概念性视角概述了机器学习开发周期。

# 谁应该阅读这本书？

由于这本书更侧重于概念而非技术，我预计它在实用性方面会更持久。因此，它可以作为广泛技术专业人士的有用参考，尽管这些人可能不会在第一天理解每一页。不过，机器学习开发的概念方面对数据科学新手来说可能不够亲切。我建议未来的读者在阅读此书之前，对以下内容有一定的了解：

+   统计分布，查找数据集中的范围、中位数、平均数和众数

+   如何在任何语言中训练一个简单的机器学习模型（例如 python、R 等）

+   对几种机器学习算法（例如线性回归、决策树、逻辑回归等）的概念性理解

+   使用计算机语言（例如 python、R、SQL）清理和处理混乱数据的经验

+   软件工程基础（即编程工作经验或上过几门计算机科学课程）

+   理解低质量与高质量数据

如果你不能自信地说你符合上述条件，我建议从一本较为基础的书开始。适当的读者范围从刚毕业的 STEM 学生到任何希望深入了解机器学习开发的有经验的技术专业人士；然而，后者可能会更喜欢这本书。没有一定的数据科学实际经验，理解这些概念会比较困难。

# 本书未涵盖哪些主题？

一些机器学习爱好者感兴趣但不在本书范围内的主题包括：

+   深入探讨特定的机器学习算法（例如决策树、神经网络、逻辑回归等）。如果你对此感兴趣，我推荐阅读我对[《百页机器学习书》](https://thedatageneralist.com/book-review-the-hundred-page-machine-learning-book/)的评论。

+   使用编程应用机器学习概念（例如 python、R 等）

+   使用编程语言（例如 python、spark 等）进行特征工程和数据准备的示例

# 本书的关键教训

本书提供了许多宝贵的经验教训，可以融入任何机器学习开发过程。由于本书更具概念性，读者必须决定如何最佳应用这些教训。以下是我在阅读本书时发现的一些最重要的观点：

# 数据泄漏

“数据泄漏是指标签的一种形式‘泄漏’到用于预测的特征集合中，而在推断过程中这些信息实际上是不可用的。” 这相当于试图猜测某人背后举起了多少根手指，而你面前有一面镜子能将答案反射给你。镜子揭示了在目标是衡量你预报准确度时本不应获取的信息。

在阅读本书之前，我没有意识到数据泄漏在模型开发生命周期中发生的频率。发生频率高的一个原因是数据泄漏并不总是显而易见的。根据个人经验，我最近在对不平衡数据集进行过采样后，数据泄漏发生了。数据泄漏的原因是一些重复记录同时存在于训练集和测试集中。我不得不调整我的代码，以便数据在过程的早期阶段就被拆分开。

# 模型开发

机器学习模型开发应分阶段进行，随着时间的推移逐渐增加复杂性。由于机器学习本质上是复杂的，第一阶段应尝试通过简单启发式方法解决业务问题，而不是开发模型。如果简单的启发式方法不够，则应尝试开发和部署一个简单的模型。早期进行端到端可以提供更多的过程可见性，并在添加更多复杂性时更容易发现错误。如果简单模型不符合预期，尝试通过不同的目标函数、超参数调优等优化简单模型。如果仍然不能满足预期，尝试更复杂的模型（例如，神经网络）。

在最近的一个项目中，开发过程中最困难的部分是部署。即使我成功部署了模型，它也揭示了几个超出我职责范围的额外障碍。如果我当时优先考虑端到端进度而不是模型优化，我们的团队可能会更早发现这些障碍。新特性的整体完成时间可能会减少。

# 模型跟踪与存储

由于模型开发是一个迭代过程，因此跟踪每个模型的信息以便于比较实验之间的性能、重现模型以及帮助维护是至关重要的。你可能考虑跟踪的信息包括：

+   模型定义- 塑造模型所需的信息（例如，损失函数）

+   模型参数- 你的模型参数的实际值

+   特征提取和预测函数- 在给定预测请求的情况下，你如何提取特征并将这些特征输入到你的模型中以获取预测？

+   依赖项- python 版本，包

+   数据- 用于训练模型的数据

+   模型生成代码- 使用的框架，如何训练等

+   实验工件- 在模型开发过程中生成的工件

+   标签- 帮助模型发现（例如，所有者）

即使跟踪了所有这些信息，由于过程中的随机性，重现任何特定模型可能仍然困难。然而，任何你认为对维护、调试、团队协调或模型可重现性有用的信息，至少应该考虑进行跟踪。

虽然跟踪非常有益，但可能会减慢实验速度，我建议尽可能制定自动化该过程的计划。在最近的一个项目中，Azure 机器学习帮助我跟踪了生产环境中的包版本，这对调试与版本相关的 scikit-learn 错误至关重要。

# 结论

《设计机器学习系统》是任何数据科学专业人员图书馆中的绝佳补充。Chip Huyen 从概念出发，而非具体实现，全面讲解了机器学习开发生命周期的每一步。阅读完本书后，你将获得新的框架，以帮助你在整个机器学习开发生命周期中应用最佳实践。对于许多概念，Chip 还提供了额外的资源以供深入探索。只需记住——她讨论的最佳实践必须与书本之外的知识相结合，才能应用于具体的模型实现。

~ 数据通用主义者

[数据科学职业顾问](https://thedatageneralist.com/career-advisor/)
