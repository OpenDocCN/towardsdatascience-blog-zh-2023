# 如何将家庭作业转化为数据科学职位

> 原文：[https://towardsdatascience.com/how-to-transform-a-take-home-assignment-into-a-data-science-job-f06878d71d26?source=collection_archive---------7-----------------------#2023-01-23](https://towardsdatascience.com/how-to-transform-a-take-home-assignment-into-a-data-science-job-f06878d71d26?source=collection_archive---------7-----------------------#2023-01-23)

## 逐步指南，教你如何从数据集中创建引人注目的故事

[](https://medium.com/@alex.vamvakaris.ds?source=post_page-----f06878d71d26--------------------------------)[![Alex Vamvakaris](../Images/bf8cf7c92a94d2b0d9242afcc03bf425.png)](https://medium.com/@alex.vamvakaris.ds?source=post_page-----f06878d71d26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f06878d71d26--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f06878d71d26--------------------------------) [Alex Vamvakaris](https://medium.com/@alex.vamvakaris.ds?source=post_page-----f06878d71d26--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8072260dd591&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-transform-a-take-home-assignment-into-a-data-science-job-f06878d71d26&user=Alex+Vamvakaris&userId=8072260dd591&source=post_page-8072260dd591----f06878d71d26---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f06878d71d26--------------------------------) · 8 分钟阅读 · 2023年1月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff06878d71d26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-transform-a-take-home-assignment-into-a-data-science-job-f06878d71d26&user=Alex+Vamvakaris&userId=8072260dd591&source=-----f06878d71d26---------------------clap_footer-----------)

--

![](../Images/872101297c607a1d66b5d57ff2169964.png)

图片由 [Patrick Federi](https://unsplash.com/@federi?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/pvbRsgY9ijY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

家庭作业是许多数据科学面试中的常见步骤，通常在筛选过程的后期阶段进行。第一轮考察你的统计学知识（假设检验等），通常包括实践编码问题（SQL、R等）。如果你需要巩固这些领域的技能，请查看以下文章。

[](/how-to-take-your-sql-from-zero-to-data-scientist-level-part-1-3-d7225d2d89ad?source=post_page-----f06878d71d26--------------------------------) [## 如何将你的SQL从零基础提升到数据科学家水平 — 第1/3部分

### 设置SQL并执行你的第一个SELECT查询

towardsdatascience.com](/how-to-take-your-sql-from-zero-to-data-scientist-level-part-1-3-d7225d2d89ad?source=post_page-----f06878d71d26--------------------------------) [](/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec?source=post_page-----f06878d71d26--------------------------------) [## 我作为数据科学家在进行一年AB测试后的收获 — 第1/2部分

### 按照这些简单步骤设置你的A/B测试，像数据科学家一样

towardsdatascience.com](/what-i-learned-after-running-ab-tests-for-one-year-as-a-data-scientist-part-1-2-5277911e89ec?source=post_page-----f06878d71d26--------------------------------)

在许多方面，家庭作业模拟了公司对你期望的情况，如果你被聘用并遇到类似的问题。在本文中，我根据我作为候选人和招聘经理的经验，创建了一个逐步指导如何应对这一挑战。该指南将按以下结构进行：

+   **家庭作业的基础知识**

+   **数据的快速QA**

+   **探索阶段：** 查找数据中的模式

+   **解释阶段：** 构建你的故事

# **1\. 家庭作业的基础知识**

![](../Images/afeb837584df225a07d606900c43ad6f.png)

照片由[Dennis Scherdt](https://unsplash.com/@ahnako?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来自[Unsplash](https://unsplash.com/photos/Mpq0LddqiTk?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **1.1 家庭作业的结构是什么？**

家庭作业由两个元素组成。一个数据集和一个作业概述。

+   **数据集**通常是公司数据的虚拟版本。它以.csv格式提供，附有所有列的描述。很少情况下，你可能会得到多个数据集，并且需要将它们结合起来。

+   **作业概述** 提供了任务的一些背景信息以及他们期望你对数据集做什么的简短描述。例如，在我为Deliveroo的数据科学职位做的家庭作业中，任务是分析他们的RGR（Rider Gets Rider）推荐程序与其他营销渠道的表现。然后评估其是否成功，如果没有（哈哈！），我们应该考虑更改的一些重要因素。任务的最后一步涉及向高级非技术利益相关者提供信息摘要，并建议下一步行动，例如收集额外的数据。

## **1.2 通过家庭作业期望获得的洞察是什么？**

家庭作业使你同时处于一种独特的优势和劣势的位置。一方面，你已经有了问题定义和数据集，这通常需要项目流程中的大量时间。你还可以自由选择如何进行分析。另一方面，你无法与业务利益相关者进一步沟通，这很可怕。这正是测试评估的内容。能够接受这样的模糊作业，并且在不让其过于复杂的情况下（事实上，这也是你将被评估的内容之一），提供以下内容：

> 对数据集有清晰的理解，使你能够发现其中隐藏的问题（或机会）。
> 
> 此外，你能够 pinpoint 可能的问题来源，并为你的目标受众提供清晰的建议和下一步计划。
> 
> 换句话说，他们希望你展示你能够从数据中提供价值！

# 2\. 数据的快速质量检查

![](../Images/d05a71293a55336a115be36ae79973b9.png)

[Ümit Yıldırım](https://unsplash.com/@umityildirim?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄于 [Unsplash](https://unsplash.com/photos/HICWw21mOD0?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

在我们继续之前，我们需要快速检查数据集的健康状况。在计算机科学中，“垃圾进，垃圾出”（GIGO）是一个概念，表示有缺陷或无意义（垃圾）的输入数据会产生无意义的输出。虽然跳过这一步直接进入分析是很诱人的，但你不希望因为这样一个简单的错误而失败。

+   确定作为主键的列（即，对于数据集中的每一行都是唯一的）。此信息通常会提供。

+   检查列中的缺失值并相应地标记它们（*NA* 适用于R，*NULL* 适用于SQL等）。隐藏的缺失值，如999的可疑峰值，也可以标记为*NA*。

+   确保所有列格式正确。例如，二进制属性（值为0和1）标记为类别型，而不是整数，或者日期不会被错误标记为字符型。

+   检查是否有不合理的值。当所有其他观察值都在0到1000之间时，1亿的值可以在可能的情况下进行修正，否则排除（确保在你的展示中解释任何排除的决定）

# 3\. 探索阶段：发现数据中的模式

![](../Images/a98fbaffec45eafb59f53f1d6a90f1d1.png)

照片由 [Daniel Lerman](https://unsplash.com/@dlerman6?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 拍摄，来源于 [Unsplash](https://unsplash.com/photos/fr3YLb9UHSQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **3.1 创建KPI**

对于大多数公司来说，兴趣点集中在数值属性上。这些属性可以是任何东西，取决于公司。例如，在我们以Deliveroo为例的情况中，可能是推荐次数；而对于像Netflix这样的公司，可能是观看视频的数量。对于大多数公司来说，这些属性将是货币性的（即基于收入的）。

首先，我们需要更好地理解这些数值属性。我们可以利用描述性统计和图表来实现这一目标。

+   **描述性统计：** 总和、均值、中位数、众数、四分位距（IQR）、标准差

+   **可视化：** 离散属性使用条形图，连续属性使用直方图或箱形图

我们现在需要确定我们的KPI（关键绩效指标）。KPI是对业务相关者有意义的描述性统计的子集。例如，相比IQR，KPI还有更好的选择。KPI最常用的统计量是平均值（每个骑手的平均推荐次数或每个用户的平均观看视频数）。你需要根据数据的分布选择适当的统计量作为你的KPI。如果存在离群值，你可以选择95%截断均值；如果分布高度偏斜，你可以选择中位数。

重要的是要让业务相关者了解这些决策。解释由于极端离群值的存在，均值为$10000，因此选择95%截断均值，其更具代表性的值为$120，这样可以展示a) 你已经识别出了离群值，b) 你知道如何处理这些离群值！

## 3.2 添加维度

我们的分析仍然过于高层次。我们需要将分类属性纳入分析中。你可以把它们看作是我们KPI的维度。以Deliveroo为例，我们可以按城市或月份拆分每个骑手的中位推荐次数。日期是最常用的维度列之一。

一些分类属性可能会有许多稀有类别（类别是分类属性的唯一值，例如“男孩”和“女孩”）。这些稀有类别可以被合并或重新分配。我们还可以以类似的方式对数值属性进行分组，以创建新的分类维度。例如，通过使用分位数，我们可以将收入分成四个区间（或桶）。但尽量避免过度复杂化分配；除非有充分的理由，否则应避免对数值属性进行分箱。

现在我们可以通过按分类属性或日期拆分我们的关键绩效指标来开始进行比较：

+   我们的关键绩效指标（KPI）在一个城市的表现是否优于另一个城市？

+   总收入与前几个月相比是下降还是上升？

+   我们还可以结合维度。如果收入同比下降了15%，这是因为一个或两个城市的表现更差，还是所有城市的降幅几乎都达到15%？

## 3.3 根本原因分析

根本原因分析旨在识别导致问题的潜在因素。让我们通过一个例子来看这如何运作。假设一个任务中**总收入**相比上个月下降了25%。你将**城市**作为维度，观察到差异几乎完全来自纽约和伦敦。其他城市的**总收入**月月保持不变。首先，我们将把总收入分解为两个组成部分。

**总收入 = 总订单数 * 平均订单价值**

从（虚构的）数据来看，两个城市的**总订单数**都增加了9%，但**平均订单价值**却下降了38%！好吧，现在我们有了一些进展。我们还可以检查用户经历的漏斗。例如，如果订单下降了，我们可以先检查一下有多少访客，多少人将产品添加到购物篮中，多少人进入了结账页面，最后有多少人确认了订单。

当然，上述内容依赖于我们可以获取的数据量。但我们只能使用现有的数据，因此如果由于数据限制导致上述步骤无法进行，请不要担心。但请确保在分析建议和下一步计划时将其作为潜在数据源添加进去。

# 4. 解释阶段：构建你的故事

![](../Images/c15080b08f254553d276dc0c4c2578db.png)

图片由 [RetroSupply](https://unsplash.com/@retrosupply?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/jLwVAUtLOAQ?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

这是你的作业的最终部分，也可能是最重要的部分。你的分析是评估的关键部分，但同样重要的是你的展示的简洁性和清晰度（利益相关者只有在理解你的观点时才能欣赏它们）。你需要遵循三个关键规则：

+   你的演示文稿应该简洁。不要展示你打开的每一只牡蛎，而只展示珍珠

+   在你的幻灯片中使用视觉效果和摘要，使洞察更易于理解。任何不促进这一方向的元素应当被移除或更改

+   在分析的基础上给出明确的建议和下一步计划

你也可以在下面找到有关如何创建视觉效果和结构化演示文稿的更详细文章。

[## 公共演讲是否是数据科学家的致命弱点？](/is-public-speaking-the-cryptonite-of-data-scientists-bb7dac5925d6?source=post_page-----f06878d71d26--------------------------------)

### 帮助我提升演讲技巧的三个关键经验

[towardsdatascience.com](/is-public-speaking-the-cryptonite-of-data-scientists-bb7dac5925d6?source=post_page-----f06878d71d26--------------------------------)

# 总结

🚀🚀 通过最后一步，我们已经完成了指南。以下你还可以找到步骤的简要总结：

✅ 仔细阅读描述并理解任务和目标

✅ 在开始分析之前对数据进行质量检查

✅ 创建关键绩效指标，并按维度拆分以精准定位问题

✅ 进行根本原因分析，以识别可能解释问题的因素

✅ 仅传达分析中的关键点。抵制展示所有开过的牡蛎的冲动

我想强调的最后一点是根据家庭作业的质量对公司进行自我评估的重要性。描述是否清晰？数据集是否具有一定挑战性？在问题和数据集上投入了足够的努力，还是看起来像是草率的工作？

如果你有任何问题或需要进一步的帮助，请随时在下方评论，我会及时回答。

# **保持联系！**

如果你喜欢阅读这篇文章并想了解更多，请别忘了[**订阅**](https://medium.com/@alex.vamvakaris.ds/subscribe)，以便直接将我的故事发送到你的邮箱。

在下面的链接中，你还可以找到一个关于在实际商业场景中使用数据科学技术和最佳实践完成客户群体分析的免费PDF指南。*👇*

[## 数据科学项目检查表 - 充满抱负的数据科学家](https://www.aspiringdatascientist.net/community?source=post_page-----f06878d71d26--------------------------------)

### 我是一名拥有7年以上分析经验的数据科学家，目前在英国伦敦的一家游戏公司工作。我的……

[www.aspiringdatascientist.net](https://www.aspiringdatascientist.net/community?source=post_page-----f06878d71d26--------------------------------)
