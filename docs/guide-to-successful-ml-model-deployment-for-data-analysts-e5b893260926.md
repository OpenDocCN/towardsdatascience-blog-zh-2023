# 数据分析师成功机器学习模型部署指南

> 原文：[`towardsdatascience.com/guide-to-successful-ml-model-deployment-for-data-analysts-e5b893260926`](https://towardsdatascience.com/guide-to-successful-ml-model-deployment-for-data-analysts-e5b893260926)

![](img/1bbe793cc056570d6ca290c61084871b.png)

照片由 [ray rui](https://unsplash.com/@ray30?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 数据模型部署如何不同于其他分析项目

[](https://tanuwidjajaolivia.medium.com/?source=post_page-----e5b893260926--------------------------------)![Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----e5b893260926--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e5b893260926--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e5b893260926--------------------------------) [Olivia Tanuwidjaja](https://tanuwidjajaolivia.medium.com/?source=post_page-----e5b893260926--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e5b893260926--------------------------------) ·6 分钟阅读·2023 年 4 月 11 日

--

恭喜你创建了一个看似结果可靠的机器学习模型！我从经验中知道，获得可靠的机器学习模型并不容易，特别是在作为数据分析师且接触有限的情况下。然而，一旦模型完成，下一个挑战就是将其投入生产，以便集成到产品中。

对于许多数据分析师来说，机器学习模型的部署可能是未涉足的领域，因为**大多数数据建模概念仅用于分析目的，而未在生产中部署**。虽然一些公司有机器学习运维团队来帮助这些部署，但分析师了解相关的关键概念和注意事项，以便与机器学习运维团队有效合作是至关重要的。

为了弥补这一知识差距，在这篇文章中，我将分享数据分析师在将模型投入生产时需要了解的关键概念和注意事项。我还将解释机器学习模型的部署如何不同于常规分析项目。了解这些内容后，数据分析师可以确保他们的模型成功部署并提供有意义的结果。

# 数据分析师的机器学习模型部署注意事项

作为数据分析师，我们习惯于消耗历史数据进行离线分析。大多数时候，我们会花几分钟从在线数据库中查询数据，再花几个小时或几周处理（分析）这些数据。对于每个项目，处理和查询的数据可能有所不同。找到一个一次性的分析项目是不奇怪的，***不需要可重复性***。

当我们谈论用于产品集成的机器学习模型时，情况会变得非常不同。需要考虑几个原则，包括

+   ***延迟*：模型提供输出所需的时间**，从检索特征到交付预测结果的时间。完成工作的时间越长，输出被客户使用的时间也越长，这会影响用户体验。

+   ***数据可用性*：在模型触发时可用的数据/特征集合**。有些数据可能不存在（或可能在延迟限制内被查询）。

+   ***鲁棒性*：模型在使用新数据与训练数据时性能变化的程度**。理想情况下，鲁棒的模型即使面对新的真实数据也会保持稳定的性能。然而，确实有时候模型性能可能会随着时间的推移而下降，需要对模型进行重新训练。

这些因素在一次性分析世界中几乎不存在，但在机器学习部署世界中非常关键。

## 数据和特征工程管道

准备/处理数据用于分析与建模（部署）之间的一个显著区别是数据管道和随后的特征工程。如上所述，我们在开发数据管道时需要考虑延迟和数据可用性约束。

在模型输出被纳入产品时，延迟要求可能会变得非常严格。假设你正在构建一个[ETA 预测模型](https://www.uber.com/en-SG/blog/deepeta-how-uber-predicts-arrival-times/)，模型输出需要在几秒钟内完成，以避免对用户体验的任何干扰。因此，你只能使用在该时间内可以获得的特征，并且需要一个可靠的管道来支持这一点。一些常见的做法包括：

+   使用[**特征存储**](https://www.kdnuggets.com/2022/03/feature-stores-realtime-ai-machine-learning.html)**。** 特征存储是一个集中平台，用于存储来自不同数据源的特征。它有助于（1）**可重复性**，因为一致的特征可以在训练和推理之间使用，以及（2）特征在多个生产模型中的**可重用性**。它可以设计用于在线服务，以最大化服务速度。特征存储及其架构在决定性能方面发挥重要作用——无论是服务速度还是准确性。

![](img/ce8007d3ae32cb174c6fd06b6f15d2e5.png)

特征存储示例（来源：[feast.dev](https://feast.dev/blog/what-is-a-feature-store/))

+   **组装预处理**步骤。在输入数据被插入模型之前，通常需要进行多个预处理步骤——从数据清洗到填补和归一化。由于需要进行多个步骤，这些步骤存在可读性和调试风险。组装这些步骤，例如使用 sklearn 中的管道 可以帮助组织这些步骤，使其对用户更清晰，减少错误的发生。

## 持续监控和不断改进

与分析工作不同，在数据建模中，**模型部署或集成后，工作并未完全完成**。在某些情况下，工作甚至才刚刚开始 😆

就像其他数字产品一样，你需要随着时间的推移来维护和改进数据模型。模型只能理解与其进行训练的数据动态。随着市场和用户行为的变化，**模型可能会变得与预测/分类不相关，因为它未能适应这些新的数据分布**。

![](img/9b66d4f13b4b7245c9e8ae84f3e9554a.png)

ML 模型的性能会随着时间的推移而下降。来源：[ML Ops](https://ml-ops.org/content/mlops-principles#monitoring)

因此，需要对模型进行持续监控和不断改进，以确保模型预测结果的质量。

需要持续监控的 ML 模型方面包括：

+   **输入数据分布的偏差。** 在大多数情况下，模型的不相关性发生是因为环境/市场变化（即用户行为），这导致数据分布的变化。考虑一个电子商务平台，它有一个欺诈检测模型，该模型使用每个客户每天的总订单作为主要预测因素来识别滥用客户。随着平台扩展其产品种类，客户订单量意外增加，这可能导致一些合法客户被模型错误地识别为滥用客户。为避免这种情况，**需要持续监控输入特征的统计分布，例如均值或标准差**。

> 当变化是持续的，并且偏差开始影响性能时，可能需要考虑一些改进措施。

+   **模型性能。** 模型性能（[模型准确率、精确度、召回率](https://www.analyticsvidhya.com/blog/2019/08/11-important-model-evaluation-error-metrics/)）**是最重要的监控方面，但有时也是最难监控的**。原因是有些情况下我们没有经过验证的标签来与模型的预测进行比较，因为模型处理的是新数据。为了实现性能监控，我们可以使用（1）**代理指标**或（2）**基准评审**。代理指标是可以间接显示模型影响的产品指标，如推荐模型的客户点击/转化率。基准评审是对模型输入和输出的抽样进行审查和手动标记，例如在为模型训练准备的数据中。

+   **系统性能。** 部署的数据模型不仅涉及模型本身，还包括数据管道中的其他组件，如特征存储、数据库和产品 API。对于分布式系统来说，常见的指标有[流量、延迟、错误和饱和度](https://sre.google/sre-book/monitoring-distributed-systems/)。

在更高级的组织中，[持续模型训练、集成和部署](https://cloud.google.com/architecture/mlops-continuous-delivery-and-automation-pipelines-in-machine-learning#mlops_level_1_ml_pipeline_automation)可以考虑用来应对数据和业务环境的快速变化。

## 结束语

随着数据分析师提升技能，成为[全栈数据分析师](https://medium.com/towards-data-science/why-i-choose-full-stack-data-analytics-as-my-career-path-d7b3986e0285)，他们可能会发现自己**在将洞察力转化为数据产品**，包括数据模型。

> 然而，必须认识到**交付（部署）数据模型**与**交付（展示）数据洞察力**是两个完全不同的领域。

在操作化过程中，有必要考虑持续监控和改进要求。通过理解这些概念，数据分析师可以确保交付的模型能够随着时间的推移满足产品/组织的需求。
