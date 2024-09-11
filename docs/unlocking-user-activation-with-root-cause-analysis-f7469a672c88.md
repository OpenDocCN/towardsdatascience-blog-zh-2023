# 解锁用户激活与根本原因分析

> 原文：[https://towardsdatascience.com/unlocking-user-activation-with-root-cause-analysis-f7469a672c88?source=collection_archive---------16-----------------------#2023-03-20](https://towardsdatascience.com/unlocking-user-activation-with-root-cause-analysis-f7469a672c88?source=collection_archive---------16-----------------------#2023-03-20)

## **逐步指南：如何进行结构化根本原因分析**

[](https://medium.com/@jordangom?source=post_page-----f7469a672c88--------------------------------)[![Jordan Gomes](../Images/d08bb9fd8b084687599a67a2221ec68c.png)](https://medium.com/@jordangom?source=post_page-----f7469a672c88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f7469a672c88--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f7469a672c88--------------------------------) [Jordan Gomes](https://medium.com/@jordangom?source=post_page-----f7469a672c88--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbd72dcfe2a5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-user-activation-with-root-cause-analysis-f7469a672c88&user=Jordan+Gomes&userId=bd72dcfe2a5a&source=post_page-bd72dcfe2a5a----f7469a672c88---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f7469a672c88--------------------------------) ·6分钟阅读·2023年3月20日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff7469a672c88&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funlocking-user-activation-with-root-cause-analysis-f7469a672c88&source=-----f7469a672c88---------------------bookmark_footer-----------)

[在我之前的文章中](https://medium.com/towards-data-science/using-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9)，我讨论了如何为你的业务定义激活指标——那些你可以在短期内推动并在长期内产生影响的指标。

建立这些是迈向产品增长伟大旅程的第一步，一旦你识别出这些内容——这时有趣的部分才开始。你接下来的任务就是理解哪些方法和最有效的杠杆可以推动这些内容。

这就是根本原因分析可以帮助你的地方。RCA是一种结构化的问题解决方法，可以提供对使用哪些杠杆以及它们对你期望结果的贡献程度的更好理解。它将帮助你深入了解当前的问题并识别其根本原因。

我特别喜欢这种分析方法，因为它位于定量和定性方法的交汇点。这使其成为一种高度跨职能和全面的研究，其中利用定性数据对于识别潜在原因和测试假设是必要的。

为了让这些内容更易于理解，我们将重用我在之前文章中使用的相同例子：

+   你经营一个健身应用程序。

+   你刚刚发现，上传锻炼视频的用户在下载应用程序后的7天内更有可能继续使用你的应用程序，而不是那些没有上传的用户。

+   你现在在思考如何才能最好地推动这个指标。

![](../Images/463e6d34a6b018e6df36b0128f8f12bb.png)

[Matteo Grando](https://unsplash.com/@mang5ta?utm_source=medium&utm_medium=referral) 拍摄的照片，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# **什么是RCA**

根本原因分析（RCA）是一种结构化的问题解决方法，它帮助你识别问题的根本原因，而不仅仅是处理其症状。它可以让你真正了解“为什么”——为什么事情会以这种方式发生。其输出可以有多种形式——但通常可以表示为“决策树”或“失败树”，这些形式易于理解，非常直观，商业利益相关者尤其喜欢。

```py
Digressing a bit — this methodology can be applied to virtually any problem, 
and in its simplest form, it doesn’t require any particular data knowledge. 
If properly used, it can be a great tool to make you think about complex 
systemic interactions and allow you to move away from a symptom to its cause. 
```

它通常包括大约5个步骤：定义问题、收集数据、识别和评估不同的原因、识别根本原因以及制定解决方案计划。此外，它不仅允许你了解“为什么”，还可以让你对如何能在你的问题上取得多大进展有个粗略估计（从这些估计中你可以建立优先级模型、OKRs等）。

# **常规的根本原因分析步骤**

**#1: 定义问题**：根本原因分析的第一步是明确你想解决的问题。在我们的例子中（以及在此阶段），问题相当直接：“我们如何在用户注册我们的移动健身应用程序后的7天内增加上传锻炼的用户数量，以提高用户保留率？”

**#2: 收集数据**：下一步是收集有关问题的数据。这包括收集有关问题的信息，例如何时发生/发生了什么/谁受到影响/他们如何受到影响。重要的是要获得问题的定量和定性视角，并与受影响的人员和/或该主题的专家交谈。

**#3: 识别和评估可能的原因**：这才是真正有趣的部分！一旦你有了数据，下一步就是识别问题的可能原因。这包括与一些主题专家进行头脑风暴，并与用户交谈，以便能够全面了解可能导致问题的所有因素——包括非常明显的因素，也包括那些可能不那么明显的潜在原因。

在这一步中，你可以使用**回归分析**来获取一些灵感。当你完全不知道从哪里着手时，回归分析会很有用。它可以帮助你找到与问题高度相关的特征，然后从那里手动深入研究，以更好地理解问题。

在我们的例子中，假设我们发现某些国家或设备与激活的可能性高度负相关。在这种情况下，可能值得进一步探讨：

+   也许翻译存在一些问题？

+   也许在某些设备上存在显示错误？

+   等等。

```py
Note that as always, correlation is not causation — this method can help you 
find subsets of users worth looking into, but the results shouldn’t be taken
directly (i.e. without further checking) for your root cause analysis. 
```

一旦你识别出所有可能的原因，并开始构建你的树形图，重要的是要评估这些原因并了解它们对问题的贡献程度。对于一些原因，这将很简单，因为你会有数据；而对于其他原因，则可能稍微复杂一些，你可能需要收集更多的数据和/或发挥一些想象力来生成一些现实的估计——但通过这些权重，当你开始撰写建议并决定调整哪些杠杆时，这将使事情变得更加简单。

**#4: 迭代直到识别出根本原因**：现在你已经识别出决策树的第一个节点，你可以重新迭代，直到找到实际的原因。作为一个经验法则，你应该尝试进行最多5次迭代（即“5个为什么”技术）——这是一个很好的强迫性方法，挑战你真正思考第二/第三层次的效果，而不仅仅是明显的原因。

如果我们回到我们的例子，它可能看起来像这样：

![](../Images/a172e2e6fef4bc2eb77a6b6e83cd2101.png)

针对健身应用激活率的根本原因分析示例（图像由作者提供）

**#5: 制定行动计划**：现在你已经对潜在根本原因有了清晰的了解，最后一步是制定一个行动计划来解决这些问题。我写了一篇关于“[如何优先考虑进行哪些数据科学项目](https://medium.com/towards-data-science/how-to-choose-which-data-projects-to-work-on-c6b8310ac04e)”的文章：这里的思路类似：你需要根据不同的参数来优先考虑要调整哪个杠杆，这些参数可以包括（但不限于）：机会的大小、执行能力、对结果的信心、市场时间等。

![](../Images/d6afe20c369f69550101d2b4f609eb56.png)

数据项目的时间投资矩阵（图像由作者提供）

# **附赠：根本原因分析（RCA）和目标设定（例如 OKRs）**

根本原因分析（RCA）不仅能让你更好地了解是什么驱动了问题，还能让你更好地理解如何解决它，以及你的修复可能会如何影响最终指标。

你在这里所做的工作，实际上也可以用于为你的公司设定目标/关键成果（OKRs）：在上述示例中，你可以使用不同的权重（可能使用你用于优先级排序的参数，如对结果的信心进行折扣）来告知公司可以设定的不同目标。

最终，根本原因分析（RCA）是一个易于使用、漂亮的工具，可以帮助你的团队做出更好的决策。它不需要高水平的技术技能，但需要有组织性，并结合定量和定性数据。如果操作得当，它可以产生巨大的影响。

## 希望你喜欢阅读这篇文章！你有什么想分享的技巧吗？请在评论区告诉大家！

**如果你想阅读更多我的文章，这里有一些你可能会喜欢的内容**：

[使用倾向评分匹配来构建领先指标](https://towardsdatascience.com/using-propensity-score-matching-to-build-leading-indicators-3e656dccbaf9?source=post_page-----f7469a672c88--------------------------------)

### 构建产品激活指标的简短指南

[如何构建成功的仪表盘](https://towardsdatascience.com/how-to-build-a-successful-dashboard-359c8cb0f610?source=post_page-----f7469a672c88--------------------------------)

### 来自于一些构建了几个不成功的项目的人提供的清单

[如何选择数据项目](https://medium.com/@jolecoco/how-to-choose-which-data-projects-to-work-on-c6b8310ac04e?source=post_page-----f7469a672c88--------------------------------) 

### 如果你对如何使用时间有合理的方法，你可以优化你产生的价值。

[选择数据项目的短指南](https://medium.com/@jolecoco/how-to-choose-which-data-projects-to-work-on-c6b8310ac04e?source=post_page-----f7469a672c88--------------------------------)
