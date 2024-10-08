# 数据科学造福社会：超越利润，迈向更美好的世界

> 原文：[`towardsdatascience.com/data-science-for-good-beyond-profits-towards-a-better-world-2d161e2d2b28`](https://towardsdatascience.com/data-science-for-good-beyond-profits-towards-a-better-world-2d161e2d2b28)

## 利用数据分析的力量来推动公司内部的积极变革，同时提高盈利能力。

[](https://s-saci95.medium.com/?source=post_page-----2d161e2d2b28--------------------------------)![Samir Saci](https://s-saci95.medium.com/?source=post_page-----2d161e2d2b28--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2d161e2d2b28--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d161e2d2b28--------------------------------) [Samir Saci](https://s-saci95.medium.com/?source=post_page-----2d161e2d2b28--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2d161e2d2b28--------------------------------) ·阅读时长 11 分钟·2023 年 8 月 25 日

--

![](http://samirsaci.com)

数据科学造福社会 — （作者提供的图片）

数据科学可以支持远超财务收益的业务转型。

这可以改善工作条件，减少不平等，并促进包容性的工作环境。

在我作为高级供应链工程师的经验中，我主要使用分析工具来提高运营绩效并降低成本。

然而，工程师的职责不仅仅是最大化利润；她还可以帮助让世界变得更美好。

> 作为数据科学家，您如何改善物流操作员的工作条件？

在这篇文章中，我将分享使用数据科学来改善物流操作员的工作条件（和奖金）的例子。

💌 免费订阅最新文章： [新闻通讯](https://www.samirsaci.com/#/portal/signup)

📘 您的完整供应链分析指南：[分析备忘单](https://bit.ly/supply-chain-cheat)

# I. 生产力与盈利能力

## 物流公司的盈利能力

作为一名前供应链解决方案设计师，我职业生涯的早期帮助物流公司优化生产力以提高利润。

我亲眼目睹了我们的客户 *(零售、时尚、奢侈品、化妆品)* 面临的巨大压力，他们需要按时交付货物，同时**最小化成本**。

![](http://samirsaci.com)

3PL 零售分销 — （作者提供的图片）

作为第三方物流提供商，这种压力持续向我们施加，我们时刻担心合同无法续签。

在这种压力巨大的环境下，削减成本和采用激进管理策略的诱惑始终存在。

> 让我们找到更聪明的工作方式吧！

通过利用数据科学的力量来改进流程，我们可以避免将这种压力转移到操作人员身上，并推动积极的变化。

## 过程生产力定义

让我们以一个假设的场景来讨论，一家主要的国际时尚零售商正在**与物流公司建立其配送网络**。

![](http://samirsaci.com)

配送网络以交付 100 家门店 — （图像来源：作者）

**I&N** 正在寻求外包其仓储和运输操作，以交付其在上海的门店。

物流团队组织招标，也称为请求提案（RFP），邀请全球物流公司提交他们的解决方案。

![](http://samirsaci.com)

请求提案 — （图像来源：作者）

作为 RFP 的一部分，I&N 的物流团队提供

+   数据和流程要求

+   一份报价单概述了**不同服务价格**，如存储、接收、箱子拣选、单件拣选、装载和退货管理。

作为解决方案设计经理，我负责提出解决方案并计算每项服务的价格。

价格通过考虑设备和劳动力成本来确定，这些成本由操作人员的生产力和销售利润率驱动。

![](http://samirsaci.com)

计算处理服务的价格 — （图像来源：作者）

*附注：销售利润率定义为营业额中代表利润的百分比。*

仓库和运输团队随后将进行为期三年的操作，关键目标是保持这一利润率。

这可以通过提高价格（几乎不可能）或降低成本来实现。

![](http://samirsaci.com)

损益平衡 — （图像来源：作者）

后者的选择通常归结为一个关键因素：**提高生产力**。

> 但是你如何提高生产力呢？

这可以通过使用高个人生产力目标、工资削减和非法劳动实践的激进管理策略来实现。

这通常会导致员工压力和不满增加，这可能会影响贵公司的 ESG 评分。

❓ 想了解更多关于 ESG 评分的内容吗？

[](/what-is-esg-reporting-d610535eed9c?source=post_page-----2d161e2d2b28--------------------------------) ## 什么是 ESG 报告？

### 利用数据分析进行全面和有效的环境、社会及治理（ESG）报告

towardsdatascience.com

第二种方法是优化流程、布局和货物流动，帮助操作人员在**相同的努力下**变得**更高效**。

这种方法不仅有利于公司，而且**改善了工作条件**。

为了说明我的观点，我将分享我在零售、快时尚和奢侈品行业的职业生涯中实施的示例。

💡 **关注我在 Medium 上的文章**，获取更多与🏭供应链分析、🌳可持续发展和🕜生产力相关的内容。

# II. 通过优化提高生产力

数据科学可以成为改善操作员工作条件和工资的重要工具。

## **提高拣货效率**

让我们从一个来自再工程项目的例子开始，我使用分析来帮助操作员提高拣货生产力。

**仓库拣货**可以定义为从库存中提取产品以准备订单（电子商务、门店配送）的任务。

![](https://towardsdatascience.com/optimizing-warehouse-operations-with-python-part-1-83d02d001845)

仓库中的拣货路线——（作者提供的图片）

在**配送中心（DC）**中，从一个位置到另一个位置的行走时间在[picking route](https://www.youtube.com/watch?v=6swvr-J_bJE)中可以占据操作员工作时间的*60%到 70%*。

![](https://towardsdatascience.com/optimizing-warehouse-operations-with-python-part-1-83d02d001845)

计算拣货的价格——（作者提供的图片）

减少行走时间可以对生产力产生显著影响。

假设操作员拣选的是完整的箱子。

+   生产力将以**（箱数/小时）**来衡量。

+   价格将以**（$/小时）**来衡量。

仓库操作员有生产力目标，他们需要达到这些目标才能获得奖金。

对于零售和快速消费品（FMCG）操作，操作员面临巨大的压力，因为他们占据了固定和变动成本的主要部分。

> 改善操作员生产力的最佳方法是什么？

如果您最大化**每米行走拣选的箱数**，可以在最小化操作员的努力的同时提高他们的生产力。

![](https://towardsdatascience.com/optimizing-warehouse-operations-with-python-part-1-83d02d001845)

提高每米行走拣选的箱子数量的 3 种方法——（作者提供的图片）

在一系列文章中，我提出了几种数据驱动的方法来减少行走距离并最大化操作员的生产力。

+   **订单批处理**以增加每条路线拣选的订单行数。

+   **拣货地点的聚类**以按**区域**分组订单。

+   使用 Google-OR 库进行**高级路径规划**以优化拣货路线。

这些不同的优化方法已经通过实际的订单行和仓库布局进行了测试。

> 更多的箱子被拣选 + 更少的距离 = 更高的生产力。

结果显示，在拣选相同订单范围时，行走距离大幅减少。

通过在您的仓库管理系统中实施这些算法，您可以在不改变流程的情况下提升操作员的生产力。

**💡 欲了解更多详细信息，您可以查看这些文章，**

[](/optimizing-warehouse-operations-with-python-part-1-83d02d001845?source=post_page-----2d161e2d2b28--------------------------------) [## 使用订单分批处理提高仓库生产力

### 设计一个仿真模型，以估算几种单拣货员路线问题策略在拣货中的影响…

[向数据科学进发](https://towardsdatascience.com/optimizing-warehouse-operations-with-python-part-1-83d02d001845?source=post_page-----2d161e2d2b28--------------------------------) [](/optimizing-warehouse-operations-with-python-part-2-clustering-with-scipy-for-waves-creation-9b7c7dd49a84?source=post_page-----2d161e2d2b28--------------------------------) [## 使用空间聚类提高仓库生产力

### 通过使用拣货位置空间聚类，将订单分批处理来提高仓库拣货生产力

[向数据科学进发](https://towardsdatascience.com/optimizing-warehouse-operations-with-python-part-2-clustering-with-scipy-for-waves-creation-9b7c7dd49a84?source=post_page-----2d161e2d2b28--------------------------------) [](/optimizing-warehouse-operations-with-python-part-3-google-ai-for-sprp-308c258cb66f?source=post_page-----2d161e2d2b28--------------------------------) [## 使用路径规划算法提高仓库生产力

### 基于旅行推销员问题设计的路径规划算法，采用 Google AI 线性优化…

[向数据科学进发](https://towardsdatascience.com/optimizing-warehouse-operations-with-python-part-3-google-ai-for-sprp-308c258cb66f?source=post_page-----2d161e2d2b28--------------------------------)

## 使用排队理论设计的包裹打包过程

现在让我们转到电子商务操作。

在线零售的兴起给履行中心带来了巨大的压力，以无与伦比的速度准备和发货订单。

> 我们的仓库每天可以发货多少订单？

在这个例子中，一名持续改进工程师的主要问题是**运输能力**。

![](https://towardsdatascience.com/supply-chain-process-design-using-the-queueing-theory-2ad75e58d1f3)

仓库中的打包区域 — （图像由作者提供）

在[拣货](https://www.youtube.com/watch?v=XejgbF2m_8g)之后，订单等待**过长**时间被打包并装上卡车。

根据现场观察和生产力分析，我们的工程师了解到打包过程**是瓶颈**。

为了减轻打包操作员的压力，她希望重新设计布局并优化流程。

现场经理决定投资**第二个打包站**。

因此，我们的工程师希望使用**排队理论**找到最佳布局。

**解决方案 1：保留一条带有两个平行工作站的单线**

![](https://towardsdatascience.com/supply-chain-process-design-using-the-queueing-theory-2ad75e58d1f3)

解决方案 1 — （图像由作者提供）

**解决方案 2：增加一条带有专用工作站的第二条线**

![](https://towardsdatascience.com/supply-chain-process-design-using-the-queueing-theory-2ad75e58d1f3)

解决方案 1 — （图像作者提供）

> 最佳解决方案是什么，以最小化排队时间并减少瓶颈？

运用**排队理论**的概念，我们可以估计考虑输入流量变异的两种布局的表现。

![](https://towardsdatascience.com/supply-chain-process-design-using-the-queueing-theory-2ad75e58d1f3)

两种解决方案的模拟结果 — （图像作者提供）

分析显示，第二种解决方案**鲁棒性较差**，可能会在面对**容量变异**时降低包装站的能力。

假设我们有 1.5 的变异性，

+   打包线队列时间减少-25%（秒）

+   在相同的包装速度下更高的整体生产力

![](https://towardsdatascience.com/supply-chain-process-design-using-the-queueing-theory-2ad75e58d1f3)

按解决方案计算的平均生产力 — （图像作者提供）

多亏了这个简单的建模，她设计了一个可以让操作员在没有额外努力的情况下提高生产力的布局。

**💡 有关更多详细信息，请查看这篇文章**

[](/optimizing-warehouse-operations-with-python-part-3-google-ai-for-sprp-308c258cb66f?source=post_page-----2d161e2d2b28--------------------------------) ## 使用路径寻找算法和 Python 提升仓库生产力

### 实施基于旅行商问题的路径寻找算法，由 Google AI 线性优化设计…

towardsdatascience.com

## **结论**

使用先进的分析工具，您可以重新设计流程，以帮助操作员提高效率。

+   提高挑选和包装操作员的奖金（👩‍🏭）

+   更少的努力就能快速行走或打包，无需经理的压力（🧘）

+   为公司带来更好的盈利能力（💰）

在下一部分，我们将探索其他基于数据的改进机会，重点关注**资源分配**。

💡 在 Medium 上关注我，获取更多关于🏭供应链分析、🌳可持续性和🕜生产力的文章。

# II. 智慧地投资于您的员工

前一部分专注于设计一种**最佳过程**，以最大化操作员的生产力。

然而，**过程设计**与**执行**之间存在一个差距，这个差距可以通过优化工具来弥补。

## 入库管理的劳动力规划

对于我们的快时尚客户**I&N**，我们必须计划劳动力以应对商店需求的波动。

![](https://towardsdatascience.com/optimize-workforce-planning-using-linear-programming-with-python-47a0b5f89a6f)

电子商务仓库的日常波动示例 — （图片由作者提供）

**I&N** 提供体积预测，如上所示，管理者正在规划所需的员工人数以满足需求。

![](img/0ead5fe404bdb40d011c24b08fdd2d1d.png)

基于体积预测的劳动力规划 — （图片由作者提供）

他们必须在确保员工保留和遵守当地法规的同时，最小化临时工的数量。

在我们的例子中，我们希望帮助[**入库经理**](https://www.youtube.com/watch?v=nz69i6l7SzI)**。**

他的团队职责包括

+   **卸货** 从卡车上卸下托盘

+   **扫描** 每个托盘并**记录**在仓库管理系统（WMS）中收到的数量

+   **将这些** 托盘放置在库存区域

![](https://towardsdatascience.com/optimize-workforce-planning-using-linear-programming-with-python-47a0b5f89a6f)

仓库卸货过程 — （图片由作者提供）

团队有两种生产力目标

+   每小时卸货的托盘数量 **每个工人** **(I)**

+   每小时卸货的托盘数量 **为整个团队支付（II）**

> 如何根据体积和生产力来确定劳动力规模？

如果经理招聘了**过多的操作员**，**总体生产力（II）可能会大幅下降。**

根据体积预测，他可以估算每天所需的资源。

![](https://towardsdatascience.com/optimize-workforce-planning-using-linear-programming-with-python-47a0b5f89a6f)

每日入库资源需求预测 — （图片由作者提供）

要**确保员工保留**，你需要保证**每周 5 个连续工作日**的最低要求。

![](https://towardsdatascience.com/optimize-workforce-planning-using-linear-programming-with-python-47a0b5f89a6f)

根据上述约束进行轮班规划 — （图片由作者提供）

为了帮助我们的入库经理平衡这些约束和目标，我们可以使用**Python 的线性规划**。

![](https://towardsdatascience.com/optimize-workforce-planning-using-linear-programming-with-python-47a0b5f89a6f)

最终结果 — （图片由作者提供）

这个优化工具的结果令人满意，

+   除了星期五和星期六，我们没有多余的资源

+   供应每天与需求相匹配

除了这两天外，总体生产力不会受到计划问题的影响。

如果操作员达成目标，他们将获得**全额奖金**，而不会影响运营利润。

**💡 要了解更多细节，你可以查看这篇文章**

[](/optimize-workforce-planning-using-linear-programming-with-python-47a0b5f89a6f?source=post_page-----2d161e2d2b28--------------------------------) ## 使用线性规划和 Python 优化劳动力规划

### 你需要雇佣多少临时工来吸收每周的工作量，同时确保……

towardsdatascience.com

## 仓库操作员的最佳激励政策

在这个最后的例子中，让我们假设你正在帮助一个拥有**22 个仓库**的物流**公司**的区域主任处理她的损益表。

![](https://towardsdatascience.com/lean-six-sigma-with-python-logistic-regression-36d160e84548)

配送中心的拣货、增值服务和包装 — （作者提供的 CAD 模型）

目标仍然是最大化**操作员的拣货生产力**。

她希望使用财务激励来**激励操作员**提高每小时的产出。

当前的激励计划为达到目标的操作员提供**每天 5 欧元**。*(日薪：62 欧元)*

然而，这并不高效，因为只有**20%的操作员**能够达成他们的目标。

> 达到 75%目标所需的最低每日奖金是多少？

**这个想法**是进行一个数据驱动的实验

1.  **随机**选择你在**22 个仓库**中的操作员

1.  实施**每日激励**金额在**1 到 20 欧元**之间

1.  验证**操作员是否达到了目标**

使用**逻辑回归**，我们可以得到一个概率图，帮助估算每个每日激励值下达到目标的概率。

![](https://towardsdatascience.com/lean-six-sigma-with-python-logistic-regression-36d160e84548)

你的样本数据的拟合线图 — （作者提供的图片）

根据趋势，以**15 欧元**的每日激励，我们有 75%的概率达到目标。

这一结果，通过统计工具的支持，为我们的主管提供了有价值的见解，以便调整她的激励政策。

**💡 更多详细信息，请查看这篇文章**

[](/lean-six-sigma-with-python-logistic-regression-36d160e84548?source=post_page-----2d161e2d2b28--------------------------------) ## 使用 Python 进行精益六西格玛 — 逻辑回归

### 用 Python 代替 Minitab 执行逻辑回归，以估算达到 75%目标所需的最低奖金。

towardsdatascience.com

## 结论

我们发现了一些优化和统计工具，帮助公司改善资源分配以实现关键运营目标。

+   提高整体团队生产力和改善利润率（👩‍🏭）

+   为操作员提供更多激励（💰）

+   减少管理层的压力（🧘）

💡 关注我在 Medium 上的更多文章，内容涉及🏭供应链分析、🌳可持续发展和🕜生产力。

# III. 下一步

希望这些现实世界的应用能够激发你设计和部署支持财务收益并改善操作条件的工具。

随着利益相关者对企业社会责任（CSR）和 ESG 评分的要求越来越高，工作条件和公平工资将成为任何业务转型的关键参数。

![](img/75bba96700dccc4fdf305126465b9cb2.png)

ESG 支柱演示——（作者提供的图片）

环境、社会和治理（ESG）是一种用于披露公司环境足迹、社会影响和治理结构的报告方法。

查看 ESG 评分中包含的指标，你会发现利用数据科学带来积极变化的额外机会。

**有关 ESG 评分的更多细节**

## 什么是 ESG 报告？

### 利用数据分析进行全面有效的公司环境、社会和治理报告

towardsdatascience.com

在本系列的下一篇文章中，我将专注于高级分析以**减少消耗品使用和废物生成**。

敬请关注！

**🌍 对全球可持续未来的路线图感到好奇吗？**

深入了解我最近关于联合国可持续发展目标如何通过数据支持的见解

[## 数据科学支持可持续发展目标（SDGs）——数据为善](https://s-saci95.medium.com/what-are-the-sustainable-development-goals-sdgs-988a1eb2b62b?source=post_page-----2d161e2d2b28--------------------------------)

### 将全球可持续发展倡议与公司供应链数字化转型通过数据科学相结合

[s-saci95.medium.com](https://s-saci95.medium.com/what-are-the-sustainable-development-goals-sdgs-988a1eb2b62b?source=post_page-----2d161e2d2b28--------------------------------)

# 关于我

在[Linkedin](https://www.linkedin.com/in/samir-saci/)和[Twitter](https://twitter.com/Samir_Saci_)上与我联系，我是一名使用数据分析改善物流操作和降低成本的[供应链工程师](https://www.samirsaci.com/blog/)。

如果你对数据分析和供应链感兴趣，可以看看我的网站。

[## Samir Saci | 数据科学与生产力](http://samirsaci.com/?source=post_page-----2d161e2d2b28--------------------------------)

### 一个关注数据科学、个人生产力、自动化、运筹学和可持续性的技术博客……

[samirsaci.com](http://samirsaci.com/?source=post_page-----2d161e2d2b28--------------------------------)

💡 在 Medium 上关注我，获取更多有关🏭供应链分析、🌳可持续性和🕜生产力的文章。

# 参考资料

+   Python 中的精益六西格玛 — 逻辑回归，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----2d161e2d2b28--------------------------------)，Towards Data Science

+   使用排队理论进行供应链过程设计，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----2d161e2d2b28--------------------------------)，Towards Data Science

+   使用 Python 的订单分批来提升仓库生产力，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----2d161e2d2b28--------------------------------)，[Towards Data Science](https://medium.com/towards-data-science/optimizing-warehouse-operations-with-python-part-1-83d02d001845)

+   使用 Python 的空间聚类来提升仓库生产力，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----2d161e2d2b28--------------------------------)，[Towards Data Science](https://medium.com/towards-data-science/optimizing-warehouse-operations-with-python-part-2-clustering-with-scipy-for-waves-creation-9b7c7dd49a84)

+   使用 Python 的路径规划算法来提升仓库生产力，[Samir Saci](https://medium.com/u/bb0f26d52754?source=post_page-----2d161e2d2b28--------------------------------)，[Towards Data Science](https://medium.com/towards-data-science/optimizing-warehouse-operations-with-python-part-3-google-ai-for-sprp-308c258cb66f)
