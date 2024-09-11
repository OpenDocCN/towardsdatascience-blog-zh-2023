# 什么是生命周期评估？LCA

> 原文：[https://towardsdatascience.com/what-is-a-life-cycle-assessment-lca-e32a5078483a?source=collection_archive---------6-----------------------#2023-01-18](https://towardsdatascience.com/what-is-a-life-cycle-assessment-lca-e32a5078483a?source=collection_archive---------6-----------------------#2023-01-18)

## 了解生命周期评估如何帮助企业评估产品在其整个生命周期中的环境影响并改善可持续性。

[](https://s-saci95.medium.com/?source=post_page-----e32a5078483a--------------------------------)[![萨米尔·萨奇](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----e32a5078483a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e32a5078483a--------------------------------)[![数据科学导向](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e32a5078483a--------------------------------) [萨米尔·萨奇](https://s-saci95.medium.com/?source=post_page-----e32a5078483a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-life-cycle-assessment-lca-e32a5078483a&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----e32a5078483a---------------------post_header-----------) 发布于 [数据科学导向](https://towardsdatascience.com/?source=post_page-----e32a5078483a--------------------------------) ·12 min read·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe32a5078483a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-life-cycle-assessment-lca-e32a5078483a&user=Samir+Saci&userId=bb0f26d52754&source=-----e32a5078483a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe32a5078483a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-a-life-cycle-assessment-lca-e32a5078483a&source=-----e32a5078483a---------------------bookmark_footer-----------)![](../Images/67002c210220204f2cdfcbd602ab4ca5.png)

什么是生命周期评估？—（作者提供的图片）

随着消费者环保意识的提高，企业必须优先考虑可持续性以保持竞争力。

生命周期评估（LCA）评估产品在其整个生命周期中的环境影响，从原材料提取到处置。

> 生产我的 T 恤需要多少水？

生命周期评估旨在识别和量化产品的环境影响，以便报告和支持可持续性倡议。

> 如何使用数据分析来估算你在 Zara 购买的便宜 T 恤的环境影响？

你需要来自多个来源的数据，以追踪产品在价值链中的影响直到其生命周期结束。

[![](../Images/6897d5e84d93ab54f76c51cb01cac837.png)](http://samirsaci.com)

资源使用和价值链中的CO2排放 ——（图像作者提供）

本文展示了数据分析如何通过从多个系统中提取和处理数据来支持**生命周期评估（LCA）**，以进行诊断和模拟场景。

💌 免费订阅最新文章：[通讯](https://www.samirsaci.com/#/portal/signup)

📘 供应链分析的完整指南：[分析备忘单](https://www.notion.so/Supply-Chain-Analytics-Cheat-Sheet-d449e3d53cfc45978aa889d3ef40f559)

```py
**I. Assessment in four steps**
**1\. Goal and scope definition**
Define the goals and scope of this assessment.
**2\. Inventory Analysis**
Gather data on the materials, energy, and other resources used 
**3\. Interpretation and evaluation**
Assessing the overall environmental performance of the product
**II. Life Cycle Assessment for Advanced Reporting**
**1\. ESG reporting**
Report the Environmental, Social and Governance of your company
**2\. Data Analytics to Fight GreenWashing**
Detect false claims of sustainability using public data
**3\. Business Intelligence for LCA**
Methodologies to collect and process from multiple sources
**III. Next Steps**
**1\. Simulate different scenarios with a Digital Twin**
What is the impact of localizing your production on your LCA?
**2\. Conduct Further Analysis with Brightway2**
Open source librairies with publicly available databases
**3\. Sustainable Supply Chain App**
Design the optimal manufacturing facilities to minimize CO2 emissions
```

# 四个步骤的评估

我们假设你是快时尚零售公司中的数据科学家，与你的可持续发展部门合作以实施自动化的可持续性报告。

> 我们希望通过生命周期评估实现什么？

## 第一步：目标和范围定义

要启动我们的生命周期评估（LCA），我们需要定义评估的目标和范围。

> 在快时尚零售公司中生产和销售一件T恤的环境影响是什么？

这包括识别正在研究的产品、关注的环境影响以及功能单位*（用于比较不同产品或服务的测量单位）*。

![](../Images/1fb2a360b2703367ccc40e4c642cdb5f.png)

生命周期评估的范围包括时间框架、生命周期阶段、地理边界和功能单位 ——（图像作者提供）

1.  确定被评估的产品及其预期用途

    *例如：包括所有尺码的常规版T恤*

1.  确定产品生命周期中要纳入评估的特定阶段：从原材料提取到处置。

1.  确定评估的地理范围：生产设施的位置、运输路线和最终处置地。

    *原材料提取和生产地点为印度*

    *储存、商店配送和欧洲的处置*

1.  确定评估的时间框架：评估期的开始和结束日期。

    *从2021年1月1日到2022年1月1日*

1.  定义功能单位，这是比较不同产品环境影响的标准度量。

    *销售单位：一件T恤*

确保所有相关利益相关者对评估范围达成一致。

这确保生命周期评估结果有意义且具有代表性，不会受到挑战。

[![](../Images/ceda4ef8340007e73c22028bcf0c6bc0.png)](http://samirsaci.com)

从系统中提取和处理数据以填充LCA报告 ——（图像作者提供）

**该产品将在你的不同系统中与一个（或多个）SKU代码关联（**[**ERP**](https://youtube.com/shorts/v0_R8P6MLQ0?feature=share)**、**[**WMS**](https://youtube.com/shorts/MW1QRJs3iuE?si=8vdTuQQ12YuTS0ku)**、TMS）**

+   *您可能会有多个代码用于不同尺码和颜色的T恤。*

+   *代码在不同系统之间可能有所不同（SKU生产代码、销售包装中的SKU等）：确保您拥有一致的* [*主数据*](https://youtu.be/StIshGrxuMc) *以跟踪价值链中的物品。*

> 从原材料到成品，您的价值链涉及仓库、工厂和货运操作。

**对于每个生命周期阶段，您可能会有不同的数据源/系统**

+   [*生产数据*](https://youtu.be/130AKb2DejM) *可以在您的生产管理系统中找到，或者在您的* [*企业资源规划*](https://youtube.com/shorts/v0_R8P6MLQ0?feature=share) *系统中找到*

+   [*运输*](https://youtu.be/PYkN24PMKd8) *将由运输管理系统进行管理*

+   [*仓库操作*](https://youtu.be/XejgbF2m_8g) *数据存储在仓库管理系统中*

+   *销售（及售后）数据可以从您的销售点（POS）和客户关系管理（CRM）系统中提取*

> 原材料在印度采购，用于在孟加拉国制造并在欧洲零售。

**地理边界可以由源系统和交易信息来定义**

+   *原材料采购信息可以在* [*采购订单*](https://youtu.be/EY9yt0BTr2M) *中找到，该订单是在* [*ERP*](https://youtube.com/shorts/v0_R8P6MLQ0?feature=share) *中创建的*

+   [*生产*](https://youtu.be/130AKb2DejM) *地点信息在PMS中（工厂、生产线）* [*运输*](https://youtu.be/lhDBTlsGDVc) *地点在您的TMS中被追踪（起点、目的地、跨越的国家）*

**确定时间范围可能具有挑战性，并会影响您的评估结果**

+   **我们是否在讨论时间范围内销售的所有产品？**

    *如果是，这是否意味着我们必须查看* [*生产*](https://youtu.be/130AKb2DejM) *、运输和仓储活动，这些活动发生在开始日期之前？*

+   **或者我们查看时间范围内的活动？** *如果是，我们来看看* [*生产*](https://youtu.be/130AKb2DejM) *和* [*运输*](https://youtu.be/PYkN24PMKd8) *在结束日期后销售的产品的影响。*

请注意，范围可以根据您的需求和期望的举措来进行更详细或更简单的定义，以便从结果中受益。

> 对于此分析，您手头上有哪些数据？

## 第2步：库存分析

第二步是收集有关材料、能源和其他资源使用情况以及整个产品或服务生命周期中产生的废料的数据。

[![](../Images/d03302e6cc9882f317dd31a95057a66e.png)](http://samirsaci.com)

简化周期下的库存分析用于生命周期评估 —（图片由作者提供）

**原材料** 您的T恤由在印度种植和加工的100%棉花制成

[![](../Images/be06d9579c458650dc03ea4607848459.png)](http://samirsaci.com)

（表格由作者提供）

您现在可以量化每个功能单元排放的CO2以及使用的能源和水。

**💡** 原材料种植的影响

+   我们有1kg原材料 = 1功能单元。

+   棉花和消耗的公用设施的数量可以由你的供应商估算或测量。

> 原材料转化的影响是什么？

**生产** T恤在你位于印度的工厂生产。

[![](../Images/ad5786d079ba960c25c9c22bb2d63fee.png)](http://samirsaci.com)

（作者提供的表格）

**运输** 你的T恤在印度生产，通过海运和公路运输到欧洲，

[![](../Images/416216dc440a39b5fd0a730ea985d6d8.png)](http://samirsaci.com)

（作者提供的表格）

**💡** 你的物流网络的影响

+   海洋运输的能源和排放数据极其不稳定，取决于船只、年份和航运路线。你的货运代理应验证*(或提供)*这些数据。

+   运输的CO2排放可以估算其他运输方式（路线、航空和铁路）的排放，如这个[CO2排放报告方法论](https://www.samirsaci.com/supply-chain-sustainability-reporting-with-python/)的例子。

+   对于水和电力消耗、CO2排放以及产生的废物，仓库操作也可以考虑。

> T恤售出后会发生什么？

**使用和处置** 你的产品在市场的国家内使用*(使用和处置)*。

T恤在被处理前使用两年。

+   洗涤和烘干的能源消耗：**100 MJ/件T恤**

+   洗涤和烘干的水消耗：**500 L/件T恤**

+   电力生成的排放：**10 kg CO2e/件T恤**

T恤在其销售的国家被丢弃在填埋场。

+   分解排放：**5 kg CO2e/件T恤**

很好，现在我们已经覆盖了整个生命周期。

## 第3步：影响评估

现在我们已经收集了每个步骤的数据，我们可以开始评估功能单元*(1件T恤)*的环境影响。

+   空气、水和土壤

+   人体健康和生态系统健康

1.  能源消耗：***870 MJ*** *58%在* [*生产过程中*](https://youtu.be/130AKb2DejM) *消耗*

1.  温室气体排放：**46 *kg CO2e*** *大部分排放发生在* [*生产过程中*](https://youtu.be/130AKb2DejM)

1.  水消耗：**3,500 L**

    *57%在* [*生产过程中*](https://youtu.be/130AKb2DejM) *消耗*

1.  固体废物：**0.5 kg**

    *在* [*生产过程中*](https://youtu.be/130AKb2DejM) *产生的*

1.  空气污染：**0.8 g SOx** 和 **0.5 g NOx** 排放

    *运输过程中* *排放*

大多数影响在[生产](https://youtu.be/130AKb2DejM)和[运输](https://youtu.be/PYkN24PMKd8)过程中产生。

## 第4步：解释和评估

这是评估产品整体环境性能并识别**改进领域**和**潜在缓解策略**的过程。

[![](../Images/0998b14f69791d5756f330aa8189b187.png)](http://samirsaci.com)

生命周期开发评估的解释和评估的四个步骤 — （作者提供的图片）

**与行业标准的比较** 影响评估结果可以与行业标准或基准进行比较，以查看T恤在环境表现上如何与类似产品相比。

**热点识别**

影响评估结果可用于识别T恤在环境影响上最显著的“热点”。

在我们的例子中，这些热点是[生产](https://youtu.be/130AKb2DejM)和[运输](https://youtu.be/PYkN24PMKd8)中的温室气体排放和能源消耗。

💡 自动化识别的诊断分析

如果你已经实施了数据管道以收集、处理和存储库存数据，可以实现[诊断分析工具和方法](https://www.samirsaci.com/what-is-supply-chain-analytics-2/)进行自动化分析

+   管理数千个具有不同价值链的SKU

+   实施自动化规则以进行监控、警报和根本原因分析，以回答诸如

    *为什么* ***SKU 132897–98*** *的CO2排放增加了20%？*

**潜在的缓解策略**

根据识别出的热点，可以制定潜在的缓解策略，以减少T恤的环境影响。

+   使用可再生能源进行[生产](https://youtu.be/130AKb2DejM)设施的运作。

+   本地化[生产](https://youtu.be/130AKb2DejM)，以减少工厂和市场之间的运输距离。

+   与使用替代燃料或拥有环保车队的货运公司合作，以减少每公里的CO2排放

+   进行[供应链网络优化](/robust-supply-chain-network-with-monte-carlo-simulation-21ef5adb1722)研究，重点是通过选择合适的供应商和工厂位置来减少环境影响

**持续改进**

解释和评估结果应用于持续改进产品或过程的环境性能。

当然，你必须考虑环境影响与其他方面（如成本和性能）之间的权衡，以减少业务影响。

**💡 更多细节，**

[](/robust-supply-chain-network-with-monte-carlo-simulation-21ef5adb1722?source=post_page-----e32a5078483a--------------------------------) [## 使用蒙特卡洛模拟的稳健供应链网络

### 在设计供应链网络时，你是否考虑了需求的波动？

towardsdatascience.com](/robust-supply-chain-network-with-monte-carlo-simulation-21ef5adb1722?source=post_page-----e32a5078483a--------------------------------) [](/what-is-supply-chain-analytics-42f1b2df4a2?source=post_page-----e32a5078483a--------------------------------) [## 什么是供应链分析？

### 利用数据分析提高运营效率，通过数据驱动的诊断和战略决策来改进...

towardsdatascience.com](/what-is-supply-chain-analytics-42f1b2df4a2?source=post_page-----e32a5078483a--------------------------------)

现在你已经实施了数据收集和处理的过程来生成洞察，你可以开始编写可持续性报告。

# 高级报告的生命周期评估

## ESG报告的生命周期评估

环境、社会和治理（ESG）报告是一种公司用来披露其环境足迹、治理结构和社会影响的方法论。

![](../Images/6c2b4624fb28bfaf7e3dd4b126afb8a9.png)

ESG支柱展示 — （图片由作者提供）

随着利益相关者越来越要求企业社会责任（CSR），报告已成为公司长期战略中的关键部分。

> 你如何利用数据分析来支持ESG报告？

环境（E）报告的核心在于生命周期评估，因为你需要报告从原材料采购到店铺交付的产品影响。

![](../Images/442b11545eb8b8baaf851b9d86ab9bca.png)

ESG报告的三个主要支柱 — （图片由作者提供）

环境部分的指标取决于生命周期评估中包含的参数。

**💡 更多详情，请见**

[## 什么是ESG报告？](/what-is-esg-reporting-d610535eed9c?source=post_page-----e32a5078483a--------------------------------)

### 利用数据分析进行全面而有效的公司环境、社会和治理报告

[towardsdatascience.com](/what-is-esg-reporting-d610535eed9c?source=post_page-----e32a5078483a--------------------------------)

> 你听说过“绿色洗涤”吗？

## 什么是绿色洗涤，我们如何利用分析来检测它

当你试图准确报告运营的环境影响时，其他公司却在撒谎。

> 使用数据分析来挑战可持续性报告。

一些公司对产品的环境效益做出误导性声明，以传达虚假的可持续性形象。

![](../Images/22f76e81be7aea4addd88358e1afacf4.png)

绿色洗涤的5种罪恶 — （图片由作者提供）

这种修饰或隐藏虚假的做法，随着公司寻求环保消费者的关注，对组织和政府构成了挑战。

> *我们能否通过高级分析来对抗绿色洗涤？*

![](../Images/b902920a2bced75d38bb633362b96f4e.png)

用于检测环境报告中的“绿色洗涤”的高级分析 — （图片由作者提供）

我们可以将**数据科学**与**生命周期评估**方法结合起来，检测这些欺诈行为

+   **公开数据**：财务和可持续性报告、足迹数据库、社交媒体

+   **像** NLP、预测或统计模型这样的高级分析模型来检测欺诈行为

**💡 更多详情，请见**

[## 什么是绿色洗涤？以及如何利用分析来检测它？](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----e32a5078483a--------------------------------)

### 探索数据分析如何帮助我们检测和预防绿色洗涤，并促进真正的可持续性。

[towardsdatascience.com](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----e32a5078483a--------------------------------)

> 从多个系统中收集和处理数据的最佳实践是什么？

## 生命周期评估中的商业智能

商业智能是一个利用软件和服务将数据转化为支持决策的可操作情报的过程。

![](../Images/79447ab5adb6df2e376f2a258a8fa046.png)

商业智能过程五步法——（作者提供的图片）

由于您的生命周期评估依赖于从多个来源收集和处理数据，您需要一种方法来实现自动化管道。

> 如何自动化这些数据的收集和处理？

通过构建数据架构，您可以检索更新的数据和指标，而不是使用估算或平均值。

![](../Images/0412f49e46f6474e0d5cf71d71f7d79f.png)

从多个来源中心化提取数据——（作者提供的图片）

+   管理您工厂的系统可能不会提供每单位的能源、水使用或废物生成信息。

    **使用如Excel文件中的能源账单等外部来源**

+   您的供应商可以提供用于棉花种植的**Excel报告**中的能源和水使用数据，这些数据将用于您的报告

+   一些货运代理商有[API](https://www.samirsaci.com/how-to-use-an-api-without-coding/)来提取每次运输的路线信息、排放量和燃料消耗

这一架构将减少手动工作量，并通过使用最新的参数和输入数据提高报告的准确性。

> 如何使用分析解决方案创建具有统一数据的中央真实来源？

商业智能工具结合了各种应用，包括数据仓库、发现和可视化。

![](../Images/8548a113c3120d69c82459fb8e3be697.png)

结合来自多个来源的数据的数据仓库——（作者提供的图片）

BI解决方案与这些系统的交互方式：

+   处理并将获取的数据转换为单一的统一来源

+   构建用户友好的报告、图表和地图

**💡 更多细节，**

[towardsdatascience.com](/what-is-business-intelligence-bf1de730319c?source=post_page-----e32a5078483a--------------------------------) [## 什么是商业智能？

### 发现适用于供应链优化的数据驱动决策工具。

[towardsdatascience.com](/what-is-business-intelligence-bf1de730319c?source=post_page-----e32a5078483a--------------------------------)

> 报告之后，下一步是什么？

您的公司可以利用结果和见解来实施具有可持续性举措的路线图。

您可以利用数据模拟这些举措的影响；接下来的部分将介绍如何操作。

# 下一步

本文主要集中在数据分析如何支持数据收集、处理和通过诊断工具进行可视化。

> 如果我们想估算再工程解决方案的影响呢？

## 使用数字双胞胎模拟不同场景

数字双胞胎是物理对象或系统的数字化复制品。

该模型表示您的 [仓库](https://youtu.be/XejgbF2m_8g)、[运输网络](https://youtu.be/aJnrEElPvvs) 和 [生产](https://youtu.be/130AKb2DejM) 设施。

[![](../Images/1733900e702105561a16178c89cfe58f.png)](https://towardsdatascience.com/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75)

使用 Python 的数字双胞胎示例 — （图像由作者提供）

在这个数字世界中，您可以建模供应链中的每一个环节，包含成本、能源、排放和交货时间参数。

> 如果我们从英国的仓库向欧洲市场配送，会怎么样？

在头脑风暴潜在的脱碳策略时，您可以模拟它们对供应链的影响。

[![](../Images/dbc93d86befd036a8a381c70b633f519.png)](https://towardsdatascience.com/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75)

使用您的数字双胞胎模拟多个场景的示例 — （图像由作者提供）

例如，

+   如果我们将 [生产](https://youtu.be/130AKb2DejM) 本地化，对物流成本和 CO2 排放会有什么影响？

+   如果我们在印度以外使用替代（绿色）种植方法，会对您的 [库存](https://www.samirsaci.com/inventory-management-for-retail-stochastic-demand-2/) 和 [店内交货时间](https://youtu.be/ssdni_n6HDc) 产生什么影响？

+   实施一个物流网络来回收店内的二手物品成本是多少？

了解如何实施此解决方案的更多细节，

[](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----e32a5078483a--------------------------------) [## 供应链数字双胞胎是什么？

### 使用 Python 发现数字双胞胎：建模供应链网络、提升决策能力与优化运营。

towardsdatascience.com](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----e32a5078483a--------------------------------)

> 您听说过供应链优化吗？

## 可持续供应链优化应用程序

可持续供应链优化是一种将成本效益与环境责任相结合的网络设计方法。

![](../Images/3c2f2c810600fcf5b0426030ecb17e18.png)

供应链可持续性应用程序的工作流程 — （图像由作者提供）

> 最小化 CO2 排放的最佳工厂组合是什么？

假设您想设计一个供应链网络以生产和交付产品到特定市场。

您有

+   每个市场的需求（单位/月）

+   一组潜在的工厂，包括其固定/可变成本、环境影响和位置

+   从工厂到每个市场的运输成本和环境影响

> 如果我们想要最小化 CO2 排放，成本影响会是什么？

![](../Images/7eeab6cd9e40e5ea79950d03683f2419.png)

多场景模拟 — （图像由作者提供）

通过这个解决方案，你可以模拟多个场景，调整目标（例如，减少二氧化碳、成本或水使用），以获得最佳网络。

![](../Images/d2da956b963c088822e461e36173d9da.png)

使用我的网络应用程序模拟场景 — [[应用](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)]

我开发了一个网络应用程序，可以模拟绿色转型的多个场景（减少二氧化碳排放、水使用等），以估算对整体生产成本的影响。

试试吧！ 👇

[![](../Images/a8a70d9001a5c305b1546d1fb33950d3.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

访问应用程序进行尝试！ — [[应用](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)]

# 关于我

在[Linkedin](https://www.linkedin.com/in/samir-saci/)和[Twitter](https://twitter.com/Samir_Saci_)上与我联系。我是一名供应链工程师，利用数据分析来改善物流操作并降低成本。

如果你对数据分析和供应链感兴趣，请查看我的网站。

[## Samir Saci | 数据科学与生产力](https://samirsaci.com/?source=post_page-----e32a5078483a--------------------------------)

### 专注于数据科学、个人生产力、自动化、运筹学和可持续性的技术博客…

[samirsaci.com](https://samirsaci.com/?source=post_page-----e32a5078483a--------------------------------)

# 参考文献

+   “什么是供应链分析？”， [数据科学前沿](/what-is-supply-chain-analytics-42f1b2df4a2)， [Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----e32a5078483a--------------------------------)

+   “4个影响深远的项目，开启你的供应链数据科学之旅”， [数据科学前沿](/4-impacting-projects-to-start-your-data-science-for-supply-chain-journey-fe068e503c29)， [Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----e32a5078483a--------------------------------)

+   可持续物流 — 减少仓储消耗品，[个人博客](https://www.samirsaci.com/)， [Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----e32a5078483a--------------------------------)

+   “定义生命周期评估”， [全球发展研究中心](https://www.gdrc.org/uem/lca/lca-define.html)
