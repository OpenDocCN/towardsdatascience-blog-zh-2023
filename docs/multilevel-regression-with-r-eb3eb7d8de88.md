# 用 R 进行的多层回归

> 原文：[`towardsdatascience.com/multilevel-regression-with-r-eb3eb7d8de88`](https://towardsdatascience.com/multilevel-regression-with-r-eb3eb7d8de88)

## 通过这个简单的解释和示例来理解层级线性模型

[](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)![Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)[](https://towardsdatascience.com/?source=post_page-----eb3eb7d8de88--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb3eb7d8de88--------------------------------) [Gustavo Santos](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)

·发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----eb3eb7d8de88--------------------------------) ·阅读时间 9 分钟·2023 年 5 月 15 日

--

![](img/df6a3d8f1bd84381780b225b2e4fde4c.png)

图片由 [Lidya Nada](https://unsplash.com/@lidyanada?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/BnzqQwerUOY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

# 介绍

回归模型已经存在很长时间了，远早于机器学习的出现。统计学家们早在 1900 年代之前就开始使用这些模型来理解变量之间的关系，当时 Sir Francis Galton（1885）提出了这一思想。

幸运的是，自那时起，理论、计算机和技术都有了很大的发展，以至于我们可以说，现如今创建这样的模型是简单的（如果不是最简单的话）。

但不要被它的实现简单性所迷惑。它确实很简单，但也不是那么简单。有不止一种回归模型可用，不仅仅是普通最小二乘法（OLS）。

我们在这篇文章中讨论的是层级线性模型（HLM），或者简单地说，就是**多层回归**。

# 准备工作

在深入内容之前，让我们先了解一下本例中将使用的数据集以及我们在编码时需要的库。

加载以下库。如果你没有安装其中任何一个，只需使用 `install.packages("library_name")` 进行安装。

```py
library(nlme)
library(tidyverse)
library(ggridges)
```

数据集**汽油**在许多 R 库中均可用。在这个例子中，它是通过 R 的 `nlme` 库访问的，许可证为 GPL 2.0。它最初来源于 *Prater, N. H. (1955), Estimate gasoline yields from crudes, Petroleum Refiner, 35*。

这个数据框包含以下变量：

+   `**yield**`：原油转化为汽油的百分比

+   `**endpoint**`：所有汽油蒸发的温度（华氏度）

+   `**Sample**`：原油样本编号

+   `**API**`：原油重力（API 度数）

+   `**vapor**`：原油的蒸汽压（lbf/in2）

+   `**ASTM**`：10%的原油变成蒸汽的温度。

```py
# Dataset
data("Gasoline")
```

> ***目标***：我们的目标是预测*`yield`*数值，即从一个样本中转化为汽油的原油量。

## 关于 OLS 的小回顾

在 OLS 回归模型中，我们取一个或多个解释变量***[X]***，并尝试使用这些数字来解释由响应变量***[y]***测量的主题行为。

对于 OLS，最佳的特征选择统计测试是相关性。毕竟，最常用的衡量线如何拟合数据的指标是 R²，这只是相关性平方（附注：确实还有其他模型性能指标，应该与 R²一起使用）。

因此，我们的目标是绘制一条最佳拟合数据的线。换句话说，我们希望这条线与数据点一致，绘制出响应变量的平均值。此外，我们希望这条线与实际数据点之间的差异尽可能小。

更技术一点，响应变量将由一个方程确定，我们希望优化 y 轴上的截距点*alpha*和这条线的斜率，而斜率由*beta*系数决定，正如下文所述。

![](img/1307ccfe43864ef7936f7d958b745a8a.png)

简单线性回归方程。作者提供的图片。

如果我们查看我们的数据集，这就是 OLS 模型将会做的事情：

![](img/0e215c86f732a90bcd6e3950d417a0cc.png)

从汽油数据集创建的 OLS 回归模型。作者提供的图片。

这是一个不错的拟合，但不是最佳模型。它可以在某种程度上解释`yield`的方差，但我们可以做得更好，如下所示。

# 层次线性模型 [HLM]

HLM 与传统 OLS 模型的不同之处在于它考虑了数据的自然分组。

> 多层回归就像为数据中的每个组创建一个不同的回归模型。

从我们的数据来看，首先引起我注意的是存在不同的`Sample`。所以，这引出了问题：

+   *不同样本会影响产量的估计吗？*

+   *如果我们通过样本添加多个回归层次，结果会更好吗？*

![](img/d7c3c3d2ceb353c828a1b820e61c3bf6.png)

汽油数据集摘录。作者提供的图片。

## 组件

HLM 模型由固定效应和随机效应组件组成。**固定效应**可以估计 X 变量和 y 之间的关系，而**随机效应**组件将为每个组确定不同的截距和斜率系数，帮助创建对每个组更合适的估计。

请看下面的多层回归模型的样子。

![](img/ca5204b24b00731e1e291072feead5cd.png)

多层次模型 HLM2，一级=观察，二级=样本。图像由作者提供。

实际上，会创建一个回归模型来计算固定组件，这对于每个组的每个观察值都是 *y = a + bx*。然后，顺序地计算随机组件，这是对更多或更少的调整，根据该组的最佳截距和/或斜率。

HLM 模型可以有随机组件，仅改变截距、仅改变斜率或两者都有。在前面的图中，随机组件是截距和斜率，因为每个样本的回归线不平行且有不同的截距点。

# 编码

现在让我们创建一个 OLS 模型和一个 2 级的 HLM，然后比较结果。

我们可以先进行相关性测试，以了解回归中使用的最佳变量。我们会发现 `endpoint` 是与 `yield` 相关性最高的变量。

```py
# Check for the best correlations
cor(Gasoline[,-3])

              yield   endpoint        API      vapor       ASTM
yield     1.0000000  0.7115262  0.2463260  0.3840706 -0.3150243
endpoint  0.7115262  1.0000000 -0.3216782 -0.2979843  0.4122466
API       0.2463260 -0.3216782  1.0000000  0.6205867 -0.7001539
vapor     0.3840706 -0.2979843  0.6205867  1.0000000 -0.9062248
ASTM     -0.3150243  0.4122466 -0.7001539 -0.9062248  1.0000000
```

在 R 中创建 OLS 模型非常简单。原生函数 `lm()` 可以轻松处理。让我们看看。

```py
# OLS model
model_ols <- lm(yield ~ endpoint, data=Gasoline)
summary(model_ols)

[OUT]
Call:
lm(formula = yield ~ endpoint, data = Gasoline)

Residuals:
     Min       1Q   Median       3Q      Max 
-14.7584  -6.2783   0.0525   5.1624  17.8481 

Coefficients:
             Estimate Std. Error t value Pr(>|t|)    
(Intercept) -16.66206    6.68721  -2.492   0.0185 *  
endpoint      0.10937    0.01972   5.546 4.98e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 7.659 on 30 degrees of freedom
Multiple R-squared:  0.5063, Adjusted R-squared:  0.4898 
F-statistic: 30.76 on 1 and 30 DF,  p-value: 4.983e-06

# Calculate Log Likelihood
logLik(model_ols)
'log Lik.' -109.5206 (df=3)
```

结果：模型在统计上显著（F 检验的 p 值<0.05），系数也显著。

+   R²: 50%

+   Loglik: -109.52

+   MAE: 5.95（yield 平均值 = 19.66）

+   RMSE: 7.41（标准差：10.72）

我们的主要绝对误差约为 *y* 平均值的 30%。这太多了。正如我们在单一回归线的图中看到的那样，这条线位置良好，但无法完全捕捉方差，因为有许多点离平均值太远。

现在让我们尝试运行一个 HLM。

```py
# Multilevel Model with fixed slope and random intercept
model_multi <- lme(fixed = yield ~ endpoint,
                       random = ~ 1 | Sample,
                       data = Gasoline,
                       method = "REML")

[OUT]
Linear mixed-effects model fit by REML
  Data: Gasoline 
       AIC      BIC   logLik
  183.4306 189.0354 -87.7153

Random effects:
 Formula: ~1 | Sample
        (Intercept) Residual
StdDev:    8.387862  1.88046

Fixed effects:  yield ~ endpoint 
                Value Std.Error DF   t-value p-value
(Intercept) -33.30626  3.287182 21 -10.13216       0
endpoint      0.15757  0.005703 21  27.62678       0
 Correlation: 
         (Intr)
endpoint -0.582

Standardized Within-Group Residuals:
       Min         Q1        Med         Q3        Max 
-1.6759908 -0.4857902 -0.0397993  0.5128182  1.8518205 

Number of Observations: 32
Number of Groups: 10 
```

这是代码中发生的情况。我们使用 `nlme` 库中的 `lme` 函数运行了一个多层次模型。**固定组件**是回归 `yield ~ endpoint`。**随机组件**仅针对截距。注意 `random = ~1 | Sample`。`~1` 意味着固定斜率——即按组的平行回归线——和按 `Sample` 计算的随机截距。

与简单的 *y = a + bx* 相比，这个模型变成了：

+   固定: [**截距 + b*endpoint**] +

+   随机: [ **由样本随机效应 + 错误**]

与 OLS 模型相比，注意它如何改进了。

+   Loglik: -87.7153（数字越高越好）。

+   MAE: 1.16

+   RMSE: 1.52

哇，已经改进了很多：LogLik 测量值提高了大约 20%。

*我们还可以进一步改进吗？* 我们可以尝试向模型中添加另一个变量。我们将选择 `vapor`，因为它与响应变量的相关性是下一个最强的。

这次，我们将运行一个带有随机斜率和截距的 HLM，这意味着我们将允许模型在交叉 y 轴的位置和拟合线的倾斜度上变化，而不是将其静态为平行线。

在下面的代码片段中，我们正在计算`yield`在`endpoint`和`vapor`函数中的一个固定部分（按观察、按样本）。随机部分将根据`endpoint`值计算回归的斜率，并根据`Sample`计算截距。下一个模型将是：

+   固定：[**截距 + (b1 * 终点) + (b2 * 蒸汽)**] +

+   随机：[**截距随机效应由样本 + 斜率随机效应 * 终点 + 误差**]

```py
# Multilevel Model with random slope and intercept
model_multi2 <- lme(fixed = yield ~ endpoint + vapor,
                   random = ~ endpoint | Sample,
                   data = Gasoline,
                   method = "REML")

# Look how the fitted values are presented
# It is divided in Fixed and the result with the levels
# "Sample" is the final prediction, with the random effects considered
model_multi2$fitted

        fixed     Sample
1   9.9034036  6.2878876
2  17.4490277 16.9089759
3   6.2864457  8.2047743
4  13.2051879 10.0773886
5  -0.3240537  6.1951732
```

为了公平起见，我还使用了蒸汽变量运行了另一个模型。接下来，我们将比较所有模型。

![](img/fb55d123301379e3f05aba3b70a1faea.png)

比较创建的 4 个模型。图像由作者提供。

尽管 OLS 模型`endpoint+vapor`给出了一个有趣的 LogLik 值，但我们还是更仔细地看看预测，以了解哪个模型更合适。

```py
# Results table
results <- data.frame(
   yield = Gasoline$yield,
   pred_ols = model_ols$fitted.values,
   pred_ols2 = model_ols2$fitted.values,
   pred_multi = model_multi$fitted[,'Sample'],
   pred_multi2 = model_multi2$fitted[,'Sample']

   yield  pred_ols  pred_ols2 pred_multi pred_multi2
1    6.9  9.040133 11.2681120  5.8234477   6.2878876
2   14.4 16.914846 17.8195910 17.5516334  16.9089759
3    7.4  6.524599  8.0634010  8.6490752   8.2047743
4    8.5 23.258365 13.5848551  9.9282663  10.0773886
5    8.0  7.180825  1.9380928  6.1502045   6.1951732
6    2.8  9.040133 -0.2448399 -0.2263362   0.6677289
7    5.0 14.508684  5.1154648  5.7778750   5.0504530
8   12.2  5.759002 13.7816308 12.3278463  11.9321042
9   10.0 12.540005 13.3171528 10.1678972  10.3006218
10  15.2 16.149249 20.3249040 16.0654477  16.3795097
11  26.8 23.477107 26.1797067 27.0057873  27.2203084
12  14.0 21.727171 17.5245089 13.0730720  13.5617889
13  14.7 24.789559 15.5355488 12.1342356  12.1958142
14   6.4 13.414973  5.3285706  6.0764330   6.4725490
15  17.6 23.258365 16.2622858 18.3834134  18.1474338
16  22.3 13.414973 23.5350992 23.3576925  23.3378940
```

OLS 模型 2 做得非常好，但多层回归更加一致。例如，查看第 4、5、9、10、12 行的拟合值，两个多层模型将提供更好的估计。然而，尽管存在这些误差，OLS2 对这些数据来说并不是一个糟糕的模型。

![](img/921c87916cb354748fb588beff3973cf.png)

比较创建的 4 个模型。图像由作者提供。

这个最终的视觉效果可以给我们一个模型的比较视图。毫无疑问，最佳拟合模型是右下角的最后一个 HLM 模型。

# 离开前

在本教程中，我想为你介绍层次线性模型或多层回归模型。当你有数据自然嵌套在不同组中时，这些模型会非常有用，并且这些组会影响估计的方式。

使用多层模型，你可以允许回归线根据组变化其斜率和/或截距。

在 R 中进行 HLM 建模所用的包是`nlme`。

这是这个练习的代码，在 GitHub 上。

[](https://github.com/gurezende/Studying/tree/master/R/Multilevel%20Regression?source=post_page-----eb3eb7d8de88--------------------------------) [## Studying/R/Multilevel Regression at master · gurezende/Studying

### 你目前无法执行该操作。你在另一个标签或窗口中登录。你在另一个标签或窗口中注销了……

github.com](https://github.com/gurezende/Studying/tree/master/R/Multilevel%20Regression?source=post_page-----eb3eb7d8de88--------------------------------)

如果你喜欢这个内容，不要忘记关注我。

[](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------) [## Gustavo Santos - Medium

### 阅读 Gustavo Santos 在 Medium 上的文章。数据科学家。我从数据中提取见解，以帮助人们和公司……

gustavorsantos.medium.com](https://gustavorsantos.medium.com/?source=post_page-----eb3eb7d8de88--------------------------------)

此外，如果你考虑加入 Medium 成为会员，以便访问我的文章以及其他成千上万的优质内容，[这是我的推荐码](https://medium.com/@gustavorsantos/membership)。

你可以在[LinkedIn](https://www.linkedin.com/in/gurezende/)找到我，也可以通过[TopMate 预订与我进行快速聊天](https://topmate.io/gustavo_santos)。

# 参考文献

[Fávero, L. & Belfiore, P. 2022. *Manual de Análise de Dados*. 第 1 版. LTC，里约热内卢，巴西。](https://www.amazon.com.br/Manual-An%C3%A1lise-Dados-Luiz-F%C3%A1vero/dp/8535270876)
