# 在 scikit-learn 中的自定义评分函数

> 原文：[`towardsdatascience.com/custom-scoring-functions-in-scikit-learn-d33a1ebc8f90`](https://towardsdatascience.com/custom-scoring-functions-in-scikit-learn-d33a1ebc8f90)

## 深入研究 RandomizedSearchCV、GridSearchCV 和 cross_val_score 中的评分函数

[](https://medium.com/@raicik.zach?source=post_page-----d33a1ebc8f90--------------------------------)![扎卡里·赖西克](https://medium.com/@raicik.zach?source=post_page-----d33a1ebc8f90--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d33a1ebc8f90--------------------------------)![数据科学导向](https://towardsdatascience.com/?source=post_page-----d33a1ebc8f90--------------------------------) [扎卡里·赖西克](https://medium.com/@raicik.zach?source=post_page-----d33a1ebc8f90--------------------------------)

·发表于 [数据科学导向](https://towardsdatascience.com/?source=post_page-----d33a1ebc8f90--------------------------------) ·7 分钟阅读·2023 年 11 月 16 日

--

![](img/8365cc9231da4ae74d2b17f8e6e04ef9.png)

照片由 [engin akyurt](https://unsplash.com/@enginakyurt?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

`RandomizedSearchCV`、`GridSearchCV` 和 `cross_val_score` 都是优化和评估 scikit-learn 中机器学习模型的工具。这些工具提供了一种系统化的方法来调整超参数和评估模型性能。

很长一段时间，我使用这些工具时没有考虑评分函数。然而，我最终了解到，在使用这些工具时，scikit-learn 默认使用模型的固有评分函数来评估性能。默认的评分指标并不总是合适的，这可能导致对模型的决策失误。

**本文的其余部分将深入探讨如何以及何时在 scikit-learn 中使用自定义评分函数。**

# 示例：保险索赔中的 Tweedie 回归

在这个示例中，我们开发了一种回归器来预测未来的保险索赔成本，这一任务因保险数据中的固有不确定性而复杂。不确定性来自几个方面。

+   一旦有人购买了保险单，就没有保证他们会提出索赔。这导致目标变量中有很高的零值浓度。

+   如果有人提出索赔，该索赔的金额可能大也可能小。这导致我们的目标变量具有较大的方差。

默认情况下，`RandomizedSearchCV`、`GridSearchCV`和`cross_val_score`使用与传递给它的分类器或回归器相关联的默认评分指标。对于许多广泛使用的回归器，传统的指标如 R²和 RMSE 是默认分数。然而，使用这些指标来决定参数调优或评估保险数据模型的性能通常会导致错误的决定和结果。

因此，在处理保险数据时，我们需要确保将适当的评分函数传递给这些工具，以确保我们正确设置模型参数并评估模型性能。更一般地说，每次使用这些工具时，您应该确保使用的评分函数适合您的数据。在某些情况下，您可能需要使用不同于默认的评分函数。

在接下来的几个小节中，我们将使用 light GBM、`RandomizedSearchCV`和自定义评分函数构建一个 tweedie 回归器。此示例旨在演示如何在像`RandomizedSearchCV`、`GridSearchCV`或`cross_val_score`这样的工具中使用评分函数。此示例并不旨在提供机器学习模型开发、超参数调优的详细概述或生成一个优秀的模型。

## 模拟我们的数据集

由于保险数据的敏感性，通常很难找到公开可用的索赔历史数据集。因此，我们使用下面提供的代码模拟了自己的数据集。该数据集旨在模拟保险索赔。每一行代表一个投保人，我们模拟了每个投保人的索赔次数及其索赔金额。模拟数据集确保数据具有一些定义特征。

+   `policy_exposure`，即个人持有保单的时间，均匀分布在 1 到 12 个月之间。

+   `num_claims`，即某人提交的索赔次数，服从泊松分布。这确保了右偏分布，其中大多数人没有提交任何索赔。

+   `claim_cost`，即个人索赔的总金额，服从 gamma 分布。如果索赔次数为 0，则索赔成本为零。

```py
import pandas as pd
import numpy as np

np.random.seed(42)

n_samples = 50000 

policy_exposure = np.random.uniform(1, 12, n_samples)

n_features = 10
X = np.random.randn(n_samples, n_features)

annual_claim_rate = 0.05  
monthly_claim_rate = annual_claim_rate / 12  
num_claims = np.random.poisson(lam=monthly_claim_rate * policy_exposure)

claim_cost = np.zeros(n_samples)
positive_cost_indices = num_claims > 0
high_variance_gamma = np.random.gamma(2, 3000, size=positive_cost_indices.sum())
claim_cost[positive_cost_indices] = high_variance_gamma

claims_df = pd.DataFrame(
    {
        "policy_exposure": policy_exposure,
        "num_claims": num_claims,
        "claim_cost": claim_cost,
    }
)

features_df = pd.DataFrame(X, columns=[f"feature_{i+1}" for i in range(n_features)])
df = pd.concat([claims_df, features_df], axis=1)

num_claims_std_dev = 2
for i in range(1, 6):
    df[f"feature_{i}"] += np.random.normal(0, num_claims_std_dev * i, n_samples)

claim_cost_std_dev = 2
for i in range(6, 11):
    df[f"feature_{i}"] *= np.random.normal(1, claim_cost_std_dev * (i - 5), n_samples)
```

## 构建我们的回归器

我们首先要导入我们的[light GBM](https://lightgbm.readthedocs.io/en/latest/index.html)（“LGBM”）回归器。我们将回归目标设置为 tweedie。评估指标与目标无关。

```py
from lightgbm import LGBMRegressor

tweedie_regressor = LGBMRegressor(objective="tweedie", verbose=0)

features = [x for x in df.columns if "feature" in x]

target = "claim_cost"

weight = "policy_exposure"
```

Tweedie 分布是一种统计分布类型，涵盖了包括正态分布、泊松分布和 gamma 分布在内的一系列其他知名分布。Tweedie 分布的幂参数决定了 Tweedie 家族内分布的具体形式。幂参数在 1 和 2 之间会导致复合泊松-伽玛分布。

在我们的案例中，由于我们模拟了数据，我们已经知道索赔数量遵循泊松分布，这些索赔的大小遵循伽玛分布。因此，我们将设置一个随机网格搜索来找到 1 到 2 之间的最佳参数值。

## 使用默认评分的`RandomizedSearchCV`

在保险数据上构建 Tweedie 模型时，一个常见的评估指标是`mean_tweedie_deviance`。然而，我们的初始网格搜索将故意使用估计器的默认评分函数（在这种情况下为 R²）来选择最佳参数。一旦搜索完成，我们将计算`mean_tweedie_deviance`，以便将其与未来旨在优化`mean_tweedie_deviance`的网格搜索进行比较。

```py
from sklearn.model_selection import RandomizedSearchCV

param_distributions = {
    "tweedie_variance_power": [x / 100 for x in range(100, 200)],
}

fit_params = {"sample_weight": df[weight]}

random_search = RandomizedSearchCV(
    estimator=tweedie_regressor,
    n_iter=50,
    param_distributions=param_distributions,
    cv=5,
    verbose=2,
    random_state=21,
    n_jobs=-1,
)

random_search.fit(df[features], df[target], **fit_params)
```

我们确认模型的评分等于 R²。

```py
from sklearn.metrics import r2_score

best_power = random_search.best_params_["tweedie_variance_power"]

best_estimator = random_search.best_estimator_

best_estimator.fit(df[features], df[target], sample_weight=df[weight])

model_score = best_estimator.score(df[features], df[target])

r2 = r2_score(df[target],best_estimator.predict(df[features]))

print(model_score == r2)

# True
```

我们还可以查看最优的幂参数。单独解释`mean_tweedie_deviance`可能会有些棘手。然而，通常情况下，值越低越好。我们将使用这个值作为基准来与未来的搜索进行比较。

```py
deviance = mean_tweedie_deviance(
    df[target],
    best_estimator.predict(df[features]),
    power=best_power,
    sample_weight=df[weight],
)

print(f"Best Power: {best_power}")
print(f"mean_tweedie_deviance: {deviance}")

# Best Power: 1.29
# mean_tweedie_deviance: 159.49
```

## 使用`make_scorer`的`RandomizedSearchCV`

在没有额外评估的情况下，使用默认评分器的结果看起来还不错。然而，假设我们是经验丰富的数据科学家，并且我们意识到使用 R²来调整模型是不合适的。我们决定将评分函数切换到`mean_tweedie_deviance`。

为了让`RandomizedSearchCV`能够使用这个功能，我们使用 scikit-learn 的`make_scorer`。根据 Scikit-learn 的[文档](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.make_scorer.html)，`make_scorer`是一个“从性能指标或损失函数中创建评分器”的函数。换句话说，这个函数允许我们使用`sklearn.metrics`中任何可用的函数作为`RandomizedSearchCV`、`GridSearchCV`或`cross_val_score`中的评分函数。文档中还有一些其他可用的参数，但我们唯一需要的是`greater_is_better`，以告知我们的搜索较低的`mean_tweedie_deviance`更好。

使用`make_scorer`设置评分指标相对容易。

```py
tweedie_loss = make_scorer(mean_tweedie_deviance, greater_is_better=False)
```

使用我们新的评分函数，我们以相同的方式运行搜索。

```py
param_distributions = {
    "tweedie_variance_power": [x / 100 for x in range(100, 200)],
}

fit_params = {"sample_weight": df["policy_exposure"]}

random_search = RandomizedSearchCV(
    estimator=tweedie_regressor,
    n_iter=10,
    param_distributions=param_distributions,
    scoring=tweedie_loss,
    cv=5,
    verbose=2,
    random_state=21,
    n_jobs=-1,
)

random_search.fit(df[features], df["claim_cost"], **fit_params)
```

再次，我们可以查看网格搜索的结果。

```py
best_power = random_search.best_params_["tweedie_variance_power"]

best_estimator = random_search.best_estimator_

best_estimator.fit(df[features], df[target], sample_weight=df[weight])

deviance = mean_tweedie_deviance(
    df[target],
    best_estimator.predict(df[features]),
    power=best_power,
    sample_weight=df[weight],
)

print(f"Best Power: {best_power}")
print(f"mean_tweedie_deviance: {deviance}")

# Best Power: 1.29
# mean_tweedie_deviance: 159.49
```

结果与默认评分器的结果相同，但为什么呢？当我们使用`make_scorer`创建自定义评分函数时，搜索不会在`mean_tweedie_deviance`的计算中使用测试的幂值。相反，它使用的是默认的幂值。根据 scikit-learn 的[文档](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.mean_tweedie_deviance.html)，`mean_tweedie_deviance`的默认幂值为 0。`Mean_tweedie_deviance`的幂值为零时，相当于均方根误差，这导致与使用默认评分指标时相同的幂选择。

```py
def weighted_mse(y_true, y_pred, weights):
    weighted_squared_diff = weights * (y_true - y_pred) ** 2

    weighted_mean_squared_error = np.sum(weighted_squared_diff) / np.sum(weights)

    return weighted_mean_squared_error

mse = weighted_mse(df[target],
    best_estimator.predict(df[features]),df[weight])

deviance = mean_tweedie_deviance(
    df[target],
    best_estimator.predict(df[features]),
    power=0,
    sample_weight=df[weight],
)

print(f"Weighte MSE: {mse}")
print(f"Deviance (Power = 0): {deviance}")

# Weighte MSE: 1566555.33
# Deviance (Power = 0): 1566555.33
```

如果我们专注于调整其他 LGBM 参数并且已经知道我们想要的功率，我们可以使用下面的评分函数。然而，由于我们正在调整功率参数，这对我们来说是不可能的。

```py
tweedie_loss = make_scorer(mean_tweedie_deviance, greater_is_better=False, power=1.5)
```

## `RandomizedSearchCV`使用自定义评分对象

我们可以编写自己的函数并将其用作评分标准，而不是使用`make_scorer`。只要函数签名是`(y_true, y_pred)`或`(estimator, X, y)`，scikit-learn 工具，如 RandomizedSearchCV、GridSearchCV 或 cross_val_score，都可以接受该函数作为评分函数。

对于我们的示例，我们选择设计签名为`(estimator, X, y)`的评分函数。函数中包含估算器使我们能够访问估算器的功率，从而在搜索过程中正确计算`mean_tweedie_deviance`。

```py
def tweedie_loss(estimator, X, y):
    power = estimator.get_params()["tweedie_variance_power"]
    y_pred = estimator.predict(X)
    return -mean_tweedie_deviance(y, y_pred, power=power)
```

现在，我们直接将这个函数传递给我们的搜索！

```py
param_distributions = {
    "tweedie_variance_power": [x / 100 for x in range(100, 200)],
}

fit_params = {"sample_weight": df["policy_exposure"]}

random_search = RandomizedSearchCV(
    estimator=tweedie_regressor,
    n_iter=10,
    param_distributions=param_distributions,
    scoring=tweedie_loss,
    cv=5,
    verbose=2,
    random_state=21,
    n_jobs=-1,
)

random_search.fit(df[features], df["claim_cost"], **fit_params) 
```

和之前一样，让我们查看一下最佳功率。

```py
best_power = random_search.best_params_["tweedie_variance_power"]

best_estimator = random_search.best_estimator_

best_estimator.fit(df[features], df[target], sample_weight=df[weight])

deviance = mean_tweedie_deviance(
    df[target],
    best_estimator.predict(df[features]),
    power=best_power,
    sample_weight=df[weight],
)

print(f"Best Power: {best_power}")
print(f"mean_tweedie_deviance: {deviance}")

# Best Power: 1.62
# mean_tweedie_deviance: 35.30
```

我们注意到`power`稍微高一些，而`mean_tweedie_deviance`则低得多！由于`mean_tweedie_deviance`是这个模型更合适的度量指标，所以我们对这个结果感到满意。

你可能已经注意到，当我们在搜索完成后计算我们的`mean_tweedie_deviance`时，我们会将样本权重作为输入。然而，在我们自定义的评分对象中，我们没有使用任何样本权重。如果所需信息在所需签名的任何变量中不可用，则需要将额外的参数传递给自定义评分对象。虽然可以提供额外的输入，但这需要构建自己的 k 折验证方法，这超出了本文的范围。

# 结论

在这篇文章中，我们探讨了在`RandomizedSearchCV`中自定义评分标准的复杂性。本文所涵盖的概念也适用于其他工具，如`GridSearchCV`或`cross_val_score`。此外，我们还讨论了这种方法的一些限制。请记住，默认评分标准可能并不总是适合当前任务！
