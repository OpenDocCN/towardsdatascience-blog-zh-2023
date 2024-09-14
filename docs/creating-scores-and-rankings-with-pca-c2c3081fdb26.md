# 使用 PCA 创建评分和排名

> 原文：[`towardsdatascience.com/creating-scores-and-rankings-with-pca-c2c3081fdb26`](https://towardsdatascience.com/creating-scores-and-rankings-with-pca-c2c3081fdb26)

## 使用 R 语言基于多个变量为观测值创建评分

[](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c2c3081fdb26--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2c3081fdb26--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c2c3081fdb26--------------------------------) ·9 分钟阅读·2023 年 4 月 10 日

--

![](img/ad848fe08cc9a8bc744f2e58a7a97f9b.png)

[Joshua Golde](https://unsplash.com/@joshgmit?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供的照片，来源于 [Unsplash](https://unsplash.com/photos/qIu77BsFdds?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

我对主成分分析[PCA]的研究越多，我越喜欢这个工具。我已经写过其他关于这个主题的文章，但我不断学习更多关于这个美丽数学“幕后”的知识，当然，我会与您分享这些知识。

主成分分析（PCA）是一组基于数据协方差和相关性的数学变换。它基本上会查看数据点，找出变异性最大的位置。一旦完成，这些数据会被投影到那个方向。新数据会被投影到一个新的轴上，称为**主成分**。

> 数据被投影到一个新的轴上，以解释尽可能多的变异性。

投影本身就是变换。而新数据具有许多属性，可以帮助我们数据科学家更好地分析数据。例如，我们可以进行因子分析，将相似的变量组合成一个因子，从而减少数据的维度。

另一个有趣的属性是通过观察相似性来创建排名的可能性，如我们将在本文中看到的。

# 使用 PCA 创建评分和排名

## 数据集

在这个练习中，我们将使用`mtcars`，一个著名的“玩具数据集”，包含有关汽车的一些信息。尽管这是一个非常著名的数据，但它仍然非常适合作为教学示例，并且是开放的，许可证为 GPL 3.0。

我们还可以加载`tidyverse`库进行数据整理，使用`psych`进行 PCA。

```py
# imports
library(tidyverse)
library(psych)

# dataset
data("mtcars")
```

这里是数据的小提取。

![](img/2be9052ceef2a7acad0fc836ddcad729.png)

mtcars：在 Dplyr 中原生支持。图片由作者提供。

## 编码

现在，让我们开始编写代码吧。

重要的是要说明，PCA 和因子分析仅适用于定量数据。因此，如果你有定性或分类数据，可能**对应分析**会更适合你的情况。

使用 PCA 进行良好的因子提取需要变量对之间存在统计显著的相关性。如果相关性矩阵中有太多低相关性，则提取的因子可能不会很好。

## Bartlett 检验

但是如何确保这些因子呢？我们可以使用**Bartlett 检验**，在*零假设*下，相关性在统计上等于零[p-value > 0.05]，而在*对立假设*下，相关性不同于 0 [p-value ≤ 0.05]。

```py
# Bartlett Test
cortest.bartlett(mtcars)

# RESULT
$chisq
[1] 408.0116

$p.value
[1] 2.226927e-55

$df
[1] 55
```

如我们所见，我们的结果是 p 值等于 0，因此可以拒绝零假设，我们可以理解提取的因子将是合适的。

接下来，我们可以使用 `psych` 库运行 PCA 部分。我们可以使用 `pca()` 函数完成这项任务。我们将输入：

+   数据集（仅包含数值）

+   所需因子的数量。在这种情况下，所有的 11 个，因此我们使用数据的第二维度位置（`dim(mtcars)[2]`）

+   旋转方法：`none`。现在这可能会改变我们的结果，正如我们将看到的。默认的旋转是 `“varimax”`，其目的是最大化因子载荷的方差，得到一个更简单的矩阵，其中每个变量高度关联于一个或几个因子，从而更易于解释。

```py
#PCA
pca <- pca(mtcars, nfactors=dim(mtcars)[2], rotate='none')
```

一旦代码运行完成，我们可以查看**碎石图**，它会告诉我们每个主成分捕获的方差量。

```py
# Scree Plot
barplot(pca$Vaccounted[2,], col='gold')
```

接下来，结果将被显示出来。

![](img/91c6acdf1c900aa880f9617b68078e72.png)

**碎石图**。84% 的方差由前两个组件捕获。图片由作者提供。

## 凯泽准则

下一步是查看我们将用于分析的主成分。一个好的方法是查看特征值，并确定哪些值大于 1。这个规则也被称为凯泽的潜在根准则。

```py
# Eigenvalues
pca$values

[1] 6.60840025 2.65046789 0.62719727 0.26959744 0.22345110 0.21159612
[7] 0.13526199 0.12290143 0.07704665 0.05203544 0.02204441
```

注意： (1) 有 11 个特征值，每个主成分一个； (2) 只有前两个符合凯泽规则的要求。因此，我们再对两个成分运行一次 PCA。

```py
# PCA after Kaiser's rule applied
pca2 <- pca(mtcars, nfactors=2, rotate='none')

# Variance
pca2$Vaccounted

                            PC1       PC2
Proportion Var        0.6007637 0.2409516
Cumulative Var        0.6007637 0.8417153
```

## 绘制变量

为了绘制变量，我们需要先收集载荷。载荷矩阵显示了每个变量与每个主成分的相关性。因此，数值将在 -1 和 1 之间，记住越接近零，主成分和变量的相关性越小。越接近 1/-1，相关性越强。

> 载荷是变量与主成分的相关程度。

```py
# PCA Not rotated
loadings <- as.data.frame(unclass(pca2$loadings))
# Adding row names as a column
loadings <- loadings %>% rownames_to_column('vars')

# RESULT
   vars        PC1         PC2
1   mpg -0.9319502  0.02625094
2   cyl  0.9612188  0.07121589
3  disp  0.9464866 -0.08030095
4    hp  0.8484710  0.40502680
5  drat -0.7561693  0.44720905
6    wt  0.8897212 -0.23286996
7  qsec -0.5153093 -0.75438614
8    vs -0.7879428 -0.37712727
9    am -0.6039632  0.69910300
10 gear -0.5319156  0.75271549
11 carb  0.5501711  0.67330434
```

然后，由于我们只有两个维度，我们可以轻松地使用 ggplot2 绘制它们。

```py
# Plot variables
ggplot(loadings, aes(x = PC1, y = PC2, label = vars)) +
  geom_point(color='purple', size=3) +
  geom_text_repel() +
  theme_classic()
```

显示的图形如下。

![](img/d90ff640f1489a49d8ae0bb3afc27b48.png)

载荷图，展示了基于 PC1 x PC2 的变量之间的关系。图片由作者提供。

太棒了！现在我们对哪些变量彼此相关有了一个很好的了解。例如，油耗（Miles Per Gallon）与齿轮数、发动机类型、传动类型、drat 等因素的相关性更大。另一方面，它与 HP 和重量正好相反，这非常有意义。我们思考一下：*一辆车的动力越大，燃烧的油也越多。同样，重量也如此。移动一辆更重的车需要更多的动力和油，导致油耗比率更低。*

# 旋转版本

好的，现在我们查看了未旋转的 PCA 版本，让我们来看一下默认的`"varimax"`旋转的旋转版本。

```py
# Rotation Varimax
prin2 <- pca(mtcars, nfactors=2, rotate='varimax')

# Variance
prin2$Vaccounted
                            RC1       RC2
Proportion Var        0.4248262 0.4168891
Cumulative Var        0.4248262 0.8417153

# PCA Rotated
loadings2 <- as.data.frame(unclass(prin2$loadings))
loadings2 <- loadings2 %>% rownames_to_column('vars')

# Plot
ggplot(loadings2, aes(x = RC1, y = RC2, label = vars))+
  geom_point(color='tomato', size=8)+
  geom_text_repel() +
  theme_classic()
```

由 2 个组件捕获的方差相同（84%）。但请注意，现在方差的分布更加分散。旋转后的组件 RC1 [42%]和 RC2 [41%]；与没有旋转的版本中的 PC1 [60%]和 PC2 [24%]相比。然而，变量保持在类似的位置，只是现在稍微旋转了一下。

![](img/367607fcb8d50c6a8878b24310bc5b4e.png)

加载量图，显示基于 RC1 x RC2 的变量之间的关系。图片由作者提供。

## 共同性

对于带旋转和不带旋转的两种主成分分析（PCA），最后的比较是关于共同性。共同性会显示在应用凯瑟规则并从分析中排除了一些主成分后，每个变量的方差损失了多少。

```py
# Comparison of communalities
communalities <- as.data.frame(unclass(pca2$communality)) %>%
  rename(comm_no_rot = 1) %>%
  cbind(unclass(prin2$communality)) %>% 
  rename(comm_varimax = 2)

     comm_no_rot comm_varimax
mpg    0.8692204    0.8692204
cyl    0.9290133    0.9290133
disp   0.9022852    0.9022852
hp     0.8839498    0.8839498
drat   0.7717880    0.7717880
wt     0.8458322    0.8458322
qsec   0.8346421    0.8346421
vs     0.7630788    0.7630788
am     0.8535166    0.8535166
gear   0.8495148    0.8495148
carb   0.7560270    0.7560270
```

如所见，两个方法中捕获的方差是相同的。

很好。但是这会影响排名吗？让我们接着检查。

# 排名

一旦我们进行了 PCA 转换，创建排名是非常简单的。我们需要做的就是收集组件的方差比例，使用`pca2$Vaccounted[2,]`和`pca$scores`，然后进行相乘。因此，对于 PC1 中的每个分数，我们将其乘以该 PCA 运行的对应方差比例。最后，我们将这两个分数添加到原始数据集 mtcars 中。

```py
### Rankings ####

#Prop. Variance Not rotated
variance <- pca2$Vaccounted[2,]

# Scores
factor_scores <- as.data.frame(pca2$scores)

# Rank
mtcars <- mtcars %>% 
  mutate(score_no_rot = (factor_scores$PC1 * variance[1] + 
                           factor_scores$PC2 * variance[2]))

#Prop. Variance Varimax
variance2 <- prin2$Vaccounted[2,]

# Scores Varimax
factor_scores2 <- as.data.frame(prin2$scores)

# Rank Varimax
mtcars <- mtcars %>% 
  mutate(score_rot = (factor_scores2$RC1 * variance2[1] + 
                        factor_scores2$RC2 * variance2[2]))

# Numbered Ranking
mtcars <- mtcars %>% 
  mutate(rank1 = dense_rank(desc(score_no_rot)),
         rank2 = dense_rank(desc(score_rot)) )
```

结果如下所示。

![](img/0a824c6c40df7344a0b4a920a7d1fdbc.png)

基于 PCA/因子分析的排名。图片由作者提供。

上面的表格是**未旋转**PCA 的 TOP10。观察它如何突出显示低`mpg`、高`hp`、`cyl`、`wt`、`disp`的汽车，就像加载量建议的那样。

底下的表格是**varimax 旋转**PCA 的 TOP10。由于方差在两个组件之间更加分散，我们看到了一些差异。例如，`disp`变量现在不那么均匀了。在未旋转的版本中，PC1 的加载量主导了该变量，具有 94%的相关性，而在 PC2 中几乎没有相关性。对于 varimax，它在 RC1 中为-73%，在 RC2 中为 60%，因此有些混乱，因此尽管排名有所不同，但它显示了高和低的数字。`mpg`也可以这样说。

## 基于相关变量的排名

在我们完成所有这些分析后，我们还可以为排名创建设定更好的标准。在我们的研究案例中，我们可以这样说：我们想要最佳的`mpg`、`drat`和`am`手动变速箱（1）。我们已经知道这些变量是相关的，因此结合使用它们进行排名会更容易。

```py
# Use only MPG and drat, am

# PCA after Kaiser's rule applied: Keep eigenvalues > 1
pca3 <- pca(mtcars[,c(1,5,9)], nfactors=2, rotate='none')

#Prop. Variance Not rotated
variance3 <- pca3$Vaccounted[2,]

# Scores
factor_scores3 <- as.data.frame(pca3$scores)

# Rank
mtcars <- mtcars %>% 
  mutate(score_ = (factor_scores3$PC1 * variance3[1] + 
                           factor_scores3$PC2 * variance3[2])) %>% 
  mutate(rank = dense_rank(desc(score_)) )
```

以及结果。

![](img/6711f380473a24f91b5239ff59bb7931.png)

按照 MPG、Drat 和变速箱进行排名。图片由作者提供。

现在结果变得很有意义。以本田思域为例：它具有高 MPG、数据集中最高的 drat 和 am = 1。现在看看排名为 4 和 5 的车。保时捷的 mpg 较低，但 drat 高得多。莲花则正好相反。成功！

# 在你离开之前

本文旨在向你展示 PCA 因子分析的介绍。我们可以在这个教程中看到工具的强大。

然而，在进行分析之前，重要的是要研究变量的相关性，然后设定排名创建的标准。还需注意 PCA 受异常值的影响很大。所以如果你的数据包含过多的异常值，排名可能会扭曲。解决办法是对数据进行标准化处理。

如果你喜欢这些内容，不要忘记关注我的博客以获取更多信息。

[`gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------`](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------) [## Gustavo Santos - Medium]

### 在 Medium 上阅读 Gustavo Santos 的文章。数据科学家。我从数据中提取见解以帮助人们和公司…

[`gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------`](https://gustavorsantos.medium.com/?source=post_page-----c2c3081fdb26--------------------------------)

在 [Linkedin](https://www.linkedin.com/in/gurezende/) 上也可以找到我。

这是这段代码的 GitHub 仓库。

[`github.com/gurezende/Studying/tree/master/R/Factor%20Analysis%20PCA?source=post_page-----c2c3081fdb26--------------------------------`](https://github.com/gurezende/Studying/tree/master/R/Factor%20Analysis%20PCA?source=post_page-----c2c3081fdb26--------------------------------) [## Studying/R/Factor Analysis PCA at master · gurezende/Studying]

### 你现在无法执行该操作。你已在另一个标签页或窗口中登录。你在另一个标签页或窗口中注销了...

[`github.com/gurezende/Studying/tree/master/R/Factor%20Analysis%20PCA?source=post_page-----c2c3081fdb26--------------------------------`](https://github.com/gurezende/Studying/tree/master/R/Factor%20Analysis%20PCA?source=post_page-----c2c3081fdb26--------------------------------)

# 参考文献

FÁVERO, L.; BELFIORE, P. 2022\. [Manual de Análise de Dados](https://www.amazon.com.br/Manual-An%C3%A1lise-Dados-Luiz-F%C3%A1vero/dp/8535270876). 第 1 版. LTC.

[`www.datacamp.com/tutorial/pca-analysis-r`](https://www.datacamp.com/tutorial/pca-analysis-r)
