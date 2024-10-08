# 有效优化你的回归模型与贝叶斯超参数调优

> 原文：[`towardsdatascience.com/effectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3`](https://towardsdatascience.com/effectively-optimize-your-regression-model-with-bayesian-hyperparameter-tuning-819c19f5dab3)

## 学习如何有效优化超参数，防止为 XGBoost、CatBoost 和 LightBoost 创建过拟合模型

[](https://erdogant.medium.com/?source=post_page-----819c19f5dab3--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----819c19f5dab3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----819c19f5dab3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----819c19f5dab3--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----819c19f5dab3--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----819c19f5dab3--------------------------------) ·15 分钟阅读·2023 年 7 月 17 日

--

![](img/85d161655226ea3f51d0055d58fa9bfe.png)

照片由[Alexey Ruban](https://unsplash.com/@intelligenciya?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/tune?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

梯度提升技术如 *XGBoost、CatBoost 和 LightBoost* 在最近几年获得了很大的普及，适用于分类和回归任务。过程中的一个重要部分是超参数的调优，以获得最佳的模型性能。关键在于优化超参数搜索空间并找到一个能够在新未见数据上泛化的模型。*在这篇博客中，我将演示 1\. 如何使用贝叶斯优化学习一个经过优化超参数的提升决策树* ***回归*** *模型，2\. 如何选择一个能够泛化（而非过拟合）的模型，3\. 如何解释并可视化优化后的超参数空间以及模型性能准确性。* [*HGBoost*](https://erdogant.github.io/hgboost/) *库非常适合这个任务，其中包括双重交叉验证来防止过拟合。*

# 简要介绍。

梯度提升算法如极端梯度提升（*XGboost*）、轻量级梯度提升（*Lightboost*）和*CatBoost* 是强大的集成机器学习算法，用于预测建模（*分类* 和 *回归任务*），适用于*表格*、*连续*和*混合*形式的数据集 *[1,2,3 ]*。***在这里，我将专注于回归任务。*** 在接下来的部分中，我们将使用双重交叉验证循环训练一个提升决策树模型。我们将仔细拆分数据集，设置搜索空间，并使用库[*Hyperopt*](http://hyperopt.github.io/hyperopt/)进行贝叶斯优化。训练模型后，我们可以通过创建有洞察力的图表来深入解读结果。

如果你需要更多背景知识或对这些概念不太熟悉，我建议阅读这篇博客：

## 如何通过贝叶斯超参数调整找到最佳的提升模型，但没有…

### 提升决策树算法可能优于其他模型，但过拟合是一个真实的危险。使用…

## 如何通过贝叶斯超参数调整找到最佳的提升模型，但没有…

如果你需要**分类任务**的实践指南，我推荐阅读这篇博客：

[## 分类模型的贝叶斯优化超参数调整实用指南。](https://erdogant.medium.com/hands-on-guide-for-hyperparameter-tuning-with-bayesian-optimization-for-classification-models-2002224bfa3d?source=post_page-----819c19f5dab3--------------------------------)

### 学习如何拆分数据，优化超参数，防止过拟合，选择最佳模型，并创建…

[erdogant.medium.com](https://erdogant.medium.com/hands-on-guide-for-hyperparameter-tuning-with-bayesian-optimization-for-classification-models-2002224bfa3d?source=post_page-----819c19f5dab3--------------------------------)

*在我们进入实际示例之前，我将简要讨论一下* [*HGBoost 库 [4]*](https://erdogant.github.io/hgboost/) *。*

# 通向超优化回归模型的步骤。

训练一个具有优化超参数的回归模型并防止创建过拟合模型需要多个步骤。前三个步骤将形成基础，并作为*HGboost*模型的输入。让我们逐步了解这些步骤。

> 使用超优化参数训练模型既耗时又复杂，相比于未超优化的模型。为了防止采用过拟合的模型，需要进行（合理性）检查。

1.  导入并初始化*HGBoost*库。

1.  导入数据集并进行预处理。

1.  确定并选择最合适的评估指标。

**hgboost 库处理所有以下步骤：**

1.  将数据分为*训练集*、*测试集*和*验证集*。

1.  创建一个双重交叉验证模型，其中内层循环用于优化超参数，外层循环用于验证和评分模型。

1.  选择性能最佳的模型。

1.  在独立验证集上评估性能最佳的模型。

1.  在整个数据集上训练最终模型。

1.  创建有洞察力的图表用于模型和搜索空间评估。

最后一步是结果解释。*我们将在下一节中详细介绍每一步。*

# 步骤 1\. 导入和初始化。

我们可以使用以下命令通过 pip 安装*HGBoost*：

```py
# Installation
pip install hgboost
```

安装完成后，我们可以导入并初始化一个用于*回归任务*的模型。输入参数可以相应地更改，或者使用默认设置（代码部分 1）。我将设置`max_eval=1000`迭代。下一步是读取（或导入）数据集。

```py
# Import the library
from hgboost import hgboost

# Initialize library.
hgb = hgboost(
    max_eval=1000,      # Search space is based  on the number of evaluations.
    threshold=0.5,     # Classification threshold. In case of two-class model this is 0.5.
    cv=5,              # k-folds cross-validation.
    test_size=0.2,     # Percentage split for the testset.
    val_size=0.2,      # Percentage split for the validationset.
    top_cv_evals=10,   # Number of top best performing models that is evaluated.
    is_unbalance=True, # Control the balance of positive and negative weights, useful for unbalanced classes.
    random_state=None, # Fix the random state to create reproducible results.
    n_jobs=-1,         # The number of CPU jobs to run in parallel. -1 means using all processors.
    gpu=False,         # Compute using GPU in case of True.
    verbose=3,         # Print progress to screen.
)
```

# 步骤 2\. 读取和预处理数据集。

为了演示，我们使用数据科学薪资数据集 [6]，可以通过函数`import_example(data=’ds_salaries’)`导入。该数据集包含 4134 个样本和 11 个特征。我将设置`salary_in_usd`作为***目标值***。对于预处理，我将使用内部的`.preprocessing()`函数***(****代码部分 2****)***，该函数利用了[***df2onehot library***](https://github.com/erdogant/df2onehot)[5]，将分类值编码为 one-hot。请注意，连续值没有编码，而是保持不变。在这一点上，强烈建议进行探索性数据分析（EDA）和进行合理性检查。

> 最大的模型性能提升通常来自于预处理和特征工程。

在预处理步骤后，有 4134 行 x 198 列。请注意，此示例中的预处理步骤假设我们将训练一个*XGBoost*模型。不同的模型（*Xgboost、Lightboost、Adaboost*）需要不同的预处理步骤。例如，*XGBoost*需要 one-hot 编码，而其他模型可以处理非数字因素。*阅读* ***这篇博客*** *以获取有关（不）优点和预处理步骤的更多信息。*

# 步骤 3\. 设置超参数搜索空间。

超参数优化的搜索空间在*HGBoost*中定义，并且在模型*XGBoost、LightBoost 和 CatBoost*之间略有不同（代码部分 3）。*注意，搜索空间（下方代码部分）在 HGboost 中是预定义的。*

```py
#########################################
# You do not need to execute these lines!
#########################################

# XGBoost
xgb_reg_params = {
    'learning_rate': hp.quniform('learning_rate', 0.05, 0.31, 0.05),
    'max_depth': hp.choice('max_depth', np.arange(5, 30, 1, dtype=int)),
    'min_child_weight': hp.choice('min_child_weight', np.arange(1, 10, 1, dtype=int)),
    'gamma': hp.choice('gamma', [0, 0.25, 0.5, 1.0]),
    'reg_lambda': hp.choice('reg_lambda', [0.1, 1.0, 5.0, 10.0, 50.0, 100.0]),
    'subsample': hp.uniform('subsample', 0.5, 1),
    'n_estimators': hp.choice('n_estimators', range(20, 205, 5)),
}

# LightBoost
lgb_reg_params = {
    'learning_rate': hp.quniform('learning_rate', 0.05, 0.31, 0.05),
    'max_depth': hp.choice('max_depth', np.arange(5, 30, 1, dtype=int)),
    'min_child_weight': hp.choice('min_child_weight', np.arange(1, 8, 1, dtype=int)),
    'subsample': hp.uniform('subsample', 0.8, 1),
    'n_estimators': hp.choice('n_estimators', range(20, 205, 5)),
}

# CatBoost
ctb_reg_params = {
    'learning_rate': hp.quniform('learning_rate', 0.05, 0.31, 0.05),
    'max_depth': hp.choice('max_depth', np.arange(2, 16, 1, dtype=int)),
    'colsample_bylevel': hp.choice('colsample_bylevel', np.arange(0.3, 0.8, 0.1)),
    'n_estimators': hp.choice('n_estimators', range(20, 205, 5)),
}
```

# 步骤 4\. 训练/测试/评估集和评估指标。

对于监督式机器学习任务，重要的是拆分数据以避免[过拟合](https://www.techtarget.com/whatis/definition/overfitting)模型。过拟合是指模型过于准确地拟合（或学习）数据，进而无法可靠地预测（新的）未见数据。

> 如果没有仔细拆分数据，你可能会过度拟合模型参数，对数据进行过度学习。这样一来，模型可能会无法正确预测新的（未见）数据样本。

在训练过程中，数据集被划分为*训练集*、*测试集*和*独立验证集*。*验证集在整个训练过程中保持不变，仅在评估最终模型性能时使用一次。* 数据集的拆分以百分比进行，如`test_size=0.2`和`eval_size=0.2`。可以使用`eval_metric`设置模型评估指标。默认的***评估指标***设为均方根误差（***RMSE***），但也可以使用其他指标，如均方误差（***MSE***）或平均绝对误差（***MAE***）。

# 第 5 步：拟合、优化超参数并选择最佳模型。

此时，我们对数据进行了*XGBoost*的预处理，并决定使用哪个评估指标。***现在我们可以开始拟合模型并优化超参数。*** 在*HGBoost*中，优化超参数的第一步是设置内部循环，以贝叶斯优化来优化超参数，以及外部循环，以测试最佳性能模型如何使用*k*-折交叉验证进行泛化。搜索空间取决于可用的超参数。评估指标设为 MAE，因为其良好的可解释性。换句话说，如果我们看到目标值为***薪资***的 MAE=10000，则表明平均预测距离真实值为 10000 美元。

```py
##########################################
# Train model
##########################################

results  = hgb.xgboost_reg(X, y, eval_metric='mae')      # XGBoost
# results = hgb.lightboost_reg(X, y, eval_metric='mae')  # LightBoost
# results = hgb.catboost_reg(X, y, eval_metric='mae')    # CatBoost

# [hgboost] >Start hgboost regression.
# [hgboost] >Collecting xgb_reg parameters.
# [hgboost] >method: xgb_reg
# [hgboost] >eval_metric: mae
# [hgboost] >greater_is_better: False
# [hgboost] >*********************************************************************************
# [hgboost] >Total dataset: (4134, 198) 
# [hgboost] >Validation set: (827, 198) 
# [hgboost] >Test-set: (827, 198) 
# [hgboost] >Train-set: (2480, 198) 
# [hgboost] >*********************************************************************************
# [hgboost] >Searching across hyperparameter space for best performing parameters using maximum nr. evaluations: 1000
# 100%|██████████| 1000/1000 [04:14<00:00,  1.02s/trial, best loss: 35223.92493340009]
# [hgboost]> Collecting the hyperparameters from the [1000] trials.
# [hgboost] >[mae]: 35220 Best performing model across 1000 iterations using Bayesian Optimization with Hyperopt.
# [hgboost] >*********************************************************************************
# [hgboost] >5-fold cross validation for the top 10 scoring models, Total nr. tests: 50
# [hgboost] >[mae] (average): 35860 Best 5-fold CV model using optimized hyperparameters.
# [hgboost] >*********************************************************************************
# [hgboost] >Evaluate best [xgb_reg] model on validation dataset (827 samples, 20%)
# [hgboost] >[mae]: 35270 using optimized hyperparameters on validation set.
# [hgboost] >[mae]: 35540 using default (not optimized) parameters on validation set.
# [hgboost] >*********************************************************************************
# [hgboost] >Retrain [xgb_reg] on the entire dataset with the optimal hyperparameters.
# [hgboost] >Fin!
```

通过运行上述代码段，我们遍历了搜索空间并创建了 1000 个不同的模型（`max_eval=1000`），对其性能进行了评估。接下来，对这些模型按初始模型性能进行排名。我们现在可以使用 5 折交叉验证方案（`cv=5`）评估排名前*K*的最佳模型的鲁棒性（`top_cv_evals=10`）。在这方面，我们旨在防止找到过度训练的模型。在这个例子中，在 1000 次迭代中得分最高的模型的*MAE=35220*，而基于 5 折交叉验证的平均 MAE 为*MAE=35860*。在 5 折交叉验证的最佳模型上，独立验证集的 MAE 为*35270*。*虽然我们没有选择得分最高的模型，但我们确实避免了选择一个可能过拟合的模型。*

> 最佳模型不一定是性能最好的模型，而是能够在（新的）未见数据上进行泛化、提供准确预测的模型。

***我们现在有了一个可以对新数据进行预测的模型。*** 但在进行预测之前，我们首先需要了解上述所有步骤中发生了什么。

# 步骤 6\. 结果解释。

到目前为止，我们已经有了一个训练好的模型，并简要查看了基于交叉验证的回归结果，并使用了独立验证集。所有测试过的超参数都返回了，可以进一步检查（见下方代码部分）*。* 另见*图 1*，其中显示了所有模型结果的汇总`results['summary']`的数据框。此外，我们还可以通过绘制图表来检查结果。

```py
# Results are stored in the object and can be found in:
print(hgb.results)

# Results are also returned by the model:
print(results.keys())

# ['params', 'summary', 'trials', 'model', 'val_results', 'comparison_results']

###########################################################################################
# The params contains the parameters to create the best performing model.
print(results['params'])

# {'gamma': 1.0,
# 'learning_rate': 0.1,
# 'max_depth': 25,
# 'min_child_weight': 1,
# 'n_estimators': 55,
# 'reg_lambda': 1.0,
# 'subsample': 0.5522011244420446}

###########################################################################################
# The summary contains the model evaluations.
print(results['summary'])

#     gamma gpu_id learning_rate  ... best_cv loss_validation default_params
# 0     0.5      0           0.1  ...     0.0             NaN          False
# 1     1.0      0           0.3  ...     0.0             NaN          False
# 2       0      0           0.1  ...     0.0             NaN          False
# 3     1.0      0          0.05  ...     0.0             NaN          False
# 4    0.25      0           0.3  ...     0.0             NaN          False
# ..    ...    ...           ...  ...     ...             ...            ...
# 246   1.0      0          0.15  ...     0.0             NaN          False
# 247  0.25      0           0.1  ...     0.0             NaN          False
# 248   1.0      0          0.05  ...     0.0             NaN          False
# 249   0.5      0           0.2  ...     0.0             NaN          False
# 250  None    NaN          None  ...     NaN    35539.674538           True

# [251 rows x 20 columns]
###########################################################################################

# The trials contains the object from hyperopt in case you want to further investigate.
print(results['trials'])
# <hyperopt.base.Trials object at 0x000002B5A42E70A0>
```

![](img/af3eda5a237184f7d32555792f83707f.png)

图 1\. 结果`['summary']`数据框的输出包含所有模型结果。（作者提供的图像）

创建图表对于深入检查模型性能以及研究贝叶斯优化过程中超参数的调整非常重要。

> 理解模型结果的直观感受很重要，这有助于判断模型参数是否选择可靠。

我们可以使用*HGBsoost*的内置功能创建以下图表：

1.  *绘制以研究超参数空间的图表。*

1.  *绘制总结所有评估模型的图表。*

1.  *绘制交叉验证的性能图表。*

1.  *绘制独立验证集的结果图表。*

1.  *绘制决策树以理解特征的使用方式。*

```py
# Plot the hyperparameter tuning.
hgb.plot_params()

# Plot the summary of all evaluted models.
hgb.plot()

# Plot results on the validation set.
hgb.plot_validation()

# Plot results on the k-fold cross-validation.
hgb.plot_cv()

# Plot the best performing tree.
hgb.treeplot()
```

# 超参数调整的解释。

我们首先从研究贝叶斯优化过程中的超参数调整开始*。* 使用函数`.plot_params()`，我们可以创建如***图 2***所示的有洞察力的图表。该图包含多个直方图（或核密度图），每个子图包含在 1000 次模型迭代中优化的单一参数。直方图底部的小条表示 1000 次评估。相比之下，黑色虚线垂直线表示在前 10 个最佳表现模型中使用的特定参数值。绿色虚线表示***不使用***交叉验证方法的最佳表现模型，而红色虚线表示***使用***交叉验证的最佳表现模型。

让我们看看 ***图 2A***。在图的左下角，有参数 ***subsample***，其值范围从 0.5 到 1。0.95 附近有一个明显的峰值，这表明贝叶斯优化在这些区域进行了更密集的探索。我们表现最好的模型似乎使用了 *subsample=0.956*（红色虚线）。但还有更多内容需要查看。当我们查看 ***图 2B*** 时，我们也可以看到 *subsample*，每个点代表 1000 个模型中的一个。横轴是迭代次数，纵轴是优化后的值。对于这个参数，优化过程中的迭代有一个明显的趋势。它首先探索了下限区域，然后移动到上限区域，因为增加 *subsample* 值后模型的得分显然更好。

*以这种方式，所有超参数都可以与不同模型进行解释。*

![](img/a78739ed854fbadd02a6f9543e2a8dcf.png)

图 2\. 回归模型在 1000 次迭代中的调整超参数。A. 七个参数的分布。B. 每个点代表一次迭代，其中选择了一个特定值。请注意，这张图片仅用于说明，并非实际结果。（图片来自作者）

# 对所有评估模型的性能进行解释。

使用 `.plot()` 函数我们可以洞察 1000 个模型的性能（在这个例子中是 MAE）（***图 3***）。绿色虚线表示在没有交叉验证方法的情况下表现最好的模型，红色虚线表示使用了交叉验证的最佳模型。***我们选择的模型是红色虚线所示的模型。*** 一般来说，我们可以看到模型性能在迭代过程中有所提升，因为 MAE 分数在降低。这表明贝叶斯优化效果非常好。此外，当我们查看得分前 10 的模型（红色方框标记的模型）时，它们在 k 折交叉验证中的表现略微较差。换句话说，尽管在 1000 次迭代中表现最好的模型用绿色虚线表示，但在交叉验证中它的表现并不是最好。因此，选择一个能更好泛化的模型（红色虚线），即从 k 折交叉验证中获得的平均 MAE 最低的模型。通过这种方式，我们旨在选择最佳表现且具有良好泛化能力的模型。

![](img/0517b126c153fc4668dea92609ab0cd1.png)

图 3\. 1000 次评估和交叉验证的模型性能。（图片来自作者）

# 对独立验证集上的模型性能进行解释。

为了确保模型能够对未见数据进行泛化，我们使用独立验证集。***图 4***展示了回归线，预测结果没有显示出强烈的异常值或一致的高估或低估。我们看到低薪资存在低估情况，因为模型预测这些情况的薪资低于实际薪资。

![](img/b15db596c5803ec89358c7171aa9c01f.png)

图 4\. 在独立验证集上的结果。 （图片来自作者）

为了更深入地研究我们最终模型的泛化情况，我们可以使用`.plot_cv()`函数绘制 5 折交叉验证的结果。这将创建不同折的 ROC 曲线，如***图 5***所示。在这里我们可以看到模型在不同折中的斜率大致相同。因此，我们的最终模型在 k 折交叉验证中没有显示出异常。

![](img/13ca0cd68ec4553664563b6d463269c7.png)

图 5\. 使用优化超参数的模型进行 5 折交叉验证的结果。 （图片来自作者）

# 最佳模型的决策树图。

通过决策树图（***图 6***），我们可以更好地理解模型如何做出决策。这也可以提供一些直觉，说明这种模型是否可以对其他数据集进行泛化。请注意，默认情况下返回的最佳树是`num_tree=0`，但可以通过指定输入参数`.treeplot(num_trees=1)`返回许多树。此外，我们还可以绘制表现最佳的特征，如***图 7***所示。*远程工作*、*工作年限*和*经验水平*是预测薪资时最重要的前三个特征。

![](img/b2e38a84963ead3515d36bff4743f45d.png)

图 6\. 最佳模型的决策树图。 （图片来自作者）

![](img/8f225c804aca28b1aba41aa280d94199.png)

图 7\. 特征重要性。 （图片来自作者）

# 对新数据进行预测

在获得最终训练模型后，我们现在可以用它对新数据进行*预测*。假设***X***是新数据，并且经过与训练过程相似的预处理，然后我们可以使用`.predict(X)`函数进行预测。这个函数返回分类概率和预测标签*(****代码部分 5)***。

```py
# Make new predictions using the model. Suppose the X is new and unseen data.
y_pred, y_proba = hgb.predict(X)
```

# 保存和加载模型

保存和加载模型非常方便。为了实现这一点，有两个函数：`.save()`和`.load()`函数。

```py
# Save model
status = hgb.save(filepath='hgboost_model.pkl', overwrite=True)
# [pypickle] Pickle file saved: [hgboost_model.pkl]
# [hgboost] >Saving.. True

# Load model
from hgboost import hgboost                      # Import library when using a fresh start
hgb = hgboost()                                  # Initialize hgboost
results = hgb.load(filepath='hgboost_model.pkl') # Load the pickle file with model parameters and trained model.
# [pypickle] Pickle file loaded: [hgboost_model.pkl]
# [hgboost] >Loading succesful!

# Make predictions again
y_pred, y_proba = hgb.predict(X)
```

# 总结。

我演示了如何通过将数据集拆分为训练集、测试集和独立验证集来训练一个**回归**模型，并优化超参数。在训练-测试集内，使用贝叶斯优化（通过 hyperopt）优化超参数，外部循环则是根据 k 折交叉验证评估顶级模型的泛化能力。这样，我们旨在选择能够以最佳准确性泛化的模型。请注意，在我们的例子中，最佳得分模型似乎与具有默认参数的模型得分（或多或少）相似。这强化了我之前提到的观点，即在训练任何模型之前，一个重要的部分是进行典型的建模工作流：*从探索性数据分析（EDA）开始，迭代进行清理、特征工程和特征选择。通常这些步骤会带来最大的性能提升。*

[HGBoost 库](https://erdogant.github.io/hgboost/)还支持学习分类模型、多分类模型，甚至是提升树模型的集成。对于所有任务，应用相同的双重交叉验证方案，以确保选择性能最佳、最稳健的模型。此外，*HGBoost*利用了[HyperOpt](http://hyperopt.github.io/hyperopt/) [7, 8]库进行贝叶斯优化算法，该库是超参数优化中最受欢迎的库之一。

*注意安全，保持冷静。*

***干杯，E.***

*如果您觉得这篇文章有帮助，欢迎* [*关注我*](http://erdogant.medium.com/) *，因为我会写更多关于模型训练和优化的内容。如果您考虑购买 Medium 会员，您可以通过使用我的* [*推荐链接*](https://medium.com/@erdogant/membership)*来支持我的工作。价格相当于一杯咖啡，但允许您每月阅读无限量的文章。*

# 软件

+   [HGBoost Github](http://github.com/erdogant/hgboost)

+   [HGBoost 文档页面](https://erdogant.github.io/hgboost/)

+   [Colab 笔记本](https://erdogant.github.io/hgboost/pages/html/Blog.html#colab-regression-notebook)

# 其他相关链接

+   [让我们在 LinkedIn 上联系](https://www.linkedin.com/in/erdogant/)

+   [关注我在 Github 上的动态](https://github.com/erdogant)

# 参考文献

1.  Nan Zhu 等，[*XGBoost: 在 Spark 和 Flink 中实现最成功的 Kaggle 算法*](https://www.kdnuggets.com/2016/03/xgboost-implementing-winningest-kaggle-algorithm-spark-flink.html)。

1.  [欢迎访问 LightGBM 的文档](https://lightgbm.readthedocs.io/)!

1.  [CatBoost 是一个高性能的开源决策树梯度提升库](https://catboost.ai/)。

1.  E. Taskesen, 2020, [*超优化梯度提升*](https://github.com/erdogant/hgboost)*.*

1.  E.Taskesen, 2019, [*df2onehot: 将非结构化 DataFrame 转换为结构化 DataFrame*](https://github.com/erdogant/df2onehot)

1.  Kaggle, [*灾难中的机器学习*](https://www.kaggle.com/c/titanic)*.*

1.  James Bergstra 等，2013 年，[*Hyperopt: 一个用于优化机器学习算法超参数的 Python 库*](https://conference.scipy.org/proceedings/scipy2013/pdfs/bergstra_hyperopt.pdf)。

1.  Kris Wright，2017 年，[*使用 Hyperopt 进行参数调优*](https://medium.com/district-data-labs/parameter-tuning-with-hyperopt-faa86acdfdce)。
