# 重新思考数据科学组合

> 原文：[`towardsdatascience.com/rethinking-the-data-science-portfolio-f0f64c625711`](https://towardsdatascience.com/rethinking-the-data-science-portfolio-f0f64c625711)

## 为什么一个易于执行且用户友好的项目会优于一个高复杂度的展示品

[](https://medium.com/@anthonybaum?source=post_page-----f0f64c625711--------------------------------)![Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----f0f64c625711--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0f64c625711--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0f64c625711--------------------------------) [Anthony Baum](https://medium.com/@anthonybaum?source=post_page-----f0f64c625711--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0f64c625711--------------------------------) ·阅读时间 5 分钟·2023 年 7 月 14 日

--

![](img/ba2ac55fab986c34cffff6f8fe4aec51.png)

照片由[Helena Lopes](https://unsplash.com/@wildlittlethingsphoto?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在技术领域存在一个长期的误解：即认为项目越复杂，体现出的能力就越强。

在我职业生涯初期，我花费了无数小时为我的项目增加一层又一层的复杂性，希望通过展示我的技术能力和处理复杂算法的能力来打动潜在雇主。随着时间的推移，我意识到这种方法不仅适得其反，而且也误导了我如何在工作中交付成果。

作为招聘经理以及一个坚定相信优秀组合价值的人，我主张将易于运行和用户友好性优先于复杂性，以展示对现实世界数据科学任务的更准确理解。

## 不要让目标变得复杂

当我进入数据科学职业大约一年时，我需要构建一个模型来计算我们归因模型的基线网络流量。我立刻看到了展示的机会。我经历了 ARIMA、Prophet 等多次迭代，试图展示我的机器学习技能。我最后选择了一个随机森林实现。

当需要向非技术高层解释方法论时，我不得不告诉他们内部机制有点像黑匣子。他们面露困惑。我的经理后来把我拉到一边，建议我尝试一些可以在一张 PowerPoint 幻灯片中完全解释的东西（把这条建议写在你的显示器上的便签纸上）。几乎是自我调侃，我尝试了一个滚动中位数。它的错误评分在合理紧凑的范围内。

我们向高层提交了“调整更新”，我只需提到滚动平均值，没人对此提出质疑。五年和多个归因模型迭代后，这一基线计算仍在生产中运行，无需重新训练或监控模型漂移。它处理了数亿美元的广告支出，并且是我们工作中少数没有受到挑剔客户质疑的元素之一。

在作品集的背景下，这可能看起来是一个极端的例子，但它突出了一个关键点。复杂的项目容易变得繁琐、难以理解，因此也更难以执行。这些项目对你和审阅你工作的人员提供的价值在逐渐减少。

> 一个精心制作的作品集项目可能展示了技术能力，但它不一定反映出创造实用解决方案的能力。

数据科学的能力不仅仅在于你处理复杂性的能力。更在于你对领域原则的理解、创造实用且用户友好的解决方案的能力，这些解决方案可以被轻松实施和运行，最重要的是，你能够用数据讲述一个引人入胜的故事。

## 保持你的工作可访问

在追求复杂性的过程中，很容易忽视这些基本原则。一个复杂的作品集项目可能展示了技术能力，但它并未反映出创造符合商业目标和约束的实用解决方案的能力。这种对复杂性的追求可能不仅会误导现实世界数据科学的需求，还会忽视学科的本质：利用数据创造有影响力、易于理解且可操作的见解。

> 当你的项目易于理解且运行简单时，它们会变得更加可访问和吸引人。

上次我的团队招聘时，我非常兴奋地看到简历上有很多作品集链接。我很快就失望了，发现其中一半以上只是几个包含单个 Jupyter notebook 的代码库，没有说明，且 notebook 中的 markdown 仅仅是对代码块的一句话描述。我给几乎所有有说明和需求文件的人安排了面试——标准真的可以这么低。

可访问且引人入胜的项目**会**使你的作品集在潜在雇主面前脱颖而出。是的，GitHub 显示了你的 notebook 内容，但如果招聘经理可以克隆你的代码库，按照你的 README 文件操作，并且在不更改任何内容的情况下运行你的所有代码，你将会发现自己接近候选人名单的顶部。

## 展示你是为了协作而编码

上述所有内容也适用于代码的*质量*与数量和复杂性之间的比较。能够生产既易于维护又易于他人理解的代码，展示了你在团队合作中的能力，并明确体现了专业级的软件工程技能。

这表明你意识到你的代码不是一个孤立的存在，而是一个更大生态系统的一部分，其他人可能需要与之互动。关注用户友好性将展示一种成熟度和对现实数据科学的理解，超越纯技术专长。

## 反思你学到的习惯

同样，从代码中退后一步，反思你如何处理你的工作也是很重要的。我在学校里遇到了很大的困难，总是知道课堂内容却无法考得很好。我的学校生活变成了如何通过考试，而不是如何获得 A 级成绩，考试的结果直接关系到我的未来。

正如你可能想象的那样，这并没有让我在需要尽可能少地向 CEO 提供信息的情况下获得大量的奖励积分。

> 我们大多数人花费了 15-20 年的时间在教育系统中努力成功，这些系统通过我们能够重复尽可能多的知识来衡量我们的成功。

尤其是在寻找入门级职位时，转变到与你直觉相悖的流程和工作流需要勇气。毕竟，我在学校的经历并不孤单；我们大多数人花费了 15-20 年的时间在教育系统中努力成功，这些系统通过我们重复知识的能力来衡量我们的成功。

## 关注最终目标

记住，你的作品集是你向潜在雇主的介绍，他们寻找的不仅仅是技术能力。他们希望看到你的问题解决能力、沟通技巧以及对现实问题的理解。

在招聘时，我最看重的是与他人合作的无缝程度。一个团队的价值在于他们共同有效解决问题的能力。
