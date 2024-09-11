# 高基数分类变量的混合效应机器学习 — 第二部分：GPBoost库

> 原文：[https://towardsdatascience.com/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492?source=collection_archive---------6-----------------------#2023-07-11](https://towardsdatascience.com/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492?source=collection_archive---------6-----------------------#2023-07-11)

## 一个使用真实数据的GPBoost在Python和R中的演示

[](https://medium.com/@fabsig?source=post_page-----3bdd9ef74492--------------------------------)[![Fabio Sigrist](../Images/f7bc2adc17255ae1efd0886a19ec202c.png)](https://medium.com/@fabsig?source=post_page-----3bdd9ef74492--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3bdd9ef74492--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3bdd9ef74492--------------------------------) [Fabio Sigrist](https://medium.com/@fabsig?source=post_page-----3bdd9ef74492--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5b503a0c329&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492&user=Fabio+Sigrist&userId=b5b503a0c329&source=post_page-b5b503a0c329----3bdd9ef74492---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3bdd9ef74492--------------------------------) ·8分钟阅读·2023年7月11日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3bdd9ef74492&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492&user=Fabio+Sigrist&userId=b5b503a0c329&source=-----3bdd9ef74492---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3bdd9ef74492&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492&source=-----3bdd9ef74492---------------------bookmark_footer-----------)![](../Images/e5dd3c4f7bc91e17c444088bc24348ba.png)

**高基数分类数据的示意图**：不同分类变量水平的响应变量的箱线图和原始数据（红点）— 图片作者提供

高卡方分类变量是指相对于数据集样本大小，具有大量不同层级的变量。在 [Part I](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-i-an-empirical-df0a43dce059) 中，我们对不同的机器学习方法进行了实证比较，发现随机效应是处理高卡方分类变量的有效工具，其中 [GPBoost 算法](https://www.jmlr.org/papers/v23/20-322.html) [Sigrist, 2022, 2023] 具有最高的预测准确性。在本文中，我们演示了如何将结合了树提升与随机效应的 GPBoost 算法应用于 Python 和 R 包的 `[GPBoost](https://github.com/fabsig/GPBoost)` [库](https://github.com/fabsig/GPBoost)。本演示中使用的 `GPBoost` 版本为 1.2.1。

## 目录

∘ [1 介绍](#c2dc)

∘ [2 数据：描述、加载和样本分割](#c3cf)

∘ [3 训练 GPBoost 模型](#a119)

∘ [4 选择调优参数](#d562)

∘ [5 预测](#9914)

∘ [6 解释](#7784)

∘ [7 进一步建模选项](#6d73)

· · [7.1 类别变量与其他预测变量的交互](#b005)

· · [7.2 （广义）线性混合效应模型](#49d6)

∘ [8 结论和参考文献](#4ebe)

# 1 介绍

应用 GPBoost 模型包括以下主要步骤：

1.  定义一个 `GPModel`，在其中指定以下内容：

    — 随机效应模型：通过 `group_data` 和/或通过 `gp_coords` 的高斯过程

    — `likelihood` *(= 固定和随机效应条件下的响应变量分布)*

1.  创建一个 `Dataset`，包含响应变量（`label`）和固定效应预测变量（`data`）

1.  选择调优参数，例如，使用函数 `gpb.grid.search.tune.parameters`

1.  训练模型

1.  进行预测和/或解释训练好的模型

在下文中，我们将逐步讲解这些点。

# 2 数据：描述、加载和样本分割

本演示中使用的数据是工资数据，该数据也在 [Sigrist (2022)](https://www.jmlr.org/papers/v23/20-322.html) 中进行了分析。可以从 [这里](https://raw.githubusercontent.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables/master/data/wages.csv.gz) 下载。数据包含 28’013 个样本和一个高卡方分类变量（person ID = `idcode`），具有 4’711 个不同的层级。响应变量是对数实际工资（`ln_wage`），数据还包括几个预测变量，如年龄、总工作经验、工作年限、日期等。

在下面的代码中，我们首先加载数据，并将数据随机分为 80% 的训练数据和 20% 的测试数据。后者被留作后用，用于测量预测准确性。

***Python***

```py
import gpboost as gpb
import pandas as pd
import numpy as np
# Load data
data = pd.read_csv("https://raw.githubusercontent.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables/master/data/wages.csv.gz")
# Partition into training and test data
n = data.shape[0]
np.random.seed(n)
permute_aux = np.random.permutation(n)
train_idx = permute_aux[0:int(0.8 * n)]
test_idx = permute_aux[int(0.8 * n):n]
data_train = data.iloc[train_idx]
data_test = data.iloc[test_idx]
# Define fixed effects predictor variables
pred_vars = [col for col in data.columns if col not in ['ln_wage', 'idcode', 't']]
```

***R***

```py
library(gpboost)
library(tidyverse)
# Load data
data <- read_csv("https://raw.githubusercontent.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables/master/data/wages.csv.gz")
data <- as.matrix(data) # Convert to matrix since the boosting part does not support data.frames
# Partition into training and test data
n <- dim(data)[1]
set.seed(n)
permute_aux <- sample.int(n, n)
train_idx <- permute_aux[1:as.integer(0.8*n)]
test_idx <- permute_aux[(as.integer(0.8*n)+1):n]
data_train <- data[train_idx,]
data_test <- data[test_idx,]
# Define fixed effects predictor variables
pred_vars <- colnames(data)[-which(colnames(data) %in% c("ln_wage", "idcode", "t"))]
```

# 3 训练 GPBoost 模型

下面的代码展示了如何训练一个GPBoost模型。我们首先定义一个随机效应模型（`gp_model`）、一个`Dataset`（`data_bst`）、调整参数（`params`和`nrounds` = 树的数量/提升迭代次数），然后通过调用`gpb.train`函数运行GPBoost算法。请注意，我们使用了事先选择的调整参数（见下文）。

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian')
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
params = {'learning_rate': 0.01, 'max_depth': 2, 'min_data_in_leaf': 10,
          'lambda_l2': 10, 'num_leaves': 2**10, 'verbose': 0}
nrounds = 391
gpbst = gpb.train(params=params, train_set=data_bst, gp_model=gp_model,
                  num_boost_round=nrounds)
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian")
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
params <- list(learning_rate = 0.01, max_depth = 2, num_leaves = 2^10,
               min_data_in_leaf = 10, lambda_l2 = 10)
nrounds <- 391
gpbst <- gpb.train(data = data_bst, gp_model = gp_model, 
                   nrounds = nrounds, params = params, verbose = 0)
```

对于这个数据，只有一个高基数的分类变量。如果有多个分类变量需要使用随机效应建模，这可以在`gp_model`中指定，如以下代码所示。*注意：当有多个随机效应时，计算可能会比只有一个随机效应时更慢。此外，使用`*nelder_mead*` *代替`*gradient_descent*`（=默认值）作为优化器在有多个随机效应时可能会更快。这可以在调用`*gpb.train*`*之前进行设置。*

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train[['idcode','t']], likelihood='gaussian')
gp_model.set_optim_params(params={"optimizer_cov": "nelder_mead"})
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,c("idcode","t")], likelihood = "gaussian")
gp_model$set_optim_params(params = list(optimizer_cov = "nelder_mead"))
```

# 4 选择调整参数

调整参数的选择对于提升效果至关重要。没有通用的默认值，每个数据集可能需要不同的调整参数。以下展示了如何使用`gpb.grid.search.tune.parameters`函数来选择调整参数。我们使用了一个确定性的网格搜索，并在代码中展示了参数组合。请注意，我们调整了`max_depth`参数，因此将`num_leaves`设置为一个大值。我们随机将训练数据拆分为80%的内训练数据和20%的验证数据，并使用均方误差（`mse`）作为验证数据上的预测准确度衡量标准。或者，也可以使用，例如，测试负对数似然（`test_neg_log_likelihood` = 默认值），它还考虑了预测的不确定性。*注意：根据数据集和网格大小，这可能需要一些时间（在我的笔记本电脑上，对于这个数据集需要几分钟）。除了下面的确定性网格搜索外，也可以进行随机网格搜索（见* `*num_try_random*`*）或其他方法如贝叶斯优化来加快速度。*

***Python***

```py
# Partition training data into inner training data and validation data
ntrain = data_train.shape[0]
np.random.seed(ntrain)
permute_aux = np.random.permutation(ntrain)
train_tune_idx = permute_aux[0:int(0.8 * ntrain)]
valid_tune_idx = permute_aux[int(0.8 * ntrain):ntrain]
folds = [(train_tune_idx, valid_tune_idx)]
# Specify parameter grid, gp_model, and gpb.Dataset
param_grid = {'learning_rate': [1,0.1,0.01], 'max_depth': [1,2,3,5,10],
              'min_data_in_leaf': [10,100,1000], 'lambda_l2': [0,1,10]}
other_params = {'num_leaves': 2**10, 'verbose': 0}
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian')
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
# Find optimal tuning parameters
opt_params = gpb.grid_search_tune_parameters(param_grid=param_grid, params=other_params,
                                             num_try_random=None, folds=folds, seed=1000,
                                             train_set=data_bst, gp_model=gp_model,
                                             num_boost_round=1000, early_stopping_rounds=10,
                                             verbose_eval=1, metric='mse')
opt_params
# {'best_params': {'learning_rate': 0.01, 'max_depth': 2, 'min_data_in_leaf': 10, 'lambda_l2': 10}, 'best_iter': 391, 'best_score': 0.08507862825877585}
```

***R***

```py
# Partition training data into inner training data and validation data
ntrain <- dim(data_train)[1]
set.seed(ntrain)
valid_tune_idx <- sample.int(ntrain, as.integer(0.2*ntrain))
folds <- list(valid_tune_idx)
# Specify parameter grid, gp_model, and gpb.Dataset
param_grid <- list("learning_rate" = c(1,0.1,0.01), "max_depth" = c(1,2,3,5,10),
                   "min_data_in_leaf" = c(10,100,1000), "lambda_l2" = c(0,1,10))
other_params <- list(num_leaves = 2^10)
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian")
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
# Find optimal tuning parameters
opt_params <- gpb.grid.search.tune.parameters(param_grid = param_grid, params = other_params,
                                              num_try_random = NULL, folds = folds,
                                              data = data_bst, gp_model = gp_model,
                                              nrounds = 1000, early_stopping_rounds = 10,
                                              verbose_eval = 1, metric = "mse")
opt_params
```

# 5 预测

可以通过`predict`函数获得预测。我们需要提供树集合的测试预测变量（`data`）和随机效应模型的测试分类变量（`group_data_pred`）。以下是对留下的测试数据进行预测并计算测试均方误差（MSE）。

***Python***

```py
pred = gpbst.predict(data=data_test[pred_vars], group_data_pred=data_test['idcode'], 
                     predict_var=False, pred_latent=False)
y_pred = pred['response_mean']
np.mean((data_test['ln_wage'] - y_pred)**2)
```

```py
## 0.0866730945477676
```

***R***

```py
pred <- predict(gpbst, data = data_test[,pred_vars], group_data_pred = data_test[,"idcode"], 
                predict_var = FALSE, pred_latent = FALSE)
y_pred <- pred[["response_mean"]]
mean((data_test[,"ln_wage"] - y_pred)^2)
```

# 6 解释

估计随机效应模型的信息可以通过以下方式获得。

***Python***

```py
gp_model.summary()
```

```py
## =====================================================
## Covariance parameters (random effects):
##             Param.
## Error_term  0.0703
## idcode      0.0448
## =====================================================
```

***R***

```py
summary(gp_model)
```

有几个工具可以帮助理解`GPBoost`库支持的固定效应树集合函数的形式：

+   SHAP值

+   SHAP和部分依赖图（二维和一维）

+   SHAP力图

+   交互统计

+   基于拆分的变量重要性

以下我们查看SHAP值和SHAP依赖图。

***Python***

```py
import shap
shap_values = shap.TreeExplainer(gpbst).shap_values(data_train[pred_vars])
feature_names = [ a + ": " + str(b) for a,b in zip(pred_vars, np.abs(shap_values).mean(0).round(2)) ]
shap.summary_plot(shap_values, data_train[pred_vars], max_display=10, feature_names=feature_names)
shap.dependence_plot("ttl_exp", shap_values, data_train[pred_vars], alpha=0.5)
```

![](../Images/74fb408e7c6fe5a23df8be4bf03aafaa.png)![](../Images/8966198e2b3bcfc1ad60eb8283de210b.png)

SHAP值和SHAP依赖图 — 作者图片

***R***

```py
library(SHAPforxgboost)
library(ggplot2)
shap.plot.summary.wrap1(gpbst, X = data_train[,pred_vars], top_n=10) + 
  ggtitle("SHAP values")
shap_long <- shap.prep(gpbst, X_train = data_train[,pred_vars])
shap.plot.dependence(data_long = shap_long, x = "ttl_exp", 
                     color_feature = "occ_code_7", smooth = FALSE, size = 2) + 
  ggtitle("SHAP dependence plot for ttl_exp")
```

# 7 进一步的建模选项

## 7.1 分类变量与其他预测变量之间的交互

在上述模型中，随机效应与固定效应预测变量之间没有交互，即高维分类变量与其他预测变量之间没有交互。这种交互可以通过将分类变量额外包含在树集成中来建模。以下代码演示了如何实现这一点。下面，我们还计算了该模型的测试均方误差（MSE）。与没有这种交互的模型相比，测试MSE略小。*注意：下面使用的调优参数已提前选择，但为了简洁起见，我们省略了这段代码。*

***Python***

```py
pred_vars_int = ['idcode'] + pred_vars
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian')
data_bst = gpb.Dataset(data=data_train[pred_vars_int], categorical_feature=[0], 
                       label=data_train['ln_wage'])
params = {'learning_rate': 0.01, 'max_depth': 2, 'min_data_in_leaf': 10,
          'lambda_l2': 1, 'num_leaves': 2**10, 'verbose': 0}
gpbst = gpb.train(params=params, train_set=data_bst,  gp_model=gp_model,
                  num_boost_round=482)
pred = gpbst.predict(data=data_test[pred_vars_int], group_data_pred=data_test['idcode'])
np.mean((data_test['ln_wage'] - pred['response_mean'])**2)
```

```py
## 0.08616085739486774
```

***R***

```py
pred_vars_int = c("idcode", pred_vars)
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian")
data_bst <- gpb.Dataset(data = data_train[,pred_vars_int], categorical_feature = c(1), 
                        label = data_train[,"ln_wage"])
params <- list(learning_rate = 0.01, max_depth = 2, num_leaves = 2^10,
               min_data_in_leaf = 10, lambda_l2 = 1)
gpbst <- gpb.train(data = data_bst, gp_model = gp_model, 
                   nrounds = 482, params = params, verbose = 0)
pred <- predict(gpbst, data = data_test[,pred_vars_int], group_data_pred = data_test[,"idcode"])
mean((data_test[,"ln_wage"] - pred[["response_mean"]])^2)
```

## 7.2（广义）线性混合效应模型

`GPBoost`库还支持（广义）线性混合效应模型（GLMMs）。在GLMM中，假设固定效应函数F()是线性函数而不是树集成。以下代码演示了如何实现这一点。注意需要手动添加一列1以包括截距。使用选项`params={"std_dev": True}` / `params = list(std_dev=TRUE)`，可以在`summary`函数中获得标准差和p值。

***Python***

```py
X = data_train[pred_vars]
X = X.assign(Intercept = 1)
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood="gaussian")
gp_model.fit(y=data_train['ln_wage'], X=X, params={"std_dev": True})
gp_model.summary()
```

```py
## =====================================================
## Model summary:
##  Log-lik      AIC     BIC
## -6191.97 12493.95 12934.9
## Nb. observations: 22410
## Nb. groups: 4567 (idcode)
## -----------------------------------------------------
## Covariance parameters (random effects):
##             Param.  Std. dev.
## Error_term  0.0794     0.0008
## idcode      0.0452     0.0014
## -----------------------------------------------------
## Linear regression coefficients (fixed effects):
##              Param.  Std. dev.  z value  P(>|z|)
## grade        0.0379     0.0025  14.8659   0.0000
## age          0.0032     0.0013   2.4665   0.0136
## ttl_exp      0.0310     0.0012  25.4221   0.0000
## tenure       0.0107     0.0009  11.9256   0.0000
## not_smsa    -0.1326     0.0079 -16.8758   0.0000
## south       -0.0884     0.0072 -12.3259   0.0000
...
```

***R***

```py
X = cbind(Intercept = rep(1,dim(data_train)[1]), data_train[,pred_vars])
gp_model <- fitGPModel(group_data = data_train[,"idcode"], X = X, 
                       y = data_train[,"ln_wage"], likelihood = "gaussian", 
                       params = list(std_dev=TRUE))
summary(gp_model)
```

# 8 结论与参考文献

我们展示了如何使用`[GPBoost](https://github.com/fabsig/GPBoost)` [库](https://github.com/fabsig/GPBoost)的Python和R包将树提升与随机效应结合应用。`GPBoost`库还有许多我们未展示的功能。您可以查看[Python示例](https://github.com/fabsig/GPBoost/tree/master/examples/python-guide)和[R示例](https://github.com/fabsig/GPBoost/tree/master/R-package/demo)。在本系列的[第三部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc)中，我们将展示如何使用`GPBoost`库对纵向数据（即面板数据）进行建模。

+   F. Sigrist. 高斯过程提升. 机器学习研究期刊, 23(1):10565–10610, 2022.

+   F. Sigrist. 潜在高斯模型提升. IEEE模式分析与机器智能期刊, 45(2):1894–1905, 2023.

+   [https://github.com/fabsig/GPBoost](https://github.com/fabsig/GPBoost)
