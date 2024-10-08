# 数据一览：为数据分析创建动态仪表板

> 原文：[`towardsdatascience.com/data-at-a-glance-creating-dynamic-dashboards-for-data-analytics-1dce62fc6638`](https://towardsdatascience.com/data-at-a-glance-creating-dynamic-dashboards-for-data-analytics-1dce62fc6638)

## 使用 Tableau 构建互动数据可视化

[](https://medium.com/@john_lenehan?source=post_page-----1dce62fc6638--------------------------------)![John Lenehan](https://medium.com/@john_lenehan?source=post_page-----1dce62fc6638--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1dce62fc6638--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1dce62fc6638--------------------------------) [John Lenehan](https://medium.com/@john_lenehan?source=post_page-----1dce62fc6638--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1dce62fc6638--------------------------------) ·阅读时间 8 分钟·2023 年 11 月 20 日

--

![](img/2d7eff865295177b406c85f597c9e957.png)

图片由 Carlos Muza 在 Unsplash 提供

数据可视化是任何数据科学家工具箱中的核心技能。任何企业生成的数据量都是巨大的，及时的决策依赖于随时掌握所有相关数据和最新分析。许多组织通过使用自动化仪表板来完成这一关键的数据显示任务，例如使用 Power BI 或 Tableau。只要源数据定期更新，这些可视化工具就会自动生成所需的图表和图形，以跟踪所需的指标。

仪表板是*综合叙事*数据可视化方法的一个例子——用户可以自行探索数据，并为自己的目的获取洞见。这些类型的可视化允许观众自行研究数据并发现关键见解。像 Power BI 和 Tableau 这样的商业智能应用是这种叙事形式的典型例子，互动仪表板允许观众在最小的指导下深入数据。

*有关叙事可视化和数据分析中不同叙事方法的更多信息，请参阅我下面关于使用数据为演示文稿创建叙事的文章：*

[](/guide-your-audience-crafting-a-cohesive-narrative-from-the-data-4032538ff252?source=post_page-----1dce62fc6638--------------------------------) ## 引导你的观众：在演示中创建引人入胜的叙事

### 如何使用叙事技巧来结构化你的数据展示

towardsdatascience.com

在这篇文章中，我想展示我使用 Tableau Public 创建的三个数据可视化示例。Tableau Public 是一个免费的服务，任何人都可以使用它来创建自己的交互式仪表板——但需要注意的是，所有生成的可视化都是公开可访问的（也可以购买许可证版，该版本更适合处理保密数据）。如果你想在自己的时间里探索数据，我在这里包含了我生成的所有仪表板的链接。

![](img/bcb291977242c53da4dd903aecc33096.png)

图片由 [Luke Chesser](https://unsplash.com/@lukechesser?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 仪表板 1：专业薪资分析

我准备的第一个仪表板展示了不同经验水平、教育水平和工作领域的薪资指标。这些薪资数据来自 Glassdoor 的一小部分用户——原始数据集可以在 Kaggle 上找到 [**这里**](https://www.kaggle.com/datasets/mohithsairamreddy/salary-data)。这种类型的仪表板对于求职者和人力资源工作者特别有用，因为它快速展示了工作市场中的薪资范围和人口统计信息。要查看此仪表板中的数据，你可以访问 [**这里**](https://public.tableau.com/app/profile/john.lenehan/viz/SalaryAnalysisDashboard_16995718437200/SalaryDashboard)。

![](img/ea159ae908c65982a4f222ebadfeb7d3.png)

薪资分析仪表板（作者提供的图像）

我包含了教育程度和职位与薪资的箱形图，以及按经验水平（按性别分段）统计的个体直方图，还有按薪资区间和教育水平分段的个体直方图。

该仪表板可以根据所有数据特征进行过滤——例如，如果我只想查看博士学位的个体，我可以在箱形图中选择 PhD 桶，其他可视化会根据过滤后的数据进行调整。

![](img/548200ee2b82eff5e4d4bae55c691787.png)

筛选博士学位的仪表板（作者提供的图像）

通过查看以上这些可视化，我们可以获得一些有趣的见解：

1.  中位薪资随着教育水平的提高而增加，在所有相关领域中，持有博士学位的人在薪资直方图中位于最高薪资区间。

1.  整体调查在经验水平上存在明显偏斜，大多数受访者具有 2-3 年的经验。这可能会在任何基于此仪表板的决策中引入偏差——理想情况下，应进行一个更具代表性的调查以获得更好的分析（值得庆幸的是，一旦在源文件中收集了额外的回应，所有可视化都会立即更新）。

1.  不出所料，研究职位由博士学位持有者主导——尽管我们也看到大量博士在网络/软件开发以及销售/市场营销领域工作。博士受访者中性别比例严重失衡，女性远少于男性。

这些只是对薪资数据集的一些分析示例——额外的分析可能会揭示薪资数据中更多的趋势。

## 仪表板 2: 纽约市驱逐趋势，2017 年至今

我创建的下一个可视化是一个显示纽约市范围内驱逐趋势的仪表板，涵盖了城市的五个区。原始数据集来自纽约市开放数据门户，可以在[**这里**](https://data.cityofnewyork.us/City-Government/Evictions/6z8x-wfk4)找到。这个仪表板对于那些在市政政府或非营利组织中工作的人尤其有用，例如想要减少全市驱逐趋势的人——你可以通过链接[**这里**](https://public.tableau.com/app/profile/john.lenehan/viz/NEwYorkCityEvictionsAnalysis/NYEvictionAnalysis)探索该仪表板中的可视化内容。

![](img/3ea4ebc88130f06a1e035f297ff151a4.png)

纽约市驱逐趋势，2017 年至今（图片由作者提供）

对于这个仪表板，我包含了过去 6 年内纽约市记录的所有驱逐地点的地图、每个区的驱逐条形图（还可以扩展到显示区或街区级别的驱逐情况）、按区显示历史驱逐趋势的折线图，以及按受影响程度排序的街区列表。仪表板顶部的滑块使得选择分析日期范围成为可能。

数据和可视化可以按时间和地点进行筛选——在下方的截图中，我已筛选出 2019 年布朗克斯的驱逐情况。与我之前的示例类似，所有其他可视化都会调整以适应筛选的数据——例如，请注意地图现在仅包括布朗克斯区。

![](img/20d039c22691bb2e3cadb4efd9f58ca6.png)

筛选 2019 年布朗克斯区驱逐情况，显示在驱逐实例上的提示框（图片由作者提供）

列举一下从上述可视化中获得的一些观察结果：

1.  布朗克斯区在本报告所涵盖的整个时间段内的驱逐趋势最高，其次是布鲁克林和皇后区。

1.  2021 年至 2022 年间，驱逐情况明显下降，这显然是由于 Covid-19 大流行期间对驱逐的禁令。

1.  2019 年布朗克斯的驱逐率似乎随着时间的推移稳定下降，这与该时期其他区的趋势一致。

再次强调，这些示例仅代表了从驱逐情况仪表板中提取的可能观察结果的一小部分。深入分析仪表板可能会揭示更多见解，这将*最终*支持围绕驱逐资源做出更明智的决策。

## 仪表板 3: 西班牙旅游趋势，2018–2021

我想讨论的最后一个仪表板是 2018 年至 2021 年西班牙旅游趋势的仪表板。这些可视化的数据来源于西班牙政府的开放数据门户，网址是[**这里**](https://datos.gob.es/en/catalogo/ea0010587-pernoctaciones-por-residencia-del-viajero-provincias-oat-identificador-api-48427)（不幸的是，西班牙政府尚未公布 2022 年或 2023 年的数据）。这有明显的商业和政府应用——例如，西班牙旅游部门的人可能会使用这些数据决定在哪里花钱发展区域旅游。同样，企业也可以利用这些数据预测某一年哪个地区的旅游量最大。任何想探索数据的人可以在链接[**这里**](https://public.tableau.com/app/profile/john.lenehan/viz/SpanishTourismTrends2018-2021/SpanishTourismTrends)找到仪表板。

![](img/e4b3c1c2aa16eb726208d82864f41030.png)

西班牙旅游趋势，2018–2021（作者提供的图片）

在这个可视化中，我包含了一个堆积柱状图，显示了按年分的旅游情况（按游客的国籍状态细分），一个按区域的旅游条形图（可以扩展以显示按省份的旅游情况），一个显示按省份和年份的旅游矩阵，以及西班牙所有省份的地图。

再次，这个仪表板可以按位置进行调整——下面我已经筛选了加泰罗尼亚地区，并突出显示了巴塞罗那省。地图工具提示显示了该省的旅游详情，条形图显示了这一期间西班牙国民与国际游客的比例。

![](img/7e21bb0158c0e8d252c01ae5f3bce656.png)

为加泰罗尼亚筛选并显示巴塞罗那的工具提示（作者提供的图片）

从查看这个仪表板的图表中，有几个要点值得注意：

1.  安达卢西亚、加泰罗尼亚、加纳利群岛和瓦伦西亚是西班牙最受欢迎的自治区，差距非常大。

1.  国际旅游在 2018 年和 2019 年远远超过了国内旅游——2020 年和 2021 年的比例大致相等，这很可能是由于 Covid-19 疫情导致的旅行限制。

1.  巴塞罗那省的旅游人数占加泰罗尼亚省整体旅游人数的一半以上，几乎是该地区第二大受欢迎省份（赫罗纳）的 2.5 倍。这很可能主要归因于巴塞罗那市。

对西班牙其他地区和省份进行类似的分析和观察，可以更好地了解这些地区的旅游预期。

## 总结

总结来说，Tableau 是一个强大的数据可视化工具，使用户能够在各种数据集中创建简洁而有影响力的可视化。我在本文中介绍了最近创建的三个相对简单的可视化示例，但在 Tableau Public 上可以找到许多其他示例。在 Tableau 上可以找到的许多视觉效果都是对极其复杂主题的高度直观的表现，这显示了这些仪表盘在数据可视化中的强大能力。这些分析工具的价值不仅在于它们能够快速生成及时的图表和视觉效果，还在于它们可以以清晰且智能的格式呈现数据，从而加快分析和决策过程。

## 参考文献

[1] Mohith Sai Ram Reddy 等人. (2023). Salary_Data. Kaggle。可用网址: [`www.kaggle.com/datasets/mohithsairamreddy/salary-data`](https://www.kaggle.com/datasets/mohithsairamreddy/salary-data) （访问日期：2023 年 11 月 6 日）。

[2] (DOI), D.of I. (2023) 驱逐：纽约市开放数据，驱逐 | 纽约市开放数据。可用网址: [`data.cityofnewyork.us/City-Government/Evictions/6z8x-wfk4`](https://data.cityofnewyork.us/City-Government/Evictions/6z8x-wfk4) （访问日期：2023 年 11 月 7 日）。

[3] Diaz, J. (2020 年 12 月 29 日). 纽约批准驱逐禁令至 5 月。NPR。 [在线] 可用网址: [`www.npr.org/sections/coronavirus-live-updates/2020/12/29/951042050/new-york-approves-eviction-moratorium-until-may`](https://www.npr.org/sections/coronavirus-live-updates/2020/12/29/951042050/new-york-approves-eviction-moratorium-until-may) （访问日期：2023 年 11 月 7 日）。

[3] Instituto Nacional de Estadística (经济事务与数字化转型部). (2022). 根据旅行者居住地的过夜住宿情况。各省。OAT（API 标识符：48427）。可用网址: [`datos.gob.es/en/catalogo/ea0010587-pernoctaciones-por-residencia-del-viajero-provincias-oat-identificador-api-48427`](https://datos.gob.es/en/catalogo/ea0010587-pernoctaciones-por-residencia-del-viajero-provincias-oat-identificador-api-48427) （访问日期：2023 年 11 月 10 日）。
