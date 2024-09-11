# 什么是ESG报告？

> 原文：[https://towardsdatascience.com/what-is-esg-reporting-d610535eed9c?source=collection_archive---------4-----------------------#2023-08-22](https://towardsdatascience.com/what-is-esg-reporting-d610535eed9c?source=collection_archive---------4-----------------------#2023-08-22)

## 利用数据科学实现公司环境、社会和治理（ESG）报告的全面与有效

[](https://s-saci95.medium.com/?source=post_page-----d610535eed9c--------------------------------)[![Samir Saci](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----d610535eed9c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d610535eed9c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d610535eed9c--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----d610535eed9c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-esg-reporting-d610535eed9c&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----d610535eed9c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d610535eed9c--------------------------------) · 13分钟阅读 · 2023年8月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd610535eed9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-esg-reporting-d610535eed9c&user=Samir+Saci&userId=bb0f26d52754&source=-----d610535eed9c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd610535eed9c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-esg-reporting-d610535eed9c&source=-----d610535eed9c---------------------bookmark_footer-----------)![](../Images/c240dd731b21f98707f07ae46b82e875.png)

什么是ESG报告——（作者提供的图像）

环境、社会和治理（ESG）报告是一种公司披露其治理结构、社会影响和环境足迹的方法。

> 作为数据科学家，你如何通过分析支持组织提升其ESG评分？

随着利益相关者对公司社会责任（CSR）的要求不断增加，ESG报告已成为公司长期战略中的关键部分。

![](../Images/01a3148beeb5faa37b43e60a3d683c7e.png)

ESG支柱展示——（作者提供的图像）

在本文中，我们将深入探讨ESG报告的细节，以突出其相关挑战，并探讨**数据分析如何提高其准确性**。

📘 您的供应链分析完整指南：[分析备忘单](https://www.notion.so/Supply-Chain-Analytics-Cheat-Sheet-d449e3d53cfc45978aa889d3ef40f559)

💌 免费直接发送到您的收件箱的新文章：[通讯](https://www.samirsaci.com/#/portal/signup)

```py
Summary
I. Understanding ESG Reporting
**1\. What is ESG Reporting?
2\. ESG Reporting supported with Data**
II. Advanced Analytics for ESG Reporting
**1\. Lack of Standardization
2\. Accuracy and Reliability of ESG Data
3\. Fighting Greenwashing with Data Science**
III. Data Science as a Game Changer
**1\. Sustainable Sourcing
2\. ESG-Friendly Budget Planning
3\. Supply Chain Network Optimization
4\. Circular Economy for Fashion Industry**
IV. Conclusion
Open the window on Business Intelligence and Sustainable Development Goals
**1\. Business Intelligence to Automate the Process
2\. Beyond ESG, Towards Sustainable Development Goals (SDGs)**
```

# 利用数据分析自动化ESG报告

## 什么是ESG报告？

ESG报告是一种非财务报告形式，组织向利益相关者传达其环境表现（E）、社会责任（S）和治理结构的**实力（G）**。

这**三个维度**提供了对公司可持续性和伦理影响的深入了解。

[![](../Images/5859db8847abb7a205851d04689f0e19.png)](http://samirsaci.com)

报告类别示例 — (图像由作者提供)

例如，一家公司可能会报告

+   其供应链的碳排放**（E）**

+   社区发展倡议**（S）**

+   董事会成员的多样性**（G）**

我们从数据分析的角度来看看这份报告。

> 你是一家时尚零售公司的数据科学家。

## 数据支持的ESG报告

我们可以考虑一个假设的全球时尚零售商：**I&N**。

**I&N**是一家快时尚零售商**，在亚洲的工厂生产服装、包袋**和配饰。

[![](../Images/e6341efa24c2be36b07aec8c1ba0d851.png)](http://samirsaci.com)

I&N的供应链网络 — (图像由作者提供)

**（位于欧洲的）商店**从**直接补给的本地仓库**发货。

I&N致力于可持续实践*（循环经济、可再生能源）*，并通过透明度建立与利益相关者的信任。

因此，它定期在年度可持续发展报告中披露其**ESG表现**。

在其最新报告中，**I&N**披露了几个关键的ESG指标。

![](../Images/90c538578dba5ba0abb9bfa51c6bd6ff.png)

I&N ESG指标 — (图像由作者提供)

**（E）：对于环境部分，I&N报告**

+   总温室气体排放量**（kg CO2eq）**

+   可再生能源的使用百分比**（%）**

这些指标，**需要先进的数据处理**，使利益相关者能够理解

+   销售产品的环境足迹。

+   向更清洁能源来源过渡的努力。

> 如何衡量这些环境指标？

**产品生命周期评估（LCA）**是一种数据驱动的方法，用于评估产品视角下的环境影响。

这个想法是分析每个过程，从原材料提取到产品处置。

![](../Images/eaf784f73bb30b100d19929d8099f5ce.png)

生命周期评估 — (图像由作者提供)

对每个过程，我们进行审视

+   自然资源、原材料和能源的消耗

+   污染物和CO2的排放

+   产生的废物

💡 欲了解更多关于这些分析解决方案的详情，

[](/what-is-a-life-cycle-assessment-lca-e32a5078483a?source=post_page-----d610535eed9c--------------------------------) [## 什么是生命周期评估？LCA

### 了解生命周期评估如何帮助企业评估产品在其整个生命周期中的环境影响……

[towardsdatascience.com](/what-is-a-life-cycle-assessment-lca-e32a5078483a?source=post_page-----d610535eed9c--------------------------------) [](/supply-chain-sustainability-reporting-with-python-161c1f63f267?source=post_page-----d610535eed9c--------------------------------) [## 使用 Python 进行供应链可持续性报告

### 4 个步骤来构建有关分销网络 CO2 排放的 ESG 报告。了解如何测量和减少您的碳排放……

[towardsdatascience.com](/supply-chain-sustainability-reporting-with-python-161c1f63f267?source=post_page-----d610535eed9c--------------------------------)

> 社会评分怎么样？

**（S）：对于社会组件，公司详细说明**

+   **社区发展计划** 的数量

+   平均员工满意度评分指示了员工的福祉。

> 我们公司如何披露平均员工满意度评分？

组织传统上依赖调查来获取这个指标，这通常会产生主观和偏见的结果。

因此，**I&N 决定使用** [自然语言处理（NLP）和社会情感分析](https://www.weekly10.com/10pulse-sentiment-analysis-how-to-measure-employee-sentiment/) 来分析来自 Glassdoor 或内部沟通渠道的员工评论文本数据。

![](../Images/380690f8dee855d7fa46a9bd9a182191.png)

社交媒体情感分析 — （作者提供的图片）

它也可以用于社交媒体上。

**ESG 情感分析** 是一种有价值的工具，投资者用来追踪利益相关者对 ESG 问题的态度，并了解这些因素如何影响公司的股票价格。

市场上有 [工具](https://www.sustainalytics.com/) 可以对社交媒体和招聘平台进行审计。

他们使用先进的 NLP 技术来获得客户和员工对关键公司话题的看法。

**（G）：对于治理领域，I&N 披露**

+   独立董事的数量

+   董事会中的女性代表百分比。

这帮助审计员和投资者评估**I&N**对公平和负责任治理的承诺。

董事会组成分析是对**董事会成员和管理层的多样性和经验**的基于数据的评估。

这可以通过分析与被认为是**公司战略关键**的选定经理相关的数据来完成。

![](../Images/41d6970a4c587137a8132a2c7dc5ecfe.png)

董事会组成分析的虚拟数据示例 — （作者提供的图片）

例如，可以构建可视化图表来分析**劳动力多样性**，通过种族分布来进行。

![](../Images/17006a2ea08d8a62454bafa21333929c.png)

种族分布示例 — （作者提供的图片）

如果 I&N 想要促进**性别平等**，我们可以分析男性和女性管理者的部门分布。

![](../Images/c68dbfbe34c28e24cfa1732e366542ad.png)

性别分布示例 — （图像来源作者）

这些可视化图表帮助识别潜在的改进领域，并有力支持**多样性和包容性**，这是良好治理的关键方面。

在下一部分中，我们将看到高级分析如何帮助公司克服 ESG 报告的挑战。

# 高级分析用于 ESG 报告

ESG 报告可能很复杂，因为报告实践缺乏**标准化**，而且确保**数据准确性**存在困难。

> 你如何支持这一工作？

## 标准化缺乏

首先是缺乏标准化报告框架，这可能导致不同公司在报告其 ESG 表现时出现**不一致**。

例如，**两家公司可能采用**完全不同的方法来衡量它们的环境影响

+   **公司 1** 是一家塑料玩具制造商

+   **公司 2** 在便利店销售新鲜水果

这两家公司报告**减少塑料使用**

+   通过对某些物品使用纸箱包装，公司 2 减少了-55%

+   通过改变玩具的设计，公司 1 减少了-10%

> 当第一家公司使用塑料作为其产品的原材料时，你能评估其努力和影响吗？

不，我们需要标准化。

**💡 数据分析如何支持标准化？**

+   政府实体可以使用按行业划分的公司数据库以及其产品的环境方面 *(例如：* [*世界银行数据库*](https://datatopics.worldbank.org/esg/)*)*

+   自动化数据管道可以提取、处理并部署标准化报告，使用来自不同来源的数据。

> 我们如何确保数据的可靠性？

## ESG 数据的准确性和可靠性

维护质量可能很艰难，因为数据来自多个来源。

商业智能（BI）提供处理和分析来自不同系统的大量数据的能力，以支持 ESG 报告。

+   来自外部供应商、公共事业账单或运营文件的**平面文件**

+   来自**工厂管理系统**的制造数据

+   来自**ERP**、**WMS** 和 **TMS** 的物流和零售操作数据

[![](../Images/c29267ced2dcfb27dca79f7105bdeca8.png)](https://towardsdatascience.com/what-is-a-life-cycle-assessment-lca-e32a5078483a)

生命周期评估所需的分析能力 — （图像来源作者）

在上面的例子中，这种数据架构用于提取、处理和存储数据，以进行**生命周期评估**。

这个想法是估算销售商品的影响

+   将生产输出数量与能源和资源使用联系起来

+   估算从生产到运输的 CO2 和污染物排放

+   包括来自供应商和物流操作的额外非财务指标

终极目标是自动化计算 ESG 指标，从原材料提取到处置的产品生命周期。

**💡 额外见解** 这也可以支持报告中数据的可追溯性。

例如，我实施了一个系统，使用 [文件哈希](https://www.cisa.gov/sites/default/files/FactSheets/NCCIC%20ICS_Factsheet_File_Hashing_S508C.pdf) 证明数据源 *(来自货运代理)* 在 CO2 排放计算过程中未被修改。

由于可能会进行审计，展示数据来源并证明结果尚未被篡改是很重要的。

> 对于那些进行欺诈的公司怎么办？

## 用数据科学对抗绿色洗涤

绿色洗涤是指对产品的环境效益做出误导性声明，以传达虚假的可持续形象。

[![](../Images/02f202f16478a122bf16dee1fcee9ac8.png)](https://medium.com/towards-data-science/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031)

绿色洗涤的五大罪行 — （作者图片）

组织使用这种不诚实的做法来创造对环境责任的虚假印象。

然而，数据分析可以显著提升通过使用

+   **公开的数据** 在可持续发展报告、社交媒体中

+   **高级分析模型，** 包括 NLP、预测或统计模型用于欺诈检测

**💡 更多关于数据分析的详细信息，**

[](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----d610535eed9c--------------------------------) [## 什么是绿色洗涤？以及如何使用分析检测它？

### 探索数据分析如何帮助我们检测和防止绿色洗涤，并促进真正的可持续性。

[towardsdatascience.com](/what-is-greenwashing-and-how-to-use-analytics-to-detect-it-15b8118031?source=post_page-----d610535eed9c--------------------------------)

我们可以通过分析实现调和和检测欺诈。

> 我们能支持公司的转型吗？

# 数据科学作为游戏规则改变者

除了测量和报告，这些技术还可以帮助你的组织利用系统生成的数据

+   **指示性见解** 支持决策：选择供应商、预算分配、供应链网络设计

+   **预测分析** 帮助公司预测和减轻未来的 ESG 风险

> 你听说过线性规划吗？

## **示例 1: 可持续采购**

这是在选择产品或服务供应商时整合社会、伦理和环境绩效因素的过程。

[![](../Images/d12b76f004abc5a031f83cd56a9b9812.png)](https://towardsdatascience.com/data-science-for-sustainable-sourcing-a72f2c4db424)

使用数据评估供应商 — （作者图片）

对于每个供应商，**I&N** 有一套衡量的分数

+   自然资源的使用（水、棉花）

+   污染物和 CO2 排放

+   社会和治理合规

利用先进的分析技术，你可以自动化整个过程

[![](../Images/244c82347a98fe58cdc5457c666ea531.png)](https://towardsdatascience.com/data-science-for-sustainable-sourcing-a72f2c4db424)

通过三个步骤选择供应商 — （作者提供的图像）

+   **收集供应商数据**

    *例如：可持续性KPI（CO2、自然资源、环境足迹）、社会和治理指标*

+   **供应商评估** 基于 **ESG** 和 **业务指标** *例如：固定和变动成本、质量、社会责任*

+   决策 **使用线性规划**

    *例如：决定一组供应商，以最小化利润，同时尊重ESG评分的最低水平*

这是一场真正的游戏规则改变者，因为它可以帮助采购团队 **使其采购策略与公司的ESG路线图对齐**。

**💡 关于可持续采购分析的更多细节，**

[](/data-science-for-sustainable-sourcing-a72f2c4db424?source=post_page-----d610535eed9c--------------------------------) [## 可持续采购的数据科学

### 如何使用数据科学来选择最佳供应商，考虑可持续性和社会指标...

towardsdatascience.com](/data-science-for-sustainable-sourcing-a72f2c4db424?source=post_page-----d610535eed9c--------------------------------)

> 使用线性规划来帮助决策。

## **示例2：ESG友好的预算规划**

线性规划还可以帮助你 **指导你的投资**，选择支持公司ESG路线图的项目。

让我们设想一下国际物流公司的预算分配场景。

一位区域总监收到 **17位仓库经理** 的预算申请，这些项目将影响未来三年。

[![](../Images/66c98e0399b36297fce4624527bc82e5.png)](https://towardsdatascience.com/automate-budget-planning-using-linear-programming-5254aace697c)

物流公司预算规划的示例 — （作者提供的图像）

对于每个预算申请，经理包括

+   项目描述（设备采购、翻新等）

+   未来三年的年度预算

+   **投资回报率** = （成本减少 + 额外收入）— （总成本）

+   影响业务发展、生产力或 **ESG指标** 的附加收益

我们的主管必须根据财务方面（投资回报率）和ESG标准来决定将预算分配给哪个项目。

> 如何在满足ESG要求的同时最大化投资回报率？

通过线性规划，我们可以 **自动化** 选择那些在尊重CSR、HSE或可持续性约束的情况下最大化投资回报率的项目。

[![](../Images/6f8bb70f3428f052461455f44a7965c6.png)](https://towardsdatascience.com/automate-budget-planning-using-linear-programming-5254aace697c)

决策过程 — （作者提供的图像）

+   **参数：** 每个项目的布尔值（1：已选择，0：未选择）

+   **约束条件：** 业务发展和ESG收益

+   **目标函数：** 最大化投资回报率

通过设置高层管理规定的ESG目标，我们的主管可以确保所选项目将支持公司的长期战略。

**💡 关于ESG友好型预算规划的更多细节，**

[](/automate-budget-planning-using-linear-programming-5254aace697c?source=post_page-----d610535eed9c--------------------------------) [## 使用线性规划自动化预算规划

### 选择那些最大化投资回报的项目，同时遵循管理指南并尊重预算…

[towardsdatascience.com](/automate-budget-planning-using-linear-programming-5254aace697c?source=post_page-----d610535eed9c--------------------------------)

> 你需要一个供应链优化应用程序吗？

## **示例3：供应链网络优化**

提升您的ESG评分的一个好方法是推动您的绿色和伦理转型。

可持续供应链优化是一种结合了成本效益与可持续性的激动人心的方法。

[![](../Images/3fa5664eca2b5bf7d1507c50cfe3b4a1.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

可持续供应链优化问题——（图片由作者提供）

您可以，

1.  每个市场位置的需求（单位/月）

1.  一组具有不同生产**成本**、**环境**影响、**社会**和**治理**评分的潜在制造地点

1.  每单位的环境足迹限制、社会和治理评分

> 最可持续（且经济上可行）的组合是什么？

通过先进的分析工具，您可以设计一个工具来测试多个场景

+   如果我想最小化成本怎么办？

    *我能达到我的ESG目标吗？*

+   如果我想最小化CO2排放怎么办？

    *我能保持盈利水平吗？*

[![](../Images/6444e081102ba8fca02ae64a265ef724.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

比较不同场景——（图片由作者提供）

我已经在网上部署的网页应用程序中实现了这样的模型：

1.  上传您的市场需求数据**（单位）每市场**

1.  添加您的制造足迹数据：按位置划分的工厂，包括*(成本、CO2排放、资源使用和社会评分)*

1.  选择目标函数：最小化成本、CO2 排放或资源使用

[![](../Images/9c907bd88be136336901e7d2363dc75b.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

三种具有不同目标函数的场景——（图片由作者提供）

您可以快速从一个目标切换到另一个目标，以决定最可行的解决方案。

**💡 如果您想尝试这个工具，我已分享一个可在线访问的POC**

[![](../Images/c35cd43320c156879fac2b6bceacf154.png)](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

[可持续供应链优化应用程序](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)——（图片由作者提供）

> 您听说过循环经济吗？重复使用或租赁以代替浪费。

## 示例4：循环经济的模拟

**循环经济**是一种旨在减少废物和最大化资源效率的经济模式。

[![](../Images/08229cf6d8fc301045f7f94b2f66cc62.png)](https://towardsdatascience.com/data-science-for-sustainability-simulate-a-circular-economy-b6a13d4b0451)

循环经济的订阅模式 — （作者提供的图像）

一些公司实施了**订阅模式**，顾客支付定期费用，以在**特定时间**内访问产品或服务。

一位顾客希望租用一件连衣裙2周。

+   连衣裙可以在商店取货。

+   该物品使用两周。

+   顾客归还物品，物品随后被收集。

+   在收集后，物品会被检查和清洁，然后再送回商店。

[![](../Images/92184ed57ffe8329df505a6d8e05f3b4.png)](https://towardsdatascience.com/data-science-for-sustainability-simulate-a-circular-economy-b6a13d4b0451)

案例研究的参数 — （作者提供的图像）

因此，一件制造过一次的连衣裙可以被多个顾客使用。

> 使用此模型我们可以减少多少CO2排放？

我开发了一个基于销售数据的模拟模型，估算了不同租赁周期下的CO2节省量。

[![](../Images/5261e6c25e2fa0b0ef96657f957c4753.png)](https://towardsdatascience.com/data-science-for-sustainability-simulate-a-circular-economy-b6a13d4b0451)

研究结果 — （作者提供的图像）

结果令人震惊，

+   短期租赁的减排率为75%

+   长期租赁周期会影响网络的效率。

**💡 欲了解更多关于本研究的详细信息**

[](/data-science-for-sustainability-simulate-a-circular-economy-b6a13d4b0451?source=post_page-----d610535eed9c--------------------------------) [## 可持续发展的数据科学 — 模拟循环经济

### 使用数据科学模拟循环模式对快速时尚的CO2排放和水使用的影响…

towardsdatascience.com](/data-science-for-sustainability-simulate-a-circular-economy-b6a13d4b0451?source=post_page-----d610535eed9c--------------------------------)

这些示例让你了解了数据分析如何帮助你改善ESG报告，并实现高层管理设定的目标。

# 结论

随着ESG报告的普及，数据分析在提高其准确性和效率方面的作用预计将不断增长。

未来可能会开发专门针对ESG报告的高级解决方案。

> 如何自动化数据收集和处理？

## 商业智能自动化过程

商业智能是一种利用软件和服务将数据转化为可操作的智能，支持决策制定的过程。

![](../Images/fe7b0e11f5173e904f2f4b0909e52b2c.png)

商业智能的5个步骤 — （作者提供的图像）

这些解决方案可以自动化ESG报告过程，从数据收集到分析和决策。

**💡 欲了解更多信息**

+   [什么是商业智能？](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------), [Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------), [Towards Data Science](/what-is-business-intelligence-bf1de730319c)

[## 什么是商业智能？](/what-is-business-intelligence-bf1de730319c?source=post_page-----d610535eed9c--------------------------------)

### 探索应用于供应链优化的数据驱动决策工具。

[towardsdatascience.com](/what-is-business-intelligence-bf1de730319c?source=post_page-----d610535eed9c--------------------------------)

通过利用这些工具，公司可以改善其ESG报告，并获得宝贵的见解以推动其可持续发展战略。

## 超越ESG，迈向可持续发展目标（SDGs）

可持续发展目标（SDGs）是联合国制定的**17个目标**，旨在应对全球挑战。

![](../Images/f23087e06a9232b5e364cc2c9ba26c64.png)

这17个目标可以分为5个类别 — （作者提供的图片）

将这些目标融入我们的操作框架是道德上的必要性，也是推动创新和提高效率的绝佳机会。

![](../Images/5b681be105a05a764c7ff9c91e56902a.png)

面向以人为本的先进分析工具 — （作者提供的图片）

通过数据分析，我们可以支持这些17个目标的设计和实施，从而提升你的ESG评分。

要深入了解数据分析如何支持这些目标，

[## 可持续发展目标是什么？ (SDGs)](https://s-saci95.medium.com/what-are-the-sustainable-development-goals-sdgs-988a1eb2b62b?source=post_page-----d610535eed9c--------------------------------)

### 将全球可持续发展倡议与公司供应链数字化转型通过数据科学相结合

[s-saci95.medium.com](https://s-saci95.medium.com/what-are-the-sustainable-development-goals-sdgs-988a1eb2b62b?source=post_page-----d610535eed9c--------------------------------)

# 关于我

让我们在 [Linkedin](https://www.linkedin.com/in/samir-saci/) 和 [Twitter](https://twitter.com/Samir_Saci_) 上联系。我是一名供应链工程师，利用数据分析来改善物流运营和降低成本。

如果你对数据分析和供应链感兴趣，可以查看我的网站。

[## Samir Saci | 数据科学与生产力](http://samirsaci.com/?source=post_page-----d610535eed9c--------------------------------)

### 一个专注于数据科学、个人生产力、自动化、运筹学和可持续发展的技术博客

[samirsaci.com](http://samirsaci.com/?source=post_page-----d610535eed9c--------------------------------)

💡 关注我在 Medium 上的文章，获取更多有关 🏭 供应链分析、🌳 可持续发展和 🕜 生产力的内容。

# 参考文献

+   使用线性规划自动化预算规划，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------)，[数据科学前沿](/automate-budget-planning-using-linear-programming-5254aace697c)

+   什么是生命周期评估？，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------)，[数据科学前沿](/what-is-a-life-cycle-assessment-lca-e32a5078483a)

+   使用Python进行供应链可持续性报告，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------)，[数据科学前沿](/supply-chain-sustainability-reporting-with-python-161c1f63f267)

+   可持续供应链优化应用，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------)，[访问应用程序](https://cloud.viktor.ai/public/sustainable-supply-chain-network-optimization)

+   什么是可持续采购？，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----d610535eed9c--------------------------------)，[Medium](https://s-saci95.medium.com/what-is-sustainable-sourcing-46ad1fade14f)
