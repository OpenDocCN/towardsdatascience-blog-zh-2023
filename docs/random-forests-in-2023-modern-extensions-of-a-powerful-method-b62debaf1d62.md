# 2023年的随机森林：一种强大方法的现代扩展

> 原文：[https://towardsdatascience.com/random-forests-in-2023-modern-extensions-of-a-powerful-method-b62debaf1d62?source=collection_archive---------9-----------------------#2023-11-07](https://towardsdatascience.com/random-forests-in-2023-modern-extensions-of-a-powerful-method-b62debaf1d62?source=collection_archive---------9-----------------------#2023-11-07)

## 随机森林取得了长足的发展

[](https://medium.com/@jeffrey_85949?source=post_page-----b62debaf1d62--------------------------------)[![Jeffrey Näf](../Images/0ce6db85501192cdebeeb910eb81a688.png)](https://medium.com/@jeffrey_85949?source=post_page-----b62debaf1d62--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b62debaf1d62--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b62debaf1d62--------------------------------) [Jeffrey Näf](https://medium.com/@jeffrey_85949?source=post_page-----b62debaf1d62--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fca780798011a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandom-forests-in-2023-modern-extensions-of-a-powerful-method-b62debaf1d62&user=Jeffrey+N%C3%A4f&userId=ca780798011a&source=post_page-ca780798011a----b62debaf1d62---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b62debaf1d62--------------------------------) ·12 min read·2023年11月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb62debaf1d62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandom-forests-in-2023-modern-extensions-of-a-powerful-method-b62debaf1d62&user=Jeffrey+N%C3%A4f&userId=ca780798011a&source=-----b62debaf1d62---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb62debaf1d62&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frandom-forests-in-2023-modern-extensions-of-a-powerful-method-b62debaf1d62&source=-----b62debaf1d62---------------------bookmark_footer-----------)![](../Images/11d2719adb78db10fac42a7884ac2918.png)

现代随机森林方法的特点。来源：作者。

在机器学习的时间线中，随机森林（RFs），在Breimann的开创性论文中提出（[1]），可以算得上古老了。尽管它们已经有些年头，但凭借其卓越的性能仍然令人印象深刻，并且仍是活跃研究的主题。本文旨在突出随机森林方法已成为多功能工具箱的特点，重点关注**广义随机森林（GRF）**和**分布随机森林（DRF）**。

简而言之，这两种方法的主要思想是，RF隐式产生的权重可以用于估计除条件期望之外的其他目标。GRF的理念是使用一个随机森林，分裂准则适应于所设想的目标（例如条件均值、条件分位数或条件处理效应）。DRF的理念是调整分裂准则，以便能够估计整个条件分布。从这个对象中，可以在第二步派生出许多不同的目标。事实上，本文主要讨论DRF，因为我对这种方法更为熟悉，而且它在处理广泛目标时更加简洁（只需为广泛的目标拟合一个森林）。然而，图中指出的所有优点也适用于GRF，实际上，R中的DRF包是建立在[GRF的专业实现](https://grf-labs.github.io/grf/)之上的。此外，GRF森林的分裂准则适应于目标，这意味着它可能比DRF表现更好。这对于二元*Y*尤其如此，此时应使用probability_forests()。因此，尽管我主要讨论DRF，但在整篇文章中应始终记住GRF。

本文的目标是提供一个概述，并提供相应部分的深入阅读链接。我们将按图中的时钟方向逐一介绍每个要点，引用相关的文章，并通过一个小示例进行强调。我首先快速总结了下面的最重要的进一步阅读链接：

多样性/性能： [Medium Article](/drf-a-random-forest-for-almost-everything-625fa5c3bcb8) 和原始论文（[DRF](https://jmlr.org/papers/v23/21-0585.html)/[GRF](https://projecteuclid.org/journals/annals-of-statistics/volume-47/issue-2/Generalized-random-forests/10.1214/18-AOS1709.full)）

缺失值纳入： [Medium Article](/random-forests-and-missing-values-3daaea103db0)

不确定性度量： [Medium Article](/inference-for-distributional-random-forests-64610bbb3927)

变量重要性： [Medium Article](https://medium.com/towards-data-science/variable-importance-in-random-forests-20c6690e44e0)

## 示例

我们取 *X_1, X_2, X_4, …, X_10* 独立地均匀分布在（-1,1）之间，并通过 *X_3=X_1 + uniform error* 来创建 *X_1* 和 *X_3* 之间的依赖关系。然后我们模拟 *Y* 为

![](../Images/db8b95d3d53203e739c5509eb1aa992e.png)

```py
 ## Load packages and functions needed
library(drf)
library(mice)
source("drfnew_v2.R")
## The function drfnew_v2.R is available below, or on 
## https://github.com/JeffNaef/drfupdate

## Set parameters
set.seed(10)
n<-1000

##Simulate Data that experiences both a mean as well as sd shift
# Simulate from X
x1 <- runif(n,-1,1)
x2 <- runif(n,-1,1)
x3 <- x1+ runif(n,-1,1)
X0 <- matrix(runif(7*n,-1,1), nrow=n, ncol=7)
Xfull <- cbind(x1,x2, x3, X0)
colnames(Xfull)<-paste0("X", 1:10)

# Simulate dependent variable Y
Y <- as.matrix(rnorm(n,mean = 0.8*(x1 > 0), sd = 1 + 1*(x2 > 0)))

##Also add MAR missing values using ampute from the mice package
X<-ampute(Xfull)$amp

head(cbind(Y,X))

           Y         X1          X2          X3          X4           X5
1 -3.0327466 -0.4689827  0.06161759  0.27462737          NA -0.624463079
2  1.2582824 -0.2557522  0.36972181          NA -0.04100963  0.009518047
3 -0.8781940  0.1457067 -0.23343321          NA -0.64519687 -0.945426305
4  3.1595623  0.8164156  0.90997600  0.69184618 -0.20573331 -0.007404298
5  1.1176545 -0.5966361          NA -1.21276055  0.62845399  0.894703422
6 -0.4377359  0.7967794 -0.92179989 -0.03863182  0.88271415 -0.237635732
          X6         X7          X8         X9        X10
1 -0.9290009  0.5401628  0.39735433 -0.7434697  0.8807558
2 -0.2885927  0.3805251 -0.09051334 -0.7446170  0.9935311
3 -0.5022541  0.3009541  0.29424395  0.5554647 -0.5341800
4  0.7583608 -0.8506881  0.22758566 -0.1596993 -0.7161976
5 -0.3640422  0.8051613 -0.46714833  0.4318039 -0.8674060
6 -0.3577590 -0.7341207  0.85504668 -0.6933918  0.4656891
```

请注意，通过[mice包](https://cran.r-project.org/web/packages/mice/index.html)中的ampute函数，我们将**缺失值非随机（MAR）**设置在***X***上，以突出GRF/DRF处理缺失值的能力。此外，在上述过程中，仅 *X_1* 和 *X_2* 对预测 *Y* 是相关的，所有其他变量都是“噪声”变量。这种“稀疏”设置在实际数据集中可能很常见。

现在我们选择一个测试点作为这个示例，将在整个过程中使用：

```py
x<-matrix(c(0.2, 0.4, runif(8,-1,1)), nrow=1, ncol=10)
print(x)

     [,1] [,2]      [,3]      [,4]      [,5]      [,6]    [,7]      [,8]
[1,]  0.2  0.4 0.7061058 0.8364877 0.2284314 0.7971179 0.78581 0.5310279
           [,9]     [,10]
[1,] -0.5067102 0.6918785
```

## 多样性

DRF 估计条件分布 *P_{Y|****X****=****x****}* 以简单权重的形式：

![](../Images/206d5127ad181f1e1458ee40d1784285.png)

从这些权重中，可以计算出广泛的目标，或者可以用来从条件分布中进行模拟。它的多功能性一个很好的参考是原始的 [研究文章](https://jmlr.org/papers/v23/21-0585.html)，其中使用了很多例子，还有相应的 [medium 文章](/drf-a-random-forest-for-almost-everything-625fa5c3bcb8)。

在这个例子中，我们首先从这个分布中进行模拟：

```py
DRF<-drfCI(X=X, Y=Y, B=50,num.trees=1000, min.node.size = 5)
DRFpred<-predictdrf(DRF, newdata=x)

## Sample from P_{Y| X=x}
Yxs<-Y[sample(1:n, size=n, replace = T, DRFpred$weights[1,])]
hist(Yxs, prob=T)
z<-seq(-6,7,by=0.01)
d<-dnorm(z, mean=0.8 * (x[1] > 0), sd=(1+(x[2] > 0)))
lines(z,d, col="darkred"  )
```

![](../Images/707d8f363302b0a531f4172989c20161.png)

模拟条件分布的直方图与真实密度（红色）叠加。来源：作者。

图表显示了从条件分布中大致抽取的样本，并与真实密度（红色）叠加。我们现在使用此数据来估计 **条件期望** 和 **条件 (0.05, 0.95) 分位数** 在 ***x:***

```py
# Calculate quantile prediction as weighted quantiles from Y
qx <- quantile(Yxs, probs = c(0.05,0.95))

# Calculate conditional mean prediction
mux <- mean(Yxs)

# True quantiles
q1<-qnorm(0.05, mean=0.8 * (x[1] > 0), sd=(1+(x[2] > 0)))
q2<-qnorm(0.95, mean=0.8 * (x[1] > 0), sd=(1+(x[2] > 0)))
mu<-0.8 * (x[1] > 0)

hist(Yxs, prob=T)
z<-seq(-6,7,by=0.01)
d<-dnorm(z, mean=0.8 * (x[1] > 0), sd=(1+(x[2] > 0)))
lines(z,d, col="darkred"  )
abline(v=q1,col="darkred" )
abline(v=q2, col="darkred" )
abline(v=qx[1], col="darkblue")
abline(v=qx[2], col="darkblue")
abline(v=mu, col="darkred")
abline(v=mux, col="darkblue")
```

![](../Images/b011f4a2998015c8a6f07661ea241c2c.png)

模拟条件分布的直方图与真实密度（红色）叠加。此外，估计的条件期望和条件 (0.05, 0.95) 分位数为蓝色，真实值为红色。来源：作者。

同样，GRF 可以计算许多目标，但在这种情况下，每个目标需要拟合不同的森林。特别是，*regression_forest()* 用于条件期望，*quantile_forest()* 用于分位数。

GRF 无法处理多变量目标，而 DRF 可以做到这一点。

## 性能

尽管有大量关于强大新（非参数）方法的工作，如神经网络，但基于树的方法在表格数据上始终能够击败竞争者。请参见，例如，这篇 [引人入胜的论文](https://arxiv.org/abs/2207.08815)，或这篇关于 [RF 在分类中的强度](https://dl.acm.org/doi/pdf/10.5555/2627435.2697065) 的旧论文。

公平地说，通过参数调优，[提升树方法](/an-overview-of-boosting-methods-catboost-xgboost-adaboost-lightboost-histogram-based-gradient-407447633ac1)，如 XGboost，通常在经典预测（对应于条件期望估计）中占据主导地位。然而，RF 方法在没有任何调优下展现的强大性能是值得注意的。此外，还有关于改进随机森林性能的研究，例如，[对冲随机森林方法](https://arxiv.org/pdf/2308.15384.pdf)。

## 缺失值整合

来自 [这篇论文](https://www.sciencedirect.com/science/article/abs/pii/S0167865508000305) 的“缺失数据合并属性标准”（MIA）是一个非常简单但非常强大的想法，它允许基于树的方法处理缺失数据。这已在 GRF R 包中实现，因此 DRF 中也可以使用。详细信息也在这篇 [medium 文章](/random-forests-and-missing-values-3daaea103db0) 中解释。尽管这一概念很简单，但在实践中效果非常好：在上述示例中，DRF 在处理训练数据中的大量 MAR 缺失值时毫不费力 ***X*** (!)

## **不确定性度量**

作为统计学家，我不仅希望得到点估计（即使是分布的点估计），还希望得到我的参数（即使“参数”是我的整个分布）的 **估计不确定性**。事实证明，DRF/GRF 中内置的简单额外子抽样可以为大样本量提供原则性的不确定性量化。关于 DRF 的理论在这篇 [研究文章](https://arxiv.org/abs/2302.05761) 中得以推导，我在这篇 [medium 文章](/inference-for-distributional-random-forests-64610bbb3927) 中也进行了解释。GRF 的所有理论在 [原始论文](https://projecteuclid.org/journals/annals-of-statistics/volume-47/issue-2/Generalized-random-forests/10.1214/18-AOS1709.full) 中都有描述。

我们将其应用于上述示例：

```py
# Calculate uncertainty
alpha<-0.05
B<-length(DRFpred$weightsb)
qxb<-matrix(NaN, nrow=B, ncol=2)
muxb<-matrix(NaN, nrow=B, ncol=1)
for (b in 1:B){
Yxsb<-Y[sample(1:n, size=n, replace = T, DRFpred$weightsb[[b]][1,])]
qxb[b,] <- quantile(Yxsb, probs = c(0.05,0.95))
muxb[b] <- mean(Yxsb)
}
CI.lower.q1 <- qx[1] - qnorm(1-alpha/2)*sqrt(var(qxb[,1]))
CI.upper.q1 <- qx[1] + qnorm(1-alpha/2)*sqrt(var(qxb[,1]))

CI.lower.q2 <- qx[2] - qnorm(1-alpha/2)*sqrt(var(qxb[,2]))
CI.upper.q2 <- qx[2] + qnorm(1-alpha/2)*sqrt(var(qxb[,2]))

CI.lower.mu <- mux - qnorm(1-alpha/2)*sqrt(var(muxb))
CI.upper.mu <- mux + qnorm(1-alpha/2)*sqrt(var(muxb))

hist(Yxs, prob=T)
z<-seq(-6,7,by=0.01)
d<-dnorm(z, mean=0.8 * (x[1] > 0), sd=(1+(x[2] > 0)))
lines(z,d, col="darkred"  )
abline(v=q1,col="darkred" )
abline(v=q2, col="darkred" )
abline(v=qx[1], col="darkblue")
abline(v=qx[2], col="darkblue")
abline(v=mu, col="darkred")
abline(v=mux, col="darkblue")
abline(v=CI.lower.q1, col="darkblue", lty=2)
abline(v=CI.upper.q1, col="darkblue", lty=2)
abline(v=CI.lower.q2, col="darkblue", lty=2)
abline(v=CI.upper.q2, col="darkblue", lty=2)
abline(v=CI.lower.mu, col="darkblue", lty=2)
abline(v=CI.upper.mu, col="darkblue", lty=2)
```

![](../Images/bb22e3661f626b01f57210c507fef1c2.png)

模拟条件分布的直方图叠加了真实密度（红色）。此外，估计的条件期望和条件 (0.05, 0.95) 分位数为蓝色，真实值为红色。此外，虚线红色的线条是由 DRF 计算出的估计置信区间。来源：作者。

从上述代码可以看出，我们实际上有 *B* 个子树可以用来每次计算度量。通过这些 *B* 个均值和分位数样本，我们可以计算方差，并使用正态近似来获得图中虚线所示的（渐近）置信区间。*尽管存在缺失值，这一切依然可以完成* ***X***(!)

**变量重要性**

随机森林的一个重要方面是有效计算变量重要性度量。尽管传统的度量方法有些随意，但对于传统 RF 和 DRF，现在已经有了有原则的度量方法，如在这篇 [medium article](https://medium.com/towards-data-science/variable-importance-in-random-forests-20c6690e44e0) 中解释的那样。对于 RF，Sobol-MDA 方法可靠地识别出对条件期望估计最重要的变量，而对于 DRF，MMD-MDA 识别出对整体分布估计最重要的变量。如文章中讨论的那样，使用 *投影随机森林* 的理念，这些度量可以非常高效地实现。我们在例子中展示了 MMD 变量重要性度量的一个较低效实现：

```py
## Variable importance for conditional Quantile Estimation

## For the conditional quantiles we use a measure that considers the whole distribution,
## i.e. the MMD based measure of DRF.
MMDVimp <- compute_drf_vimp(X=X,Y=Y, print=F)
sort(MMDVimp, decreasing = T)

         X2          X1          X8          X6          X3         X10 
0.852954299 0.124110913 0.012194176 0.009578300 0.008191663 0.007517931 
         X9          X7          X5          X4 
0.006861688 0.006632175 0.005257195 0.002401974 
```

在这里，*X_1* 和 *X_2* 在尝试估计分布时被正确地识别为最相关的变量。值得注意的是，尽管 *X_3* 和 *X_1* 之间存在依赖关系，但测量方法正确地量化了 *X_3* 对于预测 *Y* 分布的重要性。这是随机森林原始 MDA 测量往往做错的事情，如中等文章所示。此外，请再次注意，***X*** 中的缺失值在这里没有问题。

## 结论

GRF/DRF 以及传统的随机森林在任何数据科学家的工具箱中都不应缺少。尽管像 XGboost 这样的方法在传统预测中可能表现更好，但现代基于 RF 的方法的许多优点使它们成为一个极其多功能的工具。

当然，需要记住的是这些方法仍然完全是非参数的，并且需要大量的数据点才能使拟合有意义。这一点在不确定性量化中尤其真实，它仅在渐近情况下有效，即对于“大”样本。

## 文献

[1] Breiman, L. (2001). Random forests. Machine learning, 45(1):5–32.

## 附录：代码

```py
require(drf)
require(Matrix)
require(kernlab)

drfCI <- function(X, Y, B, sampling = "binomial", ...) {

  n <- dim(X)[1]

  # compute point estimator and DRF per halfsample
  # weightsb: B times n matrix of weights
  DRFlist <- lapply(seq_len(B), function(b) {

    # half-sample index
    indexb <- if (sampling == "binomial") {
      seq_len(n)[as.logical(rbinom(n, size = 1, prob = 0.5))]
    } else {
      sample(seq_len(n), floor(n / 2), replace = FALSE)
    }

    ## Using normal Bootstrap on the data and refitting DRF
    DRFb <-
      drfown(X = X[indexb, , drop = F], Y = Y[indexb, , drop = F], ...)

    return(list(DRF = DRFb, indices = indexb))
  })

  return(list(DRFlist = DRFlist, X = X, Y = Y))
}

predictdrf <- function(DRF, newdata, functional = NULL, ...) {

  ##Predict for testpoints in newdata, with B weights for each point, representing
  ##uncertainty

  ntest <- nrow(x)
  n <- nrow(DRF$Y)

  weightsb <- lapply(DRF$DRFlist, function(l) {

    weightsbfinal <- Matrix(0, nrow = ntest, ncol = n, sparse = TRUE)

    weightsbfinal[, l$indices] <- predict(l$DRF, x)$weights 

    return(weightsbfinal)
  })

  weightsall <- Reduce("+", weightsb) / length(weightsb)

  if (!is.null(functional)) {
    stopifnot("Not yet implemented for several x" = ntest == 1)

    thetahatb <- 
      lapply(weightsb, function(w)  
        functional(weights = w, X = DRF$X, Y = DRF$Y, x = x))
    thetahatbforvar <- do.call(rbind, thetahatb)
    thetahat <- functional(weights = weightsall, X = DRF$X, Y = DRF$Y, x = x)
    thetahat <- matrix(thetahat, nrow = dim(x)[1])
    var_est <- if (dim(thetahat)[2] > 1) {  
      a <- sweep(thetahatbforvar, 2, thetahat, FUN = "-")
      crossprod(a, a) / B
    } else {
      mean((c(thetahatbforvar) - c(thetahat)) ^ 2) 
    }

    return(list(weights = weightsall, thetahat = thetahat, weightsb = weightsb, 
                var_est = var_est))

  } else {
    return(list(weights = weightsall, weightsb = weightsb))
  }
}

drfown <-               function(X, Y,
                                 num.trees = 500,
                                 splitting.rule = "FourierMMD",
                                 num.features = 10,
                                 bandwidth = NULL,
                                 response.scaling = TRUE,
                                 node.scaling = FALSE,
                                 sample.weights = NULL,
                                 sample.fraction = 0.5,
                                 mtry = min(ceiling(sqrt(ncol(X)) + 20), ncol(X)),
                                 min.node.size = 15,
                                 honesty = TRUE,
                                 honesty.fraction = 0.5,
                                 honesty.prune.leaves = TRUE,
                                 alpha = 0.05,
                                 imbalance.penalty = 0,
                                 compute.oob.predictions = TRUE,
                                 num.threads = NULL,
                                 seed = stats::runif(1, 0, .Machine$integer.max),
                                 compute.variable.importance = FALSE) {

  # initial checks for X and Y
  if (is.data.frame(X)) {

    if (is.null(names(X))) {
      stop("the regressor should be named if provided under data.frame format.")
    }

    if (any(apply(X, 2, class) %in% c("factor", "character"))) {
      any.factor.or.character <- TRUE
      X.mat <- as.matrix(fastDummies::dummy_cols(X, remove_selected_columns = TRUE))
    } else {
      any.factor.or.character <- FALSE
      X.mat <- as.matrix(X)
    }

    mat.col.names.df <- names(X)
    mat.col.names <- colnames(X.mat)
  } else {
    X.mat <- X
    mat.col.names <- NULL
    mat.col.names.df <- NULL
    any.factor.or.character <- FALSE
  }

  if (is.data.frame(Y)) {

    if (any(apply(Y, 2, class) %in% c("factor", "character"))) {
      stop("Y should only contain numeric variables.")
    }
    Y <- as.matrix(Y)
  }

  if (is.vector(Y)) {
    Y <- matrix(Y,ncol=1)
  }

  #validate_X(X.mat)

  if (inherits(X, "Matrix") && !(inherits(X, "dgCMatrix"))) {
    stop("Currently only sparse data of class 'dgCMatrix' is supported.")
  }

  drf:::validate_sample_weights(sample.weights, X.mat)
  #Y <- validate_observations(Y, X)

  # set legacy GRF parameters
  clusters <- vector(mode = "numeric", length = 0)
  samples.per.cluster <- 0
  equalize.cluster.weights <- FALSE
  ci.group.size <- 1

  num.threads <- drf:::validate_num_threads(num.threads)

  all.tunable.params <- c("sample.fraction", "mtry", "min.node.size", "honesty.fraction",
                          "honesty.prune.leaves", "alpha", "imbalance.penalty")

  # should we scale or not the data
  if (response.scaling) {
    Y.transformed <- scale(Y)
  } else {
    Y.transformed <- Y
  }

  data <- drf:::create_data_matrices(X.mat, outcome = Y.transformed, sample.weights = sample.weights)

  # bandwidth using median heuristic by default
  if (is.null(bandwidth)) {
    bandwidth <- drf:::medianHeuristic(Y.transformed)
  }

  args <- list(num.trees = num.trees,
               clusters = clusters,
               samples.per.cluster = samples.per.cluster,
               sample.fraction = sample.fraction,
               mtry = mtry,
               min.node.size = min.node.size,
               honesty = honesty,
               honesty.fraction = honesty.fraction,
               honesty.prune.leaves = honesty.prune.leaves,
               alpha = alpha,
               imbalance.penalty = imbalance.penalty,
               ci.group.size = ci.group.size,
               compute.oob.predictions = compute.oob.predictions,
               num.threads = num.threads,
               seed = seed,
               num_features = num.features,
               bandwidth = bandwidth,
               node_scaling = ifelse(node.scaling, 1, 0))

  if (splitting.rule == "CART") {
    ##forest <- do.call(gini_train, c(data, args))
    forest <- drf:::do.call.rcpp(drf:::gini_train, c(data, args))
    ##forest <- do.call(gini_train, c(data, args))
  } else if (splitting.rule == "FourierMMD") {
    forest <- drf:::do.call.rcpp(drf:::fourier_train, c(data, args))
  } else {
    stop("splitting rule not available.")
  }

  class(forest) <- c("drf")
  forest[["ci.group.size"]] <- ci.group.size
  forest[["X.orig"]] <- X.mat
  forest[["is.df.X"]] <- is.data.frame(X)
  forest[["Y.orig"]] <- Y
  forest[["sample.weights"]] <- sample.weights
  forest[["clusters"]] <- clusters
  forest[["equalize.cluster.weights"]] <- equalize.cluster.weights
  forest[["tunable.params"]] <- args[all.tunable.params]
  forest[["mat.col.names"]] <- mat.col.names
  forest[["mat.col.names.df"]] <- mat.col.names.df
  forest[["any.factor.or.character"]] <- any.factor.or.character

  if (compute.variable.importance) {
    forest[['variable.importance']] <- variableImportance(forest, h = bandwidth)
  }

  forest
}

#' Variable importance for Distributional Random Forests
#'
#' @param X Matrix with input training data.
#' @param Y Matrix with output training data.
#' @param X_test Matrix with input testing data. If NULL, out-of-bag estimates are used.
#' @param num.trees Number of trees to fit DRF. Default value is 500 trees.
#' @param silent If FALSE, print variable iteration number, otherwise nothing is print. Default is FALSE.
#'
#' @return The list of importance values for all input variables.
#' @export
#'
#' @examples
compute_drf_vimp <- function(X, Y, X_test = NULL, num.trees = 500, silent = FALSE){

  # fit initial DRF
  bandwidth_Y <- drf:::medianHeuristic(Y)
  k_Y <- rbfdot(sigma = bandwidth_Y)
  K <- kernelMatrix(k_Y, Y, Y)
  DRF <- drfown(X, Y, num.trees = num.trees)
  wall <- predict(DRF, X_test)$weights

  # compute normalization constant
  wbar <- colMeans(wall)
  wall_wbar <- sweep(wall, 2, wbar, "-")
  I0 <- as.numeric(sum(diag(wall_wbar %*% K %*% t(wall_wbar))))

  # compute drf importance dropping variables one by one
  I <- sapply(1:ncol(X), function(j) {
    if (!silent){print(paste0('Running importance for variable X', j, '...'))}
    DRFj <- drfown(X = X[, -j, drop=F], Y = Y, num.trees = num.trees) 
    DRFpredj <- predict(DRFj, X_test[, -j])
    wj <- DRFpredj$weights
    Ij <- sum(diag((wj - wall) %*% K %*% t(wj - wall)))/I0
    return(Ij)
  })

  # compute retraining bias
  DRF0 <- drfown(X = X, Y = Y, num.trees = num.trees)
  DRFpred0 = predict(DRF0, X_test)
  w0 <- DRFpred0$weights
  vimp0 <- sum(diag((w0 - wall) %*% K %*% t(w0 - wall)))/I0

  # compute final importance (remove bias & truncate negative values)
  vimp <- sapply(I - vimp0, function(x){max(0,x)})

  names(vimp)<-colnames(X)

  return(vimp)

} 
```
