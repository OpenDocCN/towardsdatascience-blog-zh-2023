# 逻辑回归：看似有缺陷

> 原文：[https://towardsdatascience.com/logistic-regression-deceptively-flawed-2c3e7f77eac9?source=collection_archive---------15-----------------------#2023-05-23](https://towardsdatascience.com/logistic-regression-deceptively-flawed-2c3e7f77eac9?source=collection_archive---------15-----------------------#2023-05-23)

## 大的赔率比和完美分离的数据什么时候会让你吃亏？

[](https://medium.com/@igor-s?source=post_page-----2c3e7f77eac9--------------------------------)[![Igor Šegota](../Images/17c592b71fef9526a0679d47937837f6.png)](https://medium.com/@igor-s?source=post_page-----2c3e7f77eac9--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2c3e7f77eac9--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2c3e7f77eac9--------------------------------) [Igor Šegota](https://medium.com/@igor-s?source=post_page-----2c3e7f77eac9--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe5f8ebca4ad8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-deceptively-flawed-2c3e7f77eac9&user=Igor+%C5%A0egota&userId=e5f8ebca4ad8&source=post_page-e5f8ebca4ad8----2c3e7f77eac9---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----2c3e7f77eac9--------------------------------) ·8分钟阅读·2023年5月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2c3e7f77eac9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-deceptively-flawed-2c3e7f77eac9&user=Igor+%C5%A0egota&userId=e5f8ebca4ad8&source=-----2c3e7f77eac9---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2c3e7f77eac9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Flogistic-regression-deceptively-flawed-2c3e7f77eac9&source=-----2c3e7f77eac9---------------------bookmark_footer-----------)![](../Images/fd6846838f4d97d1fe123837c8fb36d1.png)

照片由[Alvan Nee](https://unsplash.com/es/@alvannee?utm_source=medium&utm_medium=referral)拍摄，发布在[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是上一篇关于逻辑回归概念理解的文章的第二部分：

[](/logistic-regression-faceoff-67560de4f492?source=post_page-----2c3e7f77eac9--------------------------------) [## 逻辑回归：对决与概念理解

### 对数损失和完美分离的数据与冰球棒有什么关系？

towardsdatascience.com](/logistic-regression-faceoff-67560de4f492?source=post_page-----2c3e7f77eac9--------------------------------)

上次我们可视化并解释了逻辑回归中的对数损失拟合。我们还展示了这个过程无法完美拟合完全分离的数据。换句话说，与普通最小二乘法的线性回归不同，逻辑回归实际上在数据稍微有些噪声时效果更好！

实际上，这是否重要？*这要看情况：*

+   如果我们的目标是将输出用于统计推断，这就很重要。例如，准确估计模型系数、计算置信区间并使用 p 值测试假设。

+   如果我们的目标是使用逻辑模型的输出创建一个预测分类模型，那么这并没有太大关系。

# 统计推断：statsmodels

## 示例数据

在这一部分，我们将使用 Python 的 statsmodels 库。请注意，statsmodels 和 scikit-learn（稍后使用）使用 *β* 来参数化概率，而不是 *k* 和 *x₀*：

![](../Images/6b56227f9d6f2bd6640839f01a3d90a6.png)

其中 *k*、*x₀* 和 *β₁*、*β₀* 之间的关系是：

![](../Images/b9226107d5bd05e8238a5d138809310b.png)

我们将继续使用我们在 [逻辑回归第一部分](/logistic-regression-faceoff-67560de4f492) 中生成的数据集，首先使用“非完美”数据 `sample_df` ，使用 statsmodels 的公式 API：

```py
import statsmodels.formula.api as smf
model = smf.logit('y ~ x', df).fit()
model.summary()

#> Optimization terminated successfully.
#>         Current function value: 0.230366
#>         Iterations 8
```

![](../Images/0c76474446a04887204ce5aebabe589a.png)

我们的模型参数是 *k = 3* 和 *x₀ = 2.5*，因此它们对应于 *β₁ = 3* 和 *β₀ = -7.5*。我们可以通过从底部表格的 `coef` 列中读取这些参数，与拟合的参数进行比较：

![](../Images/6e4bba19fadc12707c46ebbaba5aa5c7.png)

我们的数据点非常少，而且种子是故意选择的，以展示离群值，因此拟合略有偏差，但仍在合理范围内。这里报告的总对数损失为“对数似然”，即总对数损失的负值，等于 -6.911。

## 完全分离的数据

当我们在 statsmodels 中运行我们完全分离的数据集时，会发生什么？答案是，这要看情况！

```py
perfect_sep_model = smf.logit('y ~ x', perfect_sep_data).fit()

#> ...
#> PerfectSeparationError: Perfect separation detected, results not available
```

在这种情况下我们得到了一个错误——没有输出结果。在其他完美分离的情况下，我们可能会得到一个警告。例如，如果我们使用相同的参数但不同的随机种子：

```py
perfect2_df = create_sample_data(k=3, x0=2.5, n_points=30, seed=154)
perfect_sep2_df, perfect_sep_logloss2_df = fit_data_to_logloss(
    perfect2_df,
    k=3,
    x0=2.5
)

perfect_sep_model_2 = smf.logit('y ~ x', perfect_sep2_df).fit()
perfect_sep_model_2.summary()

#> Warning: Maximum number of iterations has been exceeded.
#>         Current function value: 0.000000
#>         Iterations: 35

#> /.../statsmodels/base/model.py:604: ConvergenceWarning: 
#>   Maximum Likelihood optimization failed to converge. Check mle_retvals
```

![](../Images/29d9a85d58eaf5d3be965f408b515d2b.png)

由于第二个模型也没有收敛，我们可以认为它可能也应该返回一个错误，而不是一个无害的警告。R 中的逻辑回归函数 `glm(..., family=binomial)` 也是这样。引用 [R Inferno](https://www.burns-stat.com/pages/Tutor/R_inferno.pdf) 的第五圈，一致性：

> 警告的问题在于，没有人会阅读它们。人们必须阅读错误信息，因为在按下按钮后没有食物颗粒掉入托盘中。而警告只会使机器发出警报，但他们仍然会得到食物颗粒。即使它可能是毒药也无所谓。

因此，在使用效应大小和 p 值进行推断时要小心！在这种情况下，数据是完全可分的——我们有一个完美的预测器——而报告的 p 值（P > |z|）为 0.998。忽视或误解这些警告可能会使你错过数据中的一些明显特征。

另外，考虑使用其他模型。逻辑回归之外还有一个崭新的世界！🌍

# 预测：scikit-learn

## 样本数据

scikit-learn 自动化和抽象了许多计算，同时提供了一种简单而一致的方式来运行许多不同的模型。然而，在这样做时，它隐藏了许多在后台发生的事情。当后台发生的事情不明显时，从 scikit-learn 文档中学习数据科学概念可能会导致误解。

其中一个误解是，当我们从 `sklearn.linear_model` 运行 `LogisticRegression()` 并使用 `.predict()` 方法时，它*看起来像*我们在运行一个分类模型：

```py
from sklearn.linear_model import LogisticRegression

def sklearn_logistic_fit(df):
    sklearn_logistic = LogisticRegression(penalty=None).fit(
        df['x'].values.reshape(-1, 1),
        df['y'].values
    )
    predicted_outcomes = sklearn_logistic.predict(
        df['x'].values.reshape(-1, 1)
    )

    print("beta_1 or log-odds-ratio: ", sklearn_logistic.coef_[0][0])
    print("beta_0 or y intercept: ", sklearn_logistic.intercept_[0])
    print("Original outcomes: ")
    print(df['y'].values)
    print("Predicted outcomes: ")
    print(predicted_outcomes)

    return predicted_outcomes

sklearn_logistic_fit(sample_df)

#> beta_1 or log-odds-ratio:  2.6187462367743803
#> beta_0 or y intercept:  -5.682896087072998
#> Original outcomes: 
#> [0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 1 0 1 1 1 1 1 1 1 1 1 1 1 1]
#> Predicted outcomes: 
#> [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1]
```

然而，我们可以通过调用其 `.predict_proba()` 方法或从模型对象中挖掘模型参数来验证这仍然是老旧的逻辑回归，我们在这里打印出来了。

在幕后，`.predict()` 根据两个概率中的较大者进行预测：*p* 或 *1-p*。如果 *p > 1-p*，则预测为 1，否则预测为 0。它不会计算置信区间，也没有 p 值，也不会进行任何类型的统计推断或假设检验。它仍然是一个回归模型，只是在其上方有一个额外的阈值层。

注意：`LogisticRegression()` 默认使用 L2 正则化，它在对数损失函数中添加了一个额外的项。为了使其与 statsmodels 可比，使用 `LogisticRegression(penalty=None)`。

## 完全可分的数据

忽略拟合系数的准确性的好处在于，即使系数不准确或完全错误，模型仍然可能足够好！正如谚语所说，“不要让完美成为优秀的敌人”。

scikit-learn 默认使用与 statsmodels 不同的拟合方法（称为 “[lbfgs](https://en.wikipedia.org/wiki/Limited-memory_BFGS)”）。在我非常有限的测试中，lbfgs 在完全分离的数据上没有出现错误。为了查看这里发生了什么，让我们运行之前用于 statsmodels 的两个相同数据集：

```py
sklearn_logistic_fit(perfect_sep_df)

#> beta_1 or log-odds-ratio:  22.65352734156491
#> beta_0 or y intercept:  -53.25978472434583
#> Original outcomes: 
#> [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
#> Predicted outcomes: 
#> [0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]

sklearn_logistic_fit(perfect_sep2_df)

#> beta_1 or log-odds-ratio:  180.11386979983104
#> beta_0 or y intercept:  -469.3714808877968
#> Original outcomes: 
#> [0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
#> Predicted outcomes: 
#> [0 0 0 0 0 0 0 0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1]
```

该算法在某个点也会停止，给出有限的 *β₁*（或 *k*）值，没有错误，但仍然能够预测正确的结果。这些都是用于分类的完全可用的模型拟合！

# 对数几率和未解决的问题

还有一个我们没有深入讨论的话题。系数 *k*（或 *β₁*）乘以 *x*，有另一种解释——*对数几率比*。几率比描述了当 *x* 增加 1 时几率的乘法变化：

![](../Images/e8f72fc1b29d7d37a5de1eee98f6643a.png)

这个定义表明**赔率比是概率比的比率**。如果将 *x* 增加一个单位，使得 *y = 1* 的概率从 0.1（赔率 0.1 / 0.9 = 0.11）增加到 0.2（赔率 = 0.2 / 0.8 = 0.25），则赔率比为 0.25 / 0.1 = 2.27。

当概率很小时，大的赔率比并不反映大的绝对概率。如果将 *x* 增加一个单位，使得 *y = 1:*

+   从 *p(x) = 0.0001*（赔率 = 0.0001 / (1–0.0001) ≈ 0.0001）

+   到 *p(x + 1) = 0.001*（赔率 = 0.001 / (1–0.001) ≈ 0.001）

那么这给出了赔率比为 0.001/0.0001 = 10。即使赔率比为 10，最终的绝对概率仍然很小：0.001。好消息是**当概率很小时，赔率比变得更容易解释**——它大致等于两个概率 *p(x + 1) / p(x)* 的比率。

将此与另一个例子进行比较，其中概率大幅增加，例如从 *p(x) = 0.5* 增加到 *p(x + 1) = 0.75*，在这种情况下赔率比仅为 3！

因此，当 *y = 1* 的概率很小的时候，高赔率比需要谨慎使用。还有其他可替代的赔率比，例如 [相对风险](https://www.statology.org/interpret-relative-risk/)，这些方法更具可解释性，但计算可能不简单。

# 结论

在前两篇文章中，我们对逻辑回归进行了直观的解释，并展示了如何在 Python 的 statsmodels 和 scikit-learn 库中运行回归模型。

关键要点：

+   逻辑回归输出一个概率——因此它是一种回归算法。

+   逻辑回归通常用于分类。例如，通过预测具有更大概率的结果——或者通过设置自定义概率阈值。

+   参数通过数据之间的差异和两个**交叉曲棍球棒对数损失曲线作为成本函数**的差异来进行数值估计。

+   当你对统计推断感兴趣（关注系数的准确性、假设检验、p 值等）时，使用 **statsmodels** 进行逻辑回归。

+   当你只是想预测结果或需要运行更大模型时，使用 **scikit-learn** 进行逻辑回归。

+   完美可分的数据破坏了统计推断：最佳拟合系数不存在或“是无限的”，而 p 值将很大（因为置信区间很大）。如果我们只关心使用模型进行分类/预测，那么 scikit-learn 的实现对完美分离的数据效果更好。

+   **不要盲目相信大的（对数）赔率比**。考虑计算额外的指标，如绝对概率。

希望这对你的数据科学之旅有所帮助！
