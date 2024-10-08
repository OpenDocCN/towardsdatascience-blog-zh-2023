# 超越流失预测和流失提升

> 原文：[`towardsdatascience.com/beyond-churn-prediction-and-churn-uplift-45225e5a7541`](https://towardsdatascience.com/beyond-churn-prediction-and-churn-uplift-45225e5a7541)

## [因果数据科学](https://towardsdatascience.com/tagged/causal-data-science)

## 如何在存在流失的情况下最佳地针对政策

[](https://medium.com/@matteo.courthoud?source=post_page-----45225e5a7541--------------------------------)![Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----45225e5a7541--------------------------------)[](https://towardsdatascience.com/?source=post_page-----45225e5a7541--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----45225e5a7541--------------------------------) [Matteo Courthoud](https://medium.com/@matteo.courthoud?source=post_page-----45225e5a7541--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----45225e5a7541--------------------------------) ·阅读时间 12 分钟·2023 年 7 月 25 日

--

![](img/af36704bf3fd645df857033ca3b5e3a6.png)

封面，图片由作者提供

数据科学中的一个非常常见的任务是*流失预测*。然而，预测流失往往只是一个中间步骤，很少是最终目标。通常，我们实际关心的是**减少流失**，这是一个独立的目标，不一定相关。事实上，例如，知道长期客户比新客户更不容易流失并不是一个可操作的见解，因为我们无法增加客户的留存时间。我们想知道的是某一（或多种）措施如何影响流失。这通常被称为**流失提升**。

在本文中，我们将**超越**流失预测和流失提升，转而考虑流失预防活动的终极目标：**增加收入**。首先，减少流失的政策可能也会对收入产生影响，这应予以考虑。然而，更重要的是，增加收入只有在客户不流失的情况下才是相关的。相反，减少流失对高收入客户更为相关。这种流失与收入之间的**互动**在理解任何措施活动的盈利性时至关重要，不应被忽视。

# 礼品和订阅

在接下来的部分中，我们将使用一个**玩具示例**来说明主要观点。假设我们是一家希望减少客户流失并最终增加收入的公司。假设我们决定测试一个新想法：向用户发送*1 美元*的**礼品**。为了测试这一措施是否有效，我们随机地仅将其发送给我们的客户基础中的一部分样本。

```py
cost = 1
```

让我们看看我们手头的数据。我从`src.dgp`导入数据生成过程`dgp_gift()`。我还从`src.utils`导入了一些绘图函数和库。

```py
from src.utils import *
from src.dgp import dgp_gift

dgp = dgp_gift(n=100_000)
df = dgp.generate_data()
df.head()
```

![](img/60c257533f7eaad8407d6061856721b5.png)

数据快照，图片由作者提供

我们有`100_000`个客户的信息，我们观察到他们作为活跃客户的`months`数量、上个月他们产生的收入（`rev_old`）、上个月与之前一个月的收入变化（`rev_change`）、他们是否被随机送了`gift`以及两个感兴趣的结果：`churn`，即他们是否不再是活跃客户，以及他们在当前月产生的`revenue`。我们用字母*Y*表示结果，用字母*W*表示处理，用字母*X*表示其他变量。

```py
Y = ['churn', 'revenue']
W = 'gift'
X = ['months', 'rev_old', 'rev_change']
```

**注意**：为了简化起见，我们考虑数据的单期快照，并仅用几个变量总结数据的面板结构。通常，我们会有更长的时间序列，但对于结果（例如，[客户终身价值](https://en.wikipedia.org/wiki/Customer_lifetime_value)）来说，时间范围也会更长。

我们可以用以下**有向无环图（DAG）**来表示潜在的数据生成过程。节点代表变量，箭头代表潜在的因果关系。我用绿色标出了两个感兴趣的关系：`gift`对`churn`和`revenue`的影响。请注意，`churn`与 revenue 相关，因为流失的客户根据定义不会产生收入。

![](img/b590f0d1f8557ac05319968b4a22c5cc.png)

数据生成过程的 DAG，图片由作者提供

重要的是，过去的收入和收入变化是`churn`和`revenue`的预测因子，但与我们的干预无关。相反，干预根据客户的总活跃`months`对`churn`和`revenue`有不同的影响。

尽管简单，这个数据生成过程旨在捕捉一个重要的**洞察**：预测`churn`或`revenue`的变量不一定是预测`churn`或`revenue`**提升**的变量。稍后我们将看到这如何影响我们的分析。

首先，让我们开始探索数据。

## 探索性数据分析

让我们从`churn`开始。公司上个月流失了多少客户？

```py
df.churn.mean()
```

```py
0.19767
```

公司上个月几乎*损失了 20%*的客户！`gift`是否有助于防止流失？

我们想要比较收到礼物的客户的流失频率与未收到礼物的客户的流失频率。由于礼物是随机发放的，因此均值差异估计量是`gift`对`churn`的[平均处理效应（ATE）](https://en.wikipedia.org/wiki/Average_treatment_effect)的无偏估计量。

![](img/a7cb2d5b9e5312b682fcea01b4346383.png)

平均处理效应，图片由作者提供

我们通过线性回归计算均值差异估计。我们还包括其他协变量以提高估计器的效率。

```py
smf.ols("churn ~ " + W + " + " + " + ".join(X), data=df).fit().summary().tables[1]
```

![](img/b6cb632b38aeb03ba248537b61fe1497.png)

流失回归表，图片来源：作者

看起来`gift`将流失率降低了大约*11*个百分点，即几乎是基准水平*32%*的三分之一！它是否对`revenue`也有影响？

至于流失率，我们将`revenue`对`gift`，即我们的处理变量，进行回归，以估计平均处理效果。

```py
smf.ols("revenue ~ " + W + " + " + " + ".join(X), data=df).fit().summary().tables[1]
```

![](img/c6b3d83f32af833c18ccf477978cb6e8.png)

收入回归表，图片来源：作者

看起来`gift`平均增加的收入为*0.63$*，这意味着它**并不盈利**。这是否意味着我们应该停止向客户赠送礼物？这要看情况。事实上，礼物可能对某些客户群体有效。我们只需要识别这些客户群体。

# 定向策略

在本节中，我们将尝试通过针对特定客户来了解是否存在数据驱动的盈利发送`gift`的方法。特别是，我们将**比较**不同的目标**策略**，目的是增加收入。

在本节中，我们将需要一些算法来预测`revenue`或`churn`，或接收`gift`的概率。我们使用来自`[lightgbm](https://lightgbm.readthedocs.io/en/latest/index.html)`库的梯度提升树模型。我们对所有策略使用相同的模型，以便我们无法将性能差异归因于预测准确性。

```py
from lightgbm import LGBMClassifier, LGBMRegressor
```

要**评估**每项政策，我们希望在单独的验证数据集中，将有政策的隐含利润*Π⁽¹⁾*与没有政策的隐含利润*Π⁽⁰⁾*进行比较。我们称这两个量为**潜在结果**，它们的差异为**利润提升**，用*τ*表示。请注意，提升从未被观察到，因为对于每个客户，我们只能观察到两种**潜在结果**之一，即有或没有`gift`。然而，由于我们使用的是合成数据，我们可以进行预言评估。如果你想了解如何用真实数据评估提升模型，我推荐我的入门文章。

[](/8a078996a113?source=post_page-----45225e5a7541--------------------------------) ## 评估提升模型

### 行业中因果推断的最广泛应用之一是提升建模，也称为估计...

towardsdatascience.com

首先，让我们定义利润*Π*为当客户不流失时的净收入*R*。

![](img/3d61704184736c560d209e823c3cba24.png)

利润公式，图片来源：作者

因此，对于处理过的个体，总体利润效应由两个假设量的差异给出：处理时的利润*Π⁽¹⁾*减去未处理时的利润*Π⁽⁰⁾*。

![](img/db1fbbbaabcafb973a46e9fd51dbe5ce.png)

盈利提升公式，作者提供的图像

对未处理个体的效果为零。

```py
def evaluate_policy(policy):
    data = dgp.generate_data(seed_data=4, seed_assignment=5, keep_po=True)
    data['profits'] = (1 - data.churn) * data.revenue
    baseline = (1-data.churn_c) * data.revenue_c
    effect = policy(data) * (1-data.churn_t) * (data.revenue_t-cost) + (1-policy(data)) * (1-data.churn_c) * data.revenue_c
    return np.sum(effect - baseline)
```

## 1\. 针对流失客户

第一个策略可能是仅针对**流失客户**。假设我们只将`礼物`发送给预测流失率高于平均水平的客户。

```py
model_churn = LGBMClassifier().fit(X=df[X], y=df['churn'])

policy_churn = lambda df : (model_churn.predict_proba(df[X])[:,1] > df.churn.mean())
evaluate_policy(policy_churn)
```

```py
-5497.46
```

该策略没有盈利，并且会导致总计**亏损**超过*5000$*。

你可能认为问题在于**任意阈值**，但事实并非如此。下面我绘制了所有可能策略阈值的总体效果。

```py
x = np.linspace(0, 1, 100)
y = [evaluate_policy(lambda df : (model_churn.predict_proba(df[X])[:,1] > p)) for pin x]

fig, ax = plt.subplots(figsize=(10, 3))
sns.lineplot(x=x, y=y).set(xlabel='Churn Policy Threshold', title='Aggregate Effect');
ax.axhline(y=0, c='k', lw=3, ls='--');
```

![](img/8068de4f2278ce28dbea3051e2afcadd.png)

按流失阈值的总体效果，作者提供的图像

正如我们所见，无论阈值如何，基本上都不可能获得任何利润。

问题在于，客户可能流失并不意味着`礼物`会对他们的流失概率产生任何影响。这两个度量并不是完全无关的（例如，我们不能降低那些流失概率为 0%的客户的流失概率），但它们并不是同一回事。

## 2\. 针对收益客户

现在我们尝试另一种策略：我们只将礼物发送给**高收益客户**。例如，我们可以只将礼物发送给按收益排名前 10%的客户。这个想法是，如果该策略确实能降低流失，那么这些客户就是那些降低流失最具盈利性的客户。

```py
model_revenue = LGBMRegressor().fit(X=df[X], y=df['revenue'])

policy_revenue = lambda df : (model_revenue.predict(df[X]) > np.quantile(df.revenue, 0.9))
evaluate_policy(policy_revenue)
```

```py
-4730.82
```

该策略再次没有盈利，导致了相当大的**亏损**。如之前所示，这不是选择阈值的问题，正如下图所示。我们能做的最好的是设置一个很高的阈值，这样我们不处理任何人，从而实现零利润。

```py
x = np.linspace(0, 100, 100)
y = [evaluate_policy(lambda df : (model_revenue.predict(df[X]) > c)) for c in x]

fig, ax = plt.subplots(figsize=(10, 3))
sns.lineplot(x=x, y=y).set(xlabel='Revenue Policy Threshold', title='Aggregate Effect');
ax.axhline(y=0, c='k', lw=3, ls='--');
```

![](img/de2a1e6a19f72c4c37f19ce64772cc3d.png)

按收益阈值的总体效果，作者提供的图像

问题在于，在我们的设置中，高收益客户的流失概率并没有下降到足以使`礼物`有盈利的程度。这部分也是因为现实中常观察到的情况，即高收益客户本身就是流失可能性最小的客户。

现在让我们考虑一组更相关的策略：基于**提升**的策略。

## 3\. 针对流失提升客户

更合理的方法是针对那些在接受*1$* `礼物` 时，`流失` 概率降低最多的客户。我们使用[双重稳健估计器](https://arxiv.org/abs/2004.14497)来估计流失提升，这是表现最好的提升模型之一。如果你对元学习者不熟悉，我建议从我的介绍文章开始。

[## 理解元学习者](https://example.org/8a9c1e340832?source=post_page-----45225e5a7541--------------------------------)

### 在许多情况下，我们不仅仅关心估计因果效应，还关心这个效应是否...

towardsdatascience.com

我们从 [econml](https://econml.azurewebsites.net/) 导入双重稳健学习器，这是一个微软库。

```py
from econml.dr import DRLearner

DR_learner_churn = DRLearner(model_regression=LGBMRegressor(), model_propensity=LGBMClassifier(), model_final=LGBMRegressor())
DR_learner_churn.fit(df['churn'], df[W], X=df[X]);
```

既然我们已经估计了客户流失的提升，我们可能会倾向于只针对那些具有高负面提升的客户（负面，因为我们想要*减少*流失）。例如，我们可能会将`礼物`发送给所有估计流失高于平均水平的客户。

```py
policy_churn_lift = lambda df : DR_learner_churn.effect(df[X]) < - np.mean(df.churn)
evaluate_policy(policy_churn_lift)
```

```py
-3925.24
```

该政策仍然不盈利，导致了接近*4000$*的损失。

问题在于我们没有考虑政策的成本。实际上，降低流失概率**仅对高收入客户**有利。以极端情况为例：避免流失一个不产生任何收入的客户是没有价值的干预。

因此，我们只将`礼物`发送给那些其流失概率加权收入下降幅度大于礼物成本的客户。

```py
model_revenue_1 = LGBMRegressor().fit(X=df.loc[df[W] == 1, X], y=df.loc[df[W] == 1, 'revenue'])

policy_churn_lift = lambda df : - DR_learner_churn.effect(df[X]) * model_revenue_1.predict(df[X]) > cost
evaluate_policy(policy_churn_lift)
```

```py
318.03
```

这一政策最终是盈利的！

然而，我们仍然没有考虑到一个渠道：干预也可能会影响现有客户的收入。

## 4\. 目标收入提升客户

与前一种方法对称的方法是只考虑对`收入`的影响，忽略对流失的影响。我们可以估计非流失客户的`收入`提升，只处理那些在流失后对收入的增量效果大于`礼物`成本的客户。

```py
DR_learner_netrevenue = DRLearner(model_regression=LGBMRegressor(), model_propensity=LGBMClassifier(), model_final=LGBMRegressor())
DR_learner_netrevenue.fit(df.loc[df.churn==0, 'revenue'], df.loc[df.churn==0, W], X=df.loc[df.churn==0, X]);
model_churn_1 = LGBMClassifier().fit(X=df.loc[df[W] == 1, X], y=df.loc[df[W] == 1, 'churn'])

policy_netrevenue_lift = lambda df : DR_learner_netrevenue.effect(df[X]) * (1-model_churn_1.predict(df[X])) > cost
evaluate_policy(policy_netrevenue_lift)
```

```py
50.80
```

这一政策也有利可图，但忽略了对流失的影响。我们如何将这一政策与之前的政策结合起来？

## 5\. 目标收入提升客户

高效**结合**对流失和对净收入影响的最佳方法就是估计**总收入提升**。隐含的最佳政策是处理那些总收入提升大于`礼物`成本的客户。

```py
DR_learner_revenue = DRLearner(model_regression=LGBMRegressor(), model_propensity=LGBMClassifier(), model_final=LGBMRegressor())
DR_learner_revenue.fit(df['revenue'], df[W], X=df[X]);

policy_revenue_lift = lambda df : (DR_learner_revenue.effect(df[X]) > cost)
evaluate_policy(policy_revenue_lift)
```

```py
2028.21
```

这一政策迄今为止表现最好，产生了超过*2000$*的总利润！

```py
policies = [policy_churn, policy_revenue, policy_churn_lift, policy_netrevenue_lift, policy_revenue_lift] 
df_results = pd.DataFrame()
df_results['policy'] = ['churn', 'revenue', 'churn_L', 'netrevenue_L', 'revenue_L']
df_results['value'] = [evaluate_policy(policy) for policy in policies]

fig, ax = plt.subplots()
sns.barplot(df_results, x='policy', y='value').set(title='Overall Incremental Effect')
plt.axhline(0, c='k');
```

![](img/df577aa7e5ec99f0697bbecb61e040a7.png)

比较政策，图像由作者提供

## 直觉与分解

如果我们比较不同的政策，很明显，针对高收入或高流失概率客户是**最糟糕的选择**。这并不总是如此，但在我们的模拟数据中发生了这种情况，因为有两个在许多实际场景中也很常见的事实：

1.  收入与流失概率呈负相关

1.  `礼物`对`流失`（或`收入`）的影响与基准值并没有强烈的负相关（或对`收入`的正相关）

这些事实中的任何一个都足以使得以收入或流失为目标的策略变得不佳。应该关注的是那些具有高**增量**效果的客户。而且，最好直接使用感兴趣的变量，即在这种情况下的`收入`，只要有数据。

为了更好地理解机制，我们可以**分解**政策对利润的整体影响为三个部分。

![](img/1ae2e768dfbeed0989162376ccf47118.png)

利润提升分解，图像由作者提供

这意味着有**三个渠道**使得对客户的处理具有盈利性。

1.  如果这是一个*高收入*客户并且处理*减少*了其流失概率

1.  如果这是一个*非流失*客户并且处理*增加*了其收入

1.  如果处理对*其收入*和流失概率*都有*强影响

通过流失提升进行的目标定位仅利用第一个渠道，通过净收入提升进行的目标定位仅利用第二个渠道，而通过总收入提升进行的目标定位利用所有三个渠道，使其成为**最有效**的方法。

## 奖金：加权

如[Lemmens, Gupta (2020)](https://www.hbs.edu/ris/Publication%20Files/14-020_2d6c9da0-94d3-4dd5-9952-d81feb432f61.pdf)所强调，有时在估计提升时可能值得对观察值进行**加权**。特别是，可能值得对接近处理政策阈值的观察值给予更多权重。

**观点**是，加权通常会降低估计量的效率。然而，我们并不关心对所有观察值获得正确的估计，而是关心正确估计**政策阈值**。实际上，无论你估计*1$*还是*1000$*的净利润都无关紧要：隐含的政策是相同的：发送`礼物`。然而，估计*1$*的净利润而不是*-1$*会颠倒政策的含义。因此，距离阈值的准确性大幅下降有时值得在阈值处的小幅提高准确性。

让我们尝试使用负指数权重，距离阈值越远权重越低。

```py
DR_learner_revenue_w = DRLearner(model_regression=LGBMRegressor(), model_propensity=LGBMClassifier(), model_final=LGBMRegressor())
w = np.exp(1 + np.abs(DR_learner_revenue.effect(df[X]) - cost))
DR_learner_revenue_w.fit(df['revenue'], df[W], X=df[X], sample_weight=w);

policy_revenue_lift_w = lambda df : (DR_learner_revenue_w.effect(df[X]) > cost)
evaluate_policy(policy_revenue_lift_w)
```

```py
1398.19
```

在我们的情况下，加权是不值得的：隐含的政策仍然有利可图，但不如未加权模型所获得的*2028$*。

# **结论**

在这篇文章中，我们探讨了为什么以及如何应当**超越**流失预测和流失提升建模。特别是，应集中于提高盈利的最终业务目标。这意味着将重点从*预测*转移到*提升*，同时将流失和收入结合成一个结果。

一个重要的警告涉及到数据的维度。我们使用了一个玩具数据集，这在至少**两个维度**上高度简化了问题。首先，回溯，我们通常有更长时间的时间序列，这些时间序列可以（并且应该）用于预测和建模目的。其次，前瞻，应该将流失与长期的客户盈利估计相结合，通常称为*客户生命周期价值*。

## 参考文献

+   Kennedy (2022), [“朝着最优双重稳健估计异质因果效应”](https://arxiv.org/abs/2004.14497)

+   Bonvini, Kennedy, Keele (2021), [“最小最大最优子组识别”](https://arxiv.org/abs/2306.17464)

+   Lemmens, Gupta (2020), [“管理流失以最大化利润”](https://www.hbs.edu/ris/Publication%20Files/14-020_2d6c9da0-94d3-4dd5-9952-d81feb432f61.pdf)

## 相关文章

+   评估提升模型

+   理解元学习者

+   理解 AIPW，即双重稳健估计器

## Code

你可以在这里找到原始的 Jupyter Notebook：

[## Blog-Posts/notebooks/beyond_churn.ipynb at main · matteocourthoud/Blog-Posts](https://github.com/matteocourthoud/Blog-Posts/blob/main/notebooks/beyond_churn.ipynb?source=post_page-----45225e5a7541--------------------------------)

### 这是我在 Medium 博客文章中的代码和笔记本。通过创建一个…来贡献于 matteocourthoud/Blog-Posts 的开发。

[github.com](https://github.com/matteocourthoud/Blog-Posts/blob/main/notebooks/beyond_churn.ipynb?source=post_page-----45225e5a7541--------------------------------)

## 感谢阅读！

*我非常感谢！* 🤗 *如果你喜欢这篇文章并想看到更多内容，请考虑* [***关注我***](https://medium.com/@matteo.courthoud)*。我每周发布一次关于因果推断和数据分析的内容。我尝试保持文章简洁而准确，始终提供代码、示例和模拟。*

*此外，一个小小的* ***免责声明***：我写作是为了学习，因此错误是常有的事，尽管我尽力而为。如果你发现了错误，请告诉我。我也很感激对新主题的建议！
