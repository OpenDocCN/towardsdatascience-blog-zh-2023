# 实验中的方差减少 —— 第2部分：协变量调整方法

> 原文：[https://towardsdatascience.com/variance-reduction-in-experiments-part-2-covariate-adjustment-methods-f5393f92eb8f?source=collection_archive---------9-----------------------#2023-02-07](https://towardsdatascience.com/variance-reduction-in-experiments-part-2-covariate-adjustment-methods-f5393f92eb8f?source=collection_archive---------9-----------------------#2023-02-07)

## 深入探讨 MLRATE —— 机器学习回归调整治疗效应估计器，并与其他方法进行比较

[](https://medium.com/@murat.unal?source=post_page-----f5393f92eb8f--------------------------------)[![Murat Unal](../Images/9f00db7597d7ece01213a6b0589c87d8.png)](https://medium.com/@murat.unal?source=post_page-----f5393f92eb8f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f5393f92eb8f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f5393f92eb8f--------------------------------) [Murat Unal](https://medium.com/@murat.unal?source=post_page-----f5393f92eb8f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F15a64c9fc55d&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-in-experiments-part-2-covariate-adjustment-methods-f5393f92eb8f&user=Murat+Unal&userId=15a64c9fc55d&source=post_page-15a64c9fc55d----f5393f92eb8f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f5393f92eb8f--------------------------------) ·9 分钟阅读·2023年2月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff5393f92eb8f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-in-experiments-part-2-covariate-adjustment-methods-f5393f92eb8f&user=Murat+Unal&userId=15a64c9fc55d&source=-----f5393f92eb8f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff5393f92eb8f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fvariance-reduction-in-experiments-part-2-covariate-adjustment-methods-f5393f92eb8f&source=-----f5393f92eb8f---------------------bookmark_footer-----------)![](../Images/48d8ff13f43f0cda293c05d88c8fd730.png)

图片由 [Sam Moghadam Khamseh](https://unsplash.com/@sammoghadamkhamseh?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

这是系列文章中的第二篇，我们讨论了实验中的方差减少。在第一篇文章中，我们讨论了为什么在实验中减少结果度量的方差是必要的，并展示了简单的回归调整如何带来显著的好处，同时对这一主题建立了直觉。

[实验中的方差减少 — 第1部分：直觉](/variance-reduction-in-experiments-part-1-intuition-68b270a0df71?source=post_page-----f5393f92eb8f--------------------------------)

### 方差减少的直觉及其在随机实验中重要的原因。

[towardsdatascience.com](/variance-reduction-in-experiments-part-1-intuition-68b270a0df71?source=post_page-----f5393f92eb8f--------------------------------)

在这篇文章中，我们将分析几种已建立的协变量调整方法的方差减少性能。具体而言，我们将对数据生成过程中的不同复杂度运行模拟，并将以下方法应用于每个实验数据：

1\. 回归调整（OLS_adj）

2\. 带交互项的回归调整（OLS_int）

3\. 使用实验前数据的控制实验（CUPED）

4\. 差异中的差异（DID）

5\. 机器学习回归调整处理效应估计器（MLRATE）

我们想要找到的平均处理效应（ATE）是处理（用1表示）和控制（用0表示）之间的结果预期差异，*Y*：

思考条件平均处理效应（CATE）也很有用，它是在描述协变量*X*的单位子集上的ATE：

对整个协变量空间的CATE取平均，得到ATE：

由于我们有一个随机分配的实验，每种方法都是ATE的无偏估计。然而，每种方法实现的方差减少水平可能非常不同，具体取决于基础数据生成过程。

首先讨论每种方法的机制。

## 均值差异（DIM）

DIM是ATE的一个简单而一致的估计，并且将在我们的分析中作为基线，因为它不涉及任何协变量调整：

## 回归调整（OLS_adj）

这是治疗指示符* T *的系数估计，如果单位*i*处于治疗中则为1，否则为0，它来自包含协变量*X*的结果回归，线性且加性，并假设所有单位的处理效应是恒定的：

要了解为何*τ*的OLS估计量是ATE的一个一致估计量，我们考虑处理组和对照组的回归函数分别计算，然后取差异：

因此：

## 带交互项的回归调整（OLS_int）

这是治疗指示符* T *的系数估计，它来自包含协变量*X*以及*T*与去均值协变量之间交互作用的结果回归。

与 OLS_adj 相反，在这里我们不假设效应在所有单位之间是恒定的，而是允许处理效应随着协变量而变化，尽管是以线性和加法的方式。

*τ* 的 OLS 估计量再次是 ATE 的一致且渐近正态的估计量。要了解原因，我们再次分别取处理组和对照组的回归函数，然后取差值：

所以，我们有：

## 使用预实验数据的对照实验 (CUPED)

CUPED (Deng et al., 2013) 的基础思想是使用与结果 *Y* 高度相关但与处理 *T* 无关的预实验协变量 *X*。结果的预实验值 *Y* 是一个自然的候选者，因为它满足这些标准。在我们的数据中，如果可以访问这样的协变量，我们将按如下方式应用 CUPED：

1.  获取 theta：

```py
theta = cov(Y,X)/var(X)
```

2\. 为每个单位 *i* 创建一个转化后的结果 *Y_cuped*：

```py
Y_cuped = Y - theta*(X - mean(X))
```

3\. 从以下估计 *τ*：

这样 *Y* 的方差被减少了 *1*-*Corr(X, Y)*：

```py
Var(Y_cuped) = Var(Y)(1-Corr(Y,X))
```

在我们的模拟中，我们设计了 *X1*，使其满足上述标准。

## 差分中的差分 (DID)

在协变量调整的背景下，DID 估计量不应与面板数据应用中的 DID 估计量混淆。在这个背景下，DID 是通过首先训练一个机器学习模型 *g(X)* 来预测 *Y* 从 *X*，然后计算 *Y − g(X)* 的处理组和对照组平均值之间的差异（Yongyi et al., 2021）。

DID 的动机在于，由于 *g(X)* 和 *T* 是独立的，结果估计量与 DIM 估计量具有相同的期望：

然而，如果 *g(X)* 是 *Y* 的一个良好预测变量，那么 *Var(Y)* 将超过 *Var(Y − g(X))*，基于 *Y − g(X)* 的平均值的 DID 估计将比基于 *Y* 的平均值的 DIM 估计具有更低的方差。

此外，使用机器学习使我们能够以数据驱动的方式捕捉结果与协变量之间的复杂相互作用，而不依赖于 OLS_adj 和 OLS_int 中的函数形式假设。

一个关键点是使用交叉拟合。简单来说，我们将数据分为两部分，用一部分建立 *g(X)*，用另一部分获取预测结果，并通过交换这两个部分重复这个过程。我们最终得到每个观测值的预测结果，这些预测结果是由仅在其他观测值上训练的模型生成的。在这个分析中，我们对 DID 和 MLRATE 都应用了交叉拟合，因为这两者在最终估计中都使用了机器学习预测，但原始论文仅对 MLRATE 应用了交叉拟合（Yongyi et al., 2021）。

DID 的计算方法如下：

1.  通过交叉拟合训练 *g(X)* 并从 *X* 预测 *Y*。

1.  为每个单位 *i* 创建 *Y_res*：

```py
Y_res = Y - g(X)
```

3\. 从以下估计 *τ*：

## **机器学习回归调整处理效应估计量（MLRATE）**

DID和MLRATE之间的主要区别在于，DID直接从结果*Y*中减去机器学习预测值*g(X)*，而MLRATE则在后续的线性回归步骤中包括预测值以及T与去均值预测值之间的交互项（Yongyi et al., 2021）。

MLRATE的步骤如下：

1.  通过交叉拟合训练*g(X)*并从*X*预测*Y*。

1.  获取*τ*估计值：

研究表明，将预测值作为回归量可以保证估计器对差预测的稳健性，并且MLRATE的渐近方差不大于DIM估计器。

```py
def mlpredict(dfml,p):
    XGB_reg = XGBRegressor(learning_rate = 0.1, 
                            max_depth = 6, 
                            n_estimators = 500, 
                            reg_lambda = 1)

    X_mat = dfml.columns.tolist()[0:p]

    kfold = StratifiedKFold(n_splits=2, shuffle=True, random_state=1)

    ix = []
    Yhat = []
    for train_index, test_index in kfold.split(dfml, dfml["T"]):

        df_train = dfml.iloc[train_index].reset_index()
        df_test = dfml.iloc[test_index].reset_index()

        X_train = df_train[X_mat].copy()
        y_train = df_train['Y'].copy()
        X_test =  df_test[X_mat].copy()

        XGB_reg.fit(X_train, y_train)

        Y_hat = XGB_reg.predict(X_test)

        ix.extend(list(test_index))
        Yhat.extend(list(Y_hat))

    df_ml = pd.DataFrame({'ix':ix,'Yhat':Yhat}).sort_values(by='ix').reset_index(drop=True)
    df_ml[['Y','T']] = dfml[['Y','T']]
    df_ml['Ytilde'] = df_ml['Yhat'] - np.mean(df_ml['Yhat'])
    df_ml['Yres'] = df_ml['Y'] - df_ml['Yhat']
    df_ml = df_ml.drop('ix', axis=1)

    return df_ml
```

## **数据生成过程 (DGP)**

为了比较这5种协变量调整方法在不同复杂度下减少方差的效果，我们有一个DGP，包含*N = 2000*个独立同分布观测值和10个协变量，分布为*N(0,I(10×10))*。处理指示符为*Ti ∼ Bernoulli(0.5)*，误差项分布为*N(0,25²)*。处理与协变量和误差项独立，误差项本身与协变量独立。结果*Yi*和处理效应函数*τ(Xi)*取决于以下函数形式：

1.  **协变量的线性效应与恒定处理效应**

**2. 协变量的线性效应与变化的处理效应**

**3. 协变量的非线性效应与恒定处理效应**

**4. 协变量的非线性效应与变化的处理效应**

```py
def dgp(n=2000, p=10, linear=True, constant=True):

    Xmat = np.random.multivariate_normal(np.zeros(p), np.eye(p), size=n).astype('float32')

    T = np.random.binomial(1, 0.5, n).astype('int8')

    col_list = ['X' + str(x) for x in range(1,(p+1))]

    df = pd.DataFrame(Xmat, columns = col_list)

    # functional form of the covariates
    if linear:
        B = 225 + 50*df['X1'] + 5*df['X2'] + 20*(df['X3']-0.5) + 10*df['X4'] + 5*df['X5']
    else:
        B = 225 + 50*df['X1'] + 5*np.sin(np.pi*df['X1']*df['X2'] ) + 10*(df['X3']-0.5)**2 + 10*df['X4']**2 + 5*df['X5']**3

    # constant ate or non-constant
    tau = 5 if constant else 5*df['X1'] + 5*np.log(1 + np.exp(df['X2']))

    Y = (B + tau*T + np.random.normal(0,25,n)).astype('float32')

    df['T'] = T
    df['Y'] = Y    
    return df
```

我们在每种函数形式下生成1000个数据集，并将每种协变量调整方法应用于这些数据。

```py
def experiment(**kwargs):

    dct = {}

    n = kwargs['n']
    p = kwargs['p']
    linear = kwargs['linear']
    constant = kwargs['constant']

    df = dgp(n,p,linear,constant)

    #1\. Difference-in-means
    mu_treated = np.mean(df.query('T==1')['Y'])
    mu_control = np.mean(df.query('T==0')['Y'])

    dct['DIM'] = mu_treated - mu_control

    #2\. OLS adjusted
    if kwargs['OLS_adj']:

        ols_adj = smf.ols('Y ~' + ('+').join(df.columns.tolist()[0:(p+1)]),
                 data = df).fit(cov_type='HC1',use_t=True)

        dct['OLS_adj'] = ols_adj.params['T']

    #3\. OLS interacted
    if kwargs['OLS_int']:
        df = df.assign(**({c+'tilde': (df[c] - df[c].mean()) for c in df.columns.tolist()[0:p]}))

        ols_int = smf.ols('Y ~' + ('+').join(df.columns.tolist()[0:(p+1)]) + '+' + 'T:('+('+').join(df.columns.tolist()[p+2:])+')',
                 data = df).fit(cov_type='HC1',use_t=True)

        dct['OLS_int'] = ols_int.params['T']

    #4\. CUPED
    if kwargs['CUPED']:
        theta = smf.ols('Y ~ X1',data = df).fit(cov_type='HC1',use_t=True).params['X1']

        df['Y_res'] = df['Y'] - theta*(df['X1'] - np.mean(df['X1']))

        cuped = smf.ols('Y_res ~ T', data=df).fit(cov_type='HC1',use_t=True)

        dct['CUPED'] = cuped.params['T']

    pred_df = mlpredict(df,p)

    #5\. Difference-in-differences
    if kwargs['DID']:
        mu2_treated = np.mean(pred_df.query('T==1')['Yres'])
        mu2_control = np.mean(pred_df.query('T==0')['Yres'])

        dct['DID'] = mu2_treated - mu2_control

    #6\. MLRATE
    if kwargs['MLRATE']:
        mlrate =  smf.ols('Y ~ T + Yhat + T:Ytilde',
                     data = pred_df).fit(cov_type='HC1',use_t=True)

        dct['MLRATE'] = mlrate.params['T']

    return dct
```

最后，我们打印估计值、它们的标准误差和95%置信区间，并绘制分布图。

```py
def plot_experiment(results, constant = True ):

    results_long = pd.melt(results, value_vars=results.columns.tolist() )

    print(round(results_long.groupby('variable').agg(mean=("value", "mean"), std=("value", "std"))
          .reset_index().sort_values(by='std', ascending=False).reset_index(drop=True)
          .assign(CI_lower= lambda x: x['mean'] - x['std']*1.96,
                CI_upper= lambda x: x['mean'] + x['std']*1.96,),3)
         )

    mu = 5 if constant else 4

    p = (ggplot(results_long, aes(x='value',color='variable') ) + 
     geom_density(size=1 )+
     scale_color_manual(values = ['black', 'blue', 'green', 'c','red', 'salmon', 'magenta' ]) + 
     geom_vline(xintercept=mu, colour='black', linetype='dashed' ) + 
     annotate("text", x=mu, y=.1, label="True Mean", size=15)+
     labs(color='Method')  +
     xlab('Estimate') +
     theme(figure_size=(10, 8))
    )

    print(p)
```

## 发现

仿真结果显示在下图中，主要发现如下：

1.  每种协变量调整方法在标准误差上都优于DIM估计器，无论DGP是什么，然而，减少的程度取决于DGP的复杂性。

1.  当DGP包括协变量的线性效应时，无论是恒定处理效应还是变化处理效应，OLS_adj和OLS_int估计器的标准误差最小。

1.  当DGP包括协变量的非线性效应时，基于机器学习的估计器DID和MLRATE在恒定和变化的处理效应下的标准误差最小。

1.  由于协变量的非线性和变化的处理效应在实际应用中更为常见，因此我们得出结论，基于机器学习的调整方法在实验中减少方差方面优于其他方法。

1.  最终，CUPED在所有场景下的表现都不如其他方法。这主要是因为CUPED的原始版本使用了单一协变量进行调整，我们也是如此实现的。可以扩展CUPED以处理多个协变量。

```py
%%time
tqdm._instances.clear() 
sim = 1000

results1 = Parallel(n_jobs=8)(delayed(experiment)(n=2000, p=10, linear=True, constant=True,
                                    OLS_adj=True, OLS_int=True, CUPED=True, DID=True, MLRATE=True)\
                                    for _ in tqdm(range(sim)) )

results_df1 = pd.DataFrame(results1)

plot_experiment(results_df1)
```

![](../Images/381323e1ea9b81c3e89c5f9ad67384d2.png)

**图1：协变量的线性效应与恒定处理效应**

```py
results2 = Parallel(n_jobs=8)(delayed(experiment)(n=2000, p=10,linear=True, constant=False,
                                    OLS_adj=True, OLS_int=True, CUPED=True, DID=True, MLRATE=True)\
                                    for _ in tqdm(range(sim)) )

results_df2 = pd.DataFrame(results2)

plot_experiment(results_df2, False)
```

![](../Images/5d51176fa47ebfcdcaf8eae475991803.png)

**图2：协变量的线性效应与变化的处理效应**

```py
results3 = Parallel(n_jobs=8)(delayed(experiment)(n=2000, p=10,linear=False, constant=True,
                                    OLS_adj=True, OLS_int=True, CUPED=True, DID=True, MLRATE=True)\
                                    for _ in tqdm(range(sim)) )

results_df3 = pd.DataFrame(results3)

plot_experiment(results_df3)
```

![](../Images/7216b9a916200745c634a41fd6840ae6.png)

**图3：协变量的非线性效应与恒定的处理效应**

```py
results4 = Parallel(n_jobs=8)(delayed(experiment)(n=2000, p=10,linear=False, constant=False,
                             OLS_adj=True, OLS_int=True, CUPED=True, DID=True, MLRATE=True)\
                             for _ in tqdm(range(sim)) )

results_df4 = pd.DataFrame(results4)

plot_experiment(results_df4, False)
```

![](../Images/c3ccf3a15c8507388d22c1c6aa15f4a2.png)

**图4：协变量的非线性效应与变化的处理效应**

## 结论

这标志着关于实验中方差减少的两篇文章系列的结束。在第1部分中，我们对方差减少在实验中的重要性建立了直觉，而在第2部分中我们分析了不同的协变量调整方法。我们发现所有这些方法都比简单的DIM估计器得到更紧的置信区间。因此，协变量调整应该成为分析实验时的标准实践。至于应用哪种方法，我们已经看到基于机器学习的估计器表现最佳，尤其是当DGP中存在复杂的相互作用时，这无疑更真实地反映了现实世界。

## 代码

原始笔记本可以在我的github [仓库](https://github.com/muratunalphd/Blog-Posts)中找到。

> 感谢阅读！希望你觉得这值得你花时间。
> 
> 我致力于为从事因果推断及市场数据科学的实践者撰写高质量且有用的文章。
> 
> 如果你对这些领域感兴趣，可以考虑[关注我](https://medium.com/@murat.unal)，也欢迎分享你的评论/建议。

## 参考文献

[1] W. Lin，[关于回归调整实验数据的无神论笔记：重新审视Freedman的批评](https://projecteuclid.org/journals/annals-of-applied-statistics/volume-7/issue-1/Agnostic-notes-on-regression-adjustments-to-experimental-data--Reexamining/10.1214/12-AOAS583.full)。 (2013)*，《应用统计年鉴》*。

[2] A. Deng, Y. Xu, R. Kohavi, T. Walker，[通过利用实验前数据提高在线对照实验的敏感性](https://dl.acm.org/doi/abs/10.1145/2433396.2433413) (2013)，*WSDM*。

[3] [G. Yongyi, C. Dominic, M. Konutgan, W. Li, C. Schoener, M. Goldman, 在线实验中的方差减少的机器学习（2021），*NeurIPS*。](https://proceedings.neurips.cc/paper/2021/hash/488b084119a1c7a4950f00706ec7ea16-Abstract.html)
