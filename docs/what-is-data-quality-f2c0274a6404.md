# 什么是数据质量？

> 原文：[https://towardsdatascience.com/what-is-data-quality-f2c0274a6404?source=collection_archive---------7-----------------------#2023-08-15](https://towardsdatascience.com/what-is-data-quality-f2c0274a6404?source=collection_archive---------7-----------------------#2023-08-15)

## 发现确保供应链数据准确性、一致性和完整性的方法论

[](https://s-saci95.medium.com/?source=post_page-----f2c0274a6404--------------------------------)[![Samir Saci](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----f2c0274a6404--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f2c0274a6404--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f2c0274a6404--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----f2c0274a6404--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-data-quality-f2c0274a6404&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----f2c0274a6404---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f2c0274a6404--------------------------------) · 11分钟阅读 · 2023年8月15日

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff2c0274a6404&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-data-quality-f2c0274a6404&source=-----f2c0274a6404---------------------bookmark_footer-----------)[![](../Images/7fdaab450b8d1e91d6c83e86c383151a.png)](https://www.samirsaci.com/)

什么是数据质量？ — （作者提供的图片）

数据质量定义了数据集如何被信任、理解和有效利用于其预期目的。

> 你如何管理你系统生成的数据？

![](../Images/d70959beecc35e7bd8edfeef47919d58.png)

供应链系统创建和交换信息 — （作者提供的图片）

确保数据准确、一致并适合其预期目的对于确保供应链管理的顺畅和高效至关重要。

> 你们组织中有哪些流程来确保良好的数据质量？

在这篇文章中，我们将深入探讨数据质量的概念，探索其维度并理解其在**供应链管理**中的重要性。

💌 新文章免费直接送到您的邮箱：[Newsletter](https://www.samirsaci.com/#/portal/signup)

📘 您的供应链分析完整指南：[Analytics Cheat Sheet](https://www.notion.so/Supply-Chain-Analytics-Cheat-Sheet-d449e3d53cfc45978aa889d3ef40f559)

```py
Summary
I. The Pillars of Data Management
1\. Why is it key?
2\. Quality vs. Integrity vs. Profiling
II. What are the 6 Dimensions of Data Quality?
1\. **Completeness**: do we have all the data?
2\. **Uniqueness**: are features all unique?
3\. **Validity**: is the format respecting business requirements?
4\. **Timeliness**: up-to-date data
5\. **Accuracy**: does it reflect reality correctly?
III. Next Steps
**1\. Data Quality for Environemental Reporting: ESG & Greenwashing**
**2\. Generative AI: Automate Data Quality Check with GTP Agent**
3\. Conclusion
```

# 数据管理的支柱是什么？

## 为什么数据管理至关重要？

高质量的数据可能是您供应链运营成功与失败的分水岭。

从规划和预测到采购和物流，供应链管理的每个方面都依赖数据来有效运行。

![](../Images/d70959beecc35e7bd8edfeef47919d58.png)

供应链系统创建和交换信息 — （作者提供的图片）

+   规划和预测算法依赖于[WMS](https://www.youtube.com/shorts/MW1QRJs3iuE)和ERP数据，以获取历史销售、库存水平和商店订单。

+   运输管理系统依赖WMS数据来正确跟踪从仓库到商店的货物。

> 有哪些不同类型的分析？

[供应链分析](https://youtu.be/3d7C4pShykI)解决方案可以分为四种类型，每种类型提供不同级别的洞察和可见性。

![](../Images/c0b8a63133e20d73965bd94367ac95d5.png)

四种类型的供应链分析 — （作者提供的图片）

**描述性解决方案** 通常代表您数字化转型的第一步：收集、处理和可视化数据。

一切都始于数据收集和处理，以分析您的过往表现。

> 不良数据对描述性解决方案的影响是什么？

让我们以一家时尚零售公司的门店配送过程为例。

[![](../Images/b3fd8ece95059c9bd9fc82a61275d6eb.png)](https://youtu.be/ssdni_n6HDc)

零售公司配送过程的时间戳分析 — （作者提供的图片）

一个简单的操作指标是准时交付的订单百分比：[On Time In Full: OTIF](https://youtu.be/qhLqu6M7lcA)

[![](../Images/a34653c240f639501ae35de4e84fb540.png)](https://youtu.be/qhLqu6M7lcA)

OTIF 计算的数据处理简单示例 — （作者提供的图片）

这个指标可以通过合并交易表来建立

+   用于订单创建的企业资源计划（ERP）

+   [仓库管理系统（WMS）](https://www.youtube.com/shorts/MW1QRJs3iuE)用于订单准备

+   用于订单交付的运输管理系统（TMS）

> 为什么我们在这里关注数据质量？

如果不确保数据的正确性，就不可能

+   适当地测量[准时交付的实际表现](https://youtu.be/qhLqu6M7lcA)

+   实施[自动诊断](https://youtu.be/ssdni_n6HDc)，通过比较实际时间戳与目标时间戳

一个无法自信测量的指标是无法分析和改进的。

## 质量、完整性和分析有什么区别？

在深入探讨数据质量之前，理解数据质量如何与相关概念如**数据完整性**或**数据分析**有所不同是至关重要的。

![](../Images/7465d7b3d0c0ed1bbff88d25a21a3843.png)

数据完整性是数据质量的一个子集 —（作者提供的图片）

尽管这三者是互相关联的，但它们关注的领域各有不同

+   数据完整性关注于在数据的整个生命周期内保持其**准确性**和**一致性**。

+   数据分析涉及**检查**和**清理**数据，以保持其**质量**。

数据分析不应与数据挖掘混淆。

![](../Images/a6ec98ba7c04e2b00177d1aecddcd73c.png)

数据挖掘与数据分析 —（作者提供的图片）

+   数据分析将重点关注数据的结构和质量，检查异常值、分布或缺失值。

+   数据挖掘侧重于从数据中提取业务和运营洞察，以支持决策制定或持续改进计划。

现在我们已经澄清了这些差异，我们可以专注于定义数据质量。

# 数据质量的6个维度是什么？

数据质量是基于多个维度来评估的，这些维度在保持数据的可靠性和可用性方面发挥着至关重要的作用。

[![](../Images/b71490b96b419039aa92ec7fa219258a.png)](http://samirsaci.com)

数据质量的6个维度 —（作者提供的图片）

## 数据完整性：我们是否拥有所有的数据？

目标是确保所有必要数据的存在。

缺失的数据可能导致误导性的分析和差劲的决策。

> **示例：主数据中的缺失记录**

在公司中，[主数据管理 (MDM)](https://youtu.be/StIshGrxuMc)是确保不同部门之间数据一致性和完整性的关键方面。

[![](../Images/b74e011ca55588822f58449561de38b9.png)](https://youtu.be/StIshGrxuMc)

示例主数据管理 —（作者提供的图片）

主数据专家在项目创建过程中将与产品相关的信息输入到ERP系统中。

+   **产品信息**：净重、尺寸等

+   **包装**：总重量、尺寸、语言等

+   **处理单位**：每（箱、托盘）的物品数量、托盘高度

+   **商品销售**：供应商名称、采购成本、市场定价

这些数据专家可能会犯错，主数据中可能会存在缺失的数据。

> **缺失数据可能带来哪些问题？**

+   **缺失净重**：与[运输管理](https://youtu.be/PYkN24PMKd8)相关的问题，因为我们需要重量用于**开票**和**海关清关**

+   **缺失成本**：你的采购部门无法向供应商发送[采购订单](https://youtu.be/yodNWnf7PQ0)

价值链中的许多其他问题，从原材料采购到[商店交付](https://youtu.be/1q4RqR0mgFY)。

💡 **我们如何检查它？**

+   **空值分析**：识别和统计数据集中空值或缺失值的数量

+   **领域特定检查**：确认每个预期的数据类别是否存在。

    *例如，如果一个列本应包含五个不同的类别而仅出现四个，这将表明缺乏完整性。*

+   **记录计数**：比较数据集中的记录数与预期记录数

+   **外部数据源比较**：使用已知完整的外部数据源作为基准

> 现在我们的数据完整，让我们去除重复项。

## 数据唯一性：特征是否都唯一？

目标是确保每条数据条目都是唯一的且没有重复。

我们希望准确地表示数据的全貌。

> **示例：用于 CO2 报告的运输货物**

投资者和客户对可持续发展的透明度需求增长。

因此，公司正在投资分析解决方案，以评估其环境足迹。

第一步是测量 [其运输的 CO2 排放](https://youtu.be/ddthuvFQdGY) 网络。

[![](../Images/59d2342f9b7937efc974451bb40e3ed6.png)](https://youtu.be/ddthuvFQdGY)

CO2 报告的数据处理 —（图片来源：作者）

运输和主数据记录从 ERP 系统中提取，并且从 [WMS](https://www.youtube.com/shorts/MW1QRJs3iuE) 中提取。

它们涵盖从仓库到商店或最终客户的订单范围。

> **我们可能面临哪些重复数据的问题**

如果我们有重复的运输记录，可能会高估 [运输的 CO2 排放](https://youtu.be/ddthuvFQdGY)，因为我们可能会多次计算排放。

💡 **我们如何检查它？**

+   **重复记录识别**：使用 Python 的 Pandas 或 SQL 的功能识别重复记录

+   **关键约束分析**：验证数据库中的主键是否唯一

## 有效性：格式是否符合业务要求？

目标是验证数据是否符合所需的格式和业务规则。

> **示例：生命周期评估**

[生命周期评估 (LCA)](/what-is-a-life-cycle-assessment-lca-e32a5078483a) 是一种评估产品或服务在其整个生命周期内环境影响的方法。

它严重依赖于数据质量。

在下面的示例中，我们从不同来源收集数据，以估算生产和交付 T 恤所使用的公用设施和自然资源。

[![](../Images/0cff91211adf8f9cbb96882aa8170d96.png)](https://towardsdatascience.com/what-is-a-life-cycle-assessment-lca-e32a5078483a)

时尚零售生命周期评估的数据要求 —（图片来源：作者）

+   生产管理系统提供每个时期的 T 恤数量

+   垃圾库存、公用设施和平面 Excel 文件中的排放

+   距离、路线和来自承运人 API 的 CO2 排放

最终结果，即一件 T 恤的整体环境足迹，取决于每个数据源的可靠性。

> **我们可能面临哪些无效数据的问题？**

+   如果某些记录的燃料消耗是（L/Shipment），而其他记录是（Gallons/Shipment），那么总评估将是错误的。

+   如果不确保所有的公用事业消耗是按月的，就无法评估每单位生产的消耗。

💡 **我们如何检查它？**

1.  **数据类型检查**：数据中的每个字段都是预期的数据类型

1.  **范围检查**：将值与预期范围进行比较

1.  **模式匹配**：对于像电子邮件或电话号码这样的数据，可以使用正则表达式来匹配预期的模式

## 时效性：数据是否是最新的？

目标是确保数据在预期时间范围内的准备情况。

> **示例：过程挖掘**

[过程挖掘](/what-is-process-mining-683b5eb6547c) 是一种分析方法，专注于发现、监控和改善运营和业务流程。

![](../Images/1bb416b23482d56bfe2ee76828b9328c.png)

时间戳收集——（作者提供的图像）

在上述示例中，我们在订单到交付的每一步收集带时间戳的状态*(来自不同系统)*。

> **如果我们没有及时获得数据，会面临什么问题？**

+   状态可能需要正确更新。

    这可能在追踪货物时产生“空白”。

    *例如，我的货物在凌晨12:05送达，但状态仍显示“打包中”。*

+   事件可能会被报告

💡 **我们如何检查它？**

+   **时间戳分析**：检查所有时间戳是否落在预期的时间范围内

+   **实时数据监控**：监控数据流，当出现中断时生成警报

## 准确性：是否正确反映了现实？

目标是确保数据值的正确性。

这对于维持数据驱动决策的信任是必需的。

**示例：零售销售预测的机器学习** 这些算法使用历史销售数据记录来预测未来X天的按店铺和商品代码的销售情况。

[![](../Images/11f2d7f1aef0d76ae3430df4e635df94.png)](https://s-saci95.medium.com/machine-learning-for-retail-sales-forecasting-features-engineering-4edfee7c9cbc)

零售销售预测的机器学习——（作者提供的图像）

对于这种业务案例，数据准确性比你[预测模型](https://s-saci95.medium.com/machine-learning-for-retail-sales-forecasting-features-engineering-4edfee7c9cbc)的复杂程度更为重要（基于树的，深度学习，统计预测，…）。

> **不准确数据可能面临什么问题？**

+   错误的历史销售数据由于数据输入错误或系统故障可能会影响模型的性能。

+   这可能导致库存过剩或缺货，产生财务和商业影响。

💡 **我们如何检查它？**

+   **来源验证**：与其他权威来源交叉验证数据，以确保信息准确

+   **数据审计**：定期审计数据可以通过手动检查数据记录样本来帮助发现不准确之处

## 一致性：它是否正确反映了现实？

目标是评估来自不同数据集的记录，以确保一致的趋势和行为。

💡 **如何执行？**

1.  **数据标准化**：执行严格的数据录入和格式规范以确保一致性。

1.  **自动数据清洗**：实施自动化工具或脚本来清洗和标准化数据。

1.  **错误报告**：建立一个稳健的错误报告和解决流程。

这些示例为你提供了足够的洞见，以了解如何在你的组织中实施数据质量检查。

# 下一步

如果你的公司投资于供应链数字化转型，数据质量不再是奢侈品，而是必要条件。

这应纳入你的战略路线图中，以确保足够的质量水平，以做出明智的决策，简化操作并实现商业目标。

> 如果你有低质量的数据用于你的报告，会发生什么？

## 可持续报告的数据质量

数据质量将影响你公司所有的分析和数据产品。

其中包括影响组织财务和法律方面的战略报告。

[![](../Images/fd0761a346c0cae6dbaf440c6fb2393c.png)](https://towardsdatascience.com/what-is-esg-reporting-d610535eed9c)

战略报告示例：ESG——（作者图片）

**环境、社会和治理（ESG）**方法是一种公司用来报告其环境足迹、社会影响和治理结构的方法论。

[![](../Images/e66e22d5f506a842c5dc8fed5671399e.png)](https://towardsdatascience.com/what-is-esg-reporting-d610535eed9c)

生成ESG报告的数据处理能力——（作者图片）

这依赖于从多个来源收集、处理和协调数据集。

这种报告通常涉及在发布前对数据和假设进行全面审计。

[![](../Images/a1eab80919134910e40e73e2ecd46cae.png)](https://towardsdatascience.com/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031)

绿色洗涤的五大罪恶——（作者图片）

为了打击绿色洗涤，审计员可能会进行审计

+   涵盖端到端供应链的数据来源

+   数据处理和协调

+   环境足迹和治理关键绩效指标的最终计算

> 绿色洗涤是指对产品环境效益做出误导性声明的做法。

因此，你的可持续部门依赖于高质量的数据，以避免可能导致合规问题的错误计算。

**💡 关于绿色洗涤和ESG报告的更多细节，**

[](/what-is-esg-reporting-d610535eed9c?source=post_page-----f2c0274a6404--------------------------------) [## 什么是ESG报告？

### 利用数据分析进行公司环境、社会和治理的全面有效报告

towardsdatascience.com](/what-is-esg-reporting-d610535eed9c?source=post_page-----f2c0274a6404--------------------------------) [](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----f2c0274a6404--------------------------------) [## 什么是绿色洗涤？以及如何使用分析检测它？]

### 探索数据分析如何帮助我们检测和防止绿色洗涤，并促进真正的可持续性。

towardsdatascience.com](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----f2c0274a6404--------------------------------)

> 你听说过生成性 AI 吗？

## 生成性 AI：使用 GPT 代理自动化数据质量检查。

OpenAI 于 2022 年底发布了 ChatGPT 的第一个版本。

从那时起，生成性 AI 成为利用大型语言模型（LLMs）提升用户数据和分析产品体验的机会。

[![](../Images/797d2f7a1779f9032e52c24e1567eb29.png)](https://medium.com/towards-data-science/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0)

使用 LangChain SQL 代理的供应链控制塔代理 [[文章链接](/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0)]——（图片由作者提供）

作为首次尝试探索这项技术，我在这篇[文章中分享了我的实验历程](https://medium.com/towards-data-science/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0)。

[![](../Images/ade703297df6aaa1f622b53151a04269.png)](https://medium.com/towards-data-science/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0)

由 GPT 增强的智能代理原型——（图片由作者提供）

目标是创建一个智能代理来

1.  收集以自然语言（英语）形式制定的用户请求。

1.  自动查询数据库以提取见解。

1.  用专业的语气制定合适的答案。

初步结果令人印象深刻！

> 我们能否创建一个“数据质量”智能代理？

这种方法可以应用于我们的数据质量问题。

+   我们可以将智能代理连接到不同的数据源。

+   配备先进的 Python 脚本以进行特定分析。

+   教会代理数据质量的基础知识。

**💡 了解更多关于如何使用 LangChain 创建智能代理的详细信息，**

[](/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0?source=post_page-----f2c0274a6404--------------------------------) [## 利用 LLM 和 LangChain 进行供应链分析——由 GPT 驱动的控制塔]

### 构建一个连接到运输数据库的 LangChain SQL 代理的自动化供应链控制塔……

towardsdatascience.com](/leveraging-llms-with-langchain-for-supply-chain-analytics-a-control-tower-powered-by-gpt-21e19b33b5f0?source=post_page-----f2c0274a6404--------------------------------)

# 关于我

让我们在[Linkedin](https://www.linkedin.com/in/samir-saci/)和[Twitter](https://twitter.com/Samir_Saci_)上联系，我是一名供应链工程师，利用数据分析来改善物流操作和降低成本。

如果你对数据分析和供应链感兴趣，可以看看我的网站。

[## Samir Saci | 数据科学与生产力

### 这是一个专注于数据科学、个人生产力、自动化、运筹学和可持续发展的技术博客。

[samirsaci.com](http://samirsaci.com/?source=post_page-----f2c0274a6404--------------------------------)

💡 **关注我在 Medium**，获取更多与🏭供应链分析、🌳可持续发展以及🕜生产力相关的文章。
