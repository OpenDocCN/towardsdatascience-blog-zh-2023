# 模型可解释性，再次探讨：SHAP 及其他

> 原文：[`towardsdatascience.com/model-explainability-revisited-shap-and-beyond-1c6ee85b294?source=collection_archive---------9-----------------------#2023-09-07`](https://towardsdatascience.com/model-explainability-revisited-shap-and-beyond-1c6ee85b294?source=collection_archive---------9-----------------------#2023-09-07)

[](https://towardsdatascience.medium.com/?source=post_page-----1c6ee85b294--------------------------------)![TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----1c6ee85b294--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1c6ee85b294--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c6ee85b294--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----1c6ee85b294--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-explainability-revisited-shap-and-beyond-1c6ee85b294&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----1c6ee85b294---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1c6ee85b294--------------------------------) · 作为 Newsletter 发送 · 3 min 阅读 · 2023 年 9 月 7 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F1c6ee85b294&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-explainability-revisited-shap-and-beyond-1c6ee85b294&user=TDS+Editors&userId=7e12c71dfa81&source=-----1c6ee85b294---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F1c6ee85b294&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmodel-explainability-revisited-shap-and-beyond-1c6ee85b294&source=-----1c6ee85b294---------------------bookmark_footer-----------)

最近几个月，大型语言模型的迅速崛起主导了关于人工智能的讨论——这是可以理解的，因为大型语言模型的创新性以及它们迅速融入数据科学和机器学习专业人士的日常工作流程中。

然而，对模型性能及其带来的风险的长期关注依然至关重要，而可解释性正是这些问题的核心：模型是如何以及为何给出它们的预测？黑箱中究竟隐藏了什么？

本周，我们将回到模型解释性的主题，介绍几篇近期文章，这些文章深入探讨了其复杂性，并提供了实践者可以尝试的实用方法。祝学习愉快！

+   在任何解释性挑战的核心问题是，你的数据中哪些特征对模型的预测贡献最大。[Khouloud El Alami](https://medium.com/u/9c6a36490614?source=post_page-----1c6ee85b294--------------------------------) 的 **SHAP 特征重要性分析介绍** 是一个适合初学者的资源，基于作者在 Spotify 的研究项目。

+   如果你过去已经使用过 SHAP，并且希望扩展你的工具集，[Conor O'Sullivan](https://medium.com/u/4ae48256fb37?source=post_page-----1c6ee85b294--------------------------------) 提供了一个 **处理更专业用例的实践指南**，特别是如何为分类问题显示 SHAP 图表以及如何汇总多类目标的 SHAP 值。

+   想要了解模型解释性所带来的新视角，不要错过 [Diksha Sen Chaudhury](https://medium.com/u/d7f3ce137d78?source=post_page-----1c6ee85b294--------------------------------) 最近的文章 **一个将医疗数据和机器学习结合的项目**。Diksha 的目标是展示如何使用 SHAP 让模型不仅具有可解释性，还对那些希望将结果与医学文献中的发现进行基准对比的研究人员有用。

![](img/9081a137af7f4c49a19c007b1f108324.png)

图片由 [Alina Kovalchuk](https://unsplash.com/@moonofviolet?utm_source=medium&utm_medium=referral) 拍摄，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

+   正如 [Vegard Flovik](https://medium.com/u/17ff8967433?source=post_page-----1c6ee85b294--------------------------------) 所言：“对于那些安全至关重要的重资产行业应用，错误可能导致灾难性后果，缺乏透明度可能是采纳的一个主要障碍。” 为了填补这一空白，Vegard 提供了一个 **详尽的开源 Iguanas 框架指南**，并展示了如何利用其自动化规则生成的功能来增强解释性。

+   虽然 SHAP 值在许多现实世界场景中证明是有益的，但它们也有局限性。[Samuele Mazzanti](https://medium.com/u/e16f3bb86e03?source=post_page-----1c6ee85b294--------------------------------)提醒不要过分重视特征重要性（文字游戏！），并**建议同等关注错误贡献**，因为“一个特征的重要性并不意味着它对模型有益。”

我们知道九月初是你们许多人一年中最忙碌的时段，但如果你有更多时间，以下是本周我们其他推荐阅读的绝对不容错过的文章：

+   如果你现在正在参加数据科学训练营，或者考虑将来参加一个，[Alexandra Oberemok](https://medium.com/u/346def7ad86?source=post_page-----1c6ee85b294--------------------------------)的全面指南以充分利用这一经历是不容错过的必读之作。

+   跑者们，这篇是为你们准备的：[barrysmyth](https://medium.com/u/a995c3b2ae8?source=post_page-----1c6ee85b294--------------------------------)的新深度探讨研究马拉松数据以评估不同策略来优化你的表现。

+   在他的 TDS 首篇文章中，[Christian Burke](https://medium.com/u/764fa444fa3?source=post_page-----1c6ee85b294--------------------------------)带我们深入了解一个创新的 MOMA 生成 AI 艺术项目，他在其中扮演了关键角色。

+   [Olga Chernytska](https://medium.com/u/cc932e019245?source=post_page-----1c6ee85b294--------------------------------)分享了她出色的“构建更好的 ML 系统”系列中的新一篇，这次专注于基线、指标和测试集。

+   不确定如何处理缺失数据？[Miriam Santos](https://medium.com/u/243289394aaa?source=post_page-----1c6ee85b294--------------------------------)提供了一个关于这个长期问题的一站式资源，并解释了如何在实际数据集中识别和标记缺失值。

+   如果你想深入了解详细的技术解释，[Antonieta Mastrogiuseppe](https://medium.com/u/a8ee237975ec?source=post_page-----1c6ee85b294--------------------------------)对梯度下降算法的概述清晰且执行良好。

感谢你对我们作者工作的支持！如果你喜欢你在 TDS 上阅读的文章，考虑[成为 Medium 会员](https://bit.ly/tds-membership) —— 这将解锁我们整个档案（以及 Medium 上的所有其他文章）。
