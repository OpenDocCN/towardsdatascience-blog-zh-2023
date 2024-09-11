# 双重机器学习，简化版：第 2 部分 — 目标设定与 CATE

> 原文：[https://towardsdatascience.com/double-machine-learning-simplified-part-2-extensions-the-cate-99926151cac?source=collection_archive---------2-----------------------#2023-07-31](https://towardsdatascience.com/double-machine-learning-simplified-part-2-extensions-the-cate-99926151cac?source=collection_archive---------2-----------------------#2023-07-31)

![](../Images/6eb4d0930e2bb0f3820e0379c8f07e34.png)

## 学习如何利用 DML 估计特定的治疗效果，以实现数据驱动的目标设定

[](https://medium.com/@jakepenzak?source=post_page-----99926151cac--------------------------------)[![Jacob Pieniazek](../Images/2d9c6295d39fcaaec4e62f11c359cb29.png)](https://medium.com/@jakepenzak?source=post_page-----99926151cac--------------------------------)[](https://towardsdatascience.com/?source=post_page-----99926151cac--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----99926151cac--------------------------------) [Jacob Pieniazek](https://medium.com/@jakepenzak?source=post_page-----99926151cac--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f0948d99b1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdouble-machine-learning-simplified-part-2-extensions-the-cate-99926151cac&user=Jacob+Pieniazek&userId=6f0948d99b1c&source=post_page-6f0948d99b1c----99926151cac---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----99926151cac--------------------------------) · 10 分钟阅读 · 2023 年 7 月 31 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F99926151cac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdouble-machine-learning-simplified-part-2-extensions-the-cate-99926151cac&user=Jacob+Pieniazek&userId=6f0948d99b1c&source=-----99926151cac---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F99926151cac&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdouble-machine-learning-simplified-part-2-extensions-the-cate-99926151cac&source=-----99926151cac---------------------bookmark_footer-----------)

> 本文是关于简化和民主化双重机器学习的**第二篇**文章。在[第一部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee)，我们介绍了双重机器学习的基础知识，以及两个基本的因果推断应用。现在，在第二部分中，我们将扩展这些知识，将我们的因果推断问题转变为预测任务，其中我们预测个体级别的治疗效应，以帮助决策和数据驱动的目标设定。

如我们在[本系列第一部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee)中所学，双重机器学习是一种高度灵活的部分线性因果推断方法，用于估计治疗的平均治疗效应（ATE）。具体来说，它可以用于建模观察数据中高度非线性的混杂关系（尤其是当我们的控制/混杂变量集合具有极高的维度时）和/或在实验设置中减少关键结果的变异性。估计 ATE 对于理解特定治疗的平均影响尤为有用，这对未来的决策非常重要。然而，外推这一治疗效应假设效应的同质性；也就是说，无论我们将治疗推广到哪个人群，我们预期效应将与 ATE 相似。如果我们在未来推广中能够针对的个体数量有限，因此希望了解哪些子群体的治疗效果最为显著，从而驱动高效的推广，该怎么办？

上述问题涉及估计治疗效应的异质性。也就是说，我们的治疗效应如何影响不同的群体？幸运的是，DML 提供了一个强大的框架来实现这一点。具体来说，我们可以利用 DML 来估计条件平均治疗效应（CATE）。首先，让我们重新审视一下 ATE 的定义：

![](../Images/595346cd86ae3d2dccd2d8172696ce3f.png)

(1) 平均治疗效应

现在有了 CATE，我们可以在一组协变量**X**的条件下估计 ATE：

![](../Images/3b305cea9d99b947be51a804dd21028b.png)

(2) 条件平均治疗效应

例如，如果我们想知道男性与女性的治疗效果，我们可以在条件变量等于每个感兴趣的子群体时估计 CATE。请注意，我们可以估计高度聚合的 CATE（即男性与女性层面），也可以允许 **X** 具有极高的维度，从而精确估计每个人的治疗效果。你可能会立即注意到这样做的好处：我们可以利用这些信息在未来的治疗目标上做出高度明智的决策！ *更值得注意的是，我们可以创建一个 CATE 函数，预测我们对以前* ***未接触过的*** *个体的治疗效果的预测！*

DML 提供了两种主要的方法来估计 CATE 函数，即线性 DML 和非参数 DML。我们将展示如何从数学上估计 CATE，然后为每种情况提供示例。

> **注意：** CATE 的无偏估计仍然需要 exogeneity/CIA/Ignorability 假设成立，如在 [第 1 部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee)中所述。
> 
> 下面演示的所有内容都可以并且应该扩展到实验设置（RCT 或 A/B 测试），其中通过构造满足 exogeneity，如在 [第 1 部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee) 的应用 2 中所述。

## 线性 DML 估计 CATE

在线性 DML 框架下估计 CATE 是对 DML 的一种简单扩展，类似于在 [第 1 部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee)中对 ATE 的估计：

![](../Images/ff40e6bdeac9222544390e90e9b8ed1a.png)

(3) DML 估计 ATE

其中 ***y*** 是我们的结果，***T*** 是我们的治疗，& 𝑀𝑦 和 ***MT*** 是两个灵活的机器学习模型（我们的干扰函数），用于在给定混杂因素和/或控制变量 ***X*** 的情况下预测 ***y*** 和 ***T***。要使用线性 DML 估计 CATE 函数，我们可以简单地包括治疗残差与协变量的交互项。观察：

![](../Images/174272210fa7fd5219b00dee168f1c53.png)

(4) 线性 DML 估计 CATE

其中 **Ω** 是交互项系数的向量。现在我们的 CATE 函数，称之为 *τ*，具有形式 *τ*(**X**) = β₁ + **XΩ**，在给定 **X** 的情况下，我们可以预测每个个体的 CATE。如果 T 是连续的，则此 CATE 函数用于 T 的 1 单位增加。请注意，*τ(****X****) =* β₁ 在公式 (3) 中，其中 *τ(****X****)* 被假定为常数。让我们看看实际应用！

首先，让我们使用来自[第一部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee)的相同因果 DAG，我们将研究个人在网站上花费的时间对其过去一个月购买金额或销售额的影响（*假设我们观察到所有混杂因素*）：

![](../Images/53ff709528289a0762c49af8579e9f7a.png)

那么我们接下来将使用与[第一部分](https://medium.com/towards-data-science/double-machine-learning-simplified-part-1-basic-causal-inference-applications-3f7afc9852ee)中类似的过程来模拟这个 DFP（*请注意，所有值和数据都是为演示目的而任意选择和生成的，因此可能并不代表我们的 CATE 估计之外的大部分实际世界直觉*）。请注意，我们现在在销售 DGP 中包含了交互项来建模 CATE 或处理效果的异质性（*请注意，第一部分中的 DGP 在构建时没有处理效果异质性*）：

```py
import numpy as np
import pandas as pd

# Sample Size
N = 100_000

# Confounders (X)
age = np.random.randint(low=18,high=75,size=N)
num_social_media_profiles = np.random.choice([0,1,2,3,4,5,6,7,8,9,10], size = N)
yr_membership = np.random.choice([0,1,2,3,4,5,6,7,8,9,10], size = N)

# Arbitrary Covariates (Z)
Z = np.random.normal(loc=50, scale = 25, size = N)

# Error Terms
ε1 = np.random.normal(loc=20,scale=5,size=N)
ε2 = np.random.normal(loc=40,scale=15,size=N)

# Treatment (T = g(X) + ε1)
time_on_website = np.maximum(10
                             - 0.01*age 
                             - 0.001*age**2 
                             + num_social_media_profiles 
                             - 0.01 * num_social_media_profiles**2
                             - 0.01*(age * num_social_media_profiles)
                             + 0.2 * yr_membership
                             + 0.001 * yr_membership**2
                             - 0.01 * (age * yr_membership)
                             + 0.2 * (num_social_media_profiles * yr_membership)
                             + 0.01 * (num_social_media_profiles * np.log(age) * age * yr_membership**(1/2))
                             + ε1
                               ,0)

# Outcome (y = f(T,X,Z) + ε2)
sales = np.maximum(25
                   +  5 * time_on_website # Baseline Treatment Effect
                   -  0.2 * time_on_website * age # Heterogeneity
                   + 2 * time_on_website * num_social_media_profiles # Heterogeneity
                   + 2 * time_on_website * yr_membership # Heterogeneity
                   - 0.1*age 
                   - 0.001*age**2 
                   + 8 * num_social_media_profiles 
                   - 0.1 * num_social_media_profiles**2
                   - 0.01*(age * num_social_media_profiles)
                   + 2 * yr_membership
                   + 0.1 * yr_membership**2
                   - 0.01 * (age * yr_membership)
                   + 3 * (num_social_media_profiles * yr_membership)
                   + 0.1 * (num_social_media_profiles * np.log(age) * age * yr_membership**(1/2))
                   + 0.5 * Z
                   + ε2
                     ,0)

df = pd.DataFrame(np.array([sales,time_on_website,age,num_social_media_profiles,yr_membership,Z]).T
                  ,columns=["sales","time_on_website","age","num_social_media_profiles","yr_membership","Z"])
```

现在，为了估计我们的 CATE 函数，如等式 (4) 中所述，我们可以运行：

```py
import statsmodels.formula.api as smf
from sklearn.ensemble import GradientBoostingRegressor
from sklearn.model_selection import cross_val_predict

# DML Procedure for Estimating the CATE
M_sales = GradientBoostingRegressor()
M_time_on_website = GradientBoostingRegressor()

df[‘residualized_sales’] = df["sales"] - cross_val_predict(M_sales, df[["age","num_social_media_profiles","yr_membership"]], df[‘sales’], cv=3)
df[‘residualized_time_on_website’] = df[‘time_on_website’] - cross_val_predict(M_time_on_website, df[["age","num_social_media_profiles","yr_membership"]], df[‘time_on_website’], cv=3)

DML_model = smf.ols(formula='residualized_sales ~ 1 + residualized_time_on_website + residualized_time_on_website:age + residualized_time_on_website:num_social_media_profiles + residualized_time_on_website:yr_membership', data = df).fit()
print(DML_model.summary())
```

得到如下结果：

![](../Images/c8ea093a7a3cbe932d91e2a3e9cd9b8d.png)

在这里，我们可以看到线性 DML 逼近了 CATE 的真实 DGP（参见销售 DGP 中的交互项系数）。让我们通过将线性 DML 预测值与增加 1 小时网站停留时间的真实 CATE 进行比较，来评估我们 CATE 函数的表现：

```py
from sklearn.metrics import mean_squared_error, r2_score, mean_absolute_error

# Predict CATE of 1 hour increase 
df_predictions = df[['residualized_time_on_website','age','num_social_media_profiles','yr_membership']].copy()
df_predictions['linear_DML_CATE']= (DML_model.predict(df_predictions.assign(residualized_time_on_website= lambda x : x.residualized_time_on_website + 1)) 
                                    - DML_model.predict(df_predictions))

# True CATE of 1 hour increase
df_predictions['true_CATE'] = 5 - 0.2 * df_predictions['age'] + 2 * df_predictions['num_social_media_profiles'] + 2 * df_predictions['yr_membership']

# Performance Metrics
mean_squared_error(df_predictions['true_CATE'], df_predictions['linear_DML_CATE'])
mean_absolute_error(df_predictions['true_CATE'], df_predictions['linear_DML_CATE'])
r2_score(df_predictions['true_CATE'], df_predictions['linear_DML_CATE'])
```

在这里，我们得到了约 0.45 的均方误差（MSE），约 0.55 的平均绝对误差（MAE），以及约 0.99 的决定系数（R2）。通过绘制预测 CATE 和真实 CATE 的分布，我们得到：

![](../Images/31f0438f39ed311f178f70a63dbf3c3c.png)

此外，通过绘制预测值与真实值的关系，我们得到：

![](../Images/1bad1bdb88ac3d54becf71b7e122b1ea.png)

总体来说，我们的表现非常令人印象深刻！然而，这种方法的主要局限性在于我们必须手动指定 CATE 函数的功能形式，因此如果我们仅包含线性交互项，可能无法捕捉到真实的 CATE 函数。在我们的例子中，我们模拟了 DGP 以仅包含这些线性交互项，因此性能强劲 **按构建**，但让我们看看当我们将 CATE 的 DGP 随意调整为非线性时会发生什么：

```py
# Outcome (y = f(T,X,Z) + ε2)
sales = np.maximum( 25
                   +  5 * time_on_website # Baseline Treatment Effect
                   -  0.2 * time_on_website * age # Heterogeneity
                   - 0.0005 * time_on_website * age**2 # Heterogeneity
                   + 0.8 * time_on_website * num_social_media_profiles # Heterogeneity
                   + 0.001 * time_on_website * num_social_media_profiles**2 # Heterogeneity
                   + 0.8 * time_on_website * yr_membership # Heterogeneity
                   + 0.001 * time_on_website * yr_membership**2 # Heterogeneity
                   + 0.005 * time_on_website * yr_membership * num_social_media_profiles * age # Heterogeneity
                   + 0.005 * time_on_website * (yr_membership**3 / (1 + num_social_media_profiles**2)) * np.log(age) ** 2 # Heterogeneity
                   - 0.1*age 
                   - 0.001*age**2 
                   + 8 * num_social_media_profiles 
                   - 0.1 * num_social_media_profiles**2
                   - 0.01*(age * num_social_media_profiles)
                   + 2 * yr_membership
                   + 0.1 * yr_membership**2
                   - 0.01 * (age * yr_membership)
                   + 3 * (num_social_media_profiles * yr_membership)
                   + 0.1 * (num_social_media_profiles * np.log(age) * age * yr_membership**(1/2))
                   + 0.5 * Z
                   + ε2
                     ,0)

df = pd.DataFrame(np.array([sales,time_on_website,age,num_social_media_profiles,yr_membership,Z]).T
                  ,columns=["sales","time_on_website","age","num_social_media_profiles","yr_membership","Z"])

# DML Procedure
M_sales = GradientBoostingRegressor()
M_time_on_website = GradientBoostingRegressor()

df[‘residualized_sales’] = df["sales"] - cross_val_predict(M_sales, df[["age","num_social_media_profiles","yr_membership"]], df[‘sales’], cv=3)
df['residualized_time_on_website'] = df[‘time_on_website’] - cross_val_predict(M_time_on_website, df[["age","num_social_media_profiles","yr_membership"]], df[‘time_on_website’], cv=3)

DML_model = smf.ols(formula='residualized_sales ~ 1 + residualized_time_on_website + residualized_time_on_website:age + residualized_time_on_website:num_social_media_profiles + residualized_time_on_website:yr_membership', data = df).fit()

# Predict CATE of 1 hour increase
df_predictions = df[['residualized_time_on_website','age','num_social_media_profiles','yr_membership']].copy()
df_predictions['linear_DML_CATE']= (DML_model.predict(df_predictions.assign(residualized_time_on_website= lambda x : x.residualized_time_on_website + 1)) 
                                    - DML_model.predict(df_predictions))

# True CATE of 1 hour increase
df_predictions['true_CATE'] = (5 - 0.2*df_predictions['age'] - 0.0005*df_predictions['age']**2 + 0.8*df_predictions['num_social_media_profiles'] + 0.001*df_predictions['num_social_media_profiles']**2 
                   + 0.8*df_predictions['yr_membership'] + 0.001*df_predictions['yr_membership']**2 + 0.005*df_predictions['yr_membership']*df_predictions['num_social_media_profiles']*df_predictions['age']
                   + 0.005 * (df_predictions['yr_membership']**3 / (1 + df_predictions['num_social_media_profiles']**2)) * np.log(df_predictions['age'])**2)

# Performance Metrics
mean_squared_error(df_predictions['true_CATE'], df_predictions['linear_DML_CATE'])
mean_absolute_error(df_predictions['true_CATE'], df_predictions['linear_DML_CATE'])
r2_score(df_predictions['true_CATE'], df_predictions['linear_DML_CATE'])
```

在这里，我们看到性能急剧下降，我们得到了约 55.92 的均方误差（MSE），约 4.50 的平均绝对误差（MAE），以及约 0.65 的决定系数（R2）。通过绘制预测 CATE 和真实 CATE 的分布，我们得到：

![](../Images/e6532b1335fc2b355fd4f617cb061781.png)

此外，通过绘制预测值与真实值的关系，我们得到：

![](../Images/94b329b73623ab360460d9d7078317c9.png)

CATE 函数中的这种非线性正是非参数 DML 可以发光的地方！

## 用于估计 CATE 的非参数 DML

非参数DML更进一步，允许另一个灵活的非参数ML模型用于学习CATE函数！让我们看看如何在数学上准确执行此操作。让 *τ(****X****)* 继续表示我们的CATE函数。让我们从相对于eq. 3定义我们的误差项开始（请注意，我们放弃了截距 β₀，因为我们对于CATE的此参数不感兴趣；在线性DML公式中我们也可以类似地放弃此参数，但为了简单起见，并与第1部分保持一致，我们没有这样做）：

![](../Images/ff0e274d37e759410518f750c82bbd0e.png)

(5) DML框架中的误差

然后定义因果损失函数如下（请注意这只是均方误差！）：

![](../Images/23b208d7d4401f64ac7676339b586054.png)

(6) 因果损失函数

这意味着什么？我们可以通过最小化我们的因果损失函数，直接用任何灵活的ML模型学习 *τ(****X****)* ！这相当于一个带有我们的目标和权重的加权回归问题，分别为：

![](../Images/60c2a9a2f33933b97d33381032c6861b.png)

(7) 非参数DML中的目标与权重

*稍作停顿，沉浸在这一结果的优雅之中…… 我们可以直接学习CATE函数，并预测个体的CATE，给定我们的残差化结果，* ***y****，和处理，* ***T****！*

现在让我们看看这在实际中是如何运作的。我们将重用在上述线性DML表现不佳示例中使用的非线性CATE函数的DGP。为了构建非参数DML模型，我们可以运行：

```py
# Define Target & Weights
df['target'] = df['residualized_sales'] / df['residualized_time_on_website']
df['weights'] = df['residualized_time_on_website']**2

# Non-Parametric CATE Model
CATE_model = GradientBoostingRegressor()
CATE_model.fit(df[["age","num_social_media_profiles","yr_membership"]], df['target'], sample_weight=df['weights'])
```

并且用来预测和评估性能：

```py
# CATE Predictions
df_predictions['Non_Parametric_DML_CATE'] = CATE_model.predict(df[["age","num_social_media_profiles","yr_membership"]])

# Performance Metrics
mean_squared_error(df_predictions['true_CATE'], df_predictions['Non_Parametric_DML_CATE'])
mean_absolute_error(df_predictions['true_CATE'], df_predictions['Non_Parametric_DML_CATE'])
r2_score(df_predictions['true_CATE'], df_predictions['Non_Parametric_DML_CATE'])
```

在这里，我们获得了比线性DML更优越的性能，MSE为4.61，MAE为1.37，R2为0.97。绘制预测CATE和真实CATE的分布，我们得到：

![](../Images/eb3e196328ea71e98e753f625a653215.png)

另外，绘制预测值与真实值的图形，我们得到：

![](../Images/970f099ec85fa61b7e45fbbaa25f1e02.png)

在这里我们可以看到，虽然不完美，但非参数DML方法能够比线性DML方法更好地建模CATE函数中的非线性。当然，我们可以通过调整我们的模型进一步提高性能。请注意，我们可以使用可解释的AI工具，如[SHAP值](https://shap.readthedocs.io/en/latest/index.html)，来理解我们处理效应异质性的性质！

## 结论

至此！感谢您抽出时间阅读我的文章。希望这篇文章教会了您如何超越仅估计ATE并利用DML估计CATE，以进一步理解处理效应的异质性，并推动更多因果推断和数据驱动的定位方案。

一如既往，希望您阅读本文能与我写作时一样愉快！

## 资源

[1] V. Chernozhukov, D. Chetverikov, M. Demirer, E. Duflo, C. Hansen, and a. W. Newey. 双机器学习用于处理和因果参数。*ArXiv电子打印*，2016年7月。

*通过这个 GitHub 仓库访问所有代码:* [https://github.com/jakepenzak/Blog-Posts](https://github.com/jakepenzak/Blog-Posts)

*感谢你阅读我的文章！我在 Medium 上的文章旨在探索利用* ***计量经济学*** *和* ***统计/机器学习*** *技术的现实世界和理论应用。此外，我还希望通过理论和模拟提供关于各种方法论的理论基础的文章。最重要的是，我写作是为了学习和帮助他人学习！我希望使复杂的话题对所有人稍微更易于理解。如果你喜欢这篇文章，请考虑* [***关注我在 Medium 上的账号***](https://medium.com/@jakepenzak)*！*
