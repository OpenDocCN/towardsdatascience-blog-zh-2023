# 致力于数据科学家的聚类分析

> 原文：[https://towardsdatascience.com/cluster-analysis-for-aspiring-data-scientists-5ad88b700a1a?source=collection_archive---------8-----------------------#2023-02-23](https://towardsdatascience.com/cluster-analysis-for-aspiring-data-scientists-5ad88b700a1a?source=collection_archive---------8-----------------------#2023-02-23)

## 数据科学家如何进行和执行聚类分析的逐步案例研究

[](https://medium.com/@alex.vamvakaris.ds?source=post_page-----5ad88b700a1a--------------------------------)[![Alex Vamvakaris](../Images/bf8cf7c92a94d2b0d9242afcc03bf425.png)](https://medium.com/@alex.vamvakaris.ds?source=post_page-----5ad88b700a1a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5ad88b700a1a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5ad88b700a1a--------------------------------) [Alex Vamvakaris](https://medium.com/@alex.vamvakaris.ds?source=post_page-----5ad88b700a1a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8072260dd591&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcluster-analysis-for-aspiring-data-scientists-5ad88b700a1a&user=Alex+Vamvakaris&userId=8072260dd591&source=post_page-8072260dd591----5ad88b700a1a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5ad88b700a1a--------------------------------) ·11分钟阅读·2023年2月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5ad88b700a1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcluster-analysis-for-aspiring-data-scientists-5ad88b700a1a&user=Alex+Vamvakaris&userId=8072260dd591&source=-----5ad88b700a1a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5ad88b700a1a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcluster-analysis-for-aspiring-data-scientists-5ad88b700a1a&source=-----5ad88b700a1a---------------------bookmark_footer-----------)![](../Images/51486cbac4597e6f2474fa48835e4801.png)

图片由 [israel palacio](https://unsplash.com/@othentikisra?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，[Unsplash](https://unsplash.com/photos/ImcUkZ72oUs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

回到我本科统计学的时期，有一天特别突出。那是多变量分析模块的第一天。这个课程当时是新的，我们惊讶地发现教授决定做些不同的事情。教授没有讲解学期议程，而是关掉灯宣布今天我们将以不同的方式学习这个模块——通过观看《数字追凶》第一集。

该系列剧集关注FBI特工唐·埃普斯及其弟弟查尔斯·埃普斯，一位利用数据科学追踪最狡猾罪犯的杰出数学家。更具体地说，在首集里，查尔斯利用聚类分析来找出罪犯的起源地。不用说，我们都被吸引住了。

快进到今天，我经历了从电子商务零售到咨询和游戏的不同行业，所有业务利益相关者的一个共同愿望是对他们的客户或用户进行细分（聚类）。看来，聚类引起了数据科学家和业务利益相关者（以及观众）的兴趣。

在这篇文章中，我为希望将聚类分析加入工具箱、并朝着获得首个数据科学工作迈出一步的有志数据科学家创建了一个指南。该指南将分为两个部分：

+   **聚类介绍：** 理解聚类分析的三个基本构建块

+   **逐步案例研究：R中的聚类：** 数据可视化、数据处理、计算相似度矩阵（距离）、选择簇的数量和描述结果

# 1\. 聚类介绍

![](../Images/bbf8869d6740330c324feba10a521c9b.png)

图片来源：[Hans-Peter Gauster](https://unsplash.com/@sloppyperfectionist?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)  via [Unsplash](https://unsplash.com/photos/3y1zF4hIPCg?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

## **1.1\. 聚类的目标是什么？**

聚类用于根据定义的一组特征将实体分组。例如，我们可以使用聚类来根据客户的购物行为（特征）对客户（实体）进行分组。

## 1.2\. 聚类是如何工作的？

在**监督式机器学习技术**（如线性回归）中，算法在带标签的数据集上进行训练，目标是最大化对新未标记数据的预测准确性。例如，可以在包含房屋特征（如面积、卧室数量等）及其对应标签（销售价格）的带标签数据集上训练算法，目标是创建一个模型，该模型可以根据房屋的特征高准确度地预测新房屋的销售价格。

聚类，另一方面，是一种**无监督的机器学习技术**。数据集没有标签（因此称为无监督）。相反，目标是以簇分配的形式创建一个新标签。每次簇分析都会有所不同，但在每种情况下，主要有三个构建块或步骤：

1.  创建一个每行唯一的数据集，以便对所需聚类的实体进行操作。因此，如果你想对客户进行聚类，每一行必须代表一个不同的客户。对于这些行中的每一行，你将拥有描述特定特征的属性（列），例如过去一年中的收入、最喜欢的产品、距离上次购买的天数等。

1.  计算数据集中每一行与所有其他行的相似性（基于可用的属性）。因此，我们将得到第一行和第二行之间、第一行和第三行之间、第二行和第三行之间等等的相似性值。最常用的相似性函数是距离。我们将在下一个章节的案例研究中更详细地讨论这些，因为它们通过示例解释得更清楚。

1.  将相似性矩阵输入到聚类算法中。R和Python中有许多可用的聚类算法，如k-means、层次聚类和PAM。聚类算法的简单目的就是将观察值分配到簇中，以便簇内的观察值尽可能相似，而不同簇之间的观察值尽可能不同。最后，你将获得每一行（观察值）的簇分配。

# 2\. **R中的逐步聚类案例研究**

![](../Images/4c48f930657764eb19f4cdc0c706d50c.png)

图片来源于[Chris Lawton](https://unsplash.com/@chrislawton?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)在[Unsplash](https://unsplash.com/wallpapers/nature/forest?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

尽管我没有找到在系列节目“Numb3rs”首集中使用的数据集，但我认为使用类似的数据集进行案例研究是合适的，所以我选择了[US Arrests数据集](https://r-data.pmagunia.com/dataset/r-dataset-package-datasets-usarrests)，从[**datasets**](https://www.rdocumentation.org/packages/datasets/versions/3.6.2)包中加载（属于R中的**base**库）。

数据集包含50行和4个属性。每一行是一个美国州（实体）。这四个属性描述了每个州的以下特征：

+   **谋杀：** 1975年每10万人中的谋杀逮捕人数

+   **袭击：** 1975年每10万人中的袭击逮捕人数

+   **强奸：** 1975年每10万人中的强奸逮捕人数

+   **城市人口：** 1975年生活在城市地区的人口百分比

+   我们还有州名作为行名。这不会被用作聚类算法的输入。

```py
##############################
# Loading libraries
##############################
library("dplyr")        # summarizing data
library("ggplot2")      # visualization
library("cluster")      # gower distance and PAM
library("ggalluvial")   # alluvial plots

##############################
# Loading the data set
##############################
data("USArrests")

##############################
# Examine data set
##############################
head(USArrests)   # head -> header
```

![](../Images/a192c79152cb8b36830af00e1c0f604c.png)

R中USArrests数据集概述 [作者提供的图片]

## 2.1\. 数据可视化

从下面的箱线图来看，所有四个属性似乎都大致对称（UrbanPop略微右偏，Rape略微左偏）。如果某个属性严重偏斜或存在异常值，我们可以考虑应用不同的转换（例如对数转换）。但我们的数据集不需要这样做。

```py
##############################
# Box plot for each attribute
##############################
USArrests %>% 
  select(c(Murder, Assault, Rape, UrbanPop)) %>%
  boxplot(.,
    boxwex = 0.7, 
    alpha = 0.2, 
    col = c("red3", "lightgreen", "lightblue", "purple"), 
    horizontal = TRUE
  )
```

![](../Images/921fe627f879b9192b2289bc05415d57.png)

美国逮捕数据集每个属性的箱线图 [作者提供的图片]

你还可以看到**Assault**属性的值远高于其他三个。在聚类中，将所有属性标准化到相同尺度是一个良好的实践。这是为了确保大尺度的属性不会过度贡献（**Assault**在几百范围，而其他三个属性在几十范围）。幸运的是，我们将使用的相似度函数会处理这一点，因此我们在此阶段不需要进行任何重标定。

## 2.2\. 计算相似度矩阵

在我们的案例研究中，我们将使用R中的[**daisy**](https://www.rdocumentation.org/packages/cluster/versions/2.1.4/topics/daisy)函数（**cluster** 包）中的Gower距离来计算相似度矩阵。更具体地说，Gower使用不相似度作为距离函数，但概念是相同的（我们最小化不相似度而不是最大化相似度）。

> Gower是少数几种可以处理混合型数据（数值和分类属性）的距离函数之一。另一个优点是Gower将所有距离缩放到0和1之间（高尺度的属性不会过度贡献）。

因此，对于我们有50行（州）的数据集，我们将拥有一个50x50的矩阵，包含1225个独特的值（*n*(n-1)/2*）。对角线是不需要的（一个州与自身的距离），上三角和下三角是相同的（矩阵是对称的）。

```py
##############################
# Get distance
##############################
gower_dist <- 
  USArrests %>% 
  select(c(Murder, Assault, Rape, UrbanPop)) %>%
  daisy(.,metric = "gower")

gower_dist
```

![](../Images/5c7aefb9ba8a15cc64ff3f09363ead40.png)

Gower距离矩阵快照 [作者提供的图片]

## 2.3\. 选择簇的数量

在我们的案例研究中，我们将使用PAM（Partitioning Around Medoids）聚类算法，具体来说是[**pam**](https://www.rdocumentation.org/packages/cluster/versions/2.1.4/topics/pam)函数（**cluster** 包）。

> PAM（以及所有其他K-媒介算法）是k-means的一个稳健替代方案，因为它使用媒介作为簇中心，而不是均值。因此，相比k-means，算法对噪声和异常值的敏感度较低（但不是免疫的！）。

首先，我们需要选择簇的数量。确定簇数量的方法非常简单。我们将使用在前一步计算的Gower相似度矩阵运行PAM算法，并在每次运行中选择不同的簇数量（从2到10）。然后，我们将计算每次运行的平均轮廓宽度（ASW）。我们将使用以下函数来完成这一步。

```py
##############################
# Function to compute ASW 
##############################
get_asw_using_pam <- function(distance, min_clusters, max_clusters) {
  average_sil_width <- c(NA)
  for (i in min_clusters:max_clusters){
    pam_fit <- 
      pam(
        distance,
        diss = TRUE,
        k=i)
    average_sil_width[i] <- pam_fit$silinfo$avg.width
  }
  return(average_sil_width)
  }

############################
## Get ASW from each run
############################
sil_width <- get_asw_using_pam(gower_dist, 2, 10)
```

ASW表示每个观察值与其当前聚类相比与最近邻聚类的匹配程度。其范围从-1到1，较高的值（接近1）表示更好的聚类结果。我们将使用轮廓图（y轴上的ASW和x轴上的聚类数）来直观地检查我们聚类的有前景的候选者。

```py
##############################
# Visualize Silhouette Plot
##############################
silhouette_plot <- sil_width %>% as.data.frame()
names(silhouette_plot)[names(silhouette_plot) == '.'] <- 'sil_width'

silhouette_plot %>%
  mutate(number = c(1:10)) %>%
  filter(number > 1) %>%
  ggplot(aes(x = number , y = sil_width)) +
  geom_line( color="turquoise3", size=1) +
  geom_point(color="darkgrey",fill = "black", size=3) +
  scale_x_continuous(breaks = seq(2, 10, 1) ) +
  ylab("Average Silhouette Width") +
  xlab("No of Clusters") +
  theme_classic()
```

![](../Images/60abd1cebaff284b93b8aa8d1f42d5f6.png)

使用PAM和Gower距离的轮廓图 [Image by the author]

ASW最高的方案是两个聚类，在急剧下降后，我们有三个和四个聚类，然后ASW再次急剧下降。我们可能的候选方案是这三个选项（2、3和4个聚类）。我们将把每个运行的聚类分配保存为USArrests数据集中的三个新属性（cluster_2、cluster_3和cluster_4）。

```py
## Set the seed of R's random number generator
## which is useful for creating reproducible simulations
## like in cases of clustering assignment
set.seed(5)

##############################
# Saving PAM results
##############################
pam_2 <- pam(gower_dist, diss = TRUE, k= 2 )
pam_3 <- pam(gower_dist, diss = TRUE, k= 3 )
pam_4 <- pam(gower_dist, diss = TRUE, k= 4 )

##############################
# Adding assignment as columns
##############################
USArrests <-
  USArrests %>%
  mutate(cluster_2 = as.factor(pam_2$clustering)) %>%
  mutate(cluster_3 = as.factor(pam_3$clustering)) %>%  
  mutate(cluster_4 = as.factor(pam_4$clustering))
```

## 2.4\. 描述聚类方案

由于聚类是一种无监督技术，因此重要的是要记住，它本质上是加了“兴奋剂”的探索性数据分析（EDA）。因此，与有监督技术不同，我们无法评估我们的聚类方案的准确性（数据集中没有用于比较的标签）。相反，您的聚类分析的“好坏”将基于不同的标准来评估：

> 结果聚类是否以反映业务利益相关者所考虑的方面的方式进行分离？

因此，最终的决定是基于除最高ASW方案之外的其他因素。它是通过检查每个聚类方案的特征来做出的。你应该始终选择对业务更具可操作性的聚类，而不是仅仅选择ASW最高的那个。

首先，我们希望探讨不同聚类方案之间的关系。我们将使用全uvial图来可视化在三个运行之间观察值（状态）的流动。

```py
##############################
# Alluvial plot
##############################
aluv <- 
  USArrests %>%
  group_by(cluster_2, cluster_3, cluster_4) %>%
  summarise(Freq = n()) 

alluvial(
  aluv[,1:3],
  freq=aluv$Freq, 
  border="lightgrey", 
  gap.width=0.2,
  alpha=0.8,
  col =  
    ifelse( 
      aluv$cluster_4 == 1, "red", 
      ifelse(aluv$cluster_4 == 2, "lightskyblue", 
      ifelse(aluv$cluster_4 == 3 & aluv$cluster_3 == 2, "lightskyblue4", 
      ifelse(aluv$cluster_4 == 3 & aluv$cluster_3 == 2, "purple",              
      "orange")))),
  cex=0.65
  ) 
```

![](../Images/b0e25d6bf3a5809afb09eac65aa24e42.png)

PAM不同运行的聚类分配的全uvial图 [Image by the author]

在上述全uvial图中，每一列代表一个不同的运行，不同颜色的带子表示固定的状态块，当它们在三个聚类运行之间分裂或重新分配时。

例如，红色带子代表阿拉巴马州、乔治亚州、路易斯安那州、密西西比州、北卡罗来纳州、南卡罗来纳州和田纳西州（见下方代码）。这些州在两个和三个聚类的运行（cluster_2和cluster_3）中与蓝色带子的州分配到了相同的聚类（“1”），但在四个聚类的运行（cluster_4）中被分开。

```py
##################################################
# Filter for the red ribbon (cluster_4 = 1)
##################################################
USArrests %>% filter(cluster_4 == "1")
```

![](../Images/c32d4ba1de826372746eba7b15d4c5a4.png)

在全uvial图中红色带子的状态 [Image by the author]

接下来，我们希望更好地理解每个聚类的特征以及它们在三个解决方案之间的差异。

```py
##################################################
# Summarizing (average) attributes, 2 clusters
##################################################
USArrests %>% 
  group_by(cluster_2)  %>%
  summarise(
    count = n(),
    mean_murder = mean(Murder),
    mean_assault = mean(Assault),
    mean_rape = mean(Rape),
    mean_urbanpop = mean(UrbanPop)
    )

##################################################
# Summarizing (average) attributes, 3 clusters
##################################################
USArrests %>% 
  group_by(cluster_3)  %>%
  summarise(
    count = n(),
    mean_murder = mean(Murder),
    mean_assault = mean(Assault),
    mean_rape = mean(Rape),
    mean_urbanpop = mean(UrbanPop)
    )

##################################################
# Summarizing (average) attributes, 4 clusters
##################################################
USArrests %>% 
  group_by(cluster_4)  %>%
  summarise(
    count = n(),
    mean_murder = mean(Murder),
    mean_assault = mean(Assault),
    mean_rape = mean(Rape),
    mean_urbanpop = mean(UrbanPop)
    )
```

![](../Images/7ccac6397ba79535347fc9351ca193a6.png)

按聚类运行的描述性统计 [Image by the author]

**两个聚类的运行：**

+   聚类“1”在所有犯罪类别中的平均逮捕次数较高

+   平均城市人口百分比没有明显差异

**使用三个簇的运行：**

+   这三个簇似乎分隔得很好

+   与使用两个簇的运行中的高（簇“1”）和低（簇“2”）逮捕相比，我们现在有高（簇“1”）、中等（簇“2”）和低逮捕（簇“3”）

+   簇“3”也有明显更低的平均城市人口百分比

**使用四个簇的运行：**

+   分隔不如预期清晰

+   使用三个簇运行得到的中等（簇“2”）和低（簇“3”）簇保持不变（仅分别重命名为“3”和“4”）

+   使用三个簇的运行中的高（簇“1”）被分成了两个簇。簇“1”具有明显较低的平均城市人口百分比，并且强奸和攻击的逮捕率较低

根据簇分析的需求，我们会选择合适的聚类解决方案。记住，不是ASW最高的那个，而是对业务利益相关者更具影响力的那个（可能是在3个和4个簇之间选择）。

# 摘要

🚀🚀 完成最后一步后，我们已经到达了指南的终点。下面，你还可以找到步骤的快速总结：

✅ 创建一个数据集，其中每一行代表一个实体，属性反映你聚类感兴趣的方面

✅ 可视化数据以检查偏斜、离群值和缩放问题

✅ 使用轮廓图选择簇的数量

✅ 检查每个聚类解决方案的特征，并选择对业务更具操作性的那个（更符合业务目标）

在下面的链接中，你还可以找到一个关于使用数据科学技术和最佳实践在现实商业场景中完成客户簇分析的更深入的免费PDF指南。 👇

[](https://www.aspiringdatascientist.net/community?source=post_page-----5ad88b700a1a--------------------------------) [## 数据科学项目检查清单 — 有志数据科学家

### 我是一名拥有7年以上分析经验的数据科学家，目前在英国伦敦的一家游戏公司工作。我的...

www.aspiringdatascientist.net](https://www.aspiringdatascientist.net/community?source=post_page-----5ad88b700a1a--------------------------------)

# **保持联系！**

如果你喜欢阅读这篇文章并想了解更多，请不要忘记[订阅](https://medium.com/@alex.vamvakaris.ds/subscribe)，以便直接将我的故事发送到你的收件箱。

# 参考文献

R文档和数据集来自R项目，并且是GPL许可证。OpenIntro文档是Creative Commons BY-SA 3.0许可证。
