# 推导显示地理区域相对社会经济优势和劣势的评分

> 原文：[https://towardsdatascience.com/deriving-a-score-to-show-relative-socio-economic-advantage-and-disadvantage-of-a-geographic-area-25ffe684bd3f?source=collection_archive---------6-----------------------#2023-12-29](https://towardsdatascience.com/deriving-a-score-to-show-relative-socio-economic-advantage-and-disadvantage-of-a-geographic-area-25ffe684bd3f?source=collection_archive---------6-----------------------#2023-12-29)

## 使用主成分分析（PCA）与实际数据

[](https://jin-cui.medium.com/?source=post_page-----25ffe684bd3f--------------------------------)[![Jin Cui](../Images/e5ddcbaa6d7da38f960d2c5fea71b538.png)](https://jin-cui.medium.com/?source=post_page-----25ffe684bd3f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----25ffe684bd3f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----25ffe684bd3f--------------------------------) [Jin Cui](https://jin-cui.medium.com/?source=post_page-----25ffe684bd3f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F333e9ae026bc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fderiving-a-score-to-show-relative-socio-economic-advantage-and-disadvantage-of-a-geographic-area-25ffe684bd3f&user=Jin+Cui&userId=333e9ae026bc&source=post_page-333e9ae026bc----25ffe684bd3f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----25ffe684bd3f--------------------------------) ·9分钟阅读·2023年12月29日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F25ffe684bd3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fderiving-a-score-to-show-relative-socio-economic-advantage-and-disadvantage-of-a-geographic-area-25ffe684bd3f&user=Jin+Cui&userId=333e9ae026bc&source=-----25ffe684bd3f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F25ffe684bd3f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fderiving-a-score-to-show-relative-socio-economic-advantage-and-disadvantage-of-a-geographic-area-25ffe684bd3f&source=-----25ffe684bd3f---------------------bookmark_footer-----------)![](../Images/8b176f6dced012647f86781bc3b928ef.png)

图片由 [Vista Wei](https://unsplash.com/@weista?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 动机

存在公开获取的数据，描述了地理位置的社会经济特征。在我所在的澳大利亚，政府通过[澳大利亚统计局](https://www.abs.gov.au/)（ABS）定期收集和发布有关收入、职业、教育、就业和住房的个人和家庭数据，按区域级别发布。一些发布的数据点示例如下：

+   收入相对较高/较低的人的百分比

+   在各自职业中被归类为管理者的人的百分比

+   没有正式教育水平的人的百分比

+   失业人数的百分比

+   具有4个或更多卧室的房产百分比

虽然这些数据点似乎重点关注个人，但它反映了人们对物质和社会资源的获取及其在特定地理区域内参与社会的能力，*最终*反映了该区域的社会经济优势和劣势。

给定这些数据点，有没有办法推导出一个评分，从最有利到最不利对地理区域进行排名？

# 问题

目标是推导一个评分，可以将其制定为回归问题，其中每个数据点或特征用于预测目标变量，在这种情况下是一个数值评分。这需要目标变量在某些实例中可用以训练预测模型。

然而，由于我们没有目标变量作为起点，我们可能需要用另一种方式来解决这个问题。例如，在假设每个地理区域在社会经济上有所不同的前提下，我们是否可以理解哪些数据点有助于解释最多的变异，从而基于这些数据点的数值组合推导出一个评分。

我们可以使用一种叫做主成分分析（PCA）的技术来实现这一点，本文演示了如何操作！

# 数据

ABS在此[网页](https://www.abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/latest-release#data-downloads)的“数据下载”部分发布指示地理区域社会经济特征的数据点，数据位于“标准化变量比例数据立方体”[1]中。这些数据点以[统计区域1（SA1）](https://www.abs.gov.au/statistics/standards/australian-statistical-geography-standard-asgs-edition-3/jul2021-jun2026/main-structure-and-greater-capital-city-statistical-areas/statistical-area-level-1)级别发布，这是一个将澳大利亚划分为大约200–800人的区域的数字边界。这比邮政编码（Zipcode）或州的数字边界要更为详细。

为了在本文中进行演示，我将基于上述数据源表 1 中提供的 44 个数据点中的 14 个推导出社会经济评分（稍后我会解释为什么选择这个子集）。这些是：

+   INC_LOW: 生活在年家庭等效收入在 1 至 25,999 澳元之间的家庭中的人口百分比

+   INC_HIGH: 年家庭等效收入超过 91,000 澳元的人口百分比

+   UNEMPLOYED_IER: 15 岁及以上失业人口的百分比

+   HIGHBED: 占用私人物业中有四个或更多卧室的百分比

+   HIGHMORTGAGE: 支付每月按揭金额大于 2,800 澳元的占用私人物业的百分比

+   LOWRENT: 每周租金少于 250 澳元的占用私人物业的百分比

+   OWNING: 不带按揭的占用私人物业的百分比

+   MORTGAGE: 有按揭的占用私人物业的百分比

+   GROUP: 占用私人物业中属于群体占用的私人物业的百分比（例如公寓或单元）

+   LONE: 占用物业中仅有一个人居住的私人物业的百分比

+   OVERCROWD: 根据加拿大国家占用标准，需要一个或多个额外卧室的占用私人物业的百分比

+   NOCAR: 占用私人物业中没有汽车的百分比

+   ONEPARENT: 单亲家庭的百分比

+   UNINCORP: 至少有一个人是企业主的物业的百分比

# 步骤

在这一部分，我将逐步展示如何使用主成分分析（PCA）从 Python 代码中推导出澳大利亚 SA1 区域的社会经济评分。

我将从加载所需的 Python 包和数据开始。

```py
## Load the required Python packages

### For dataframe operations
import numpy as np
import pandas as pd

### For PCA
from sklearn.decomposition import PCA
from sklearn.preprocessing import StandardScaler

### For Visualization
import matplotlib.pyplot as plt
import seaborn as sns

### For Validation
from scipy.stats import pearsonr
```

```py
## Load data

file1 = 'data/standardised_variables_seifa_2021.xlsx'

### Reading from Table 1, from row 5 onwards, for column A to AT
data1 = pd.read_excel(file1, sheet_name = 'Table 1', header = 5,
                      usecols = 'A:AT') 
```

```py
## Remove rows with missing value (113 out of 60k rows)

data1_dropna = data1.dropna()
```

在执行 PCA 之前，一个重要的清理步骤是将每个 14 个数据点（特征）标准化到均值为 0 和标准差为 1。这主要是为了确保 PCA 分配给每个特征的载荷（可以将其视为特征重要性的指示器）在特征之间是可比的。否则，可能会对一个实际上并不重要的特征给予更多的重视或更高的载荷，反之亦然。

请注意，上述引用的 ABS 数据源已经标准化了特征。也就是说，对于未标准化的数据源：

```py
## Standardise data for PCA

### Take all but the first column which is merely a location indicator
data_final = data1_dropna.iloc[:,1:]

### Perform standardisation of data
sc = StandardScaler()
sc.fit(data_final)

### Standardised data
data_final = sc.transform(data_final)
```

使用标准化数据，PCA 可以用几行代码完成：

```py
## Perform PCA

pca = PCA()
pca.fit_transform(data_final)
```

PCA 旨在通过主成分（PC）来表示基础数据。PCA 中提供的主成分数量等于数据中标准化特征的数量。在这种情况下，返回了 14 个主成分。

每个主成分（PC）是所有标准化特征的线性组合，只是通过其各自的标准化特征的载荷进行区分。例如，下面的图像展示了分配给第一个和第二个主成分（PC1 和 PC2）的特征载荷。

![](../Images/8059fff5621020ec3dcacb90d6731f51.png)

图 1 — 返回前两个主成分的代码。图片由作者提供。

使用14个主成分，下面的代码提供了每个主成分解释了多少变异性的可视化：

```py
 ## Create visualization for variations explained by each PC

exp_var_pca = pca.explained_variance_ratio_
plt.bar(range(1, len(exp_var_pca) + 1), exp_var_pca, alpha = 0.7,
            label = '% of Variation Explained',color = 'darkseagreen')

plt.ylabel('Explained Variation')
plt.xlabel('Principal Component')
plt.legend(loc = 'best')
plt.show()
```

如下图所示，主成分1（PC1）在原始数据集中占据了最大的变异比例，而每个后续的主成分解释的变异较少。具体来说，PC1解释了数据中约35%的变异。

![](../Images/53a2272a8b4aafdc907ce19ec5dfdda2.png)

图像2 — 变异性由主成分（PC）解释。图片由作者提供。

为了演示本文，PC1被选择为唯一的主成分来推导社会经济评分，原因如下：

+   PC1在数据中相对地解释了足够大的变异性。

+   尽管选择更多主成分可能会（略微）解释更多的变异性，但在特定地理区域的社会经济优势和劣势背景下，这使得评分的解释变得困难。例如，如下图所示，PC1和PC2可能提供关于特定特征（如‘INC_LOW’）如何影响地理区域社会经济变异的相互矛盾的叙述。

```py
## Show and compare loadings for PC1 and PC2

### Using df_plot dataframe per Image 1

sns.heatmap(df_plot, annot = False, fmt = ".1f", cmap = 'summer') 
plt.show()
```

![](../Images/1a4ec9f689c86fd7fac2d557182dae72.png)

图像3 — PC1和PC2的不同载荷。图片由作者提供。

要为每个SA1获得评分，我们只需将每个特征的标准化部分乘以其PC1载荷。可以通过以下方式实现：

```py
 ## Obtain raw score based on PC1

### Perform sum product of standardised feature and PC1 loading
pca.fit_transform(data_final)

### Reverse the sign of the sum product above to make output more interpretable
pca_data_transformed = -1.0*pca.fit_transform(data_final)

### Convert to Pandas dataframe, and join raw score with SA1 column
pca1 = pd.DataFrame(pca_data_transformed[:,0], columns = ['Score_Raw'])
score_SA1 = pd.concat([data1_dropna['SA1_2021'].reset_index(drop = True), pca1]
                    , axis = 1)

### Inspect the raw score
score_SA1.head()
```

![](../Images/be0bbe2d495f6cab155ac1cd9de52519.png)

图像4 — 按SA1划分的原始社会经济评分。图片由作者提供。

分数越高，SA1在获取社会经济资源方面越有优势。

# 验证

我们怎么知道我们得出的评分甚至远远正确？

为了提供背景，ABS实际发布了一个称为[经济资源指数（IER）](https://www.abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/latest-release#index-of-economic-resources-ier-)的社会经济评分，ABS网站上定义为：

*“经济资源指数（IER）专注于相对社会经济优势和劣势的财务方面，通过总结与收入和住房相关的变量。IER排除了教育和职业变量，因为它们不是经济资源的直接衡量标准。它还排除了储蓄或股权等资产，虽然相关，但不能包括，因为这些资产在普查中未被收集。”*

在不透露详细步骤的情况下，ABS在其[技术报告](https://www.abs.gov.au/statistics/detailed-methodology-information/concepts-sources-methods/socio-economic-indexes-areas-seifa-technical-paper/2021)中表示，IER的推导使用了与我们上面执行的相同的特征（14）和方法（PCA，仅PC1）。也就是说，如果我们推导了正确的分数，它们应该可以与在[此处](https://www.abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/latest-release#data-downloads)发布的IER分数进行比较（“Statistical Area Level 1, Indexes, SEIFA 2021.xlsx”，表4）。

由于发布的分数是标准化到均值1,000和标准差100，我们首先通过将原始分数标准化到相同的水平来进行验证：

```py
## Standardise raw scores

score_SA1['IER_recreated'] = 
          (score_SA1['Score_Raw']/score_SA1['Score_Raw'].std())*100 + 1000 
```

为了进行比较，我们读取了按SA1发布的IER分数：

```py
## Read in ABS published IER scores
## similarly to how we read in the standardised portion of the features

file2 = 'data/Statistical Area Level 1, Indexes, SEIFA 2021.xlsx'

data2 = pd.read_excel(file2, sheet_name = 'Table 4', header = 5,
                      usecols = 'A:C')

data2.rename(columns =  {'2021 Statistical Area Level 1 (SA1)': 'SA1_2021', 'Score': 'IER_2021'}, inplace = True)

col_select = ['SA1_2021', 'IER_2021']
data2 = data2[col_select]

ABS_IER_dropna = data2.dropna().reset_index(drop = True)
```

**验证 1— PC1负荷**

如下图所示，将上述得到的PC1负荷与[ABS发布的PC1负荷](https://www.abs.gov.au/statistics/detailed-methodology-information/concepts-sources-methods/socio-economic-indexes-areas-seifa-technical-paper/2021/construction-indexes)进行比较，表明它们之间相差-45%的常数。由于这只是一个缩放差异，因此不会影响标准化后的分数（均值为1,000，标准差为100）。

![](../Images/e3b3dcd7d33667dc3d04740a0ff09039.png)

图像5 — 比较PC1负荷。作者提供的图像。

（你应该能够用图像1中的PC1负荷来验证“Derived (A)”列）。

**验证 2— 分数分布**

以下代码创建了两个分数的直方图，其形状看起来几乎相同。

```py
## Check distribution of scores

score_SA1.hist(column = 'IER_recreated', bins = 100, color = 'darkseagreen')
plt.title('Distribution of recreated IER scores')

ABS_IER_dropna.hist(column = 'IER_2021', bins = 100, color = 'lightskyblue')
plt.title('Distribution of ABS IER scores')

plt.show()
```

![](../Images/27d3bd0c58d1e0f638a42b9ffdc14864.png)

图像6— IER分布，重建与发布。作者提供的图像。

**验证 3— SA1的IER分数**

作为**最终**验证，我们比较一下由SA1得出的IER分数：

```py
 ## Join the two scores by SA1 for comparison
IER_join = pd.merge(ABS_IER_dropna, score_SA1, how = 'left', on = 'SA1_2021')

## Plot scores on x-y axis. 
## If scores are identical, it should show a straight line.

plt.scatter('IER_recreated', 'IER_2021', data = IER_join, color = 'darkseagreen')
plt.title('Comparison of recreated and ABS IER scores')
plt.xlabel('Recreated IER score')
plt.ylabel('ABS IER score')

plt.show()
```

如下图所示的对角直线表明这两个分数基本相同。

![](../Images/4eacbb132b7c4bb99093f1cb7d8505b5.png)

图像7— 按SA1比较分数。作者提供的图像。

此外，以下代码显示了两个分数之间的相关性接近1：

![](../Images/1d4c76d9728404d0a485515231d79a18.png)

图像8— 重建分数与发布分数之间的相关性。作者提供的图像。

# 总结性思考

本文的演示有效复制了ABS如何校准IER，这是其发布的四个社会经济指数之一，可用于排名地理区域的社会经济状况。

退一步看，本质上我们所取得的成就是将数据的维度从14减少到1，丢失了一些数据传达的信息。

PCA等降维技术也常用于将高维空间（如文本嵌入）减少到2–3个（可视化的）主成分。

# 参考资料

[1] 澳大利亚统计局（2021年），[地区社会经济指数（SEIFA）](https://www.abs.gov.au/statistics/people/people-and-communities/socio-economic-indexes-areas-seifa-australia/latest-release#data-downloads)，ABS 网站，访问日期：2023年12月29日（[知识共享许可](https://www.abs.gov.au/privacy-and-legals)）

# 致谢

我从这个 [精算师学会代码库](https://actuariesinstitute.github.io/cookbook/docs/DAA_M06_CS1.html) 中获取了一些 Python 代码，该库试图复制 ABS 发布的另一个指数（尽管我设法以更高的精度部分复制了 IER，如图 7 所示）。

*在我乘风AI/ML浪潮之际，我喜欢以一种全面的语言编写和分享逐步指南和操作教程，并附带可运行的代码。如果你想访问我所有的文章（以及来自其他实践者/作者在 Medium 上的文章），你可以通过* [*这个链接*](https://medium.com/@jin-cui/membership) *注册！*
