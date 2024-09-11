# 分布式随机森林的推断

> 原文：[https://towardsdatascience.com/inference-for-distributional-random-forests-64610bbb3927?source=collection_archive---------10-----------------------#2023-02-17](https://towardsdatascience.com/inference-for-distributional-random-forests-64610bbb3927?source=collection_archive---------10-----------------------#2023-02-17)

## 一种强大的非参数方法的置信区间

[](https://medium.com/@jeffrey_85949?source=post_page-----64610bbb3927--------------------------------)[![Jeffrey Näf](../Images/0ce6db85501192cdebeeb910eb81a688.png)](https://medium.com/@jeffrey_85949?source=post_page-----64610bbb3927--------------------------------)[](https://towardsdatascience.com/?source=post_page-----64610bbb3927--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----64610bbb3927--------------------------------) [Jeffrey Näf](https://medium.com/@jeffrey_85949?source=post_page-----64610bbb3927--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca780798011a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finference-for-distributional-random-forests-64610bbb3927&user=Jeffrey+N%C3%A4f&userId=ca780798011a&source=post_page-ca780798011a----64610bbb3927---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----64610bbb3927--------------------------------) ·17分钟阅读·2023年2月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F64610bbb3927&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finference-for-distributional-random-forests-64610bbb3927&user=Jeffrey+N%C3%A4f&userId=ca780798011a&source=-----64610bbb3927---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F64610bbb3927&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finference-for-distributional-random-forests-64610bbb3927&source=-----64610bbb3927---------------------bookmark_footer-----------)![](../Images/a83708db984a87353847003d7999656b.png)

（分布式）随机森林的特性。本文：提供不确定性度量的能力。来源：作者。

在之前的[文章](/drf-a-random-forest-for-almost-everything-625fa5c3bcb8)中，我详细讨论了分布随机森林方法，这是一种可以非参数地估计多变量条件分布的随机森林类型算法。这意味着我们能够在给定一些协变量***X***的情况下，非参数地学习多变量响应***Y***的整个分布，而不仅仅是学习其条件期望。DRF通过学习权重 *w_i(****x****)* 对于 *i=1,…,n* 训练点来完成这项工作，这些权重定义了分布，并且可以用于估计广泛的目标。

到目前为止，这种方法仅产生了分布的“点估计”（即* n* 权重 *w_i(****x****)* 的点估计）。虽然这足以预测响应的整个分布，但它并未提供考虑数据生成机制随机性的方法。也就是说，即使这个点估计在大样本量下（在一系列假设下）越来越接近真实值，但在有限样本量下其估计仍然存在不确定性。幸运的是，现在有一种（可证明的）方法来量化这种不确定性，如我在本文中阐述的。这基于我们关于[arXiv](https://arxiv.org/pdf/2302.05761.pdf)的新论文。

本文的目标有两个：首先，我想讨论如何基于我们的论文将不确定性估计添加到 DRF 中。论文相当理论化，因此我从几个示例开始。接下来的部分快速浏览这些理论结果，以供感兴趣的人参考。然后我解释了如何利用这些结果获取广泛目标的（基于采样的）不确定性度量。其次，我讨论了[1]的 CoDiTE 和这一概念的一个特别有趣的例子，即条件见证函数。这个函数是一个复杂的对象，但正如我们将在下面看到的那样，我们可以轻松地使用 DRF 进行估计，并且基于本文中介绍的概念，甚至可以提供渐近置信带。一个如何应用的详细真实数据示例见[这篇文章](https://medium.com/@jeffrey_85949/studying-the-gender-wage-gap-in-the-us-using-distributional-random-forests-ec4c2a69abf0)。

在整个过程中，我们假设有一个 *d* 变量的 i.i.d. 样本 ***Y****_1, …,* ***Y****_n* 和一个 *p* 变量的 i.i.d. 样本 ***X****_1,…,****X****_n*。目标是估计 ***Y****|****X=x*** 的条件分布。

我们将需要以下软件包和函数：

```py
library(kernlab)
library(drf)
library(Matrix)
library(Hmisc)

source("CIdrf.R")
```

文件“CIdrf.R”中的函数如下所示。

以下所有图片，除非另有说明，均由作者提供。

# 示例

我们从一个简单的例子开始模拟，其中 *d=1* 和 *p=2*：

```py
 set.seed(2)

n<-2000
beta1<-1
beta2<--1.8

# Model Simulation
X<-mvrnorm(n = n, mu=c(0,0), Sigma=matrix(c(1,0.7,0.7,1), nrow=2,ncol=2))
u<-rnorm(n=n, sd = sqrt(exp(X[,1])))
Y<- matrix(beta1*X[,1] + beta2*X[,2] + u, ncol=1)
```

请注意，这只是一个异方差线性模型，其中误差项的方差依赖于 *X_1* 的值。当然，如果你知道 ***X*** 对 *Y* 的影响是线性的，你就不会使用 DRF 或任何随机森林，而是直接使用线性回归。但为了这个目的，知道真实情况是很方便的。由于 DRF 的工作是估计给定 ***X****=****x*** 的条件分布，我们现在固定 ***x*** 并估计给定 ***X****=****x*** 的条件期望和方差。

我们选择一个位于 ***X*** 分布中心的点，周围有大量观察数据。一般来说，当使用任何随机森林方法处理 ***X*** 观测数据的边界点时，应格外小心。

```py
# Choose an x that is not too far out
x<-matrix(c(1,1),ncol=2)

# Choose alpha for CIs
alpha<-0.05
```

最后，我们拟合我们的 DRF 并获得权重 *w_i(****x****)*：

```py
## Fit the new DRF framework
drf_fit <- drfCI(X=X, Y=Y, min.node.size = 2, splitting.rule='FourierMMD', num.features=10, B=100)

## predict weights
DRF = predictdrf(drf_fit, x=x)
weights <- DRF$weights[1,]
```

如下所述，我们在这里构建的 DRF 对象不仅包含权重 *w_i(****x****)*，还包含一个 *B* 权重样本，这些权重对应于从 *w_i(****x****)* 的分布中抽取的样本。我们可以使用这 *B* 次抽样来逼近我们想要估计的任何分布，如我现在在两个示例中所说明的。

## **示例 1：条件期望**

首先，我们简单地做了大多数预测方法会做的事情：我们估计条件期望。使用我们的方法，我们还在其周围构建了一个置信区间。

```py
# Estimate the conditional expectation at x:
condexpest<- sum(weights*Y)

# Use the distribution of weights, see below
distofcondexpest<-unlist(lapply(DRF$weightsb, function(wb)  sum(wb[1,]*Y)  ))

# Can either use the above directly to build confidence interval, or can use the normal approximation.
# We will use the latter
varest<-var(distofcondexpest-condexpest)

# build 95%-CI
lower<-condexpest - qnorm(1-alpha/2)*sqrt(varest)
upper<-condexpest + qnorm(1-alpha/2)*sqrt(varest)
c(lower, condexpest, upper)

(-1.00, -0.69 -0.37)
```

重要的是，虽然估计值有点偏差，但这个置信区间包含了真实值，真实值如下：

![](../Images/52ee2c7bbc6815322309981bda7f19fe.png)

## **示例 2：条件方差**

现在假设我们想要找到方差 Var(Y|**X**=**x**)，而不是条件均值。这对于一个无法利用线性特性的非参数方法来说是一个相当具有挑战性的例子。实际情况如下：

![](../Images/d2db80f11b107106d566d7dcc249eae5.png)

使用 DRF，我们可以如下估计：

```py
# Estimate the conditional expectation at x:
condvarest<- sum(weights*Y^2) - condexpest^2

distofcondvarest<-unlist(lapply(DRF$weightsb, function(wb)  {
  sum(wb[1,]*Y^2) - sum(wb[1,]*Y)^2
  } ))

# Can either use the above directly to build confidence interval, or can use the normal approximation.
# We will use the latter
varest<-var(distofcondvarest-condvarest)

# build 95%-CI
lower<-condvarest - qnorm(1-alpha/2)*sqrt(varest)
upper<-condvarest + qnorm(1-alpha/2)*sqrt(varest)

c(lower, condvarest, upper)

(1.89, 2.65, 3.42) 
```

因此，真实参数被包含在置信区间内，这正是我们所期望的，事实上，我们的估计值与真实值非常接近！

我们现在研究这些示例背后的理论，然后再来看一个因果分析中的第三个示例。

# RKHS 中的渐近正态性

在本节和下一节中，我们简要关注论文中推导的理论结果。如上所述和文章中解释的，DRF 在测试点 **x** 上呈现分布预测。也就是说，我们获得一个估计值

![](../Images/ef4a0a592b625a61439725d7f4146eca.png)

条件分布的 ***Y*** 给定 ***X****=****x***。这只是一个典型的经验度量方式，魔力在于权重 *w_i(****x****)* — 它们可以用来轻松地获得感兴趣量的估计量，甚至直接从分布中采样。

为了获得这个估计，DRF 实际上是在再生核希尔伯特空间（RKHS）中估计条件均值。RKHS 是通过核函数 *k(****y****_1,* ***y****_2)* 定义的。通过这个核，我们可以将每个观测 ***Y****_i* 映射到希尔伯特空间中，作为 *k(****Y****_i, .)*。利用这个极其强大的工具可以进行多种方法，例如核岭回归。关键点是，在某些条件下，任何分布都可以表示为这个 RKHS 的一个元素。事实证明，真正的条件分布可以表示为 RKHS 中的以下期望：

![](../Images/bd156eb31d44300738e13e547b07d8d3.png)

所以这只是另一种表达 ***Y*** 给定 ***X****=****x*** 的条件分布的方式。然后我们尝试用 DRF 来估计这个元素，如下所示：

![](../Images/1cb7f9837a62a83ca4fb45179088ebf7.png)

我们再次使用了从 DRF 获得的权重，但这次用 *k(****Y****_i,.)* 来形成加权和，而不是上述的狄拉克测度。我们可以通过编写任意两个中的任何一个来在两个估计之间来回映射。这很重要，因为我们可以将条件分布估计写成 RKHS 中的加权均值！就像原始的随机森林在实数中估计均值（*Y* 给定 ***X****=****x*** 的条件期望）一样，DRF 在 RKHS 中估计均值。结果表明，我们还获得了条件分布的估计。

这对我们的故事很重要，因为在 RKHS 中，这个加权均值在某些方面表现得与 *d* 维的（加权）均值非常相似。也就是说，我们可以利用现有的工具来研究其一致性和渐近正态性。这是相当显著的，因为所有有趣的 RKHS 都是无限维的。[第一篇 DRF 论文](https://www.jmlr.org/papers/v23/21-0585.html) 已经确立了 RKHS 中估计器（1）的均衡性。我们的新论文现在进一步证明了，此外，

![](../Images/beb966c92670eb9ee62fecb884bf5abb.png)

其中 sigma_n 是趋近于零的标准差，而***Sigma****_****x*** 是一个替代协方差矩阵的算子（这与 *d* 维欧几里得空间中的表现非常相似）。

# 获得采样分布

好的，那么，我们在无限维空间中得到了一个渐近正态性结果，这到底意味着什么？首先，这意味着从 DRF 估计中得出的估计器如果“平滑”到足够程度，也将趋向于渐近正态。然而，这还不够有用，因为我们还需要有方差估计。这里我们论文中的进一步结果派上用场。

我们在这里省略了很多细节，但本质上我们可以使用以下子样本方案：不是仅仅拟合*N*棵树来构建我们的森林，而是构建*B*组*L*棵树（使得*N=B*L*）。现在，对于每一组树或迷你森林，我们随机子样本约一半的数据点，然后仅使用此子样本拟合森林。我们称这个样本子集为*S*。对于每个抽取的*S*，我们得到另一个在希尔伯特空间中表示的DRF估计量

![](../Images/6c96b09934e8fd69d152821605f9250f.png)

仅使用*S*中的样本。请注意，与自助法一样，我们现在有两个随机性来源，即使忽略森林的随机性（理论上我们假设*B*足够大，以使森林的随机性可忽略不计）。一个来源是数据本身，另一个来源是我们在随机选择*S*时引入的人工随机性。关键是，给定数据的情况下，*S*的随机性在我们的控制之下——我们可以绘制任意数量的子集*S*。所以问题是，如果我们只考虑*S*的随机性并固定数据，那么我们的估计量（2）会发生什么？值得注意的是，我们可以证明

![](../Images/3a5d0db02cd4a1152ab28bbfa2d32eb0.png)

这仅仅意味着，如果我们固定数据的随机性，仅考虑来自*S*的随机性，那么估计量（2）减去估计量（1）将以相同的极限收敛到与原始估计量减去真实值相同的极限！这实际上是自助法理论的工作原理：我们已经证明了我们可以从中采样的东西，即

![](../Images/284228a4f892f49794336481ce31ae76.png)

收敛到我们无法访问的东西，即

![](../Images/84b2fe390815c3b56f196fde0bf1bf82.png)

因此，为了对后者进行推断，我们可以使用前者！这实际上是人们在自助法理论中提出的标准论点，以证明为什么自助法可以用来近似采样分布！没错，即使是自助法，尽管人们经常在小样本中使用，它也只在大样本范围内才真正有意义（理论上）。

现在我们来使用这个。

# 这实际上是什么意思？

我们现在展示这在实践中的含义。接下来，我们定义两个从CRAN包[drf](https://cran.r-project.org/web/packages/drf/index.html)的drf函数派生的新函数。

```py
## Functions in CIdrf.R that is loaded above ##

drfCI <- function(X, Y, B, sampling = "binomial",...) {

### Function that uses DRF with subsampling to obtain confidence regions as
### as described in https://arxiv.org/pdf/2302.05761.pdf
### X: Matrix of predictors
### Y: Matrix of variables of interest
### B: Number of half-samples/mini-forests

  n <- dim(X)[1]

  # compute point estimator and DRF per halfsample S
  # weightsb: B times n matrix of weights
  DRFlist <- lapply(seq_len(B), function(b) {

    # half-sample index
    indexb <- if (sampling == "binomial") {
      seq_len(n)[as.logical(rbinom(n, size = 1, prob = 0.5))]
    } else {
      sample(seq_len(n), floor(n / 2), replace = FALSE)
    }

    ## Using refitting DRF on S
    DRFb <- 
      drf(X = X[indexb, , drop = F], Y = Y[indexb, , drop = F],
          ci.group.size = 1, ...)

    return(list(DRF = DRFb, indices = indexb))
  })

  return(list(DRFlist = DRFlist, X = X, Y = Y) )
}

predictdrf<- function(DRF, x, ...) {

### Function to predict from DRF with Confidence Bands
### DRF: DRF object
### x: Testpoint

  ntest <- nrow(x)
  n <- nrow(DRF$Y)

  ## extract the weights w^S(x)
  weightsb <- lapply(DRF$DRFlist, function(l) {

    weightsbfinal <- Matrix(0, nrow = ntest, ncol = n , sparse = TRUE)

    weightsbfinal[, l$indices] <- predict(l$DRF, x)$weights 

    return(weightsbfinal)
  })

  ## obtain the overall weights w
  weights<- Reduce("+", weightsb) / length(weightsb)

return(list(weights = weights, weightsb = weightsb ))
}

Witdrf<- function(DRF, x, groupingvar, alpha=0.05, ...){

### Function to calculate the conditional witness function with
### confidence bands from DRF
### DRF: DRF object
### x: Testpoint

  if (is.null(dim(x)) ){

  stop("x needs to have dim(x) > 0")
  }

  ntest <- nrow(x)
  n <- nrow(DRF$Y)
  coln<-colnames(DRF$Y)

  ## Collect w^S
  weightsb <- lapply(DRF$DRFlist, function(l) {

    weightsbfinal <- Matrix(0, nrow = ntest, ncol = n , sparse = TRUE)

    weightsbfinal[, l$indices] <- predict(l$DRF, x)$weights 

    return(weightsbfinal)
  })

  ## Obtain w
  weightsall <- Reduce("+", weightsb) / length(weightsb)

  #weightsall0<-weightsall[, DRF$Y[, groupingvar]==0, drop=F]
  #weightsall1<-weightsall[,DRF$Y[, groupingvar]==1, drop=F]

  # Get the weights of the respective classes (need to standardize by propensity!)
  weightsall0<-weightsall*(DRF$Y[, groupingvar]==0)/sum(weightsall*(DRF$Y[, groupingvar]==0))
  weightsall1<-weightsall*(DRF$Y[, groupingvar]==1)/sum(weightsall*(DRF$Y[, groupingvar]==1))

  bandwidth_Y <- drf:::medianHeuristic(DRF$Y)
  k_Y <- rbfdot(sigma = bandwidth_Y)

  K<-kernelMatrix(k_Y, DRF$Y[,coln[coln!=groupingvar]], y = DRF$Y[,coln[coln!=groupingvar]])

  nulldist <- sapply(weightsb, function(wb){
    # iterate over class 1

    wb0<-wb*(DRF$Y[, groupingvar]==0)/sum(wb*(DRF$Y[, groupingvar]==0))
    wb1<-wb*(DRF$Y[, groupingvar]==1)/sum(wb*(DRF$Y[, groupingvar]==1))

    diag( ( wb0-weightsall0 - (wb1-weightsall1) )%*%K%*%t( wb0-weightsall0 - (wb1-weightsall1) )  )

  })

  # Choose the right quantile
  c<-quantile(nulldist, 1-alpha)

  return(list(c=c, k_Y=k_Y, Y=DRF$Y[,coln[coln!=groupingvar]], nulldist=nulldist, weightsall0=weightsall0, weightsall1=weightsall1))

} 
```

因此，通过我们的方法，我们不仅得到点估计形式的权重*w_i(****x****)*，还得到*B*个权重的样本，每个样本表示从条件分布的估计量的分布中独立抽取的结果（这听起来比实际更令人困惑，请记住这些例子）。这仅仅意味着我们不仅有一个估计量，还有一个其分布的近似！

现在我转向一个更有趣的例子，这是我们仅能使用DRF做的（据我所知）。

# **因果分析示例：见证函数**

假设我们有两组观测值，分别是组 *W=1* 和组 *W=0*，我们想要找出组别与变量 *Y* 之间的因果关系。在 [*这篇文章*](https://medium.com/@jeffrey_85949/studying-the-gender-wage-gap-in-the-us-using-distributional-random-forests-ec4c2a69abf0) 的例子中，这两组分别为男性和女性，而 *Y* 为小时工资。此外，我们有混杂变量 ***X***，我们假设它们影响 *W* 和 *Y*。我们在这里假设 ***X*** 确实包含所有相关的混杂变量。这是一个大假设。形式上，我们假设无混杂：

![](../Images/41019dc0794c12529c1e97f67d558a3a.png)

和重叠：

![](../Images/d2628db016d7b856e7d35b9b4262ae61.png)

通常，人们会比较两个组之间的条件期望：

![](../Images/f68a21df53759761756554f6e747085c.png)

这是 ***x*** 处的条件平均处理效应（CATE）。这是一个自然的起点，但在最近的一篇论文 ([1]) 中，引入了 CoDiTE 作为这一思想的推广。CoDiTE 不仅仅关注期望值的差异，还建议查看其他量的差异。一个特别有趣的例子是*条件见证函数*：对于两个组，我们如上所述

![](../Images/afdd6de7c361390ea2305e8804c7bbb8.png)

因此，我们考虑在 RKHS 中两个条件分布的表示。除了作为条件分布的表示，这些量也是实值函数：对于 *j=0,1*，

![](../Images/98b487eeaf5072792c5c007b3fd822b8.png)

给出这两个量之间差异的函数，

![](../Images/b42cf61f8df2f6f91847f49944db0f41.png)

称为*条件见证函数*。

为什么这个函数很有趣？事实证明，这个函数展示了两个密度如何相互关系：对于函数值为负的 *y*，类 1 在 *y* 处的条件密度小于 0 的条件密度。类似地，如果函数在 *y* 处为正，这意味着 1 的密度在 *y* 处高于 0 的条件密度（其中“条件”始终指的是条件于 ***X***=****x***）。至关重要的是，这可以*不需要估计密度*，这很困难，尤其是对于多变量 ***Y***。

最后，我们可以为估计的条件见证函数提供*均匀置信带*，通过使用上面的 *B* 样本。我在这里不详细说明，但这些基本上是我们上面使用的条件均值置信区间的类比。至关重要的是，这些置信带应在特定的 ***x*** 上对函数值 *y* 有效。

让我们用一个例子来说明：我们模拟以下数据生成过程：

![](../Images/d50fab95d19e8b586d478f249897a0d4.png)

即，*X_1, X_2*在（0,1）上独立均匀分布，*W*为0或1，概率取决于*X_2*，*Y*是*W*和*X_1*的函数。这是一个非常困难的问题；不仅***X***影响属于类别1的概率（即倾向），它还改变了*W*对*Y*的处理效果。事实上，简单计算表明CATE给定为：

(1 - 0.2)*X_1 - (0 - 0.2)*X_1 = X_1。

![](../Images/c54d252b2f10464351b4cdbfbd3d16f6.png)

与数据生成过程相对应的图形

```py
set.seed(2)

n<-4000
p<-2

X<-matrix(runif(n*p), ncol=2)
W<-rbinom(n,size=1, prob= exp(-X[,2])/(1+exp(-X[,2])))

Y<-(W-0.2)*X[,1] + rnorm(n)
Y<-matrix(Y,ncol=1) 
```

我们现在随机选择一个测试点***x***，并使用以下代码来估计证据函数及其置信区间：

```py
 x<-matrix(runif(1*p), ncol=2)
Yall<-cbind(Y,W)
## For the current version of the Witdrf function, we need to give
## colnames to Yall
colnames(Yall) <- c("Y", "W")

## Fit the new DRF framework
drf_fit <- drfCI(X=X, Y=Yall, min.node.size = 5, splitting.rule='FourierMMD', num.features=10, B=100)

Witobj<-Witdrf(drf_fit, x=x, groupingvar="W", alpha=0.05)

hatmun<-function(y,Witobj){

  c<-Witobj$c
  k_Y<-Witobj$k_Y
  Y<-Witobj$Y
  weightsall1<-Witobj$weightsall1
  weightsall0<-Witobj$weightsall0
  Ky=t(kernelMatrix(k_Y, Y , y = y))

  #K1y <- t(kernelMatrix(k_Y, DRF$Y[DRF$Y[, groupingvar]==1,coln[coln!=groupingvar]], y = y))
  #K0y <- t(kernelMatrix(k_Y, DRF$Y[DRF$Y[, groupingvar]==0,coln[coln!=groupingvar]], y = y))
  out<-list()
  out$val <- tcrossprod(Ky, weightsall1  ) - tcrossprod(Ky, weightsall0  )
  out$upper<-  out$val+sqrt(c)
  out$lower<-  out$val-sqrt(c)

  return( out )

}

all<-hatmun(sort(Witobj$Y),Witobj)

plot(sort(Witobj$Y),all$val , type="l", col="darkblue", lwd=2, ylim=c(min(all$lower), max(all$upper)),
     xlab="y", ylab="witness function")
lines(sort(Witobj$Y),all$upper , type="l", col="darkgreen", lwd=2 )
lines(sort(Witobj$Y),all$lower , type="l", col="darkgreen", lwd=2 )
abline(h=0)
```

![](../Images/8e33cecd895b14726387eef7739c5687.png)

我们可以从这个图中看到：

(1) 组1的条件密度在*y*值介于-3和0.3之间时*低于*组0的密度。此外，这种差异随着*y*的增大而增大，直到约*y = -1*，之后密度差异开始再次减少，直到两者在约0.3时相同。

(2) 对称地，组1的密度在*y*值介于0.3到3之间时高于组0的密度，这种差异逐渐增大，直到在约*y = 1.5*时达到最大值。在此点之后，差异逐渐减少，直到在*y = 3*时几乎回到零。

(3) 两个密度之间的差异在95%置信水平上具有统计学意义，可以从事实中看到，对于*y*大致在-1.5到-0.5以及1到2之间，渐近置信区间不包括零线。

让我们检查（1）和（2）对模拟的真实条件密度的适用情况。也就是说，我们大量模拟真实情况：

```py
# Simulate truth for a large number of samples ntest
ntest<-10000
Xtest<-matrix(runif(ntest*p), ncol=2)

Y1<-(1-0.2)*Xtest[,1] + rnorm(ntest)
Y0<-(0-0.2)*Xtest[,1] + rnorm(ntest)

## Plot the test data without adjustment
plotdf = data.frame(Y=c(Y1,Y0), W=c(rep(1,ntest),rep(0,ntest) ))
plotdf$weight=1
plotdf$plotweight[plotdf$W==0] = plotdf$weight[plotdf$W==0]/sum(plotdf$weight[plotdf$W==0])
plotdf$plotweight[plotdf$W==1] = plotdf$weight[plotdf$W==1]/sum(plotdf$weight[plotdf$W==1])

plotdf$W <- factor(plotdf$W)

#plot pooled data
ggplot(plotdf, aes(Y)) +
  geom_density(adjust=2.5, alpha = 0.3, show.legend=TRUE,  aes(fill=W, weight=plotweight)) +
  theme_light()+
  scale_fill_discrete(name = "Group", labels = c('0', "1"))+
  theme(legend.position = c(0.83, 0.66),
        legend.text=element_text(size=18),
        legend.title=element_text(size=20),
        legend.background = element_rect(fill=alpha('white', 0.5)),
        axis.text.x = element_text(size=14),
        axis.text.y = element_text(size=14),
        axis.title.x = element_text(size=19),
        axis.title.y = element_text(size=19))+
  labs(x='y') 
```

这导致：

![](../Images/d07cca78bc95a50ec272a3d9684e9e03.png)

视觉上比较有点困难，但我们看到这两个密度与上面的证据函数预测的结果非常接近。特别是，我们看到在0.3左右密度几乎相同，密度差异在大约-1和1.5之间达到最大。因此，实际密度中可以看到点（1）和（2）！

此外，为了将（3）置于背景中，论文中的重复模拟展示了在没有可见效果时估计的证据函数的趋向：

![](../Images/b78fb42e38b3246856be32072c56a1b2.png)

在与此处描述的类似设置中模拟了1000个证据函数。蓝色的是1000个估计的证据函数，而灰色的显示了相应的置信区间。摘自我们在arXiv上的论文。此示例中没有效果，99%的置信区间不包含零线。

在[这篇文章](https://medium.com/p/ec4c2a69abf0/edit)中给出了因果推断中的一个真实数据示例。

## 结论

在这篇文章中，我讨论了适用于分布随机森林的新推断工具。我还查看了这些新能力的重要应用；用均匀置信区间估计条件见证函数。

不过，我也想提供一些警告：

1.  结果仅对给定的测试点***x***有效。

1.  结果仅在渐近意义上有效。

1.  当前的代码比它应该有的慢得多。

实际上，第一点并不是那么糟糕，在模拟中，渐近正态性通常在一范围内的**x**上也成立。*只要小心样本边界附近的测试点！* 直观地说，DRF（以及所有其他最近邻方法）需要测试点***x***周围的许多样本点来估计***x***的响应。因此，如果你训练集中的协变量*X*是标准正态分布，大多数点在-2和2之间，那么预测[-1,1]中的*x*应该没有问题。但如果你的*x*达到了-2或2，性能会迅速恶化。

> 随机森林（以及一般的最近邻方法）在预测训练集中只有少量邻居的点时表现不佳，例如***X***的支持边界上的点。

第二点也相当重要。渐近结果在当代研究中已经有些过时，研究者们更倾向于有限样本结果，这些结果需要诸如“次高斯性”的假设。我个人觉得这有点可笑，渐近结果在像这样的复杂设置中提供了极其强大的近似。实际上，对于许多目标来说，这种近似在超过1000或2000个数据点时相当准确（也许你的条件均值/分位数的覆盖率是92%而不是95%）。然而，我们引入的见证函数是一个复杂的对象，因此你需要更多的数据点来估计其不确定性区间，这样效果会更好！

最后第三点只是我们的一个缺陷：虽然DRF本身是用C语言高效编写的，但用*S*估计不确定性目前完全基于R语言。修复这一点将极大地加快代码速度。我们希望将来能够解决这个问题。

## 引用

[1] Junhyung Park, Uri Shalit, Bernhard Schölkopf 和 Krikamol Muandet. “使用核条件均值嵌入和 U 统计量回归的条件分布治疗效果。” 见《第38届国际机器学习大会（ICML）论文集》，第139卷，第8401–8412页。PMLR，2021年7月。
