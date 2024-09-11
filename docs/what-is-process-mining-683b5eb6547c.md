# 什么是过程挖掘？

> 原文：[https://towardsdatascience.com/what-is-process-mining-683b5eb6547c?source=collection_archive---------9-----------------------#2023-01-04](https://towardsdatascience.com/what-is-process-mining-683b5eb6547c?source=collection_archive---------9-----------------------#2023-01-04)

[](https://s-saci95.medium.com/?source=post_page-----683b5eb6547c--------------------------------)[![Samir Saci](../Images/722d1f56a3308f6527d82b5ab97064ec.png)](https://s-saci95.medium.com/?source=post_page-----683b5eb6547c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----683b5eb6547c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----683b5eb6547c--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----683b5eb6547c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fbb0f26d52754&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-process-mining-683b5eb6547c&user=Samir+Saci&userId=bb0f26d52754&source=post_page-bb0f26d52754----683b5eb6547c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----683b5eb6547c--------------------------------) ·12分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F683b5eb6547c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-process-mining-683b5eb6547c&user=Samir+Saci&userId=bb0f26d52754&source=-----683b5eb6547c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F683b5eb6547c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhat-is-process-mining-683b5eb6547c&source=-----683b5eb6547c---------------------bookmark_footer-----------)

通过本全面指南，学习如何使用 Python 进行过程挖掘，并解锁业务数据的潜力。

![](../Images/91ce597bba1574238d02ecc04ff89b7d.png)

过程挖掘是一种数据分析类型，专注于发现、监控和改进业务流程。

过程挖掘涉及分析来自各种来源的数据，例如过程日志，以了解过程如何执行，识别瓶颈和低效之处，并建议改进方法。

[![](../Images/f6e21365943c215a55c324a6a39c074f.png)](http://samirsaci.com)

供应链信息系统在过程挖掘中的示例 — （作者提供的图片）

在之前的文章中，我分享了用于监控和可视化[物流绩效](/logistic-performance-management-using-data-analytics-82b2da978e5f)、建立自动化[供应链控制塔](/automated-supply-chain-control-tower-with-python-17dbf93a18d0)或创建[智能可视化](/4-smart-visualizations-for-supply-chain-descriptive-analytics-fda4dfc2829a)的Python工具示例。

> 作为数据科学家，你如何利用流程挖掘来优化物流操作？

本文将超越监控，探讨流程挖掘如何帮助你识别瓶颈并提高生产力。

💌 免费订阅直达你的邮箱：[通讯](https://www.samirsaci.com/#/portal/signup)

📘 供应链分析的完整指南：[分析备忘单](https://www.notion.so/Supply-Chain-Analytics-Cheat-Sheet-d449e3d53cfc45978aa889d3ef40f559)

```py
I. What is Process Mining?
1\. Understanding Process Mining
2\. How Process Mining Can Benefit Your Business Operations
II. How to Implement Process Mining with Python
1\. Real-World Examples of Process Mining with Python
2\. Approach 1: Discovery
3\. Approach 2: Conformance
4\. Approach 3: Enhancement
5\. Best Practices for Process Mining with Python
III. Conclusion
1\. You need clean data
2\. Simulate initiatives using a Digital Twin
3\. Focus on sustainability
```

# 什么是流程挖掘？

## 理解流程挖掘

流程挖掘技术可以应用于许多过程，包括制造、供应链管理、医疗保健和客户服务。

> 我们想要实现什么目标？

流程挖掘可以帮助组织理解其流程如何运作，识别改进领域，并通过分析过程数据做出优化决策。

## 平均来说，客户团队处理一个订单需要多长时间？

[![](../Images/c1e8e1d3d4d3bdf0c3c99e974db017d2.png)](https://towardsdatascience.com/statistical-sampling-for-process-improvement-using-python-9decc7b8288d)

客户服务的流程挖掘示例 — （图片由作者提供）

例如，在[这篇文章](/statistical-sampling-for-process-improvement-using-python-9decc7b8288d)中，我们探讨了一种用于估算客户服务团队订单处理时间的统计方法。

> 这对你的组织有什么好处？

## 流程挖掘如何使你的业务运营受益

流程挖掘有多种不同的方法，包括发现、符合性和改进。

+   **发现**涉及使用流程挖掘技术根据流程日志中的数据揭示流程的结构和行为。

+   **符合性**涉及将实际过程与期望的过程模型进行比较，以识别偏差和差异。

+   **改进**涉及使用流程挖掘基于数据分析建议改进措施。

让我们看看如何使用Python实现这一点。

# 如何使用Python实施流程挖掘

## 使用Python的流程挖掘的实际案例

以一个国际**服装集团**的供应链网络为例。

这家公司销售**生产**于**海外工厂**的服装、包袋和配件，以补充**区域仓库网络**。

[![](../Images/3ce9852dd3e21ef91a9121403f1d4dcd.png)](http://samirsaci.com)

时尚零售公司的供应链网络 — （图片由作者提供）

本文**将关注商店的**从**这些区域仓库的**交付。

+   配送计划员在[ERP](https://youtu.be/v0_R8P6MLQ0)中创建补货订单。

+   计划人员包括所需物品的清单（数量）和[要求的交付日期](https://youtu.be/v0_R8P6MLQ0)。

+   订单传输到[仓库管理系统](https://www.youtube.com/shorts/MW1QRJs3iuE)。

+   仓库团队在托盘中[准备订单](https://www.youtube.com/watch?v=XejgbF2m_8g)以便发货。

+   运输团队组织**仓库取货**。

+   发货被[送达](https://www.youtube.com/watch?v=PYkN24PMKd8)并在商店**接收**。

[![](../Images/0d54ef641d23c6d43c94c0570bca40c3.png)](http://samirsaci.com)

信息系统之间的数据交换 — （图像由作者提供）

每个过程步骤由特定系统（[ERP](https://www.youtube.com/shorts/v0_R8P6MLQ0)、WMS、TMS）管理，这些系统记录信息和时间戳，我们将用于过程挖掘。

> 我们如何利用生成的数据来检测瓶颈并提高整体效率？

## 方法 1：发现

**时间戳定义** 从订单创建到商店接收，时间戳由不同系统记录。

[![](../Images/e77e394fe6c8bbfaf128da8dde06da73.png)](http://samirsaci.com)

每个过程的时间戳 — （图像由作者提供）

+   **订单创建时间** 在[ERP](https://www.youtube.com/shorts/v0_R8P6MLQ0)中由配送计划员创建的时间。

+   **订单接收时间** 在仓库管理系统（WMS）*(现在准备由仓库团队进行)*。

+   **拣货时间** 包括[拣货任务](https://www.youtube.com/watch?v=XejgbF2m_8g)的开始和结束时间，该任务包含订单。

+   **包装时间** 指[包装过程](https://www.youtube.com/watch?v=COcoxQ8NhzM)结束的时间。

+   **运输时间** 当你的订单离开仓库时的时间。

+   **交付时间** 当你的[卡车送货](https://www.youtube.com/watch?v=PYkN24PMKd8)到商店时的时间。

+   **商店接收时间** 是结束交付过程的时间：商店团队在[ERP](https://www.youtube.com/shorts/v0_R8P6MLQ0)中记录收到的物品的时间。

> ***💡 了解你的流程*** *系统在你的公司中可能被不同地使用。因此，确保流程已用详细的工作流程进行映射。*

**提前期定义**

由于时间戳本身没有意义，我们通过两个时间戳之间的差异来定义每个过程的提前期。

![](../Images/30df0779117216dd602a2eee341a5dec.png)

配送的物流提前期定义 — （图像由作者提供）

他们通常指的是特定团队的责任。

+   **端到端提前期** 用于衡量整个物流部门的绩效，也定义为[按时全量（OTIF）](https://www.youtube.com/watch?v=qhLqu6M7lcA#:~:text=Description,Time%20In%20Full%20(OTIF)%20KPI)。

+   **订单转移周期**由**IT/基础设施团队**负责，确保订单的快速传输

+   [仓库运营团队绩效](https://www.youtube.com/watch?v=KR_ziEiPcDk)由**准备周期**来衡量

+   **发货周期**是一个灰色地带，因为仓库团队、财务部门（开票）或运输团队（寻找运输卡车）都可以影响它

+   **运输周期**完全在运输团队的范围内

+   **接收周期**是门店团队的责任

> ***💡 你需要系统专家的支持*** *你的系统可能没有每个过程的时间戳，但你的基础设施团队总能找到替代解决方案。*

例如，当打包过程结束时，没有时间戳显示。

+   但订单状态正在变化*(从打包到准备发货)。*

+   可以开发脚本来在状态变化时创建时间戳。

+   然后可以将其填充到你用作数据源的数据湖中。

> 我们该如何处理这些数据？

**c) 使用Python进行数据处理** 使用Python，你可以连接到不同的系统，从其各自的数据库中提取事务记录。

![](../Images/dea584a56be4f3e965f1d2aa8a2814c8.png)

用Python处理数据处理 — （作者提供的图片）

这些系统可能有不同的字段和指标定义。使用订单号将表格合并到一个数据框中，并计算周期时间。

![](../Images/089e69c689a6617923bb6833f410250e.png)

周期时间图例 — （作者提供的图片）

上图显示了周期时间图例*(以分钟为单位)*，提供了订单传输、拣货和包装以及仓库-机场转移的性能波动概览。

有关如何构建物流绩效仪表板的更多信息，请查看这个简短的视频，

## **方法2：合规性**

**a) 交付周期：按时全面交付**

最重要的**关键绩效指标**是从订单创建到交付时间的周期。

![](../Images/dcec23e6e439c0dbcdc76fa6f5185d94.png)

[了解更多关于OTIF的信息](https://youtu.be/qhLqu6M7lcA) — （作者提供的图片）

分销计划员利用它来管理库存，设定安全库存并计划新产品发布。

> 在门店接收到ERP中创建的订单需要多长时间？

门店经理还会利用它来挑战物流部门在不同的供应绩效评估中。

**b) 端到端交付流程**

在我们的例子中，我们的交付周期目标是**72小时**。

[![](../Images/5afd4792bf685af73bcf8fb12c60c2d1.png)](https://www.youtube.com/watch?v=qhLqu6M7lcA)

交付订单流程示例 — （作者提供的图片）

上面的例子显示了符合目标周期时间的交付。

> ***💡 截止时间定义*** *订单接收截止时间是18:00:00。*

如果在这个时间之后收到订单，你必须等待24小时才能准备好。

它们是[延迟交货和干扰](https://youtu.be/YNvOX3CT3hQ)的主要原因。

> 我们现在可以可视化数据。

**c) 可视化** 定义后，你可以使用你的Python脚本将提前时间与这些目标进行比较。

![](../Images/033ac6fd274a32c6f7335cf28b67283f.png)

使用Python Matplotlib的提前时间目标图 — （作者提供的图像）

在这个可视化中，你可以看到。

+   对于一些订单，仓库团队需要遵守5.5小时（630分钟）的准备提前时间目标。

+   订单传输或仓库机场转运永远不会成为问题，因为你远低于最大提前时间。

> 仅此图表就可以帮助运营团队发现分销过程中的故障并制定行动计划。

## 方法3：改进

**a) 可视化延迟。** 目标是确保你所有的订单都在**72小时**内传输、准备和交付。

它从帮助你回答简单问题的可视化开始。

+   有多少订单已迟交？

+   哪个过程对你的整体提前时间影响最大？

[![](../Images/4622869c1782becae438bddd4d056853.png)](https://www.youtube.com/watch?v=ssdni_n6HDc)

延迟根本原因甜甜圈图 — （作者提供的图像）

[![](../Images/9c1651b7d20f464ed70287f8b3065e9e.png)](https://www.youtube.com/watch?v=ssdni_n6HDc)

根本原因分析的可视化 — （作者提供的图像）

上面的可视化将帮助你发现迟交货并快速筛选不同的环节，以了解哪一部分影响了你的提前时间。

1.  开始查看底部图表。

1.  你发现一个订单的提前时间超过了黄色线。

1.  筛选上述提前时间以找出导致延迟的部分。

> ***💡 原因代码映射*** *作为诊断分析的一部分，你可以开始自动创建*
> 
> 迟交货的原因代码将用于上面如甜甜圈图所示的可视化。
> 
> *如果你的货物延迟且装载提前时间高于目标，你可以添加一个迟交货原因代码：装载。*

**b) 过程挖掘以减少延迟** 现在你知道了根本原因，你可以专注于设计解决方案以避免未来的延迟。

> 在过去三个月的延迟中是否存在任何模式？

例如，

1.  你发现许多订单由于长时间的运输提前时间而延迟交付。

1.  经过进一步分析，你发现根本原因是**开票过程**。

1.  这个过程取决于交货地点*(地区、商店类型（免税店、加盟店等）*，产品类别，订单类型*(交叉对接、补货、收集启动)*，以及订单创建日期。

![](../Images/b46417b701147707c88061709dd8dc8a.png)

延迟开票预测模型 — （作者提供的图像）

你使用数据训练一个模型，预测使用上述特征的开票过程延迟的概率。

1.  你发现布尔输出*(按时开票)*与订单创建日期*(周一，.. 周日)*、以及商店区域*(欧洲、中东、亚洲、美洲)*之间存在高度相关性。

1.  绝大多数迟到的发票来自**一周的后半段**为**中东**地区的商店创建的订单。

1.  在与中东地区的本地物流团队对齐后，你发现发票处理在周四和周五停滞，因为这些是该地区的周末。

然后你可以要求需求规划师*(位于欧洲)*调整他们的订单创建流程以适应这些地方特性。

> ***💡 为操作和IT团队带来洞察*** *上面的例子表明，单靠数据无法设计解决方案或解决问题。*

## 使用Python进行流程挖掘的最佳实践

它们需要被纳入。

+   定义时间戳以确保从系统中检索的信息与操作现实相匹配。

+   确定交付时间定义，以便与贵公司的供应链绩效管理对接。

    *(例如：如果商店接收未纳入运输团队的绩效评估中，你需要将其移除)*

+   使用Python处理数据，以确保你用于计算的字段是正确的。

+   创建可视化以确保操作团队能够使用它们获取洞察。

你设计的模型和可视化将提供洞察，支持持续改进举措并提升战略决策。

# 结论

总体而言，流程挖掘可以成为提高业务流程效率和效果的强大工具，组织越来越多地利用它来推动流程改进工作。

> 你有经过统一清洗的数据吗？

## **这需要统一且清洁的数据。**

这一基本要求可能是你数字化转型过程中一个重要障碍。

最坏的情况，遗憾的是非常常见，将是一个组织：

+   多个系统无法相互通信，如多个[ERP](https://www.youtube.com/shorts/v0_R8P6MLQ0)实例、一个由第三方物流完全控制的每个仓库的WMS，以及几个未与[ERP](https://www.youtube.com/shorts/v0_R8P6MLQ0)接口的TMS。

+   许多未记录的自定义设置，如由离职开发人员创建的附加字段。

+   缺乏像数据湖这样的单一来源，无法整合你不同系统的数据库。

在这个阶段，你需要在进行流程挖掘之前，致力于统一和建立一个坚实的数据架构*(具有数据治理以维护数据完整性)*。

诊断后，你需要实施行动计划。

> 我们能否使用分析来模拟持续改进举措的影响？

## 使用数字双胞胎模拟举措

数字双胞胎是物理对象或系统的数字复制品。

![](../Images/7436811ed89540c9f3aa826509a06588.png)

使用 Python 的供应链数字双胞胎 — （图片来源：作者）

就像你有一个计算机模型来表示供应链中的各种组件和过程，比如仓库、运输网络和生产设施。

> 你是否想模拟运营提出的举措的影响？

在发现操作故障并找到根本原因后，你可以模拟几种情景，并测试解决方案的弹性。

+   如果我们使用最后一公里快递配送订单会怎么样？

+   如果我们增加仓库员工，延迟减少将会是多少？

这个模型可以估算性能改进，计算投资回报，并说服管理层投资额外的资本支出。

更多详情，👇

[](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----683b5eb6547c--------------------------------) [## 什么是供应链数字双胞胎？

### 使用 Python 探索数字双胞胎：建模供应链网络，提升决策能力，优化运营。

towardsdatascience.com](/what-is-a-supply-chain-digital-twin-e7a8cd9aeb75?source=post_page-----683b5eb6547c--------------------------------)

> 除了性能，你的操作对环境的影响如何？

本文聚焦于性能和物流成本。

然而，现在公司被迫减少环境足迹，我希望能打开供应链可持续性的窗口。

> 你听说过绿色库存管理吗？

## 专注于可持续性

当我担任解决方案设计师时，我们的零售客户主要关注的是减少运输成本。

> Samir，我们需要将每吨的成本降低 25%。

我一直建议减少配送频率，并增加每次发货的数量，以提高卡车的装载率。

> 这就是绿色库存管理的起点。

这可以被定义为以环保的方式管理库存。

![](../Images/ee5b4aa9450c5efe0bf89dc190f33608.png)

（图片来源：作者）

确实，除了减少二氧化碳排放外，这种方法在成本上也具有优势。

![](../Images/23266770489309c7875a5a71c6019b5f.png)

店铺补货系统 — （图片来源：作者）

大多数零售商使用 **库存管理系统**，这些系统采用基于规则的方法来补充商店，但没有考虑运输路线的效率。

**补货频率** 是一个重要的参数，可以用来优化分销网络。

> 我们可以模拟配送频率对二氧化碳排放的影响吗？可以！

为了证明我的观点，在本博客中分享的案例研究中，我模拟了不同配送频率对包装材料使用和二氧化碳排放的影响。

![](../Images/13b01189e3e1e2258580cc2755c27cfc.png)

模拟模型 — （图片来源：作者）

对于每种情景，该模型可以提供

+   准备好的 **混合纸箱** 的百分比 **(%)**

+   部分填充卡车用于配送商店的百分比 **(%)**

+   使用的 **纸箱材料** 总量 **(kg)**

+   道路运输的总CO2排放**(kg CO2eq)**

如果您对这种方法有信心或想挑战我的方法论，请查看这篇文章，

[](/data-science-for-sustainability-green-inventory-management-e7ddfd97696f?source=post_page-----683b5eb6547c--------------------------------) [## 数据科学与可持续性 - 绿色库存管理

### 模拟商店配送频率对时尚零售商CO2排放的影响

towardsdatascience.com](/data-science-for-sustainability-green-inventory-management-e7ddfd97696f?source=post_page-----683b5eb6547c--------------------------------)

# 关于我

在[Linkedin](https://www.linkedin.com/in/samir-saci/)和[Twitter](https://twitter.com/Samir_Saci_)上联系我。我是一名供应链工程师，利用数据分析来改善物流操作并降低成本。

如果您对数据分析和供应链感兴趣，请查看我的网站。

[](https://samirsaci.com/?source=post_page-----683b5eb6547c--------------------------------) [## 萨米尔·萨西 | 数据科学与生产力

### 一篇技术博客，专注于数据科学、个人生产力、自动化、运筹学和可持续性……

samirsaci.com](https://samirsaci.com/?source=post_page-----683b5eb6547c--------------------------------)

# 参考文献

+   “什么是供应链控制塔？”，[数据科学前沿](https://medium.com/towards-data-science/automated-supply-chain-control-tower-with-python-17dbf93a18d0)，[萨米尔·萨西](https://medium.com/u/bb0f26d52754?source=post_page-----683b5eb6547c--------------------------------)

+   “什么是供应链分析？”，[数据科学前沿](/what-is-supply-chain-analytics-42f1b2df4a2)，[萨米尔·萨西](https://medium.com/u/bb0f26d52754?source=post_page-----683b5eb6547c--------------------------------)

+   “4个影响项目启动您的供应链数据科学之旅”，[数据科学前沿](/4-impacting-projects-to-start-your-data-science-for-supply-chain-journey-fe068e503c29)，[萨米尔·萨西](https://medium.com/u/bb0f26d52754?source=post_page-----683b5eb6547c--------------------------------)

+   “使用数据分析进行物流绩效管理”，[数据科学前沿](https://medium.com/towards-data-science/logistic-performance-management-using-data-analytics-82b2da978e5f)，[萨米尔·萨西](https://medium.com/u/bb0f26d52754?source=post_page-----683b5eb6547c--------------------------------)

+   “4种智能可视化技术用于供应链描述性分析”，[数据科学前沿](/4-smart-visualizations-for-supply-chain-descriptive-analytics-fda4dfc2829a)，[萨米尔·萨西](https://medium.com/u/bb0f26d52754?source=post_page-----683b5eb6547c--------------------------------)
