# 使用GPBoost进行分组和区域空间计量经济数据的混合效应机器学习

> 原文：[https://towardsdatascience.com/mixed-effects-machine-learning-with-gpboost-for-grouped-and-areal-spatial-econometric-data-b26f8bddd385?source=collection_archive---------3-----------------------#2023-06-21](https://towardsdatascience.com/mixed-effects-machine-learning-with-gpboost-for-grouped-and-areal-spatial-econometric-data-b26f8bddd385?source=collection_archive---------3-----------------------#2023-06-21)

## 使用欧洲GDP数据的演示

[](https://medium.com/@fabsig?source=post_page-----b26f8bddd385--------------------------------)[![Fabio Sigrist](../Images/f7bc2adc17255ae1efd0886a19ec202c.png)](https://medium.com/@fabsig?source=post_page-----b26f8bddd385--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b26f8bddd385--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b26f8bddd385--------------------------------) [Fabio Sigrist](https://medium.com/@fabsig?source=post_page-----b26f8bddd385--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5b503a0c329&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-with-gpboost-for-grouped-and-areal-spatial-econometric-data-b26f8bddd385&user=Fabio+Sigrist&userId=b5b503a0c329&source=post_page-b5b503a0c329----b26f8bddd385---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b26f8bddd385--------------------------------) ·14分钟阅读·2023年6月21日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb26f8bddd385&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-with-gpboost-for-grouped-and-areal-spatial-econometric-data-b26f8bddd385&user=Fabio+Sigrist&userId=b5b503a0c329&source=-----b26f8bddd385---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb26f8bddd385&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-with-gpboost-for-grouped-and-areal-spatial-econometric-data-b26f8bddd385&source=-----b26f8bddd385---------------------bookmark_footer-----------)![](../Images/062de699818ab3d483814ea714b96cbb.png)

欧洲GDP数据：预测变量‘edu’的空间效应和SHAP依赖图 — 作者提供的图片

[GPBoost算法](https://www.jmlr.org/papers/v23/20-322.html)通过用树提升建模的非参数非线性函数替换线性固定效应函数，扩展了线性混合效应和高斯过程模型。本文展示了如何使用在`[GPBoost](https://github.com/fabsig/GPBoost)` [库](https://github.com/fabsig/GPBoost)中实现的GPBoost算法来建模具有空间和分组结构的数据。我们使用欧洲GDP数据展示`GPBoost`库的功能，该数据是一个区域空间计量经济学数据的例子。

## 目录

∘ [介绍](#2d36)

· ·[GPBoost的基本工作流程](#7b6e)

· ·[数据描述](#d3b9)

· ·[数据加载和简短可视化](#be54)

∘ [训练GPBoost模型](#ef69)

∘ [选择调优参数](#e5e7)

∘ [模型解释](#b7b0)

· ·[估计的随机效应模型](#54e9)

· ·[空间效应图](#c547)

· ·[理解固定效应函数](#0ff8)

∘ [扩展](#af79)

· ·[不同时间段的独立随机效应](#756a)

· ·[空间与固定效应预测变量之间的交互](#7121)

· ·[大数据](#c6c4)

· ·[其他空间随机效应模型](#24bc)

· ·[(广义)线性混合效应和高斯过程模型](#4342)

∘ [参考文献](#376a)

# 介绍

## GPBoost的基本工作流程

应用GPBoost模型（= 组合树提升和随机效应/GP模型）包括以下主要步骤：

1.  定义一个`GPModel`，在其中指定以下内容：

    - 随机效应模型（例如，空间随机效应、分组随机效应、组合空间和分组等）

    - 似然（= 条件于固定和随机效应的响应变量的分布）

1.  创建一个包含响应变量和固定效应预测变量的`gpb.Dataset`

1.  选择调优参数，例如，使用函数`gpb.grid.search.tune.parameters`

1.  使用`gpboost / gpb.train`函数训练模型

1.  解释训练好的模型和/或进行预测

在本文中，我们使用一个实际数据集展示这些步骤。此外，我们还展示了如何解释拟合模型，并查看`GPBoost`库的几个扩展和附加功能。所有结果均使用`GPBoost`版本1.2.1获得。此演示使用R包，但相应的Python包提供相同的功能。

## 数据描述

本演示使用的数据是欧洲国内生产总值（GDP）数据。可以从[https://raw.githubusercontent.com/fabsig/GPBoost/master/data/gdp_european_regions.csv](https://raw.githubusercontent.com/fabsig/GPBoost/master/data/gdp_european_regions.csv)下载。数据由罗马第三大学Massimo Giannini从Eurostat收集，并于2023年6月16日提供给Fabio Sigrist，用于罗马第三大学的讲座。

数据收集了2000年和2021年这两年的242个欧洲区域的数据。即，总数据点数为484。响应变量是（对数）人均GDP。有四个预测变量：

+   L: （对数）就业份额（empl/pop）

+   K: （对数）固定资本/人口

+   Pop: 对数（人口）

+   Edu: 高等教育份额

此外，还有每个区域的中心空间坐标（经度和纬度），242个不同欧洲区域的空间区域ID，以及一个额外的空间集群ID，用于标识区域所属的集群（共有两个集群）。

## 数据加载与简短可视化

我们首先加载数据，并创建一张地图来展示空间上的（对数）人均GDP。我们创建了两张地图：一张包含所有数据，另一张则排除了一些偏远的岛屿。在下面的注释代码中，我们还展示了在没有空间多边形形状文件的情况下如何创建空间图。

```py
library(gpboost)
library(ggplot2)
library(gridExtra)
library(viridis)
library(sf)

# Load data
data <- read.csv("https://raw.githubusercontent.com/fabsig/GPBoost/master/data/gdp_european_regions.csv")
FID <- data$FID
data <- as.matrix(data[,names(data)!="FID"]) # convert to matrix since the boosting part currently does not support data.frames
covars <- c("L", "K", "pop", "edu")
# Load shape file for spatial plots
cur_tempfile <- tempfile()
download.file(url = "https://raw.githubusercontent.com/fabsig/GPBoost/master/data/shape_European_regions.zip", destfile = cur_tempfile)
out_directory <- tempfile()
unzip(cur_tempfile, exdir = out_directory)
shape <- st_read(dsn = out_directory)

# Create spatial plot of GDP
data_plot <- merge(shape, data.frame(FID = FID, y = data[,"y"]), by="FID")
p1 <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
  scale_fill_viridis(name="GDP / capita (log)", option = "B") + 
   ggtitle("GDP / capita (log)")
# Sample plot excluding islands
p2 <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
  scale_fill_viridis(name="GDP / capita (log)", option = "B") +
  xlim(2700000,6400000) + ylim(1500000,5200000) + 
  ggtitle("GDP / capita (log) -- excluding islands")
grid.arrange(p1, p2, ncol=2)

# # Spatial plot without a shape file
# p1 <- ggplot(data = data.frame(Lat=data[,"Lat"], Long=data[,"Long"],
#                                GDP=data[,"y"]), aes(x=Long,y=Lat,color=GDP)) +
#   geom_point(size=2, alpha=0.5) + scale_color_viridis(option = "B") + 
#   ggtitle("GDP / capita (log)")
# p2 <- ggplot(data = data.frame(Lat=data[,"Lat"], Long=data[,"Long"], 
#                                GDP=data[,"y"]), aes(x=Long,y=Lat,color=GDP)) +
#   geom_point(size=3, alpha=0.5) + scale_color_viridis(option = "B") + 
#   ggtitle("GDP / capita (log) -- Europe excluding islands") + xlim(-10,28) + ylim(35,67)
# grid.arrange(p1, p2, ncol=2)
```

![](../Images/a9d02a89d33ceec0b1f6cc8e5f25df23.png)

242个欧洲区域的人均GDP（对数）——作者图片

# 训练 GPBoost 模型

接下来，我们使用带有指数协方差函数的高斯过程模型来建模空间随机效应。此外，我们还包括了集群变量cl的分组随机效应。在`GPBoost`库中，高斯过程随机效应由`gp_coords`参数定义，分组随机效应通过`GPModel`构造函数的`group_data`参数来定义。上述预测变量用于固定效应的树集成函数。我们使用`gpboost`，或等效地`gpb.train`函数来拟合模型。请注意，我们使用的调整参数是通过交叉验证选择的。

```py
gp_model <- GPModel(group_data = data[, c("cl")], 
                    gp_coords = data[, c("Long", "Lat")],
                    likelihood = "gaussian", cov_function = "exponential")
params <- list(learning_rate = 0.01, max_depth = 2, num_leaves = 2^10,
               min_data_in_leaf = 10, lambda_l2 = 0)
nrounds <- 37
# gp_model$set_optim_params(params = list(trace=TRUE)) # To monitor hyperparameter estimation
boost_data <- gpb.Dataset(data = data[, covars], label = data[, "y"])
gpboost_model <- gpboost(data = boost_data, gp_model = gp_model, 
                         nrounds = nrounds, params = params, 
                         verbose = 1) # same as gpb.train# same as gpb.train gpb.train
```

# 选择调整参数

调整参数对于提升模型的性能至关重要。没有通用的默认值，每个数据集可能需要不同的调整参数。下面我们展示了如何使用`gpb.grid.search.tune.parameters`函数选择调整参数。我们使用均方误差（`mse`）作为验证数据上的预测准确性指标。或者，也可以使用例如测试负对数似然（`test_neg_log_likelihood` = 默认值，如果没有指定）来考虑预测的不确定性。*根据数据集和网格大小，这可能需要一些时间。除了下面的确定性网格搜索，还可以使用随机网格搜索来加快速度（参见* `*num_try_random*`*）。*

```py
gp_model <- GPModel(group_data = data[, "cl"], 
                    gp_coords = data[, c("Long", "Lat")],
                    likelihood = "gaussian", cov_function = "exponential")
boost_data <- gpb.Dataset(data = data[, covars], label = data[, "y"])
param_grid = list("learning_rate" = c(1,0.1,0.01), 
                  "min_data_in_leaf" = c(10,100,1000),
                  "max_depth" = c(1,2,3,5,10),
                  "lambda_l2" = c(0,1,10))
other_params <- list(num_leaves = 2^10)
set.seed(1)
opt_params <- gpb.grid.search.tune.parameters(param_grid = param_grid, params = other_params,
                                              num_try_random = NULL, nfold = 4,
                                              data = boost_data, gp_model = gp_model, 
                                              nrounds = 1000, early_stopping_rounds = 10,
                                              verbose_eval = 1, metric = "mse") # metric = "test_neg_log_likelihood"
opt_params
# ***** New best test score (l2 = 0.0255393919591794) found for the following parameter combination: learning_rate: 0.01, min_data_in_leaf: 10, max_depth: 2, lambda_l2: 0, nrounds: 37
```

# 模型解释

## 估计的随机效应模型

通过对 `gp_model` 调用 `summary` 函数，可以获取估计的随机效应模型的信息。对于高斯过程，`GP_var` 是边际方差，即空间相关性或结构空间变化的量，`GP_range` 是测量相关性随空间衰减速度的范围或尺度参数。对于指数协方差函数，该值的三倍（这里大约为 17）是（残差）空间相关性基本为零的距离（低于 5%）。如下面的结果所示，由于边际方差 0.0089 相对于响应变量的总方差约 0.29 较小，空间相关性量相对较小。此外，误差项和 cl 分组随机效应的方差也较小（0.013 和 0.022）。因此，我们得出结论，大部分响应变量的方差由固定效应预测变量解释。

```py
summary(gp_model)
```

```py
## =====================================================
## Covariance parameters (random effects):
##            Param.
## Error_term 0.0131
## Group_1    0.0221
## GP_var     0.0089
## GP_range   5.7502
## =====================================================
```

## 空间效应图

我们可以通过对训练数据调用 `predict` 函数来绘制在训练数据位置估计的（“预测”）空间随机效应；见下面的代码。这样的图显示了在剔除固定效应预测变量的效应后，空间效应的情况。注意，这些空间效应考虑了空间高斯过程和分组区域集群随机效应。如果想仅获取来自高斯过程部分的空间随机效应，可以使用 `predict_training_data_random_effects` 函数（见下面的注释代码）。或者，也可以进行空间外推（“克里金插值”），但这对于区域数据并没有太大意义。

```py
pred <- predict(gpboost_model, group_data_pred = data[1:242, c("cl")], 
                gp_coords_pred = data[1:242, c("Long", "Lat")],
                data = data[1:242, covars], predict_var = TRUE, pred_latent = TRUE)
data_plot <- merge(shape, data.frame(FID = FID, y = pred$random_effect_mean), by="FID")
plot_mu <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
  scale_fill_viridis(name="Spatial effect", option = "B") +
  ggtitle("Spatial effect (mean)") + xlim(2700000,6400000) + ylim(1500000,5200000)
data_plot <- merge(shape, data.frame(FID = FID, y = pred$random_effect_cov), by="FID")
plot_sd <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
  scale_fill_viridis(name="Std. dev.", option = "B") +
  ggtitle("Uncertainty (std. dev.)") + xlim(2700000,6400000) + ylim(1500000,5200000)
grid.arrange(plot_mu, plot_sd, ncol=2)

# Only spatial effects from the Gaussian process ignoring the grouped random effects
# rand_effs <- predict_training_data_random_effects(gp_model, predict_var = TRUE)[1:242, c("GP", "GP_var")]
# data_plot <- merge(shape, data.frame(FID = FID, y = rand_effs[,1]), by="FID")
# plot_mu <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
#   scale_fill_viridis(name="Spatial effect (mean)", option = "B") +
#   ggtitle("Spatial effect from Gausian process (mean)") + xlim(2700000,6400000) + ylim(1500000,5200000)
# data_plot <- merge(shape, data.frame(FID = FID, y = rand_effs[,2]), by="FID")
# plot_sd <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
#   scale_fill_viridis(name="Uncertainty (std. dev.)", option = "B") +
#   ggtitle("Uncertainty (std. dev.)") + xlim(2700000,6400000) + ylim(1500000,5200000)
# grid.arrange(plot_mu, plot_sd, ncol=2)
```

![](../Images/eec9eefd13bc12eb6b1d990b57abf579.png)

空间效应（在剔除预测变量后）— 作者提供的图片

## 理解固定效应函数

有几个工具可以帮助理解固定效应函数的形式。下面我们考虑变量重要性度量、交互作用度量和依赖图。具体而言，我们查看

+   SHAP 值

+   SHAP 依赖图

+   基于分裂的变量重要性

+   Friedman 的 H 统计量

+   部分依赖图（单维和二维）

+   SHAP 力量图

如下结果所示，从 SHAP 值和 SHAP 依赖图获得的信息与查看传统变量重要性度量和部分依赖图的信息相同。最重要的变量是‘K’和‘edu’。从依赖图中，我们得出有非线性的结论。例如，K 的效果在 K 的大值和小值之间几乎是平的，中间增加。此外，edu 的效果在 edu 的小值时更陡峭，而在 edu 的大值时变平坦。Friedman 的 H 统计量表明存在一些交互。对于交互量最大的两个变量 L 和 pop，我们在下面创建了一个二维部分依赖图。此外，SHAP 力量图显示预测变量在 2000 年与 2021 年的效果不同。

```py
# SHAP values
library(SHAPforxgboost)
shap.plot.summary.wrap1(gpboost_model, X = data[,covars]) + ggtitle("SHAP values")
```

![](../Images/f27d1ca8969afa1d0b3a58297a1bd031.png)

SHAP 值 — 作者提供的图像

```py
# SHAP dependence plots
shap_long <- shap.prep(gpboost_model, X_train = data[,covars])
shap.plot.dependence(data_long = shap_long, x = covars[2], 
                           color_feature = covars[4], smooth = FALSE, size = 2) + 
  ggtitle("SHAP dependence plot for K")
shap.plot.dependence(data_long = shap_long, x = covars[4], 
                           color_feature = covars[2], smooth = FALSE, size = 2) + 
  ggtitle("SHAP dependence plot for edu")
```

![](../Images/6e0fac74d9d2505850ce6b95fa2fd422.png)![](../Images/0784fee59410db145bdd0cc7cc305343.png)

K 和 edu 的 SHAP 依赖图 — 作者提供的图像

```py
# SHAP force plot
shap_contrib <- shap.values(gpboost_model, X_train = data[,covars])
plot_data <- cbind(shap_contrib$shap_score, ID = 1:dim(data)[1])
shap.plot.force_plot(plot_data, zoom_in=FALSE, id = "ID") + 
  scale_fill_discrete(name="Variable") + xlab("Regions") + 
  geom_vline(xintercept=242.5, linewidth=1) + ggtitle("SHAP force plot") + 
  geom_text(x=100,y=-1.2,label="2000") + geom_text(x=400,y=-1.2,label="2021")
```

![](../Images/0e321d9eeb6a5fbb05aab9dc23ac2352.png)

SHAP 力量图 — 作者提供的图像

```py
# Split-based feature importances
feature_importances <- gpb.importance(gpboost_model, percentage = TRUE)
gpb.plot.importance(feature_importances, top_n = 25, measure = "Gain", 
                    main = "Split-based variable importances")
```

![](../Images/0d4f954fa575c4dc0464cb08c414494b.png)

变量重要性 — 作者提供的图像

```py
# H-statistic for interactions
library(flashlight)
fl <- flashlight(model = gpboost_model, data = data.frame(y = data[,"y"], data[,covars]), 
                 y = "y", label = "gpb",
                 predict_fun = function(m, X) predict(m, data.matrix(X[,covars]), 
                                                      gp_coords_pred = matrix(-100, ncol = 2, nrow = dim(X)[1]),
                                                      group_data_pred = matrix(-1, ncol = 1, nrow = dim(X)[1]),
                                                      pred_latent = TRUE)$fixed_effect)
plot(imp <- light_interaction(fl, v = covars, pairwise = TRUE)) + 
  ggtitle("H interaction statistic") # takes a few seconds
```

![](../Images/69443fe3fb62bf9412af097b9fa98373.png)

H 交互统计量 — 作者提供的图像

```py
# Partial dependence plots
gpb.plot.partial.dependence(gpboost_model, data[,covars], variable = 2, xlab = covars[2], 
                            ylab = "gdp", main = "Partial dependence plot" )
gpb.plot.partial.dependence(gpboost_model, data[,covars], variable = 4, xlab = covars[4], 
                            ylab = "gdp", main = "Partial dependence plot" )
```

![](../Images/54e13345a6c9d75571cebbdfc6150a27.png)![](../Images/ea282da3803707bb230d8ee1538975da.png)

K 和 edu 的部分依赖图 — 作者提供的图像

```py
# Two-dimensional partial dependence plot (to visualize interactions)
i = 1; j = 3;# i vs j
gpb.plot.part.dep.interact(gpboost_model, data[,covars], variables = c(i,j), xlab = covars[i], 
                           ylab = covars[j], main = "Pairwise partial dependence plot")
```

![](../Images/796902abd05b6916537a9a57081eb09e.png)

pop 和 L 的二维部分依赖图 — 作者提供的图像

# 扩展

## 不同时间段的随机效应

在上述模型中，我们对 2000 年和 2021 年使用了相同的随机效应。或者，也可以对不同时间段使用独立的空间和分组随机效应（*在先验下独立，基于数据有依赖……*）。在 `GPBoost` 库中，这可以通过 `cluster_ids` 参数完成。`cluster_ids` 需要是一个长度为样本量的向量，每个条目表示观测值所属的“集群”。不同集群具有独立的空间和分组随机效应，但超参数（例如，边际方差、方差成分等）和固定效应函数在集群之间是相同的。

下面我们展示了如何拟合这样的模型并创建两个单独的空间地图。如下结果所示，2000年和2021年的空间效应相差较大。特别是，与2000年相比，2021年的（残余）空间变异（或异质性）较少。这一点通过查看预测空间随机效应的标准差得到了证实，2000年的标准差几乎是2021年的两倍（见下文）。进一步的结论是，2000年有更多的“低”GDP数值区域（空间效应低于0），而2021年不再如此。

```py
gp_model <- GPModel(group_data = data[, c("cl")], gp_coords = data[, c("Long", "Lat")],
                    likelihood = "gaussian", cov_function = "exponential",
                    cluster_ids  = c(rep(1,242), rep(2,242)))
boost_data <- gpb.Dataset(data = data[, covars], label = data[, "y"])
params <- list(learning_rate = 0.01, max_depth = 1, num_leaves = 2^10,
               min_data_in_leaf = 10, lambda_l2 = 1) 
# Note: we use the same tuning parameters as above. Ideally, the would have to be chosen again
gpboost_model <- gpboost(data = boost_data, gp_model = gp_model, nrounds = nrounds,
                         params = params, verbose = 0)
# Separate spatial maps for the years 2000 and 2021
pred <- predict(gpboost_model, group_data_pred = data[, c("cl")], 
                gp_coords_pred = data[, c("Long", "Lat")],
                data = data[, covars], predict_var = TRUE, pred_latent = TRUE,
                cluster_ids_pred = c(rep(1,242), rep(2,242)))
data_plot <- merge(shape, data.frame(FID = FID, y = pred$random_effect_mean[1:242]), by="FID")
plot_mu_2000 <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
  scale_fill_viridis(name="Spatial effect", option = "B") +
  ggtitle("Spatial effect for 2000 (mean)") + xlim(2700000,6400000) + ylim(1500000,5200000)
data_plot <- merge(shape, data.frame(FID = FID, y = pred$random_effect_mean[243:484]), by="FID")
plot_mu_2021 <- ggplot(data_plot) + geom_sf(aes(group=FID, fill=y)) +
  scale_fill_viridis(name="Spatial effect", option = "B") +
  ggtitle("Spatial effect for 2021 (mean)") + xlim(2700000,6400000) + ylim(1500000,5200000)
grid.arrange(plot_mu_2000, plot_mu_2021, ncol=2)
sd(pred$random_effect_mean[1:242]) # 0.2321267
sd(pred$random_effect_mean[243:484]) # 0.1286398
```

![](../Images/6dab4d5f4eeff34fcfd44e99bc23831b.png)

2000年和2021年的单独空间效应 — 作者提供的图片

## 空间与固定效应预测变量的交互

在上述模型中，随机效应和固定效应预测变量之间没有交互。即，空间坐标与固定效应预测变量之间没有交互。可以通过将随机效应输入变量（即空间坐标或分类分组变量）额外包含在固定效应函数中来建模这种交互。下面的代码展示了如何做到这一点。正如下面的变量重要性图所示，坐标在树集成中未被使用，因此在该数据集中没有检测到这种交互。

```py
gp_model <- GPModel(group_data = data[, c("cl")], gp_coords = data[, c("Long", "Lat")],
                    likelihood = "gaussian", cov_function = "exponential")
covars_interact <- c(c("Long", "Lat"), covars) ## add spatial coordinates to fixed effects predictor variables
boost_data <- gpb.Dataset(data = data[, covars_interact], label = data[, "y"])
params <- list(learning_rate = 0.01, max_depth = 1, num_leaves = 2^10,
               min_data_in_leaf = 10, lambda_l2 = 1) 
# Note: we use the same tuning parameters as above. Ideally, the would have to be chosen again
gpboost_model <- gpboost(data = boost_data, gp_model = gp_model, nrounds = nrounds,
                         params = params, verbose = 0)
feature_importances <- gpb.importance(gpboost_model, percentage = TRUE)
gpb.plot.importance(feature_importances, top_n = 25, measure = "Gain", 
                    main = "Variable importances when including coordinates in the fixed effects")
```

![](../Images/771ff055b304b447e95a45ce89ba7cd2.png)

包含坐标的树集成中的变量重要性 — 作者提供的图片

## 大数据

对于大数据集，高斯过程的计算较慢，需要使用近似方法。`GPBoost`库实现了几种近似方法。例如，在`GPModel`构造函数中设置`gp_approx="vecchia"`将使用Vecchia近似。本文使用的数据集相对较小，因此我们可以准确地进行所有计算。

## 其他空间随机效应模型

上述中，我们使用了高斯过程来建模空间随机效应。由于数据是区域数据，另一种选择是使用依赖于邻域信息的模型，如CAR和SAR模型。这些模型目前尚未在`GPBoost`库中实现（*可能会在未来添加 -> 联系作者*）。

另一种选择是使用由分类区域ID变量定义的分组随机效应模型来建模空间效应。下面的代码展示了如何在`GPBoost`中实现这一点。然而，该模型实质上忽略了空间结构。

```py
gp_model <- GPModel(group_data = data[, c("group", "cl")], likelihood = "gaussian")
```

## (广义)线性混合效应和高斯过程模型

(广义)线性混合效应和高斯过程模型也可以在`GPBoost`库中运行。下面的代码展示了如何使用`fitGPModel`函数来实现这一点。请注意，需要手动添加一列1来包含截距。

```py
X_incl_1 = cbind(Intercept = rep(1,dim(data)[1]), data[, covars])
gp_model <- fitGPModel(group_data = data[, c("cl")], gp_coords = data[, c("Long", "Lat")],
                       likelihood = "gaussian", cov_function = "exponential",
                       y = data[, "y"], X = X_incl_1, params = list(std_dev=TRUE))
summary(gp_model)
```

```py
## =====================================================
## Model summary:
## Log-lik     AIC     BIC 
##  195.97 -373.94 -336.30 
## Nb. observations: 484
## Nb. groups: 2 (Group_1)
## -----------------------------------------------------
## Covariance parameters (random effects):
##            Param. Std. dev.
## Error_term 0.0215    0.0017
## Group_1    0.0003    0.0008
## GP_var     0.0126    0.0047
## GP_range   7.2823    3.6585
## -----------------------------------------------------
## Linear regression coefficients (fixed effects):
##            Param. Std. dev. z value P(>|z|)
## Intercept 16.2816    0.4559 35.7128  0.0000
## L          0.4243    0.0565  7.5042  0.0000
## K          0.6493    0.0212 30.6755  0.0000
## pop        0.0134    0.0097  1.3812  0.1672
## edu        0.0100    0.0009 10.9645  0.0000
## =====================================================
```

# 参考文献

+   Sigrist Fabio. “[高斯过程提升](https://www.jmlr.org/papers/v23/20-322.html)”。*机器学习研究杂志*（2022年）。

+   Sigrist Fabio. “[潜在高斯模型提升](https://ieeexplore.ieee.org/document/9759834)”。*IEEE模式分析与机器智能汇刊*（2023年）。

+   [https://github.com/fabsig/GPBoost](https://github.com/fabsig/GPBoost)

+   *数据集来源于Eurostat；有关版权的更多信息，请参见* [*https://ec.europa.eu/eurostat/about-us/policies/copyright*](https://ec.europa.eu/eurostat/about-us/policies/copyright) *（“Eurostat的政策是鼓励免费重用其数据，无论是用于非商业还是商业目的。”）*
