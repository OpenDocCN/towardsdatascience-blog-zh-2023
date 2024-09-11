# 解密Cox回归：Cox回归的隐藏黑暗秘密

> 原文：[https://towardsdatascience.com/unbox-the-cox-a-hidden-dark-secret-of-cox-regression-fb432137fd6?source=collection_archive---------9-----------------------#2023-06-27](https://towardsdatascience.com/unbox-the-cox-a-hidden-dark-secret-of-cox-regression-fb432137fd6?source=collection_archive---------9-----------------------#2023-06-27)

## 为什么完美预测因子的p值会为0.93？

[](https://medium.com/@igor-s?source=post_page-----fb432137fd6--------------------------------)[![Igor Šegota](../Images/17c592b71fef9526a0679d47937837f6.png)](https://medium.com/@igor-s?source=post_page-----fb432137fd6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fb432137fd6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fb432137fd6--------------------------------) [Igor Šegota](https://medium.com/@igor-s?source=post_page-----fb432137fd6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe5f8ebca4ad8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funbox-the-cox-a-hidden-dark-secret-of-cox-regression-fb432137fd6&user=Igor+%C5%A0egota&userId=e5f8ebca4ad8&source=post_page-e5f8ebca4ad8----fb432137fd6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fb432137fd6--------------------------------) · 8 min 阅读 · 2023年6月27日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffb432137fd6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funbox-the-cox-a-hidden-dark-secret-of-cox-regression-fb432137fd6&user=Igor+%C5%A0egota&userId=e5f8ebca4ad8&source=-----fb432137fd6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffb432137fd6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funbox-the-cox-a-hidden-dark-secret-of-cox-regression-fb432137fd6&source=-----fb432137fd6---------------------bookmark_footer-----------)![](../Images/5854f8dc3addbe438ea7037fcb639fe1.png)

图片由 [Dima Pechurin](https://unsplash.com/pt-br/@pechka?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 探索完美预测因子

如果你一直关注我的之前的博客帖子，你可能会记得逻辑回归在尝试完美分离数据时会遇到问题，[导致无限的赔率比](https://medium.com/towards-data-science/logistic-regression-faceoff-67560de4f492)*。在 Cox 回归中，风险替代了赔率，你可能会想知道完美预测变量是否会出现类似的问题。确实会出现，但是与逻辑回归不同，这里的问题不那么明显，也不容易界定什么是“完美预测变量”。如后面会更加明确，完美预测变量被定义为预测变量*x*，其排名恰好与事件时间的排名一致（它们的斯皮尔曼相关系数为 1）。

之前，在“Unbox the Cox”中：

[](/unbox-the-cox-intuitive-guide-to-cox-regressions-c485408ae15d?source=post_page-----fb432137fd6--------------------------------) [## Unbox the Cox: 直观指南到 Cox 回归

### 风险和最大似然估计如何预测事件排名？

[towardsdatascience.com](/unbox-the-cox-intuitive-guide-to-cox-regressions-c485408ae15d?source=post_page-----fb432137fd6--------------------------------)

我们解释了最大似然估计，并介绍了一个虚构的数据集，其中有五个主体，一个预测变量*x*，代表了一种延长生命的药物的剂量。为了使*x*成为事件时间的完美预测变量，我们在这里交换了主体 C 和 D 的事件时间：

```py
import numpy as np
import pandas as pd
import plotnine as p9

from cox.plots import (
    plot_subject_event_times,
    animate_subject_event_times_and_mark_at_risk,
    plot_cost_vs_beta,
)

perfect_df =  pd.DataFrame({
    'subject': ['A', 'B', 'C', 'D', 'E'],
    'time': [1, 3, 4, 5, 6],
    'event': [1, 1, 1, 1, 0],
    'x': [-1.7, -0.4, 0.0, 0.9, 1.2],
})

plot_subject_event_times(perfect_df, color_map='x')
```

![](../Images/15880abec14a17404267728d9a3afd82.png)

图片由作者提供。

为了理解为什么这些“完美预测变量”可能会有问题，让我们从上次的内容继续，查看负对数似然图与*β*的关系：

```py
negloglik_sweep_betas_perfect_df = neg_log_likelihood_all_subjects_sweep_betas(
    perfect_df,
    betas=np.arange(-5, 5, 0.1)
)
plot_cost_vs_beta(negloglik_sweep_betas_perfect_df, width=0.1)
```

![](../Images/84e4aa14eea51ed69bc51e9c23ff5840.png)

图片由作者提供。

你可以立即看到*β*没有最小值：如果我们使用非常大的负值的*β*，我们最终会得到几乎完美的对数似然拟合结果。

现在，让我们深入探讨一下背后的数学原理，看看事件 A 的可能性。我们将研究分子和分母在调整*β*时的变化情况：

![](../Images/f5509f0ba94030c965b8c45e143af6d3.png)

当*β*很高或是一个很大的正数时，分母中的最后一个项（具有最大* x* 为 1.2），代表了主体 E 的风险，主导了整个分母，并变得极其巨大。因此，似然变得很小，接近于零：

![](../Images/4a33d3c071776416288bc2bd87eed8bb.png)

这会导致很大的负对数似然。每个单独的似然情况也是如此，因为主体 E 的最后一个风险总是会超过分子中的任何风险。因此，主体 A 到 D 的负对数似然增加。在这种情况下，当我们有较高的*β*时，它会降低所有的似然，导致所有事件的拟合效果较差。

现在，当*β*值较低或为一个很大的负数时，分母中的第一个项，代表了对象A的风险，因为它的*x*值最低，主导了分母。由于对象A的风险也出现在分子中，通过使*β*越来越负，似乎能使L(A)接近1，从而创造出几乎完美的拟合：

![](../Images/606a470b8a080c47cafdcf39a47ac2d8.png)

对所有其他单个可能性来说也是一样的：负*β*现在同时提升所有事件的可能性。基本上，负*β*不会带来任何缺点。同时，某些个体风险增加（对象A和B的负*x*），有些保持不变（对象C的*x =* 0），其他则减少（对象D的正*x*）。但请记住，真正重要的是风险的比率。我们可以通过绘制单个风险来验证这一点：

```py
def plot_likelihoods(df, ylim=[-20, 20]):
    betas = np.arange(ylim[0], ylim[1], 0.5)
    subjects = df.query("event == 1")['subject'].tolist()
    likelihoods_per_subject = []
    for subject in subjects:
        likelihoods = [
            np.exp(log_likelihood(df, subject, beta))
            for beta in betas
        ]
        likelihoods_per_subject.append(
            pd.DataFrame({
                'beta': betas,
                'likelihood': likelihoods,
                'subject': [subject] * len(betas),
            })        
        )
    lik_df = pd.concat(likelihoods_per_subject)
    return (
        p9.ggplot(lik_df, p9.aes('beta', 'likelihood', color='subject'))
        + p9.geom_line(size=2)
        + p9.theme_classic()
    )    

plot_likelihoods(perfect_df)
```

![](../Images/e3607c24c61922b16d990450fa47d835.png)

图片由作者提供。

可能性的组合方式，即风险与所有仍然处于风险中的对象的风险总和的比率，意味着负*β*值为每个事件时间排名大于或等于预测因子排名的对象的**可能性提供了一个完美的拟合**！作为附带说明，如果*x*与事件时间有完美的*负*斯皮尔曼相关性，情况将会反转：任意正的*β*会给我们带来任意好的拟合。

# 不匹配的预测因子和时间排名

我们实际上可以看到这一点，并向你展示当事件时间排名和预测因子排名不匹配时会发生什么，使用另一个虚构的例子：

```py
sample_df = pd.DataFrame({
    'subject': ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'],
    'x': [-1.7, -0.4, 0.0, 0.5, 0.9, 1.2, 1.3, 1.45],
    'time': [1, 2, 4, 3, 5, 7, 6, 8],
    'rank_x': [1, 2, 3, 4, 5, 6, 7, 8],
    'event': [1, 1, 1, 1, 1, 1, 1, 0],
})

sample_df
```

![](../Images/3c639010a4db2b20b224393ffed42ef7.png)

在这个特定的例子中，`time`列的范围从1到8，每个值代表其自己的排名。我们还有一个`x_rank`列，用于排名预测因子*x*。现在，关键观察点是：对于D和G对象，它们的`x_rank`实际上高于其对应的`time`排名。因此，当我们有大负值的*β*时，D和G的可能性不会在分子和分母之间经历抵消效应：

![](../Images/a59a9dda57fca6a1bf85aa40863c81c7.png)![](../Images/f6fb2510e40f11c7ed08408509ac2da5.png)

他们的可能性现在在某些中间有限的*β*值处达到最大。我们来看看单个可能性的图表：

```py
plot_likelihoods(sample_df)
```

![](../Images/963e381927c49fa06858b34c9ac5f2dc.png)

图片由作者提供。

时间和预测因子之间的这些“错位”排名起着至关重要的作用：它们阻止所有可能性在我们有显著负*β*值时基本上坍缩成一个。

总结来说，在Cox回归中，为了获得预测因子*x*的有限系数*β*，我们需要至少有一个实例，其中预测因子*x*的排名低于事件时间的排名。

# 完美确实是良好的敌人（p值）

那么，这些完美的预测因子在现实场景中实际上表现如何？为了找出答案，让我们再次转向lifelines库进行一些调查：

```py
from lifelines import CoxPHFitter

perfect_cox_model = CoxPHFitter()
perfect_cox_model.fit(
  perfect_df,
  duration_col='time',
  event_col='event',
  formula='x'
)
perfect_cox_model.print_summary()
```

```py
#> /.../coxph_fitter.py:1586: ConvergenceWarning:
#> The log-likelihood is getting suspiciously close to 0 and the delta is still large.
#> There may be complete separation in the dataset.
#> This may result in incorrect inference of coefficients.
#> See https://stats.stackexchange.com/q/11109/11867 for more.

#> /.../__init__.py:1165: ConvergenceWarning:
#> Column x has high sample correlation with the duration column.
#> This may harm convergence.
#> This could be a form of 'complete separation'.
#> See https://stats.stackexchange.com/questions/11109/how-to-deal-with-perfect-separation-in-logistic-regression

#> /.../coxph_fitter.py:1611:
#> ConvergenceWarning: Newton-Rhaphson failed to converge sufficiently.
#> Please see the following tips in the lifelines documentation:
#> https://lifelines.readthedocs.io/en/latest/Examples.html#problems-with-convergence-in-the-cox-proportional-hazard-model
```

![](../Images/f2e4431769f8c082ba5d63a695e27887.png)

就像在逻辑回归中一样，我们遇到了收敛警告，并且得到了极宽的预测因子系数*β*的置信区间**因此，我们得到的p值为0.93**！

如果我们仅仅基于p值过滤模型而不考虑这个问题或进行进一步调查，我们可能会忽视这些完美的预测因子。

为了应对这个收敛问题，lifelines库文档和一些有用的StackOverflow线程建议了一个潜在的解决方案：将正则化项纳入成本函数。这个项有效地增加了大系数值的成本，你可以通过将`penalizer`参数设置为大于零的值来激活L2正则化：

```py
perfect_pen_cox_model = CoxPHFitter(penalizer=0.01, l1_ratio=0)
perfect_pen_cox_model.fit(perfect_df, duration_col='time', event_col='event', formula='x')
perfect_pen_cox_model.print_summary()
```

![](../Images/6b85fb942524535ad537bab989e4d1a7.png)

这种方法修复了收敛警告，但并没有在缩小那个讨厌的p值上取得巨大进展。即使使用了这种正则化技巧，完美预测因子的p值仍然维持在一个相对较大的值0.11。

# 时间是相对的：只有排名才重要

最后，我们将验证事件时间的绝对值对Cox回归拟合没有影响，使用我们之前的例子。为此，我们将引入一个名为`time2`的新列，其中包含与`time`列相同顺序的随机数字：

```py
sample_df = pd.DataFrame({
    'subject': ['A', 'B', 'C', 'D', 'E', 'F', 'G', 'H'],
    'x': [-1.7, -0.4, 0.0, 0.5, 0.9, 1.2, 1.3, 1.45],
    'time': [1, 2, 4, 3, 5, 7, 6, 8],
    'rank_x': [1, 2, 3, 4, 5, 6, 7, 8],
    'event': [1, 1, 1, 1, 1, 1, 1, 0],
}).sort_values('time')

np.random.seed(42)
sample_df['time2'] = sorted(np.random.randint(low=-42, high=888, size=8))
sample_df
```

![](../Images/c640ed3ed5010c5ab0d8da500b31c714.png)

它们的拟合确实是相同的：

```py
sample_cox_model = CoxPHFitter()
sample_cox_model.fit(
  sample_df,
  duration_col='time',
  event_col='event',
  formula='x'
)
sample_cox_model.print_summary()
```

![](../Images/99c7b05224f1a308f298c14adcce3a87.png)

```py
sample_cox_model = CoxPHFitter()
sample_cox_model.fit(
  sample_df,
  duration_col='time2',
  event_col='event',
  formula='x'
)
sample_cox_model.print_summary()
```

![](../Images/c49f79ac66e6621a31ffc051e8188e87.png)

# 结论

我们从中学到了什么？

+   生存模型中的完美预测因子是那些排名与事件时间的排名完全匹配的预测因子。

+   Cox回归无法用有限的系数*β*来拟合这些完美预测因子，导致了宽置信区间和大的p值。

+   事件时间的实际值并不重要——关键在于它们的排名。

+   当事件时间和预测因子的排名不一致时，我们无法在似然中获得大*β*值的便捷取消效应。因此，我们至少需要一个排名不匹配的案例，以获得一个有限系数的拟合。

+   即使我们尝试一些高级的正则化技术，完美预测因子在现实情况下仍然会给我们那些恼人的宽置信区间和高p值。

+   就像在逻辑回归中一样，如果我们不太关心这些p值，使用正则化方法仍然可以提供一个方便的模型拟合，准确进行预测！

如果你想自己运行代码，可以使用我Github上的IPython笔记本：[https://github.com/igor-sb/blog/blob/main/posts/cox_perfect.ipynb](https://github.com/igor-sb/blog/blob/main/posts/cox_perfect.ipynb)

告别，直到下一篇文章！ 👋
