# 3月版：数据与因果关系

> 原文：[https://towardsdatascience.com/march-edition-data-and-causality-396b2881aea9?source=collection_archive---------9-----------------------#2023-03-02](https://towardsdatascience.com/march-edition-data-and-causality-396b2881aea9?source=collection_archive---------9-----------------------#2023-03-02)

## [每月版](https://towardsdatascience.com/tagged/monthly-edition)

## 数据科学家如何处理因果推断

[](https://towardsdatascience.medium.com/?source=post_page-----396b2881aea9--------------------------------)[![TDS Editors](../Images/4b2d1beaf4f6dcf024ffa6535de3b794.png)](https://towardsdatascience.medium.com/?source=post_page-----396b2881aea9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----396b2881aea9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----396b2881aea9--------------------------------) [TDS Editors](https://towardsdatascience.medium.com/?source=post_page-----396b2881aea9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7e12c71dfa81&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmarch-edition-data-and-causality-396b2881aea9&user=TDS+Editors&userId=7e12c71dfa81&source=post_page-7e12c71dfa81----396b2881aea9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----396b2881aea9--------------------------------) ·4分钟阅读·2023年3月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F396b2881aea9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmarch-edition-data-and-causality-396b2881aea9&user=TDS+Editors&userId=7e12c71dfa81&source=-----396b2881aea9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F396b2881aea9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmarch-edition-data-and-causality-396b2881aea9&source=-----396b2881aea9---------------------bookmark_footer-----------)![](../Images/93527ff7b57f6dc49778434eda9f9008.png)

照片由 [Joey Genovese](https://unsplash.com/@joeyguns?utm_source=medium&utm_medium=referral) 提供，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在最近的作者焦点问答中，[Matteo Courthoud](https://medium.com/u/666130fb420f?source=post_page-----396b2881aea9--------------------------------) 反映了在工业界或学术界工作时，做出可靠预测的重要性日益增加：

> 我认为在未来，因果推断将变得越来越重要，我们将看到社会科学的理论方法与计算机科学的数据驱动方法之间的趋同。

我们希望你能阅读[我们生动对话的其余部分](/the-main-driver-behind-my-writing-has-always-been-learning-ab07ca8c45a3)；与此同时，Matteo的观察激励我们深入档案，寻找关于因果推断及更广泛的因果主题的其他有见地的文章。我们在本月版中分享的选集内容从入门到更高级别，展示了数据科学和机器学习从业者在工作中每天使用的一些不同方法。

我们希望你喜欢探索这些推荐阅读！一如既往，我们很感激你将TDS作为你学习旅程的一部分；如果你希望以其他方式支持我们的工作（并在此过程中获得我们整个档案的访问权限），请考虑[成为Medium会员](https://bit.ly/tds-membership)。

[TDS 编辑]（https://medium.com/u/7e12c71dfa81?source=post_page-----396b2881aea9--------------------------------）

## TDS 编辑亮点

+   [**因果关系的科学与艺术（第一部分）**](/the-science-and-art-of-causality-part-1-5d6fb55b7a7c)（2023年1月，11分钟）

    [Quentin Gallea, PhD](https://medium.com/u/a52dcb9793ad?source=post_page-----396b2881aea9--------------------------------)对因果推断基础知识的易懂介绍旨在回答两个关键问题：理解因果关系为何如此重要，因果关系为何如此难以评估？

+   [**使用合成控制的因果推断**](/causal-inference-using-synthetic-control-4377b457c6bb)（2022年11月，9分钟）

    在A/B测试不是建立因果关系的好选择的情况下，准实验技术可能是解决方案。[diksha tiwari](https://medium.com/u/5af398da0b0b?source=post_page-----396b2881aea9--------------------------------)提出合成控制作为一种替代方案。

+   [**通过回归的因果效应**](/causal-effects-via-regression-28cb58a2fffc)（2023年1月，8分钟）

    [Shawhin Talebi](https://medium.com/u/f3998e1cd186?source=post_page-----396b2881aea9--------------------------------)已覆盖因果推断的理论与实践超过一年。你可以回到这个系列的[起点](/causality-an-introduction-f8a3f6ac4c4a)，或直接跳转到这篇关于回归技术及如何利用它们建立变量之间关系的最新文章。

+   [**因果推断的事件研究：应该做与不应该做的**](/event-studies-for-causal-inference-the-dos-and-donts-863f29ca7b65)（2022年12月，17分钟）

    事件研究在准实验背景中是另一种有帮助的方法；正如[Nazlı Alagöz](https://medium.com/u/4ba02da50bf?source=post_page-----396b2881aea9--------------------------------)在这篇深入说明的文章中指出的，有许多陷阱需要避免，以便我们不从数据中得出错误的见解。

+   [**使用SciPy进行两组独立样本均值的统计显著性检验**](/statistical-significance-testing-of-two-independent-sample-means-with-scipy-638cb834b4d1)（2022年11月，8分钟）

    对于那些热衷于动手实验数据的读者，[Zolzaya Luvsandorj](https://medium.com/u/5bca2b935223?source=post_page-----396b2881aea9--------------------------------)的教程是理想的起点：它提供了进入假设检验的实用入口，并包括所有所需的Python代码。

+   [**识别：可信因果推断的关键**](/identification-the-key-to-credible-causal-inference-c3023143349e)（2023年2月，8分钟）

    正如[Murat Unal](https://medium.com/u/15a64c9fc55d?source=post_page-----396b2881aea9--------------------------------)在一篇深刻的新文章中解释的那样，“没有明确的识别，没有任何复杂的建模或估计能够帮助我们从数据中确定因果关系。” 阅读Murat的概述，以更好地理解识别及其重要性。

## 原创特点

探索我们最新的问答和阅读推荐。

+   [**“我写作的主要驱动力一直是学习**](/the-main-driver-behind-my-writing-has-always-been-learning-ab07ca8c45a3)**”** 我们与[Matteo Courthoud](https://medium.com/u/666130fb420f?source=post_page-----396b2881aea9--------------------------------)的问答，他反思了离开学术界、对因果推断的兴趣以及公开写作的价值。

+   [**面对数据偏差依然困难且必要**](/confronting-bias-in-data-is-still-difficult-and-necessary-e0982fd7416c) 我们分享了在机器学习和人工智能领域中持续主导对话的话题的核心阅读资料。

+   [**如何作为数据科学家培养良好习惯**](/how-to-build-good-habits-as-a-data-scientist-74212a7b411f) 在最近的一次总结中，我们汇总了经验丰富的数据从业者提供的技巧和策略，以便更流畅和可靠的工作流程。

## 热门文章

如果你错过了，这里是上个月TDS中最受欢迎的一些文章。

+   [**ChatGPT的工作原理：模型背后的技术**](/how-chatgpt-works-the-models-behind-the-bot-1ce5fca96286) 由[Molly Ruby](https://medium.com/u/7a38f8e9fb80?source=post_page-----396b2881aea9--------------------------------)撰写

+   [**你可能在不知不觉中成为高级Pythonista的5个迹象**](/5-signs-youve-become-an-advanced-pythonista-without-even-realizing-it-2b1dd7ef57f3) 由[Bex T.](https://medium.com/u/39db050c2ac2?source=post_page-----396b2881aea9--------------------------------)撰写

+   [**使用OpenAI和Python提升你的简历：一步一步的指南**](/using-openai-and-python-to-enhance-your-resume-a-step-by-step-guide-e2c1a359e194) 由[Piero Paialunga](https://medium.com/u/254e653181d2?source=post_page-----396b2881aea9--------------------------------)撰写

+   [**如何用 Python 构建 ELT**](/how-to-build-an-elt-with-python-8f5d9d75a12e) 作者：[玛丽·陈](https://medium.com/u/4cfa1d0b321f?source=post_page-----396b2881aea9--------------------------------)

+   [**如何创建有效的自学计划，以成功自学数据科学**](/how-to-create-an-effective-self-study-routine-to-teach-yourself-data-science-successfully-6248c7ec3a53) 作者：[马迪逊·亨特](https://medium.com/u/6a8c6841e521?source=post_page-----396b2881aea9--------------------------------)

+   [**你还在使用肘部法则吗？**](/are-you-still-using-the-elbow-method-5d271b3063bd) 作者：[萨穆埃尔·马赞提](https://medium.com/u/e16f3bb86e03?source=post_page-----396b2881aea9--------------------------------)

+   [**如何为你的数据找到最佳理论分布**](/how-to-find-the-best-theoretical-distribution-for-your-data-a26e5673b4bd) 作者：[厄尔多安·塔斯克森](https://medium.com/u/4e636e2ef813?source=post_page-----396b2881aea9--------------------------------)

我们很高兴在二月迎来了全新的 TDS 作者团队——他们包括 [萨曼莎·霍德](https://medium.com/u/c71432db9be0?source=post_page-----396b2881aea9--------------------------------)、[阿尔瓦罗·佩尼亚](https://medium.com/u/3884c9f0acaf?source=post_page-----396b2881aea9--------------------------------)、[泰米托普·索博杜](https://medium.com/u/3b87c271bf6?source=post_page-----396b2881aea9--------------------------------)、[弗雷德里克·霍特尔](https://medium.com/u/de281205c742?source=post_page-----396b2881aea9--------------------------------)、[吉尔·肖姆龙](https://medium.com/u/6f73b54221a8?source=post_page-----396b2881aea9--------------------------------)、[拉斐尔·比绍夫](https://medium.com/u/913c6c1e6a94?source=post_page-----396b2881aea9--------------------------------)、[肖恩·史密斯](https://medium.com/u/6957f6523097?source=post_page-----396b2881aea9--------------------------------)、[布鲁诺·阿尔维西奥](https://medium.com/u/ddb474c44925?source=post_page-----396b2881aea9--------------------------------)、[乔里斯·盖林](https://medium.com/u/c2fba9c25ca1?source=post_page-----396b2881aea9--------------------------------)、[德米特里·埃柳塞耶夫](https://medium.com/u/65c1f6ba75db?source=post_page-----396b2881aea9--------------------------------)、[科里·贝克](https://medium.com/u/9f206469e308?source=post_page-----396b2881aea9--------------------------------)、[波尔·马林](https://medium.com/u/1fa43cc443e7?source=post_page-----396b2881aea9--------------------------------)、[皮奥特·拉赫特](https://medium.com/u/a8d3ad6ac83c?source=post_page-----396b2881aea9--------------------------------)、[布鲁诺·波尼](https://medium.com/u/2819bc6617ce?source=post_page-----396b2881aea9--------------------------------) 和 [诺布尔·阿克森](https://medium.com/u/68605bd278a3?source=post_page-----396b2881aea9--------------------------------) 等。如果你有有趣的项目或想法要与我们分享，[我们非常乐意听取你的意见](http://bit.ly/write-for-tds)!

下个月见。
