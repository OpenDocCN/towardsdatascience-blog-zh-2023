# 基于数据驱动的方法来减少员工调查长度

> 原文：[https://towardsdatascience.com/a-data-driven-method-to-reduce-employee-survey-length-8aecedcf5df9?source=collection_archive---------2-----------------------#2023-01-14](https://towardsdatascience.com/a-data-driven-method-to-reduce-employee-survey-length-8aecedcf5df9?source=collection_archive---------2-----------------------#2023-01-14)

## 减少调查长度，同时最大化可靠性和有效性

[](https://medium.com/@trevorcoppins?source=post_page-----8aecedcf5df9--------------------------------)[![Trevor Coppins](../Images/32dbd12ef6b18798131c7b48060c69f5.png)](https://medium.com/@trevorcoppins?source=post_page-----8aecedcf5df9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8aecedcf5df9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----8aecedcf5df9--------------------------------) [Trevor Coppins](https://medium.com/@trevorcoppins?source=post_page-----8aecedcf5df9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F88581b94ffb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-driven-method-to-reduce-employee-survey-length-8aecedcf5df9&user=Trevor+Coppins&userId=88581b94ffb&source=post_page-88581b94ffb----8aecedcf5df9---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8aecedcf5df9--------------------------------) ·16分钟阅读·2023年1月14日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F8aecedcf5df9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-driven-method-to-reduce-employee-survey-length-8aecedcf5df9&user=Trevor+Coppins&userId=88581b94ffb&source=-----8aecedcf5df9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F8aecedcf5df9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fa-data-driven-method-to-reduce-employee-survey-length-8aecedcf5df9&source=-----8aecedcf5df9---------------------bookmark_footer-----------)![](../Images/93b3c15c9e86ed98d4a7f8f3fcd341e2.png)

图片来源：[Marvin Meyer](https://unsplash.com/@marvelous?utm_source=medium&utm_medium=referral) 于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

员工调查正迅速成为组织生活中的一个重要方面。事实上，人力分析领域的增长和数据驱动的人才管理方法的采纳就是证明（见麦肯锡[报告](mckinsey.com/capabilities/people-and-organizational-performance/our-insights/how-to-be-great-at-people-analytics)）。通过一次调查，我们可以收集有关领导者表现的信息、员工是否有动力以及员工是否考虑离职。唯一一个相当*长*的“隐形问题”就是我们的调查长度。

员工调查的创建者（例如，人力资源部门和/或行为数据科学家）希望准确测量多个重要主题，这通常需要大量的问题。另一方面，参与时间较长的调查的受访者更有可能中途退出调查（Hoerger, 2010; Galesic & Bosnjak, 2009），并且引入测量误差（例如，Peytchev & Peytcheva, 2017; Holtom et al., 2022）。尽管如此，参与调查的受访者比例却有所增加：组织行为学文献中的研究报告显示，在15年期间（2005–2020年），受访率从48%大幅提高至68%（Holtom et al., 2022）。虽然调查长度只是决定数据质量和受访者比例的众多因素之一（例如，激励措施、后续跟进；Edwards et al., 2002; Holtom et al., 2022），但调查长度是一个易于调整且直接受调查创建者控制的因素。

本文提出了一种通过选择尽可能少的条目来缩短员工调查的方法，以实现最大的期望条目特征、可靠性和有效性。通过这种方法，员工调查可以被缩短，以节省员工时间，同时希望改善参与率/退出率和测量误差，这些都是较长调查中常见的关注点（例如，Edwards et al., 2002; Holtom et al., 2022; Jeong et al., 2023; Peytchev & Peytcheva, 2017; Porter, 2004; Rolstad et al., 2011; Yammarino et al., 1991）。

*缩短调查的经济效益*

不相信？让我们看看缩短调查的实际经济效益。作为一个说明性的例子，我们计算一下如果将一个季度的15分钟调查缩短为10分钟，对于一个拥有10万人的大型组织（例如，《财富》100强公司），投资回报率会如何。使用美国工人的中位工资（$56,287；见[报告](https://www.census.gov/content/dam/Census/library/publications/2021/demo/p60-273.pdf)），将调查时间缩短5分钟可以为组织节省超过100万美元的员工时间。虽然这些计算不是精确科学，但这是理解调查时间如何影响组织底线的有用指标。

![](../Images/ff87268b8dbbbde595a22c75daecd276.png)

显示减少员工调查时间的成本节约图

***解决方案：缩短员工调查问卷***

为了缩短我们的调查问卷，但保留理想的项目级统计数据、可靠性和有效性，我们采用两步过程，其中Python和R程序将帮助确定最佳保留的项目。在**第1步**中，我们将利用多标准决策（MCDM）程序（`Scikit-criteria`；Cabral等，2016）根据多个标准（标准差、偏斜度、峰度和主题专家评分）选择表现最佳的项目。在**第2步**中，我们将利用R程序（OASIS；Cortina等，2020）选择第1步中排名最高的项目的最佳组合，以进一步缩短我们的量表，但保持最大的可靠性和其他有效性问题。

简而言之，最终输出将是一个减少后的项目集，具有理想的项目级统计数据以及最大的可靠性和有效性。

**此方法适用于谁？**

+   从事调查创建和人员数据分析的人员分析专家、数据科学家、I/O 心理学家或人力资源（HR）专业人员

+   理想情况下，用户应具有一些Python或R和统计学的初级经验。

**你需要什么？**

+   Python

+   R

+   数据集（选择一个）：

1.  实践数据集 — 我利用了国际人格项目库（IPIP；[https://ipip.ori.org/](https://ipip.ori.org/)；Goldberg, 1992）公开数据集的前1000个回应，由开放心理测量学（[openpsychometrics.org](http://openpsychometrics.org/)）提供。为简便起见，我只使用了10个责任心项目。*数据来源说明*：IPIP是一个公共领域的人格测试，可以在没有作者许可或费用的情况下使用。类似地，openpsychometrics.org是开放源数据，已在其他几篇学术出版物中使用（见[此处](https://openpsychometrics.org/_rawdata/cited/)）。

1.  你自己的数据集（包含员工回应）用于你想要缩短的调查问卷。理想情况下，这应该是尽可能大的数据集，以提高准确性和复制的可能性。通常，大多数用户希望拥有100到200+个回应的数据集，以期消除抽样或偏斜回应的影响（见Hinkin, 1998以进一步讨论）。

+   **可选**：数据集中每个候选缩短项目的主题专家（SME）评分。仅在使用自己的数据集时适用。

+   **可选**：聚合效度和区分效度测量。这些可以在第二步中使用，但不是必需的。这些效度测量对于新量表开发更为重要，而不是缩短现有的已建立量表。聚合效度是指一个测量与其他类似测量的相关程度，而区分效度则是指它与非相关测量的无关程度（Hinkin, 1998；Levy, 2010）。同样，仅在你拥有自己的数据集时适用。

*Github 页面代码:* [https://github.com/TrevorCoppins/SurveyReductionCode](https://github.com/TrevorCoppins/SurveyReductionCode)

***请注意***：除非另有说明，否则所有图片均为作者提供

# **第一步：项目级分析与多准则决策方法（MCDM）**

***项目级统计说明***

对于‘纯粹’的项目级统计（或每个项目的属性），我们利用**标准差**（即，平均而言，受访者的回答有多大差异）以及**偏度**和**峰度**（即，数据分布的不对称程度及其与正态分布理想‘峰度’的偏离程度）。每个项目的适度标准差是可取的，因为我们的多数构念（例如，工作满意度、动机）在个体间自然有所不同。这种个体间的*变异性*是我们用来进行预测的依据（例如，“*为什么销售部门的工作满意度比研发部门高？*”）。对于偏度和峰度，我们理想上希望其值最小，因为这表明我们的数据呈正态分布，这是绝大多数统计模型（例如，回归分析）的假设。尽管某些偏度和峰度在具体构念上是可以接受的，或者甚至是正常的，但真正的问题出现在分数分布与正态分布的差异较大时（Warner, 2013）。

***注意***：某些变量自然不符合正态分布，不应在此使用。例如，问题：“*在过去一个月，你是否经历过工作场所事故？*”的频率数据确实是不符合正态分布的，因为绝大多数人会选择‘无’（或 0）。

***项目级分析与 MCDM***

首先，我们需要安装一些后续分析所需的程序。第一个是 MCDM 程序：scikit-criteria（请参阅文档 [这里](https://scikit-criteria.quatrope.org/en/latest/); 使用 Conda 安装可能需要一两分钟）。我们还需要导入 `pandas`、`skcriteria` 和 `scipy.stats` 的 *skew* 和 *kurtosis* 模块。

```py
conda install -c conda-forge scikit-criteria
```

```py
import pandas as pd
import skcriteria as skc

from scipy.stats import skew
from scipy.stats import kurtosis
```

*数据输入*

接下来，我们需要选择数据：1）自己的数据集或 2）练习数据集（如上所述，我使用了来自 IPIP-50 开源数据集的前 1000 个回答，这些回答涉及 10 个良心度项目）。

***注意***：如果你使用的是自己的数据集，你需要在进行其余分析之前清理数据（例如，处理缺失数据）。

```py
# Data file #

# 1) load your own datafile here 
# OR
# 2) Utilize the practice dataset of the first 1000 responses of IPIP-50 
# which is available at http://openpsychometrics.org/_rawdata/.
# For simplicity, we only utilized the 10-conscientious items (CSN)

## The original IPIP-50 survey can be found here: 
## https://ipip.ori.org/New_IPIP-50-item-scale.htm

Data = pd.read_csv(r'InsertFilePathHere.csv')
```

如果你使用练习数据集，某些项目需要重新编码（请参阅 [这里](https://ipip.ori.org/New_IPIP-50-item-scale.htm) 获取评分要点）。这确保所有回答在我们的李克特量表上具有相同的方向（例如，5 表示所有项目的高度良心回应）。

```py
#Recoding conscientiousness items
Data['CSN2'] = Data['CSN2'].replace({5:1, 4:2, 3:3, 2:4, 1:5})
Data['CSN4'] = Data['CSN4'].replace({5:1, 4:2, 3:3, 2:4, 1:5})
Data['CSN6'] = Data['CSN6'].replace({5:1, 4:2, 3:3, 2:4, 1:5})
Data['CSN8'] = Data['CSN8'].replace({5:1, 4:2, 3:3, 2:4, 1:5})
```

***注意***：对于这种方法，你应该一次只处理一个测量或‘量表’。例如，如果你想缩短你的工作满意度和组织文化测量，请分别对每个测量进行此分析。

*生成项目级统计数据*

接下来，我们收集所有需要的项目级统计数据，用于 scikit-criteria 以帮助进行最终的最佳项目排名。这包括标准差、偏度和峰度。需要注意的是，峰度程序这里利用了 Fisher 的峰度，其中正态分布的峰度为 0。

```py
## Standard Deviation ##
std = pd.DataFrame(Data.std())
std = std.T

## Skewness ##
skewdf = pd.DataFrame(skew(Data, axis=0, bias=False, nan_policy='omit'))
skewdf = skewdf.T
skewdf = pd.DataFrame(data=skewdf.values, columns=Data.columns)

## Kurtosis ##
kurtosisdf = pd.DataFrame(kurtosis(Data, axis=0, bias=False, nan_policy='omit'))
kurtosisdf = kurtosisdf.T
kurtosisdf = pd.DataFrame(data=kurtosisdf.values, columns=Data.columns)
```

*可选：主题专家评分（定义对应性）*

虽然**可选**，但如果你在学术或应用工作中建立新的量表或测量，强烈推荐收集主题专家（SME）评分。通常，SME 评分有助于建立内容效度或定义对应性，即你的项目与提供的定义的对应程度（Hinkin & Tracey, 1999）。这种方法涉及对少数个体进行调查，询问一个项目与您提供的定义在 1 (*完全不符合*) 到 5 (*完全符合*) 的李克特量表上相符的程度。如 Colquitt 等人（2019）所述，我们甚至可以使用这些信息计算 HTC 指数：定义对应性评分的平均值 / 可能的锚点数量。例如，如果 5 位 SME 对项目 *i* 的平均对应性评分为 4.20：4.20/5 = 0.84。

如果你已经收集了 SME 评分，你应该将其格式化并作为单独的数据框包含在这里。 **注意**：你应该将 SME 评分格式化为单列，每个项目列为一行。这将使合并不同的数据框成为可能。

```py
#SME = pd.read_csv(r'C:\XXX insert own filepath here)
#SME = SME.T
#SME.columns = Data.columns
```

*合并数据和绝对值*

现在，我们简单地合并这些不同的数据框（SME（可选）和项目级统计数据）。项目的名称需要在数据框之间匹配，否则 pandas 会添加额外的行。然后，我们转置数据以符合最终 scikit-criteria 程序的要求。

```py
mergeddata = pd.concat([std, skewdf, kurtosisdf], axis=0)
mergeddata.index = ['STD', 'Skew', "Kurtosis"]
mergeddata = mergeddata.T
mergeddata
```

最后，由于偏度和峰度的范围可以是负值或正值，我们取绝对值，因为这样更容易处理。

```py
mergeddata['Skew'] = mergeddata['Skew'].abs()
mergeddata['Kurtosis'] = mergeddata['Kurtosis'].abs()
```

*Scikit-criteria 决策矩阵和排名项目*

现在我们利用 scikit-criteria 决策程序根据多个标准对这些项目进行排名。如下面所示，我们必须传递数据框的值（`mergeddata.values`）、每个标准的输入目标（例如，最大值或最小值更为理想）和权重。虽然默认代码对每个标准具有相等的权重，但如果你使用 SME 评分，我强烈建议将更多的权重分配给这些评分。其他项目级统计数据只有在我们测量的构念是我们打算测量的构念时才重要！

最后，替代方案和标准仅是传递到 scikit-criteria 包中的名称，以便理解我们的输出。

```py
dmat = skc.mkdm(
    mergeddata.values, objectives=[max, min, min],
    weights=[.33, .33, .33],
    alternatives=["it1", "it2", "it3", "it4", "it5", "it6", "it7", "it8", "it9", "it10"],
    criteria=["SD", "Skew", "Kurt"])
```

*过滤器*

scikit-criteria 最棒的部分之一是它们的 `filters` 函数。这允许我们过滤掉不需要的项目级统计数据，并防止这些项目进入最终选择排名阶段。例如，如果一个项目的标准差极高——这表明回答问题的受访者差异很大，我们不希望该项目进入最终选择阶段。对于 SME 评分（如上所述为可选项），这一点尤其重要。在这里，我们只会保留那些分数高于最低阈值的项目——这可以防止那些定义对应极差的项目（例如，SME 平均评分为1或2）在其他项目级统计数据良好的情况下成为排名靠前的项目。下面是过滤器的应用，但由于我们的数据已经在这些值范围内，因此不会影响最终结果。

```py
from skcriteria.preprocessing import filters

########################### SD FILTER ###########################
# For this, we apply a filter: to only view items with SD higher than .50 and lower than 1.50
# These ranges will shift based upon your likert scale options (e.g., 1-5, 1-7, 1-100)

## SD lower limit filter
SDLL = filters.FilterGE({"SD": 0.50})
SDLL

dmatSDLL = SDLL.transform(dmat)
dmatSDLL

## SD upper limit filter
SDUL = filters.FilterLT({"SD": 1.50})
dmatSDUL = SDUL.transform(dmatSDLL)
dmatSDUL

## Whenever it is your final filter applied, I suggest changing the name
dmatfinal = dmatSDUL
dmatfinal

# Similarly, for SME ratings (if used), we may only want to consider items that have an SME above the median of our scale.
# For example, we may set the filter to only consider items with SME ratings above 3 on a 5-point likert scale

########################### SME FILTER ###########################

# Values are not set to run because we don't have SME ratings
# To utilize this: simply remove the # and change the decision matrix input
# in the below sections

#SMEFILT = filters.FilterGE({"SME": 3.00})

#dmatfinal = SME.transform(dmatSDUL)
#dmatfinal
```

***注意***：这也可以应用于偏度和峰度值。许多科学家会利用一个通用的经验规则，即偏度和峰度在-1.00到+1.00之间是可以接受的（Warner, 2013）；你只需创建如上所示的上限和下限过滤器，结合标准差即可。

*反转和缩放标准*

接下来，我们将偏度和峰度值反转，通过 `invert_objects.InvertMinimize()` 使所有标准最大化。scikit-criteria 程序更倾向于将所有标准最大化，因为这有助于最后一步（例如，总和权重）。最后，我们对每个标准进行缩放，以便于比较和权重求和。每个值都除以该列中所有标准的总和，以便于每个标准的最佳值比较（例如，it1 的标准差为 1.199，这除以列总数 12.031 得到 0.099）。

```py
# skcriteria prefers to deal with maxmizing all criteria
# Here, we invert our skewness and kurtosis. Higher values will then be more desirable

from skcriteria.preprocessing import invert_objectives, scalers

inv = invert_objectives.InvertMinimize()
dmatfinal = inv.transform(dmatfinal)

# Now we scale each criteria into an easy to understand 0 to 1 index
# The closer to 1, the more desirable the item statistic

scaler = scalers.SumScaler(target="both")
dmatfinal = scaler.transform(dmatfinal)
dmatfinal
```

![](../Images/6079314f5a8704f7e81a393561242ba5.png)

求和和排名之前的最终标准值。

*最终排名（总和权重）*

最后，我们可以使用多种方法来应用这个决策矩阵，但最简单的方法之一是计算加权总和。在这里，将每个项目的行进行求和（例如，标准差 + 偏度 + 峰度），然后由程序进行排名。

```py
## Now we simply rank these items ##

from skcriteria.madm import simple
decision = simple.WeightedSumModel()
ranking = decision.evaluate(dmatfinal)
ranking
```

对于练习数据集，排名如下：

![](../Images/e6d28ba49802984e2bdd664adbf28b23.png)

*保存第二步的数据*

最后，我们保存原始数据集和清理后的数据集用于第二步（这里是我们的原始‘Data’数据框，**不是**我们的决策矩阵‘dmatfinal’）。在第二步中，我们将输入在第一步中排名较高的项目。

```py
## Save this data for step 2 ##

Data.to_csv(r'C:\InputYourDesiredFilePathandName.csv')
```

# **第二步：最终项目优化以确保可靠性和有效性**

在第一步中，我们根据各项目的统计数据对所有项目进行了排序。现在，我们利用 R 中的优化应用程序**OASIS**（由 Cortina 等人开发，2020 年；见 [用户指南](https://orgscience.charlotte.edu/sites/orgscience.charlotte.edu/files/media/ScaleShorteningApp/Scale%20Shortening%20Shiny%20App%20Instructions_0.pdf)）。OASIS 计算器运行多个项目组合，并确定哪种项目组合可以产生最高的可靠性（以及适用时的收敛和区分效度）。在这个例子中，我们关注两个常见的可靠性指标：cronbach’s alpha 和 omega。这些指标通常非常相似，但许多研究人员主张 omega 应作为主要的可靠性指标（参见 Cho & Kim，2015；McNeish，2018）。**Omega** 是衡量可靠性的指标，它确定一组项目在单一“因素”（例如 *构念*，如工作满意度）上的加载程度。类似于 Cronbach's alpha（内部可靠性的一种衡量方式），值越高越好，通常在学术研究中，值超过 .70（最大上限 = 1.00）被认为是可靠的。

由于 shiny 应用程序的存在，OASIS 计算器使用起来非常简单。以下代码将安装所需的程序并弹出一个对话框（如下所示）。现在，我们从第一步中选择我们的原始清理数据集。在我们的示例中，我选择了前 8 个项目，要求至少 3 个项目和最多 8 个项目。如果你有收敛或区分效度测量，你可以在这一步输入它们。否则，我们请求计算 omega-h。

```py
install.packages(c("shiny","shinythemes","dplyr","gtools","Lambda4","DT","psych", "GPArotation", "mice"))
library(shiny)
runUrl("https://orgscience.uncc.edu/sites/orgscience.uncc.edu/files/media/OASIS.zip")
```

![](../Images/a7edb941bcc481da85f4be3992372025.png)

OASIS 计算器的输入

***最终结果***

如下所示，5 项目方案产生了最高的 omega (ω = .73) 和 Cronbach alpha 系数 (α = .75)，符合传统的学术可靠性标准。如果我们有收敛和区分效度的测量，我们还可以使用这些值对项目组合进行排序。OASIS 计算器还允许你为每个值选择一般范围（例如，只显示高于某些值的组合）。

![](../Images/8a9a89c3b8eff618d3c2c1470d6e2703.png)

让我们比较一下我们的最终解决方案：

![](../Images/17f6b1099c26cda1af71958943f44c11.png)

缩短量表与全长量表的比较

与完整的 10 项目测量相比，我们的最终项目集花费的时间只有一半，具有可比的、可接受的可靠性水平（ω 和 α >.70），稍高的标准差和较低的偏度，但不幸的是，峰度较高（不过，它仍在 -1.00 到 +1.00 的可接受范围内）。

这个最终缩短的项目集可能是一个非常合适的候选者来替代完整的测量。如果成功复制到所有调查测量中，这可能会将调查长度减少一半。用户可能需要采取额外步骤来验证新的缩短测量是否按预期工作（例如，预测有效性和调查名义网络——*缩短的测量是否与完整长度量表具有可比的预测？*）。

***警示***

1.  这种方法可能会产生在语法上冗余或缺乏内容覆盖的最终结果。用户应通过确保第二步中选择的最终项目集具有足够的内容覆盖，或使用OASIS计算器的内容映射功能（见[文档](https://orgscience.charlotte.edu/sites/orgscience.charlotte.edu/files/media/ScaleShorteningApp/Scale%20Shortening%20Shiny%20App%20Instructions_0.pdf)）来调整。例如，您可能有一个*人格*或*动机*评估，它有多个‘子因素’（例如，*您是否是外部或内在动机*）。如果您没有在OASIS计算器中进行内容映射或考虑这一点，您可能会仅得到来自一个子因素的条目。

1.  您的结果可能会因样本而有所不同。由于两个步骤都使用现有数据来‘最大化’结果，您可能会看到未来样本中的可靠性或项目级统计数据有所下降。然而，这不应是显著的。

1.  依赖于您的组织/样本，您的数据可能因为来源单一而自然偏斜。例如，如果公司X要求*所有经理*参与某些行为，那么询问这些行为的条目可能（希望）会偏斜（即，所有经理的评分都很高）。

# 结论

本文介绍了一种两步法，旨在显著减少调查问卷的长度，同时最大化可靠性和有效性。在以开源人格数据为例的说明中，调查问卷的长度减少了一半，但仍保持了高水平的Cronbach和Omega可靠性。虽然可能需要额外的步骤（例如，复制和比较预测有效性），但这种方法为用户提供了一种数据驱动的强大方法，可以显著减少员工调查的长度，这最终可以改善数据质量、减少受访者流失，并节省员工时间。

**参考文献**

J. B. Cabral, N. A. Luczywo 和 J. L. Zanazzi, Scikit-Criteria：集成到Python科学堆栈的多标准分析方法集合（2016），*第45届阿根廷计算机与运筹学会议（45JAIIO）——第十四届阿根廷运筹学研讨会（SIO）*，59–66。

E. Cho 和 S. Kim, Cronbach的α系数：知名但理解不深（2015），*组织研究方法*，*18*(2), 207–230。

J. Colquitt, T. Sabey, J. Rodell 和 E. Hill, 内容验证指南：定义对应性和定义独特性的评估标准（2019），*应用心理学期刊，104*(10), 1243–1265。

J. Cortina, Z. Sheng, S. Keener, K. Keeler, L. Grubb, N. Schmitt, S. Tonidandel, K. Summerville, E. Heggestad 和 G. Banks, 从阿尔法到欧米伽及更远！回顾《应用心理学期刊》中心理测量的过去、现在与（可能的）未来（2020），*《应用心理学期刊》, 105*(12), 1351–1381。

P. Edwards, I. Roberts, M. Clarke, C. DiGuiseppi, S. Pratap, R. Wentz 和 I. Kwan, 提高邮寄问卷的响应率：系统综述（2002），*《BMJ》*, *324*, 1–9。

M. Galesic 和 M. Bosnjak, 问卷长度对参与和响应质量指标的影响（2009），*《公众意见季刊》, 73*(2), 349–360。

L. Goldberg, 大五因素结构标记的发展（1992），*《心理评估》, 4*, 26–42。

T. Hinkin, 问卷测量开发的简要教程（1998），*《组织研究方法》*, *1*(1), 104–121。

T. Hinkin 和 J. Tracey, 一种方差分析方法用于内容验证（1999），*《组织研究方法》*, *2*(2), 175–186。

M. Hoerger, 互联网介导的大学研究中参与者退选与调查长度的关系：对研究设计和心理学研究中自愿参与的影响（2010），*《网络心理学、行为与社会网络》, 13*(6), 697–700。

B. Holtom, Y. Baruch, H. Aguinis 和 G. Ballinger, 调查响应率：趋势与有效性评估框架（2022），*《人际关系》, 75*(8), 1560–1584。

D. Jeong, S. Aggarwal, J. Robinson, N. Kumar, A. Spearot 和 D. Park, 详尽还是令人疲惫？关于长问卷中受访者疲劳的证据（2023），*《发展经济学期刊》*, *161*, 1–20。

P. Levy, *《工业/组织心理学：理解工作场所》*（第3版）（2010），Worth出版。

D. McNeish, 感谢系数阿尔法，我们会从这里继续（2018），*《心理学方法》, 23*(3), 412–433。

A. Peytchev 和 E. Peytcheva, 调查长度导致的测量误差减少：分裂问卷设计方法的评估（2017），*《调查研究方法》*, *4*(11), 361–368。

S. Porter, 提高响应率：有效的措施是什么？（2004），*《机构研究新方向》*, 5–21。

A. Rolstad 和 A. Rydén, 提高邮寄问卷的响应率：系统综述（2002）。*《BMJ》*, *324*。

R. Warner, *《应用统计学：从双变量到多变量技术》*（第2版）（2013），SAGE出版。

F. Yammarino, S. Skinner 和 T. Childers, 理解邮件调查响应行为的元分析（1991），*《公众意见季刊》*, *55*(4), 613–639。
