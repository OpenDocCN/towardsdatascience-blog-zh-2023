# 创建一个可持续的供应链优化网络应用

> 原文：[https://towardsdatascience.com/create-a-sustainable-supply-chain-optimization-web-app-20599b98cab6?source=collection_archive---------10-----------------------#2023-06-15](https://towardsdatascience.com/create-a-sustainable-supply-chain-optimization-web-app-20599b98cab6?source=collection_archive---------10-----------------------#2023-06-15)

## 帮助你的组织结合**可持续采购**和**供应链优化**，以遏制成本和环境影响。

[](https://s-saci95.medium.com/?source=post_page-----20599b98cab6--------------------------------)[![Samir Saci](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----20599b98cab6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----20599b98cab6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----20599b98cab6--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----20599b98cab6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-a-sustainable-supply-chain-optimization-web-app-20599b98cab6&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----20599b98cab6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----20599b98cab6--------------------------------) ·10 min 阅读·2023年6月15日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F20599b98cab6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-a-sustainable-supply-chain-optimization-web-app-20599b98cab6&user=Samir+Saci&userId=bb0f26d52754&source=-----20599b98cab6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F20599b98cab6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreate-a-sustainable-supply-chain-optimization-web-app-20599b98cab6&source=-----20599b98cab6---------------------bookmark_footer-----------)![](../Images/a9bb044d191368e8201356de3d413009.png)

创建一个可持续的供应链优化网络应用 —（作者图像）

可持续供应链优化是一种网络设计方法，它将成本效益与环境责任相结合。

它突显了当企业试图将环境考虑与利润目标协调时出现的复杂性。

![](../Images/e8ac8a5b8e09ab223726cdef995d319a.png)

供应链网络设计问题：成本与二氧化碳 —（作者图像）

由于组织面临着**日益增加的减少碳足迹的压力**，因此这一主题变得越来越相关。

> 作为数据科学家，您如何帮助组织实现其可持续性目标并提高其 ESG 评分？

与传统模型优先考虑外包到低成本地区不同，现在有明显的趋势向在环境高效设施中本地化生产。

![](../Images/0ff8765be3e3cbd8fa8d983b3fd98271.png)

低成本解决方案与低碳解决方案的供应链网络设计 — （图片来源：作者）

然而，在成本效率与 CO2 排放减少之间取得平衡是一个复杂的任务，需要仔细规划和战略决策。

在本文中，我们介绍了一款**应用程序**，旨在促进数据驱动的决策制定，以实现供应链网络的可持续优化。

💌 免费获取最新文章： [Newsletter](https://www.samirsaci.com/#/portal/signup)

📘 供应链分析的完整指南：[Analytics Cheat Sheet](https://bit.ly/supply-chain-cheat)

```py
Summary
I. Introduction
II. Sustainable Supply Chain Optimization
  1\. The Challenges of Sustainability
  2\. The Support of Data Analytics
III. Overview of the Sustainable Supply Chain Optimization App
  1\. Purpose and Functionality
  2\. Initial Step: Data Input
  3\. Second Step: Data Visualization
  4\. Third Step: Selecting the Objective Function
  5\. Final Step: Visualize the Results
IV. Introduction to VIKTOR
  1\. Key Features of VIKTOR
  2\. Benefits to Supply Chain Data Scientists
V. Conclusion
```

[![](../Images/86f0288a0aa6bc896e608913a97b0b9f.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

访问应用程序进行尝试！ — [[应用程序](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)]

# 使用 Python 进行可持续供应链网络设计

这个想法是利用线性编程能力，以满足全球需求，同时最小化成本、CO2 排放和资源消耗。

[![](../Images/b2b51be83ebd001eaf2e1a920e4ac8c8.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

供应链网络设计问题 — （图片来源：作者）

在接下来的章节中，我们将深入探讨可持续供应链优化概念以及将可持续性融入战略决策的必要性。

[![](../Images/1fbeb247f03b47415682c10ee18cf4b6.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

问题介绍 [[应用程序用户指南](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)] — （图片来源：作者）

此外，您还将通过应用程序中包含的样本数据集的实际示例，获得应用程序的全面概述。

# 可持续供应链优化

这种网络设计方法将环境责任和供应链效率结合在一起。

> 我们能否在成本效益和可持续性之间找到平衡？

这是我在之前的文章中分享的两个概念的结合

![](../Images/440c14fd6f3dee8193f80a3464edec51.png)

供应链网络优化 x 可持续采购 — （图片来源：作者）

+   [**可持续采购**](/data-science-for-sustainable-sourcing-a72f2c4db424): 在选择供应商时整合社会和环境绩效因素

+   [**供应链优化**](/supply-chain-optimization-with-python-23ae9b28fd0b): 设计最佳网络以以最低成本匹配供应和需求

> 什么阻碍了你公司的绿色转型？

## 可持续性挑战

向可持续供应链优化过渡带来了独特的一组挑战。

[![](../Images/c6d41c522604ef5a1e22d33a2a8917f6.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

选择你想要最小化的指标 [[应用用户指南](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)]——（图片来源：作者）

核心复杂性在于将效率和成本效益与环境保护对齐。

如果你在**海外生产**，在**低劳动和生产成本**的国家

+   你最小化了**生产总成本**

+   **增加环境影响**，由于**运输**和**低效率工厂**的排放。

> 然后我们只需要在绿色设施中本地化！

如果你在**本地绿色设施**生产

+   你**增加了成本**，因为**高劳动成本**和**绿色设备的资本支出**，这些设备减少了CO2排放和资源使用。

+   通过减少运输和使用高端制造设施，你**降低了环境足迹**。

该应用将提供不同的场景，帮助你平衡这些不同的约束。

[![](../Images/843116a573f7fec944ad9a1d6c259fa4.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

基于环境约束的多个场景——（图片来源：作者）

> 使用数据来支持决策。

## 数据分析的支持

在之前的文章中，我们发现线性编程可以在优化工厂和配送中心之间的流动方面发挥关键作用。

这些模型可以帮助你自动化对**成本参数**（固定、可变和运输）和**足迹指标**的深入分析，以找到满足业务目标的正确平衡。

[![](../Images/03ef76491237a44f54516cee5869d676.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

样本数据集中使用的参数 [[应用用户指南](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)]——（图片来源：作者）

从算法的角度来看，你有一组外部参数

+   **需求**：每个市场的需求**（单位/月）**

+   **每个地点的生产能力**：高产能工厂**（XX单位/月）**，低产能工厂**（YY单位/月）**

+   **环境足迹**：**CO2 排放**（kgCO2eq/单位）、**资源消耗**（L/单位）或（MJ/单位）和**废物产生**（kg/单位）

+   **成本**：每个设施的固定成本**($/月)**和每单位的可变成本**($/单位)**

> 约束条件是什么？

+   **约束 1:** 生产的单位数量 ≥ 总需求

+   **约束 2:** CO2 排放 ≤ XX（kgCO2eq/月）

算法将选择一组制造地点进行开放

+   为**每个潜在地点**定义一个变量：（印度，低容量）= [0或1]

+   如果**值为1，该位置开放**，并且可以**达到其生产能力**

[![](../Images/2a63053ae7465d81aec3954c81585fe7.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

在这个示例中开启了4个位置 —（图片由作者提供）

根据**用户定义的目标指标**，模型可以提出**最优的布尔值集合**，以最小化该指标。

你可以在下面共享的视频中找到数据分析在供应链可持续性方面的其他应用，

现在，显然可持续性是至关重要的。

现在让我们来看看这个工具。

# 可持续供应链优化应用概述

## 目的和功能

主要目标是为供应链工程师提供一个互动平台，以模拟和评估不同的网络设计策略。

需要一个包含多个工作表的Excel文件作为输入，并提供多个模拟场景的结果。

> 如果没有数据，应用程序中提供了一个示例文件供测试。

你可以在这里试用

+   [可持续供应链优化应用程序](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

## **初步步骤：数据输入**

[![](../Images/15ec360a87783e009965206b3697b1c8.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

数据输入 [[用户指南](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)] —（图片由作者提供）

用户可以输入自己的数据或使用预加载的数据集，其中包含与市场需求和制造设施相关的信息。

[![](../Images/a520a3c5e0541ad844c93fb76453ff12.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

第一步：数据输入 —（图片由作者提供）

## **第二步：数据可视化**

根据上传文件中包含的数据可视化模型的不同参数。

[![](../Images/03ef76491237a44f54516cee5869d676.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

从数据集中可视化输入参数 [[应用程序用户指南](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)] —（图片由作者提供）

**🛍️ 市场需求** 在数据集中包含的位置，你有客户（或商店），其**每月需求**以*单位/月*为单位。

**🏭 市场供应** 在数据集中包含的位置，你有潜在的制造地点（低容量和高容量工厂），每个工厂的**每月最大生产量**以*单位/月*为单位。

[![](../Images/4978a28bf0dbf45a152c0f6a0ea8ca59.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

使用甜甜圈图和条形图展示市场需求和生产能力 —（图片由作者提供）

**⚡ 能源使用** 每个生产地点生产单个单位所消耗的能源（MJ/Unit）。

**🗑️ 废物生成** 每个生产地点生产单个单位的废物生成量（Kg/Unit）。

🚰 **水资源使用**

每个生产地点生产单个单位所使用的水量 *(L/Unit)*。

**🌲 CO2 排放** 每个生产地点生产单个单位的CO2排放量 *(Kg CO2eq/Unit)*。

[![](../Images/ba16c22fd13a9bce0874f2d58f514e4e.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

每个制造地点每单位生产的环境影响 — (图片来源作者)

> 我如何为我的公司收集这些数据？

有多个来源来收集这些输入参数：

+   [什么是生命周期评估？LCA](/what-is-a-life-cycle-assessment-lca-e32a5078483a): 使用数据分析评估产品从生产到处置的整个生命周期中的环境影响。

+   [使用Python进行供应链可持续性报告](/supply-chain-sustainability-reporting-with-python-161c1f63f267): 4个步骤构建一个关注分销网络CO2排放的ESG报告

该工具将帮助您决定在哪里设立工厂，以满足所有市场的需求，考虑运输、生产成本和**环境方面**。

> 你想实现什么目标？

## **第三步：选择目标函数**

这将导致明智的战略决策，提高供应链的效率和可持续性。

[![](../Images/8e28e16f9c0f04dd2b869463fd3a9c92.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

当前目标函数集合 [UI] — (图片来源作者)

用户可以从四个目标函数中选择，

**💰 生产成本**

将生产和运输产品到不同市场的整体成本最小化 *($/Unit)*

🚰 **水资源使用**

将每单位生产的水使用量最小化 *(L/Unit)*

**⚡ 能源使用**

将每单位生产的能源消耗量最小化 *(MJ/Unit)*

**🌲 CO2 排放**

将每单位生产和交付的CO2排放量最小化 *(kgCO2eq/Unit)*

应用程序会自动返回结果，

[![](../Images/88bf4fabc424ebfadc9a79f7b010a757.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

选择目标函数将触发优化 — (图片来源作者)

## 最后一步：可视化结果

[![](../Images/5f9de905eeb983b0eac61cb53d1c1010.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

按国家生产细分 — (图片来源作者)

应用程序提供了每种情况的综合结果概述，包括成本和环境影响的详细细分。

[![](../Images/aba608c4428dd6c3480489f56e955b69.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

按来源分类的水资源使用和排放 — (图片来源作者)

Sankey 图帮助你追踪货物从生产地点到各自市场的流动。

[![](../Images/27d30aa9a798d7b8d3736cb94f9b64e0.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

货物流动的 Sankey 图 [应用界面] —（作者提供的图片）

在上面的示例中，

+   巴西每月生产15,000个单位：1,450个单位用于**本地市场**，其余的**用于美国**。

+   美国、德国和日本完全依赖进口

决策者可以对他们的网络设计采取动手实践的方法。

通过使用数据驱动的处方，他们将了解每个目标指标（CO2、水、能源等）的影响。

[![](../Images/33fd0959077088bf1c1c444896f50e55.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

如果你**最小化成本**、**水资源使用**或**CO2 排放**，总预算会如何 [从左到右] —（作者提供的图片）

例如，上述视觉效果可以提出以下问题

+   **Q1:** 我们是否能承担 100% 的成本增加以最小化 CO2 排放？

+   **Q2:** 为什么最小化水资源使用比减少 CO2 排放更便宜？

+   **Q3:** 在印度寻找绿色制造地点是否会产生影响？

为了回答**问题 Q3**，我们来看看下面的视觉效果

[![](../Images/cd4b3e63ccee29a21ae49ff881de6851.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

成本最小化场景的排放量 —（作者提供的图片）

正如我们在右侧看到的，**每个市场的主要排放量**来自于从制造工厂运输**货物**。

因此，即使在印度使用最环保的设备，我们仍然会因运输而产生高排放。

如果你需要一个简短的应用指南，请查看这个教程 👇

# VIKTOR 介绍

VIKTOR 是一个直观的平台，旨在简化工程项目的创建。

它提供强大的工具，用于快速部署基于 Python 的算法。

## VIKTOR 的主要特性

VIKTOR 以其快速开发、测试和部署 Web 应用程序的能力而脱颖而出。

**我的第一个应用程序**是一个简单的工具，用于**自动化 ABC 分析**。

[![](../Images/086e3776b2752f82aab2f577528689e7.png)](https://cloud.viktor.ai/public/product-segmentation-abc-analysis)

[ABC 分析应用程序](https://cloud.viktor.ai/public/product-segmentation-abc-analysis) 截图 —（作者提供的图片）

经过几个简单的步骤，我通过一个直观的用户 Web 界面部署了我的 Python 模型，以展示帕累托图和 ABC 图表。

尝试这个应用程序，

[![](../Images/c0108a9094d56bee7c5eda3d30a0a3ac.png)](https://cloud.viktor.ai/public/product-segmentation-abc-analysis)

ABC 分析与帕累托图应用程序 — [[链接](https://cloud.viktor.ai/public/product-segmentation-abc-analysis)]

> 想要部署你的模型吗？试试 VIKTOR。

## 对供应链数据科学家的好处

VIKTOR为数据科学家和分析师提供了一个用户友好的平台，与现有工作流程良好集成，提高了生产力和效率。

对于这个应用程序，我从我在Github上分享的一个模型开始 ([链接](https://github.com/samirsaci/monte-carlo))，与供应链优化的文章相关。

![](../Images/57d14693b178b56eb4f569faea15191a.png)

供应链优化简单模型——（作者提供的图像）

在“入门指南” ([文档](https://docs.viktor.ai/docs/getting-started/)) 中，你可以找到部署一个简单应用所需的所有不同步骤。

+   采用用户输入，如Excel文件和参数选择，运行优化模型。

+   使用Plotly展示动态视觉效果

+   导出Word和PDF报告 *(ABC分析应用)*

现在，我可以轻松地与需要获得编程技能以运行Python脚本的用户分享这个模型。

欲了解更多信息，请查看官方的 [VIKTOR文档](https://docs.viktor.ai/docs/welcome/)。

# 结论

企业需要更有效地响应利益相关者和监管机构对环境、社会和治理（ESG）日益增长的需求。

这个简单的原型，通过VIKTOR部署，可以帮助推动他们**向绿色供应链的过渡**。

可以通过添加更多高级功能来轻松改进，例如，

+   **最大资源使用或二氧化碳排放的限制**

+   引入**需求的变化**以建立一个强健的网络

+   可视化**制造足迹的效率**（最大生产输出/容量）

我邀请你探索这个应用程序，利用其功能**测试使用你的数据集的不同优化场景**。

> **你听说过全球可持续未来的路线图吗？**

可持续发展目标（SDGs）是由联合国设立的**17个目标**，旨在应对全球挑战。

![](../Images/b7a4da442cc7c60a2e6e1941c12e534f.png)

17个目标可以分为5个类别——（作者提供的图像）

深入了解我的近期见解，探讨数据分析如何支持联合国的可持续发展目标

[](https://s-saci95.medium.com/what-are-the-sustainable-development-goals-sdgs-988a1eb2b62b?source=post_page-----20599b98cab6--------------------------------) [## 什么是可持续发展目标？（SDGs）

### 使用数据科学将全球可持续性倡议与公司的供应链数字化转型联系起来

[s-saci95.medium.com](https://s-saci95.medium.com/what-are-the-sustainable-development-goals-sdgs-988a1eb2b62b?source=post_page-----20599b98cab6--------------------------------)

# 关于我

让我们在 [Linkedin](https://www.linkedin.com/in/samir-saci/) 和 [Twitter](https://twitter.com/Samir_Saci_) 上联系。我是一名供应链工程师，利用数据分析改进物流操作和降低成本。

如果你对数据分析和供应链感兴趣，可以查看我的网站。

[](http://samirsaci.com/?source=post_page-----20599b98cab6--------------------------------) [## Samir Saci | 数据科学与生产力

### 一个关注数据科学、个人生产力、自动化、运筹学和可持续发展的技术博客

samirsaci.com](http://samirsaci.com/?source=post_page-----20599b98cab6--------------------------------)

# 参考资料

+   可持续供应链网络优化应用，Samir Saci，[应用](https://cloud.viktor.ai/public/production-planning-optimization)

+   产品细分与ABC分析，Samir Saci，[应用](https://cloud.viktor.ai/public/product-segmentation-abc-analysis)
