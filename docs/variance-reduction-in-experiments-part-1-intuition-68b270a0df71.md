# 实验中的方差减少 — 第1部分：直觉

> 原文：[https://towardsdatascience.com/variance-reduction-in-experiments-part-1-intuition-68b270a0df71?source=collection_archive---------4-----------------------#2023-02-02](https://towardsdatascience.com/variance-reduction-in-experiments-part-1-intuition-68b270a0df71?source=collection_archive---------4-----------------------#2023-02-02)

## 方差减少的直觉及其在随机实验中的重要性。

[](https://medium.com/@murat.unal?source=post_page-----68b270a0df71--------------------------------)[![Murat Unal](../Images/9f00db7597d7ece01213a6b0589c87d8.png)](https://medium.com/@murat.unal?source=post_page-----68b270a0df71--------------------------------)[](https://towardsdatascience.com/?source=post_page-----68b270a0df71--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----68b270a0df71--------------------------------) [Murat Unal](https://medium.com/@murat.unal?source=post_page-----68b270a0df71--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a64c9fc55d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-in-experiments-part-1-intuition-68b270a0df71&user=Murat+Unal&userId=15a64c9fc55d&source=post_page-15a64c9fc55d----68b270a0df71---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----68b270a0df71--------------------------------) ·7 min read·2023年2月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F68b270a0df71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-in-experiments-part-1-intuition-68b270a0df71&user=Murat+Unal&userId=15a64c9fc55d&source=-----68b270a0df71---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F68b270a0df71&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-in-experiments-part-1-intuition-68b270a0df71&source=-----68b270a0df71---------------------bookmark_footer-----------)![](../Images/d834f0d9e4aeb93318682085641fbfa8.png)

图片由 [Mars Plex](https://unsplash.com/@mars_plex?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是一个由两篇文章组成的系列的第一部分，我们将在其中深入探讨实验中的方差减少。在本文中，我们将讨论为何方差减少是必要的，并建立其机制的直觉。在第二部分中，我们将评估该领域的最新方法：MLRATE，并将其与其他成熟方法如CUPED进行比较。

让我们先讨论一下为何在实验中需要减少方差。看看，当涉及因果推断时，我们的结论可能由于两种不同的原因而错误：系统性偏差和随机变异。系统性偏差主要是由于自我选择或未测量的混杂因素造成的坏结果，而随机变异发生是因为我们获得的数据仅是我们尝试研究的总体的一个样本。随机实验允许我们消除系统性偏差，但它们并非免疫于基于随机变异的错误。

## 均值差估计器

让我们考虑一个例子，我们的营销团队想要弄清楚发送电子邮件促销的影响。作为营销数据科学团队，我们决定通过从客户基础中随机选择2000个客户来进行实验。我们的响应变量是电子邮件发送后的两周内的支出金额，对于每个客户，我们让硬币投掷决定该客户是否接收到促销。

由于治疗是随机分配的，我们没有系统性偏差，而实验中处理组(T)和对照组(C)之间的简单均值差（DIM）是总体平均治疗效应(ATE)的无偏估计：

关键点在于，这个估计量在平均情况下给我们真实效果，但对于我们最终分析的某个样本，它可能会远离真实值。

在以下代码中，我们首先生成一个2000个客户的样本，随机选择其中一半进行治疗，该治疗对客户支出的效果为$5。然后，我们使用线性回归对这个样本应用DIM估计器，希望能恢复真实效果。

```py
def dgp(n=2000, p=10):

    Xmat = np.random.multivariate_normal(np.zeros(p), np.eye(p), size=n).astype('float32')

    T = np.random.binomial(1, 0.5, n).astype('int8')

    col_list = ['X' + str(x) for x in range(1,(p+1))]

    df = pd.DataFrame(Xmat, columns = col_list)

    # functional form of the covariates
    B = 225 + 50*df['X1'] + 5*df['X2'] + 20*(df['X3']-0.5) + 10*df['X4'] + 5*df['X5']

    # constant ate
    tau = 5 

    Y = (B + tau*T + np.random.normal(0,25,n)).astype('float32')

    df['T'] = T
    df['Y'] = Y

    return df
```

```py
data = dgp()
ols = smf.ols('Y ~ T', data = data).fit(cov_type='HC1',use_t=True)
ols.summary().tables[1]
```

![](../Images/7ed9d19d0d4e46fb5cda6de68d5fe278.png)

不幸的是，这个特定样本的DIM估计值远离真实效果并且是负值。它还伴随着一个相当宽的置信区间，这使我们无法得出任何结论。

另一方面，如果我们有机会在每次不同样本的情况下平行重复这个实验成千上万次，我们会看到估计值的密度会在真实效果附近达到峰值。为了验证这一点，让我们创建一个模拟，生成2000个客户的样本，并在每次调用时运行前述实验，共10000次。

```py
def experiment(**kwargs):
    dct = {}

    n = kwargs['n']
    p = kwargs['p']

    df = dgp(n,p)

    #Difference-in-means
    mu_treated = np.mean(df.query('T==1')['Y'])
    mu_control = np.mean(df.query('T==0')['Y'])

    dct['DIM'] = mu_treated - mu_control

    return dct
```

```py
def plot_experiment(results):
    results_long = pd.melt(results, value_vars=results.columns.tolist() )  
    mu = 5
    p = (ggplot(results_long, aes(x='value') ) + 
     geom_density(size=1, color='salmon')+
     geom_vline(xintercept=mu, colour='black', linetype='dashed' ) + 
     annotate("text", x=mu, y=.1, label="True Mean", size=15)+
     labs(color='Method')  +
     xlab('Estimate') +
     theme(figure_size=(10, 8))
    )
    return p
```

```py
%%time
tqdm._instances.clear() 
sim = 10000
results = Parallel(n_jobs=8)(delayed(experiment)(n=2000, p=10)\
                                    for _ in tqdm(range(sim)) )

results_df = pd.DataFrame(results)
plot = plot_experiment(results_df) 
```

果然，我们的10000个估计值的平均值恢复了真实效果$5。

![](../Images/b2f354713855084bb8c4ccf46d73352f.png)

作者的图1

## 结果的变异

为了理解为什么我们在实际存在处理效果时未能揭示该效果，我们需要考虑到底是什么决定了客户支出。如果我们的电子邮件促销与收入、与业务的关系长度、最近性、购买频率和以前购买的价值等其他因素相比，对客户支出的影响有限，那么看到支出变异性更多地由其他因素解释就不足为奇了。

事实上，将客户支出与处理指标绘图揭示了支出从大约$4到超过$400的变异情况。由于我们知道促销对支出的影响很小，因此在所有变异中检测这一点变得相当具有挑战性。因此，从对照组到处理组的回归线在下面有一个轻微的负斜率。

![](../Images/b1c34ae64769e0dfc9ec94be2c50d8a5.png)

作者图2

增加样本量是让DIM估计器更容易检测真实效果的一种方法。如果我们能接触到10000名客户而不是2000名客户，我们可以获得更接近真实效果的估计，如下所示。

```py
data = dgp(n=10000, p=10)
ols = smf.ols('Y ~ T', data = data).fit(cov_type='HC1',use_t=True)
ols.summary().tables[1]
```

![](../Images/b99859165ca55d5fb463bd6aee2c0c8e.png)

## 回归调整

现在，如果我们在时间或其他资源上受到限制，不能增加样本量，但仍然希望能够检测到小效果怎么办？这引出了减少结果方差的想法，这与增加样本量类似，如果效果确实存在，它使检测效果变得更容易。

实现这一目标最简单的方法之一是使用回归调整来控制所有其他决定支出的因素。请注意，我们为此示例生成的样本有10个协变量，其中5个直接与结果相关。如前所述，我们可以将其视为以下几个因素：收入、与业务的关系长度、最近性、购买频率和以前购买的价值。将它们作为回归中的控制变量意味着在观察处理效果时保持这些因素不变。这意味着如果我们观察在实验时收入水平和购买行为相似的客户，结果的方差会更小，我们将能够捕捉到如下所示的小处理效果：

```py
data = dgp(n=2000, p=10)
ols = smf.ols('Y ~ T+' + ('+').join(data.columns.tolist()[0:10]),
                 data = data).fit(cov_type='HC1',use_t=True)

ols.summary().tables[1]
```

![](../Images/b66403cb770bc847546dd9990e3dd786.png)

为了帮助直观理解，让我们首先将协变量对处理和结果的影响部分剔除，然后绘制结果残差。

```py
model_t = smf.ols('T ~ ' + ('+').join(data.columns.tolist()[0:10]), data=data).fit(cov_type='HC1',use_t=True)
model_y = smf.ols('Y ~ ' + ('+').join(data.columns.tolist()[0:10]), data=data).fit(cov_type='HC1',use_t=True)

residuals = pd.DataFrame(dict(res_y=model_y.resid, res_t=model_t.resid))

p1=(ggplot(residuals, aes(x='res_t', y='res_y'))+
 geom_point(color='c') + 
 ylab("Spending Amount ($) Residual") +
 xlab("Received Coupon Residual") +
  geom_smooth(method='lm',se=False, color="salmon")+
 theme(figure_size=(10, 8)) +
  theme(axis_text_x = element_text(angle = 0, hjust = 1))
)

p1 
```

我们看到两个组中结果残差的剩余变异性都大大减少，并且从对照组残差到处理组残差的回归线现在有了一个正斜率。

![](../Images/f77ecc67940de24a77020a1d1163c88f.png)

作者图3

将原始支出金额的方差与支出金额残差的方差进行比较，确认我们能够实现超过80%的方差减少。

```py
print("Spending Amount Variance:", round(np.var(data["Y"]),2))
print("Spending Amount Residual Variance:", round(np.var(residuals["res_y"]),2))
```

![](../Images/16998e616ed4576669826c802cb1b17b.png)

由于回归机制，你如果感兴趣可以在这篇[文章](https://medium.com/towards-data-science/the-fwl-theorem-or-how-to-make-all-regressions-intuitive-59f801eb3299)中了解更多，我们可以通过对支出金额的残差进行回归，得到与对处理的残差回归相同的结果：

```py
model_res = smf.ols('res_y ~ res_t', data=residuals).fit()
model_res.summary().tables[1]
```

![](../Images/14e5501f9f9d8f261e39a6f4685148f5.png)

## 结论

对我们的实验分析应用简单的回归调整，可以大大减少结果指标的方差，这在我们尝试检测实验中的小效果时尤为有用。在本系列的第2部分，我们将深入探讨各种方法在这个领域的表现。具体而言，我们将运行具有不同复杂度的数据生成过程的模拟，并应用MLRATE - 机器学习回归调整处理效应估计器，这一领域的最新发明，以及其他方法如CUPED和回归调整。敬请关注。

## 代码

此分析的代码可以在我的github[仓库](https://github.com/muratunalphd/Blog-Posts)中找到。

> 感谢阅读！欢迎评论/建议。
