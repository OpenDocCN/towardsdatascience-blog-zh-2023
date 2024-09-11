# 使用 GPBoost 进行纵向数据和面板数据的混合效应机器学习（第三部分）

> 原文：[https://towardsdatascience.com/mixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc?source=collection_archive---------3-----------------------#2023-07-17](https://towardsdatascience.com/mixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc?source=collection_archive---------3-----------------------#2023-07-17)

## 使用真实世界数据的 Python 和 R 中 GPBoost 的演示

[](https://medium.com/@fabsig?source=post_page-----523bb38effc--------------------------------)[![Fabio Sigrist](../Images/f7bc2adc17255ae1efd0886a19ec202c.png)](https://medium.com/@fabsig?source=post_page-----523bb38effc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----523bb38effc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----523bb38effc--------------------------------) [Fabio Sigrist](https://medium.com/@fabsig?source=post_page-----523bb38effc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb5b503a0c329&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc&user=Fabio+Sigrist&userId=b5b503a0c329&source=post_page-b5b503a0c329----523bb38effc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----523bb38effc--------------------------------) ·11 分钟阅读·2023年7月17日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F523bb38effc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc&user=Fabio+Sigrist&userId=b5b503a0c329&source=-----523bb38effc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F523bb38effc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmixed-effects-machine-learning-for-longitudinal-panel-data-with-gpboost-part-iii-523bb38effc&source=-----523bb38effc---------------------bookmark_footer-----------)![](../Images/afd55898ea6f1eb8180a279480fcead9.png)

**纵向数据的示例**：不同对象（idcode）的时间序列图 — 图片由作者提供

在 [第 I 部分](/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-i-an-empirical-df0a43dce059) 和 [第 II 部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492) 的系列文章中，我们展示了如何使用随机效应对机器学习模型中的高基数类别变量进行建模，并介绍了实现 [GPBoost 算法](https://www.jmlr.org/papers/v23/20-322.html) 的 `[GPBoost](https://github.com/fabsig/GPBoost)` [库](https://github.com/fabsig/GPBoost)，该算法将树提升与随机效应结合。在本文中，我们演示了如何使用 `GPBoost` 库的 Python 和 R 包进行纵向数据（也称为重复测量或面板数据）分析。你可能需要先阅读 [第 II 部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492)，因为它对 `GPBoost` 库进行了初步介绍。此演示中使用的是 `GPBoost` 版本 1.2.1。

## 目录

∘ [1 数据：描述、加载和样本分割](#2605)

∘ [2 GPBoost 中的纵向数据建模选项](#3fc9)

· · [2.1 主题分组随机效应](#fd01)

· · [2.2 仅固定效应](#f29f)

· · [2.3 主题和时间分组的随机效应](#a34f)

· · [2.4 带时间随机斜率的主题随机效应](#d0bc)

· · [2.5 主题特定的 AR(1) / 高斯过程模型](#b7a6)

· · [2.6 主题分组随机效应和联合 AR(1) 模型](#447d)

∘ [3 训练 GPBoost 模型](#cf4d)

∘ [4 选择调优参数](#8791)

∘ [5 预测](#171b)

∘ [6 结论和参考文献](#b4d0)

# 1 数据：描述、加载和样本分割

此演示中使用的数据是已在 [第 II 部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492) 使用过的工资数据。数据可以从 [这里](https://raw.githubusercontent.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables/master/data/wages.csv.gz) 下载。数据集包含 28,013 个样本，涉及 4,711 个人，这些数据在几年内被测量。此类数据称为纵向数据或面板数据，因为对于每个主题（个人 ID = `idcode`），数据在时间上（年份 = `t`）被重复收集。换句话说，每个类别变量 `idcode` 的级别的样本都是随时间重复测量的。响应变量是对数实际工资 (`ln_wage`)，数据还包括年龄、总工作经验、工作年限、日期等多个预测变量。

在下面的代码中，我们加载数据并将其随机分成 80% 的训练数据和 20% 的测试数据。

***Python***

```py
import gpboost as gpb
import pandas as pd
import numpy as np
# Load data
data = pd.read_csv("https://raw.githubusercontent.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables/master/data/wages.csv.gz")
data = data.assign(t_sq = data['t']**2)# Add t^2
# Partition into training and test data
n = data.shape[0]
np.random.seed(n)
permute_aux = np.random.permutation(n)
train_idx = permute_aux[0:int(0.8 * n)]
test_idx = permute_aux[int(0.8 * n):n]
data_train = data.iloc[train_idx]
data_test = data.iloc[test_idx]
# Define fixed effects predictor variables
pred_vars = [col for col in data.columns if col not in ['ln_wage', 'idcode', 't', 't_sq']]
```

***R***

```py
library(gpboost)
library(tidyverse)
# Load data
data <- read_csv("https://raw.githubusercontent.com/fabsig/Compare_ML_HighCardinality_Categorical_Variables/master/data/wages.csv.gz")
data[,"t_sq"] <- data[,"t"]^2 # Add t^2
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
pred_vars <- colnames(data)[-which(colnames(data) %in% c("ln_wage", "idcode", "t", "t_sq"))]
```

# 2 GPBoost 中的纵向数据建模选项

使用`GPBoost`库可以处理纵向数据的各种固定和随机效应模型组合。下面，我们演示了几种模型。这个列表并不详尽，还有其他模型可能。定义了随机效应模型（`gp_model`）和`Dataset`（`data_bst`）之后，可以使用`gpb.train`函数进行训练。后续的训练步骤对所有模型都是相同的，因此在这里我们不包括训练步骤，而是首先仅展示如何定义相应的模型。关于如何进行训练，请参见后续的第3节。

## 2.1 受试者分组的随机效应

第一种选择是对分类受试者变量（`idcode`）使用分组随机效应，对时间变量使用固定效应。这对应于以下模型：

![](../Images/e61757b9a3cce538403dfbc6fddce6b3.png)

其中i是人员索引（=`idcode`），j是时间索引，并且我们假设xij包含时间变量（或其“虚拟化”/独热编码版本）和其他预测变量。人员编号i的`idcode`随机效应是bi，F(xij)是树集成固定效应函数。这样的模型可以在`GPBoost`中指定，如下所示。受试者特定的随机效应通过将`idcode`分类变量传递给`GPModel()`构造函数的`group_data`参数来定义。注意时间变量`t`的基数非常低，因此我们可以使用独热编码或虚拟变量。我们在下面所做的即为此，`pred_vars`定义的固定效应预测变量已经包含了年份虚拟变量。

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian')
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian")
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
```

## 2.2 仅固定效应

另一种选择是使用仅固定效应模型，其中受试者（`idcode`）和时间都使用固定效应进行建模，没有随机效应。这对应于以下模型：

![](../Images/1302953138704753c9828513d538b462.png)

再次，为了简化符号，假设xij包含时间变量。这样的模型可以通过在`GPBoost`中简单地将受试者（`idcode`）和时间变量包含在树提升预测变量中并且没有随机效应模型来指定。

***Python***

```py
pred_vars_FE = ['idcode'] + pred_vars
data_bst = gpb.Dataset(data=data_train[pred_vars_FE], categorical_feature=[0],
                       label=data_train['ln_wage'])
```

***R***

```py
pred_vars_FE = c("idcode", pred_vars)
data_bst <- gpb.Dataset(data = data_train[,pred_vars_FE], categorical_feature = c(1),
                        label = data_train[,"ln_wage"])
```

## 2.3 受试者和时间分组的随机效应

也可以对受试者（`idcode`）和时间（`t`）使用随机效应。这对应于以下模型：

![](../Images/faf9ee83a76b0a5c1b5206c9f70df28c.png)

其中bi和bj是`idcode`和`t`随机效应。这样的模型可以在`GPBoost`中如下指定。与上述第2.1节中的单级随机效应模型相比，我们现在将两个变量（`idcode`和`t`）传递给`GPModel()`构造函数的`group_data`参数。

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train[['idcode','t']], likelihood='gaussian')
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,c("idcode","t")], likelihood = "gaussian")
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
```

## 2.4 受试者随机效应与时间随机斜率

在迄今为止介绍的随机效应模型中（例如，在第 2.1 节中），每个个体（`idcode`）都有不同的截距（=均值）。然而，除了这种“偏移”之外，时间上的演变对所有个体是相同的。通过包括多项式随机系数（= 随机斜率），可以放宽这一限制。例如，一个对`t`和`t^2`具有随机斜率的模型为

![](../Images/cd89f3740c0715815bf5a81984dd1e6c.png)

其中 b(0,i) 是截距随机效应，b(1,i) 和 b(2,i) 是随机斜率，tij 表示样本 ij 的时间`t`。由于时间上的随机斜率，在此模型中每个人在时间`t`上的演变是不同的。这种模型可以在`GPBoost`中进行指定，如下代码所示。`group_rand_coef_data` 变量包含随机系数协变量数据`t` 和 `t^2`。此外，`ind_effect_group_rand_coef`中的索引指示应该使用哪些`group_data`中的分类变量来获取随机系数。在这里，只有一个分类变量（`idcode`），因此 `ind_effect_group_rand_coef` 只是 (1,1)（即，`t` 和 `t^2` 都属于 `idcode`）。

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian',
                       group_rand_coef_data=data_train[["t","t_sq"]],
                       ind_effect_group_rand_coef=[1,1])
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian",
                    group_rand_coef_data = cbind(data_train[,"t"], data_train[,"t_sq"]),
                    ind_effect_group_rand_coef = c(1,1))
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
```

## 2.5 个体特定的 AR(1) / 高斯过程模型

对上述随机斜率模型的进一步扩展是使用时间序列模型或高斯过程，而不是多项式随机斜率。这允许以更灵活的方式分别建模每个个体的时间演变。例如，可以使用个体特定的 AR(1) 模型：

![](../Images/55d67efe369fecf9328b9271cdd93ba8.png)

在此模型中，每个个体 i 都有独立的随机效应 bij，这些效应随时间按照 AR(1) 模型演变（= 离散化的高斯过程，具有指数协方差函数）。这种模型可以在`GPBoost`中进行指定，如下代码所示。时间高斯过程通过将时间变量`t`传递给`gp_coords`参数来定义。我们使用具有指数协方差函数的高斯过程，这相当于 AR(1) 模型。此外，`cluster_ids` 变量指示每个人（`idcode`）都有一个独立的（= 先验独立）高斯过程。

***Python***

```py
gp_model = gpb.GPModel(gp_coords=data_train['t'], cov_function='exponential',
                       cluster_ids=data_train['idcode'], likelihood='gaussian')
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
```

***R***

```py
gp_model <- GPModel(gp_coords = data_train[,"t"], cov_function="exponential",
                    cluster_ids = data_train[,"idcode"], likelihood = "gaussian")
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
```

训练后，AR(1) 模型参数可以从高斯过程协方差参数中获得，如下所示。

***Python***

```py
cov_pars = gp_model.get_cov_pars()
phi_hat = np.exp(-1 / cov_pars['GP_range'][0])
sigma2_hat = cov_pars['GP_var'][0] * (1\. - phi_hat ** 2)
print("Estimated innovation variance and AR(1) coefficient of year effect:")
print([sigma2_hat, phi_hat])
```

***R***

```py
cov_pars = gp_model$get_cov_pars()
phi_hat = exp(-1 / cov_pars["GP_range"])
sigma2_hat = cov_pars["GP_var"] * (1 - phi_hat^2)
print("Estimated innovation variance and AR(1) coefficient:")
print(c(sigma2 = sigma2_hat, phi = phi_hat))
```

## 2.6 主题分组随机效应和联合 AR(1) 模型

另一种选择是使用一个在所有个体中共享的单一时间 AR(1) 模型，但包括单独的时间常数个体（`idcode`）随机效应。这对应于以下模型：

![](../Images/a931905ccac70d85093438b147020273.png)

这种模型可以在`GPBoost`中进行如下指定。

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian'
                        gp_coords=data_train['t'], cov_function='exponential')
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian",
                    gp_coords = data_train[,"t"], cov_function="exponential")
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
```

# 3 训练 GPBoost 模型

以下代码展示了如何训练一个GPBoost模型。我们首先定义了一个随机效应模型（`gp_model`）、一个`Dataset`（`data_bst`）、调整参数（`params`和`nrounds` = 树的数量/提升迭代次数），然后通过调用`gpb.train`函数运行GPBoost算法。我们使用一个带有时间随机斜率的受试者（`idcode`）随机效应模型（见第2.4节），因为这样的模型通常在灵活性/准确性和运行时间之间提供了很好的折衷。请注意，我们使用的是提前选择好的调整参数（见下文）。

***Python***

```py
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian',
                       group_rand_coef_data=data_train[["t","t_sq"]],
                       ind_effect_group_rand_coef=[1,1])
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
params = {'learning_rate': 0.01, 'max_depth': 2, 'min_data_in_leaf': 10,
          'lambda_l2': 10, 'num_leaves': 2**10, 'verbose': 0}
nrounds = 379
gpbst = gpb.train(params=params, train_set=data_bst,  gp_model=gp_model,
                  num_boost_round=nrounds) 
gp_model.summary() # Estimated random effects model
```

```py
## =====================================================
## Covariance parameters (random effects):
##                        Param.
## Error_term             0.0531
## idcode                 0.0432
## idcode_rand_coef_t     0.0004
## idcode_rand_coef_t_sq  0.0000
## =====================================================
```

***R***

```py
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian",
                    group_rand_coef_data = cbind(data_train[,"t"], data_train[,"t_sq"]),
                    ind_effect_group_rand_coef = c(1,1))
data_bst <- gpb.Dataset(data = data_train[,pred_vars], label = data_train[,"ln_wage"])
params <- list(learning_rate = 0.01, max_depth = 2, num_leaves = 2^10,
               min_data_in_leaf = 10, lambda_l2 = 10)
nrounds <- 379
gpbst <- gpb.train(data = data_bst, gp_model = gp_model, 
                   nrounds = nrounds, params = params, verbose = 0)
summary(gp_model) # Estimated random effects mode
```

当存在多个随机效应（如这里的情况）时，使用`nelder_mead`作为优化器通常比`gradient_descent`（默认为优化器）更快*。* 这可以在调用`gpb.train`之前设置。然而，对于这个数据集来说，这并不重要。

***Python***

```py
gp_model.set_optim_params(params={"optimizer_cov": "nelder_mead"})
```

***R***

```py
gp_model$set_optim_params(params = list(optimizer_cov = "nelder_mead"))
```

# 4 选择调整参数

调整参数在提升模型中选择得当是非常重要的。没有通用的默认值，每个数据集可能需要不同的调整参数。下面，我们展示了如何使用`gpb.grid.search.tune.parameters`函数来选择调整参数。我们使用带有下述参数组合的确定性网格搜索。请注意，我们调整了`max_depth`参数，并将`num_leaves`设置为较大的值。我们将训练数据随机拆分为80%的内部训练数据和20%的验证数据，并使用均方误差（`mse`）作为验证数据上的预测准确度测量。或者，也可以使用例如测试负对数似然（`metric = "test_neg_log_likelihood"` = 默认），它也会考虑预测的不确定性。*注意：根据数据集和网格大小，这可能需要一些时间（在我的笔记本电脑上大约半小时）。除了下面的确定性网格搜索外，还可以进行随机网格搜索（见`*num_try_random*`）或其他方法，如贝叶斯优化，以加快速度。*

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
gp_model = gpb.GPModel(group_data=data_train['idcode'], likelihood='gaussian',
                       group_rand_coef_data=data_train[["t","t_sq"]],
                       ind_effect_group_rand_coef=[1,1])
data_bst = gpb.Dataset(data=data_train[pred_vars], label=data_train['ln_wage'])
# Find optimal tuning parameters
opt_params = gpb.grid_search_tune_parameters(param_grid=param_grid, params=other_params,
                                             num_try_random=None, folds=folds, seed=1000,
                                             train_set=data_bst, gp_model=gp_model,
                                             num_boost_round=1000, early_stopping_rounds=10,
                                             verbose_eval=1, metric='mse')
opt_params
# {'best_params': {'learning_rate': 0.01, 'max_depth': 2, 'min_data_in_leaf': 10, 'lambda_l2': 10}, 'best_iter': 379, 'best_score': 0.08000593485771226}
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
gp_model <- GPModel(group_data = data_train[,"idcode"], likelihood = "gaussian",
                    group_rand_coef_data = cbind(data_train[,"t"], data_train[,"t_sq"]),
                    ind_effect_group_rand_coef = c(1,1))
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

可以通过`predict`函数获得预测结果。我们需要提供树集成的测试预测变量（`data`）以及随机效应模型的测试受试者变量和随机斜率数据（`group_data_pred`和`group_rand_coef_data_pred`）。下面，我们对遗漏的测试数据进行预测，并计算测试均方误差（MSE）。正如下面的结果所示，与第2.1节中介绍的没有引入随机斜率的简单受试者随机效应模型相比，这个随机斜率模型的预测准确度更高（有关后者模型的结果，请参见[第二部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492)）。

***Python***

```py
pred = gpbst.predict(data=data_test[pred_vars], group_data_pred=data_test['idcode'],
                     group_rand_coef_data_pred=data_test[["t","t_sq"]],
                     predict_var=False, pred_latent=False)
y_pred = pred['response_mean']
np.mean((data_test['ln_wage'] - y_pred)**2)
```

```py
## 0.08117682279905969
```

***R***

```py
pred <- predict(gpbst, data = data_test[,pred_vars], group_data_pred = data_test[,"idcode"], 
                group_rand_coef_data_pred = cbind(data_test[,"t"], data_test[,"t_sq"]),
                predict_var = FALSE, pred_latent = FALSE)
y_pred <- pred[["response_mean"]]
mean((data_test[,"ln_wage"] - y_pred)^2)
```

# 6 结论和参考文献

我们展示了如何使用`GPBoost`库来建模纵向数据。`GPBoost`库还有更多我们未展示的功能。例如，在[第二部分](https://medium.com/towards-data-science/mixed-effects-machine-learning-for-high-cardinality-categorical-variables-part-ii-gpboost-3bdd9ef74492)中，我们展示了如何利用机器学习可解释性技术理解固定效应函数，同时也演示了`GPBoost`库如何处理（广义）线性混合效应模型（GLMMs → F() 是线性的而不是树集合）。你还可以查看[Python 示例](https://github.com/fabsig/GPBoost/tree/master/examples/python-guide)和[R 示例](https://github.com/fabsig/GPBoost/tree/master/R-package/demo)。

+   F. Sigrist. 高斯过程提升. 机器学习研究杂志, 23(1):10565–10610, 2022。

+   F. Sigrist. 潜在高斯模型提升. IEEE模式分析与机器智能学报, 45(2):1894–1905, 2023。

+   [https://github.com/fabsig/GPBoost](https://github.com/fabsig/GPBoost)
