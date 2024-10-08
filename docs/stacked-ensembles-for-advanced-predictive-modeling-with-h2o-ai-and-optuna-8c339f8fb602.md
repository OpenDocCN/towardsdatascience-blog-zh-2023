# **我如何通过堆叠集成模型在欧洲最大机器学习竞赛中获得前 10%**

> 原文：[`towardsdatascience.com/stacked-ensembles-for-advanced-predictive-modeling-with-h2o-ai-and-optuna-8c339f8fb602`](https://towardsdatascience.com/stacked-ensembles-for-advanced-predictive-modeling-with-h2o-ai-and-optuna-8c339f8fb602)

## 关于使用 H2O.ai 和 Optuna 训练堆叠集成模型的概念性和实践编码指南

[](https://medium.com/@sheilateozy?source=post_page-----8c339f8fb602--------------------------------)![Sheila Teo](https://medium.com/@sheilateozy?source=post_page-----8c339f8fb602--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8c339f8fb602--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8c339f8fb602--------------------------------) [Sheila Teo](https://medium.com/@sheilateozy?source=post_page-----8c339f8fb602--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----8c339f8fb602--------------------------------) ·阅读时间 13 分钟·2023 年 12 月 18 日

--

![](img/4aa1fd2dabd2834494a342ca18c09ddd.png)

图像由 DALL·E 3 生成

我们都知道，集成模型在预测建模中优于任何单一模型。你可能听说过 Bagging 和 Boosting 作为常见的集成方法，以随机森林和梯度提升机作为各自的例子。

但是，如何在一个独立的更高层次模型下组合不同的模型呢？这就是堆叠集成模型的作用所在。**本文是如何使用流行的机器学习库 H2O 训练堆叠集成模型的逐步指南。**

为了展示堆叠集成模型的强大功能，我将提供一个完整的代码演示，介绍如何训练一个由 40 个深度神经网络、XGBoost 和 LightGBM 模型组成的堆叠集成模型，以完成 2023 年 Cloudflight 编程竞赛（AI 类别）中的预测任务。这是欧洲最大的编程竞赛之一，我在其中的训练时间为 1 小时内获得了前 10% 的名次！

**本指南将涵盖：**

1.  **什么是堆叠集成模型，它们是如何工作的？**

1.  **如何使用 H2O.ai 训练堆叠集成模型** **—

    通过 Python 的完整代码演示**

1.  **比较堆叠集成模型与单独模型的性能**

# 1\. 什么是堆叠集成模型，它们是如何工作的？

堆叠集成模型通过另一个更高层次的模型将多个模型的预测结果进行结合，旨在通过利用每个组成模型的独特优势来提高整体预测性能。它包括两个阶段：

## **阶段 1：多个基础模型**

首先，多个基础模型在相同的训练数据集上独立训练。这些模型应当尽可能多样化，从简单的线性回归到复杂的深度学习模型都可以。关键是它们在某些方面应有所不同，无论是使用不同的算法还是使用相同的算法但具有不同的超参数设置。

**基础模型的多样性越高，最终的堆叠集成模型越强大。** 这是因为不同的模型能够捕捉数据中的不同模式。例如，基于树的模型可能擅长捕捉非线性关系，而线性模型擅长理解线性趋势。当这些多样化的基础模型结合在一起时，堆叠集成模型可以利用每个基础模型的不同优势，从而提高预测性能。

## 第二阶段：一个元模型

在所有基础模型训练完成后，每个基础模型对目标的预测将用作训练一个更高层次模型（称为元模型）的特征。这意味着元模型不是在原始数据集的特征上训练，而是在基础模型的预测上进行训练。如果有`n`个基础模型，则生成`n`个预测，这些预测就是用于训练元模型的`n`个特征。

虽然训练特征在基础模型和元模型之间有所不同，但目标保持不变，即数据集中的原始目标。

元模型学习如何最佳地结合基础模型的预测，以做出最终的、更准确的预测。

## 堆叠集成训练的详细步骤

**对于每个基础模型：**

1\. 选择一个算法（例如，随机森林）。

2\. 使用交叉验证获得算法的最佳超参数集合。

3\. 获取训练集中目标的交叉验证预测。这些将随后用于训练元模型。

> *为了说明这一点，假设在步骤 1 中选择了随机森林算法，并且在步骤 2 中确定了其最优超参数为* `*h*` *。*
> 
> *交叉验证预测是通过以下方式获得的，假设使用 5 折交叉验证：
> 
> 1\. 在第 1 到第 4 折上训练具有超参数* `*h*` *的随机森林。
> 
> 2\. 使用训练好的随机森林对第 5 折进行预测。这些是第 5 折的交叉验证预测。
> 
> 3\. 重复上述步骤以获取每一折的交叉验证预测。之后，将获得整个训练集的目标交叉验证预测。*

**对于元模型：**

1\. 获取用于训练元模型的特征。这些是每个基础模型的预测。

2\. 获取用于训练元模型的目标。这是来自训练集的原始目标。

3\. 选择一个算法（例如，线性回归）。

4\. 使用交叉验证获得算法的最佳超参数集合。

看，完成了！你现在拥有：

- 多个经过优化超参数训练的基础模型

- 一个也经过优化超参数训练的元模型

这意味着你已经成功训练了一个堆叠集成模型！

# 2\. 如何使用 H2O.ai 训练堆叠集成模型

现在，让我们开始编写代码吧！

如前所述，本节包括我为 2023 Cloudflight 编码竞赛（AI 类别）中的预测任务训练堆叠集成模型的完整代码，这是一个使用表格数据的回归任务。在竞赛时间限制下，我从 40 个基础模型中创建了一个堆叠集成模型，涉及 3 种算法类型——深度神经网络、XGBoost 和 LightGBM，这些特定算法被选择是因为它们在实践中通常表现优异。

## 2.1\. 数据准备

首先，让我们导入必要的库。

```py
import pandas as pd
import h2o
from h2o.estimators.deeplearning import H2ODeepLearningEstimator
from h2o.estimators import H2OXGBoostEstimator
from h2o.estimators.stackedensemble import H2OStackedEnsembleEstimator
import optuna
from tqdm import tqdm

seed = 1
```

并初始化 H2O 集群。

```py
h2o.init()
```

接下来，加载数据集。

```py
data = pd.read_csv('path_to_your_tabular_dataset')
```

在使用 H2O 进行模型构建之前，让我们首先了解 H2O 模型的以下特性：

1.  H2O 模型不能接收 Pandas DataFrame 对象，因此 `data` 必须从 Pandas DataFrame 转换为 H2O 对应的 H2OFrame。

1.  H2O 模型可以自动编码分类特征，这很好，因为这一步预处理被省略了。为了确保这些特征被模型识别为分类特征，它们必须显式转换为因子（分类）数据类型。

```py
data_h2o = h2o.H2OFrame(data)

categorical_cols = [...]  #insert the names of the categorical features here
for col in categorical_cols:
  data_h2o[col] = data_h2o[col].asfactor()
```

现在我们可以使用 H2OFrame 对象的 `split_frame()` 方法将数据集拆分为训练集（90%）和验证集（10%）。

```py
splits = data_h2o.split_frame(ratios=[0.9], seed=seed)
train = splits[0]
val = splits[1]
```

最后，让我们获取建模的特征和目标。与接受*特征*和目标的*值*的 Scikit-Learn 模型不同，H2O 模型接受的是*特征*和目标的*名称*。

```py
y = '...'  #insert name of the target column here
x = list(train.columns)
x.remove(y) 
```

现在，让模型训练的乐趣开始吧！

## 2.2\. 训练深度神经网络（DNN）作为基础模型

我们先从训练将组成堆叠集成模型的 DNN 开始，使用 H2O 的 `H2ODeepLearningEstimator`。

> **附注：为什么在 H2O 中训练 DNN，而不是使用 Tensorflow、Keras 或 PyTorch？**
> 
> *在跳入代码之前，你可能会好奇为什么我选择使用 H2O 的* `*H2ODeepLearningEstimator*`* 来训练 DNN，而不是使用 Tensorflow、Keras 或 PyTorch 这些常用的 DNN 构建库。*
> 
> *简单的答案是，在 H2O 中构建堆叠集成模型使用的是* `*H2OStackedEnsembleEstimator*`*，它只能接受 H2O 模型家族中的基础模型。然而，更关键的原因是 H2O 的* `*H2ODeepLearningEstimator*` *比其他框架更容易调整 DNN，这就是原因。*
> 
> *在 TensorFlow、Keras 或 PyTorch 中，像 dropout 层这样的正则化效果必须手动添加到模型架构中，例如使用* `*keras.layers.Dropout()*`*。这允许更大的自定义，但也需要更多的详细知识和精力。例如，你必须决定在模型架构中在哪里以及多少次包括* `*keras.layers.Dropout()*` *层。*
> 
> *另一方面，H2O 的* `*H2ODeepLearningEstimator*` *更加抽象，易于普通人使用。可以通过模型超参数以简单的方式启用正则化，从而减少手动设置这些组件为层的需求。此外，* 默认 *的模型超参数已包含正则化。常见的特征预处理步骤，例如数值特征的缩放和分类特征的编码，也作为模型超参数包含在内，以实现自动特征预处理。这些功能使得调整深度神经网络（DNN）的过程变得更加直接和简单，而无需深入了解深度学习模型架构的复杂性。在比赛的时间紧迫背景下，这对我来说非常有用！*

那么，我们应该使用哪一组超参数来训练`H2ODeepLearningEstimator`呢？这就是*optuna*的作用所在。*Optuna*是一个超参数优化框架，类似于传统的网格搜索和随机搜索方法，但它采用了更复杂的方法。

网格搜索系统地探索预定义范围内的超参数值，而随机搜索则在这些指定的限制范围内选择随机组合。然而，*optuna*利用贝叶斯优化，从之前的搜索中学习，提出每次后续搜索中表现更好的超参数集，提高了寻找最佳模型超参数的效率。这在复杂且大型的超参数空间中尤其有效，在这些空间中，传统的搜索方法可能会耗时极长，最终仍可能无法找到最佳的超参数集。

现在，让我们进入代码部分。我们将使用*optuna*来调整 H2O 的`H2ODeepLearningEstimator`的超参数，并将所有训练过的模型跟踪到列表`dnn_models`中。

```py
dnn_models = []

def objective(trial):
    #params to tune
    num_hidden_layers = trial.suggest_int('num_hidden_layers', 1, 10)
    hidden_layer_size = trial.suggest_int('hidden_layer_size', 100, 300, step=50)

    params = {
        'hidden': [hidden_layer_size]*num_hidden_layers,
        'epochs': trial.suggest_int('epochs', 5, 100),
        'input_dropout_ratio': trial.suggest_float('input_dropout_ratio', 0.1, 0.3),  #dropout for input layer
        'l1': trial.suggest_float('l1', 1e-5, 1e-1, log=True),  #l1 regularization
        'l2': trial.suggest_float('l2', 1e-5, 1e-1, log=True),  #l2 regularization
        'activation': trial.suggest_categorical('activation', ['rectifier', 'rectifierwithdropout', 'tanh', 'tanh_with_dropout', 'maxout', 'maxout_with_dropout'])
}

    #param 'hidden_dropout_ratios' is applicable only if the activation type is rectifier_with_dropout, tanh_with_dropout, or maxout_with_dropout
    if params['activation'] in ['rectifierwithdropout', 'tanh_with_dropout', 'maxout_with_dropout']:
        hidden_dropout_ratio = trial.suggest_float('hidden_dropout_ratio', 0.1, 1.0)  
        params['hidden_dropout_ratios'] = [hidden_dropout_ratio]*num_hidden_layers  #dropout for hidden layers

    #train model
    model = H2ODeepLearningEstimator(**params,
                                     standardize=True,  #h2o models can do this feature preprocessing automatically
                                     categorical_encoding='auto',  #h2o models can do this feature preprocessing automatically
                                     nfolds=5,
                                     keep_cross_validation_predictions=True,  #need this for training the meta-model later
                                     seed=seed)
    model.train(x=x, y=y, training_frame=train)

    #store model
    dnn_models.append(model)

    #get cross-validation rmse 
    cv_metrics_df = model.cross_validation_metrics_summary().as_data_frame()
    cv_rmse_index = cv_metrics_df[cv_metrics_df[''] == 'rmse'].index
    cv_rmse = cv_metrics_df['mean'].iloc[cv_rmse_index]
    return cv_rmse

study = optuna.create_study(direction='minimize')
study.optimize(objective, n_trials=20)
```

上述*optuna*研究用于寻找能够最小化交叉验证 RMSE 的`H2ODeepLearningEstimator`超参数集（因为这是一个回归任务），优化过程运行 20 次试验，使用参数`n_trials=20`。这意味着训练了 20 个 DNN，并将它们存储在列表`dnn_models`中，以供稍后作为堆叠集成的基模型使用，每个模型都有不同的超参数集。考虑到比赛的时间限制，我选择训练 20 个 DNN，但你可以根据自己的需求设置`n_trials`为你希望训练的 DNN 数量。

重要的是，`H2ODeepLearningEstimator`必须使用`keep_cross_validation_predictions=True`进行训练，因为这些交叉验证预测将被用作训练元模型的特征。

## 2.3\. 将 XGBoost 和 LightGBM 作为基模型进行训练

接下来，让我们训练将形成堆叠集成基模型集合的 XGBoost 和 LightGBM 模型。我们将再次使用*optuna*来调优 H2O 的`H2OXGBoostEstimator`的超参数，并将所有训练过的模型记录在`xgboost_lightgbm_models`列表中。

在深入代码之前，我们必须首先了解`H2OXGBoostEstimator`是将流行的*xgboost*库中的 XGBoost 框架集成到 H2O 中。另一方面，H2O 并未集成*lightgbm*库。然而，它确实提供了一种方法，通过在`H2OXGBoostEstimator`中使用特定参数集来模拟 LightGBM 框架——这正是我们将实现的，以便使用`H2OXGBoostEstimator`来训练 XGBoost 和 LightGBM 模型。

```py
xgboost_lightgbm_models = []

def objective(trial):
    #common params between xgboost and lightgbm
    params = {
        'ntrees': trial.suggest_int('ntrees', 50, 5000),
        'max_depth': trial.suggest_int('max_depth', 1, 9),
        'min_rows': trial.suggest_int('min_rows', 1, 5),
        'sample_rate': trial.suggest_float('sample_rate', 0.8, 1.0),
        'col_sample_rate': trial.suggest_float('col_sample_rate', 0.2, 1.0),
        'col_sample_rate_per_tree': trial.suggest_float('col_sample_rate_per_tree', 0.5, 1.0)
    }

    grow_policy = trial.suggest_categorical('grow_policy', ['depthwise', 'lossguide'])

     #######################################################################################################################
     #from H2OXGBoostEstimator's documentation, (https://docs.h2o.ai/h2o/latest-stable/h2o-docs/data-science/xgboost.html) # 
     #lightgbm is emulated when grow_policy=lossguide and tree_method=hist                                                 #
     #so we will tune lightgbm-specific hyperparameters when this set of hyperparameters is used                           #
     #and tune xgboost-specific hyperparameters otherwise                                                                  #
     #######################################################################################################################

    #add lightgbm-specific params
    if grow_policy == 'lossguide':  
        tree_method = 'hist'  
        params['max_bins'] = trial.suggest_int('max_bins', 20, 256)
        params['max_leaves'] = trial.suggest_int('max_leaves', 31, 1024)

    #add xgboost-specific params
    else:
        tree_method = 'auto'
        params['booster'] = trial.suggest_categorical('booster', ['gbtree', 'gblinear', 'dart'])
        params['reg_alpha'] = trial.suggest_float('reg_alpha', 0.001, 1)
        params['reg_lambda'] = trial.suggest_float('reg_lambda', 0.001, 1)
        params['min_split_improvement'] = trial.suggest_float('min_split_improvement', 1e-10, 1e-3, log=True)

    #add grow_policy and tree_method into params dict
    params['grow_policy'] = grow_policy
    params['tree_method'] = tree_method

    #train model
    model = H2OXGBoostEstimator(**params,
                                learn_rate=0.1,
                                categorical_encoding='auto',  #h2o models can do this feature preprocessing automatically
                                nfolds=5,
                                keep_cross_validation_predictions=True,  #need this for training the meta-model later
                                seed=seed) 
    model.train(x=x, y=y, training_frame=train)

    #store model
    xgboost_lightgbm_models.append(model)

    #get cross-validation rmse
    cv_metrics_df = model.cross_validation_metrics_summary().as_data_frame()
    cv_rmse_index = cv_metrics_df[cv_metrics_df[''] == 'rmse'].index
    cv_rmse = cv_metrics_df['mean'].iloc[cv_rmse_index]
    return cv_rmse

study = optuna.create_study(direction='minimize')
study.optimize(objective, n_trials=20)
```

类似地，20 个 XGBoost 和 LightGBM 模型被训练并存储在`xgboost_lightgbm_models`列表中，以备后续作为堆叠集成的基模型使用，每个模型有一组不同的超参数。你可以设置`n_trials`为任何你希望训练的 XGBoost/LightGBM 模型的数量。

重要的是，`H2OXGBoostEstimator`还必须使用`keep_cross_validation_predictions=True`进行训练，因为这些交叉验证预测将被用作训练元模型的特征。

## 2.4\. 训练元模型

我们将使用上述训练的*所有*深度神经网络、XGBoost 和 LightGBM 模型作为基模型。然而，这并不意味着所有这些模型都会被用于堆叠集成，因为在调优我们的元模型时，我们将进行自动基模型选择（稍后会详细介绍）！

记住，我们将每个训练过的基模型存储在`dnn_models`（20 个模型）和`xgboost_lightgbm_models`（20 个模型）列表中，总共为我们的堆叠集成提供了 40 个基模型。接下来，我们将它们合并成一个最终的基模型列表`base_models`。

```py
base_models = dnn_models + xgboost_lightgbm_models
```

现在，我们已经准备好使用这些基模型来训练元模型。但首先，我们必须决定元模型算法，在此过程中涉及一些概念：

1.  大多数关于堆叠集成的学术论文建议为元模型选择**简单**的线性算法。这是为了避免元模型对基模型的预测结果过拟合。

1.  H2O 推荐在回归任务中使用广义线性模型（GLM）而非线性回归，或在分类任务中使用广义线性模型而非逻辑回归。这是因为 GLM 是一种灵活的线性模型，不像后者那样强加正态性和同方差性的关键假设，这使得它能够更好地建模目标值的真实行为，因为这些假设在实际操作中可能很难满足。关于这一点的进一步解释可以在[这篇学术论文](https://github.com/ledell/phd-thesis/blob/main/ledell-phd-thesis.pdf)中找到，H2O 的工作就是基于这篇论文的。

因此，我们将使用 `H2OStackedEnsembleEstimator` 并设置 `metalearner_algorithm='glm'` 来实例化元模型，并使用 *optuna* 调整 GLM 元模型的超参数以优化性能。

```py
def objective(trial):
    #GLM params to tune
    meta_model_params = {
        'alpha': trial.suggest_float('alpha', 0, 1),  #regularization distribution between L1 and L2
        'family': trial.suggest_categorical('family', ['gaussian', 'tweedie']),  #read the documentation here on which family your target may fall into: https://docs.h2o.ai/h2o/latest-stable/h2o-docs/data-science/glm.html
        'standardize': trial.suggest_categorical('standardize', [True, False]),
        'non_negative': True  #predictions of each base model cannot be subtracted from one another
    }

    ensemble = H2OStackedEnsembleEstimator(metalearner_algorithm='glm',
                                             metalearner_params=meta_model_params,
                                             metalearner_nfolds=5,
                                             base_models=base_models,  
                                             seed=seed)

    ensemble.train(x=x, y=y, training_frame=train)

    #get cross-validation rmse
    cv_metrics_df = ensemble.cross_validation_metrics_summary().as_data_frame()
    cv_rmse_index = cv_metrics_df[cv_metrics_df[''] == 'rmse'].index
    cv_rmse = cv_metrics_df['mean'].iloc[cv_rmse_index]
    return cv_rmse

study = optuna.create_study(direction='minimize')
study.optimize(objective, n_trials=20)
```

注意，每个基础模型的交叉验证预测没有被显式地传递给 `H2OStackedEnsembleEstimator`。这是因为 H2O 在后台自动完成了这一步骤，使我们更加轻松！我们只需在之前训练基础模型时设置 `keep_cross_validation_predictions=True`，并使用参数 `base_models=base_models` 实例化 `H2OStackedEnsembleEstimator` 即可。

现在，我们可以最终构建 `best_ensemble` 模型，使用 *optuna* 找到的最佳超参数。

```py
best_meta_model_params = study.best_params
best_ensemble = H2OStackedEnsembleEstimator(metalearner_algorithm='glm',
                                            metalearner_params=best_meta_model_params,
                                            base_models=base_models,
                                            seed=seed)

best_ensemble.train(x=x, y=y, training_frame=train)
```

完成了，我们成功地在 H2O 中训练了一个堆叠集成模型！让我们来看看吧。

```py
best_ensemble.summary()
```

![](img/f03397fcef61f99f6cb428a7698d2f5d.png)

作者提供的图片

注意到堆叠集成模型只使用了我们提供的 40 个基础模型中的 16 个，其中 3 个是 XGBoost/LightGBM 模型，13 个是深度神经网络模型。这是因为我们为 GLM 元模型调整的超参数`alpha`，它代表了 L1（LASSO）和 L2（Ridge）之间的正则化分布。`1` 的值仅涉及 L1 正则化，而 `0` 的值仅涉及 L2 正则化。

如上所述，最佳值被发现为 `alpha=0.16`，因此使用了 L1 和 L2 的混合。一些基础模型的预测在 L1 正则化下的回归中系数被设置为 `0`，这意味着这些基础模型在堆叠集成中完全没有被使用，因此实际使用的基础模型数量少于 40 个。

这里的关键要点是，我们上面的设置还通过元模型的正则化超参数自动选择了用于最佳性能的基础模型，而不是简单地使用所有提供的 40 个基础模型。

# 3\. 性能比较：堆叠集成与独立基础模型

为了展示堆叠集成的威力，让我们使用它为从一开始就被保留的验证集生成预测。下面的 RMSE 数字仅针对我使用的数据集，但也可以在自己的数据集上运行本文的代码，亲自查看模型性能的差异！

```py
ensemble_val_rmse = best_ensemble.model_performance(val).rmse()
ensemble_val_rmse   #0.31475634111745304
```

堆叠集成在验证集上产生了 0.31 的 RMSE。

接下来，让我们深入分析每个基础模型在这个相同验证集上的表现。

```py
base_val_rmse = []
for i in range(len(base_models)):
    base_val_rmse = base_models[i].model_performance(val).rmse()

models = ['H2ODeepLearningEstimator'] * len(dnn_models) + ['H2OXGBoostEstimator'] * len(xgboost_lightgbm_models)

base_val_rmse_df = pd.DataFrame([models, base_val_rmse]).T
base_val_rmse_df.columns = ['model', 'val_rmse']
base_val_rmse_df = base_val_rmse_df.sort_values(by='val_rmse', ascending=True).reset_index(drop=True)
base_val_rmse_df.head(15)  #show only the top 15 in terms of lowest val_rmse
```

![](img/853f94aa62127c3dce0a3c3c83c84219.png)

图片由作者提供

与实现了 0.31 RMSE 的堆叠集成模型相比，表现最佳的独立基础模型的 RMSE 为 0.35。

> 这意味着堆叠方法能够在未见数据上提高预测性能 11%！

现在你已经见证了堆叠集成的威力，轮到你亲自尝试了！

写这篇文章我感到非常愉快，如果你阅读起来也觉得有趣，我会非常感激你花一点时间留下点赞和关注！

下次见！

Sheila
