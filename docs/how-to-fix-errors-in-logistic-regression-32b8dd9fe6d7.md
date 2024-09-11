# 使用Python调试逻辑回归错误的最佳实践

> 原文：[https://towardsdatascience.com/how-to-fix-errors-in-logistic-regression-32b8dd9fe6d7?source=collection_archive---------4-----------------------#2023-11-13](https://towardsdatascience.com/how-to-fix-errors-in-logistic-regression-32b8dd9fe6d7?source=collection_archive---------4-----------------------#2023-11-13)

## 优化性能使用非结构化、真实世界数据

[](https://gabeverzino.medium.com/?source=post_page-----32b8dd9fe6d7--------------------------------)![](../Images/36452afec54430c55594a26247136f6f.png)](https://gabeverzino.medium.com/?source=post_page-----32b8dd9fe6d7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----32b8dd9fe6d7--------------------------------)![](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----32b8dd9fe6d7--------------------------------) [Gabe Verzino](https://gabeverzino.medium.com/?source=post_page-----32b8dd9fe6d7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb4abbbfdcbbb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fix-errors-in-logistic-regression-32b8dd9fe6d7&user=Gabe+Verzino&userId=b4abbbfdcbbb&source=post_page-b4abbbfdcbbb----32b8dd9fe6d7---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----32b8dd9fe6d7--------------------------------) ·13 分钟阅读·2023 年 11 月 13 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F32b8dd9fe6d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fix-errors-in-logistic-regression-32b8dd9fe6d7&user=Gabe+Verzino&userId=b4abbbfdcbbb&source=-----32b8dd9fe6d7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F32b8dd9fe6d7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-to-fix-errors-in-logistic-regression-32b8dd9fe6d7&source=-----32b8dd9fe6d7---------------------bookmark_footer-----------)![](../Images/ece08cc83ca0517ebe73fcf363268779.png)

Vardan Papikyan（[Unsplash](https://unsplash.com/photos/a-person-holding-two-pieces-of-a-puzzle-JzE1dHEaAew))

许多文章都详细介绍了逻辑回归（LR）的基础知识 —— 它的多功能性、经过时间验证的性能，甚至其潜在的数学原理。但是，了解*如何*成功实施LR并调试不可避免的错误要困难得多。这些信息通常隐藏在问答网站、学术论文中，或者仅仅是通过经验获得。

现实是，并不是每一个输出都像标志性的鸢尾花数据集那样清晰，迅速地分类花的类型。在大数据集上（你可能在工作中使用的数据集），LR 模型很可能会遇到问题。不合理高的系数。NaN 的标准误差。无法收敛。

责任归谁？是坏数据？还是你的模型？

为了对这个领域进行整理，我进行了一些研究并编制了一个常见 LR 错误、原因和可能解决方法的列表。

![](../Images/7a1c7c056d5da9f85adf5dacee78042e.png)

LR 模型输出的问题、原因和解决方法（Gabe Verzino）

上述表格并不是详尽无遗的，但都在一个地方。下面我将使用虚构的数据（不平衡、稀疏、分类等……但典型的）来人为地再次制造这些错误，然后再次修复它们。

但首先。

## 快速背景

当你开始思考 LR 时，有一些对这些模型独特的事情你应该记住：

1.  从技术上讲，LR 是一个概率模型，而不是分类器

1.  LR 需要一个线性的决策边界；它假设特征和目标变量之间的线性关系，你可以通过视觉、交叉表或 *scipy.spatial* 中的 ConvexHull 分析来确定这一点。

1.  你不能有缺失值或主要的异常值

1.  随着特征数的增加，你更可能遇到多重共线性和过拟合问题（可以分别通过 VIF 和正则化来解决）

1.  Python 中最流行的 LR 包来自 *sklearn* 和 *statsmodels*

现在让我们来看一些常见的问题。

## 问题 1: 收敛警告，优化失败

使用 Python 的 *statsmodels* 包，你可以运行一个基本的 LR 并得到警告：“ConvergenceWarning: Maximum Likelihood optimization failed to converge.”

```py
import statsmodels.formula.api as smf

model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing'))", data=df_a)

results = model.fit(method='bfgs')
print(results.summary())
```

![](../Images/6b6a0fb8106ed7abe831c5aa339c19a3.png)

系数和标准误差可能会像平常一样出现，包括来自 *sklearn.linear_model* 包中的 *LogisticRegression*。

提供了结果后，你可能会停下来。但是不要停止。你希望确保你的模型收敛以产生最佳（最低）成本函数和模型拟合 [1]。

你可以在 *statsmodels* 或 *sklearn* 中通过改变求解器/方法或增加 *maxiter* 参数来解决这个问题。在 *sklearn* 中，调整参数 *C* 可以降低应用增加的 L2 正则化，并可以在对数尺度上进行迭代测试（100, 10, 1, 0.1, 0.01 等） [2]。

在我的特定案例中，我增加到 *maxiter*=300000，模型就收敛了。它有效的原因是我提供了更多的尝试次数给模型（例如求解器），以便收敛并找到最佳参数 [3]。而这些参数，如系数和 p 值，确实变得更加准确。

![](../Images/555d15dead43f9bcefb8fbfa4d459617.png)

method=’bfgs’, maxiter=0

![](../Images/1320d7f33ac6abab459908ebe51bc6f7.png)

method=’bfgs’, maxiter=30000

## 问题 2: 添加了一个特征，但 LR 输出没有更新

这个错误很容易被忽略，但也很容易诊断。如果您正在处理许多特征或在数据清理中没有注意到它，您可能会意外地在LR模型中包含一个几乎恒定或只有一个级别的分类特征……这是个坏事。包含这样一个特征将导致系数和标准误差没有任何警告。

这是我们的模型和没有坏特征的输出。

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing'))", data=df_a)

results = model.fit(method='bfgs',maxiter=30000)
print(results.summary())
```

![](../Images/d2cf4366c6e491f9009ce96fad58e785.png)

现在，让我们添加一个一级特征*type*，其参考类别设为‘Missing’。

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing')) + \
    C(type,Treatment('Missing'))", data=df_a) 

results = model.fit(method='bfgs', maxiter=300000)
print(results.summary())
```

然后我们看到了那个多余的特征的影响……实际上并没有影响。R平方值保持不变，如上下所示。

![](../Images/477a18c48288ae152b0ef4decec8cd8c.png)

逻辑回归无法对具有一个级别或常数值的特征进行有意义的估计，并且可能会从模型中丢弃它。这对模型本身没有影响，但最佳做法仍然是仅包含对模型有正面影响的特征。

对我们的特征*type*进行简单的*value_counts()*，发现它只有一个级别，表明您想从模型中删除该特征。

![](../Images/8d0043b00e630e12ae3b6ddab946aeee.png)

## 问题3：“反转Hessian失败”（和NaN标准误差）

这是一个大问题，而且经常发生。让我们通过将VA、VB、VC和VI这四个特征包含到我们的LR模型中来创建这个新错误。它们都是分类的，每个有3个级别（0、1和“Missing”作为参考）。

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing')) + \
    C(type,Treatment('Missing')) + \
    C(VA,Treatment('Missing')) + \
    C(VB,Treatment('Missing')) + \
    C(VC,Treatment('Missing')) + \
    C(VI,Treatment('Missing'))", data=df_a) 

results = model.fit(method='bfgs', maxiter=300000)
print(results.summary())
```

这是我们的新错误：

![](../Images/299d000bb77ddb4dcaafb9fbb2ad3835.png)

探索输出时，我们看到系数已经计算，但标准误差或p值没有。*Sklearn*也提供系数。

![](../Images/57210c6e56574499fa797890631e8af5.png)

但是NaN值为什么？为什么需要反转Hessian呢？

许多优化算法使用Hessian逆（或其估计）来最大化似然函数。因此，当反转失败时，意味着Hessian（技术上是对数似然的二阶导数）不是正定[4]。从视觉上看，某些特征具有巨大的曲率，而其他特征具有较小的曲率，结果是模型无法为参数生成标准误差。

更重要的是，当Hessian不可逆时，没有计算上的更改可以使其反转，因为它根本不存在[5]。

造成这种情况的原因是什么？有三种最常见的：

1.  特征变量比观察值更多

1.  分类特征的频率级别非常低

1.  特征之间存在多重共线性

由于我们的数据集有许多行（~25,000）相对于特征（14），我们可以安全地忽略第一种可能性（尽管存在解决这个确切问题的解决方案[6]）。第三种可能性可能存在，并且我们可以通过方差膨胀因子（VIF）来检查。第二种可能性更容易诊断，所以让我们从这里开始。

请注意，特征VA、VB、VC和VI主要由1和0组成。

![](../Images/77be00b2bb75c73c665a183fef49031b.png)

特别是VI特征在“缺失”类别中有非常低的（相对）频率，实际上大约为0.03%（9/24,874）。

![](../Images/29944d309a986ad9a1426f60a9fd8dfc.png)

假设我们在业务背景下确认可以将1和“缺失”合并在一起。 或者，至少，以这种方式重构数据可能带来的任何潜在后果都远远小于接受一个已知存在错误的模型（并且没有标准错误可言）。

![](../Images/93868b80adb5151783e2976628d35f9b.png)

因此，我们创建了VI_alt，它有2个级别，而0可以作为我们的参考。

将VI_alt替换为模型中的VI，

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing')) + \
    C(type,Treatment('Missing')) + \
    C(VA,Treatment('Missing')) + \
    C(VB,Treatment('Missing')) + \
    C(VC,Treatment('Missing')) + \
    C(VI_alt,Treatment(0))", data=df_a) 

results = model.fit(method='bfgs', maxiter=300000)
print(results.summary())
```

因为它不再出现任何错误，系数和标准错误现在出现了，所以整体模型稍微有所改善。 再次强调，这仍然是一个拟合不良的模型，但现在它是一个可以工作的模型，这是我们在这里的目标。

![](../Images/de5b2d336595f759055c5b16bbf8f2aa.png)

我们第三个和最后一个Hessian反演失败的可能性是多重共线性。 现在，多重共线性是机器学习中的热门话题，我认为这是因为（1）它困扰了许多流行的模型，如LR、线性回归、KNN和朴素贝叶斯（2）用于删除特征的VIF启发式方法更像是一门艺术而不是科学，以及（3）最终，从业者们对于是否甚至首先移除这些特征存在分歧，如果这样做会导致注入选择偏差或丢失关键模型信息 [5–9]。

我不会深入探讨这个问题。 这很复杂。

但我将展示如何计算VIFs，并且也许我们可以得出自己的结论。

首先，VIF确实询问“一个特征如何被所有其他特征共同解释？” 当一个特征与其他特征存在线性依赖时，该特征的VIF估计将会“膨胀”。

鉴于我们数据集中的所有特征，让我们计算下面的VIFs。 

```py
from statsmodels.stats.outliers_influence import variance_inflation_factor

variables = results.model.exog
vif = [variance_inflation_factor(variables, i) for i in range(variables.shape[1])]

vifs = pd.DataFrame({'variables':results.model.exog_names,'vif':[ '%.2f' % elem for elem in vif ]})
vifs.sort_index(ascending=False).head(14)
```

![](../Images/0a6fb0ad0da304321f4d0e4dee906ffb.png)

从这个部分列表可以看出，特征VA、VB、VD都显示出无限接近无穷大的VIF，远远超过“经验法则”阈值5。但是，我们需要对此类启发式方法保持谨慎，因为VIF阈值有两个注意事项：

1.  5的截止值是相对于其他特征而言的 — 例如，如果大多数特征的VIF低于7，而一小部分特征的VIF高于7，则7可能是一个更合理的阈值。

1.  参考类别中的案例数量较少的分类特征与模型中的其他变量产生高VIF，即使这些特征与模型中的其他变量没有相关性 [8]。

在我们的情况下，特征VA、VB、VC都高度共线。 交叉表确认了这一点，如果它们是连续变量的话，皮尔逊相关矩阵也会确认。

解决这个问题的一般共识是逐个系统地去掉一个特征，检查所有的VIF，注意可能的改进，并继续，直到所有的VIF低于您选择的阈值。需要特别注意，相对于业务背景和目标变量，不要失去任何可能的解释能力。确认性统计检验如卡方检验和可视化可以帮助确定在两个可能选择之间去掉哪个特征。

让我们去掉VB，注意VIF的变化：

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing')) + \
    C(type,Treatment('Missing')) + \
    C(VA,Treatment('Missing')) + \
    C(VC,Treatment('Missing')) + \
    C(VI_alt,Treatment(0))", data=df_a) 

results = model.fit(method='bfgs', maxiter=300000)
print(results.summary())
```

![](../Images/176578df50e36401a64478974feef6ec.png)

VA和VC的VIF仍然是无穷大。更糟糕的是，模型仍然呈现NaN的标准误差（未显示）。让我们去掉VC。

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing')) + \
    C(type,Treatment('Missing')) + \
    C(VA,Treatment('Missing')) + \
    C(VI_alt,Treatment(0))", data=df_a) 

results = model.fit(method='bfgs', maxiter=300000)
print(results.summary())
```

![](../Images/024cb3841d6d1360ca2142f0522399e2.png)

最后，模型产生了标准误差，因此即使特征VA的VIF仍然> 5，这并不是问题，因为以上第二个注意事项，参考类别相对于其他水平的案例数量较少。

![](../Images/e150ba20ded7a4ad0e38e6426751b90f.png)

***额外学分：假设您绝对知道VA、VB和VC对解释目标变量至关重要，并且需要它们出现在模型中。鉴于额外的特征，假设优化正在复杂空间中进行，可能通过选择新的求解器和起始点（sklearn中的start_params）绕过“反转海森矩阵失败”。训练一个不假设线性边界的新模型也是一个选择。***

## 问题4：“可能完全拟合”错误（以及不合理地大的系数和/或标准误差）

我们可能会因为看到大的系数和高的准确性分数而兴奋。但通常，这些估计值是不可信的大，这是由另一个常见问题引起的：完美分离。

完美（或接近完美）分离发生在一个或多个特征与目标变量强相关时。实际上，它们可能几乎与目标变量相同。

我可以通过将目标变量*target_satisfaction*创造一个新特征*new_target_satisfaction*，它与它相似度达到95%来制造这个错误。

```py
df_a['new_target_satisfaction'] = 
pd.concat([pd.DataFrame(np.where(df_a.target_satisfaction.iloc[:1000]==1,0,df_a.target_satisfaction[:1000]),columns=["target_satisfaction"]),
pd.DataFrame(df_a.target_satisfaction.iloc[1000:],columns=['target_satisfaction'])],axis=0)
```

并将*new_target_satisfaction*放入模型中。

```py
model = smf.logit("target_satisfaction ~ C(age,Treatment('0-13')) + \
    C(educ,Treatment('Some High School')) + \
    C(emp,Treatment('Not Employed')) + \
    C(gender,Treatment('Male')) + \
    C(hispanic,Treatment('N')) + \
    C(dept,Treatment('Medical Oncology')) + \
    C(sign_status,Treatment('Activated')) + \
    C(mode,Treatment('Internet')) + \
    C(inf,Treatment('Missing')) + \
    C(type,Treatment('Missing')) + \
    C(VA,Treatment('Missing')) + \
    C(VI_alt,Treatment(0)) + \
    C(new_target_satisfaction,Treatment(0))", data=df_a) 

results = model.fit(method='lbfgs',  maxiter=1000000)
print(results.summary())
```

系数和标准误差渲染，但我们得到了这个准分离警告：

![](../Images/30ed46310d28bd23b07ab46054b3255a.png)

这个特征的系数非常高，约为70,000,000，这是不可信的。

![](../Images/f2ed5a4d314402957da555c7f2e7d3d0.png)

在幕后，这是因为分离“太好”的特征造成了夸张的斜率，从而导致大的系数和标准误差。可能，LR模型也无法收敛[10]。

![](../Images/e347c68f2585e834a20328b5da03bb4e.png)

完美分离（康奈尔大学统计咨询单位）[9]

那两个红色点，被错误分类的案例移除，实际上会防止完美分离，有助于LR模型收敛和标准误差估计更加现实。

在*sklearn*中，关于完美分离要记住的是，它可能会输出一个看似接近完美准确度的模型，在我们的例子中是98%，但实际上有一个特征*new_target_satisfaction*几乎是目标特征*target_satisfaction*的重复。

```py
 categorical_features = ['educ','emp','gender','hispanic','dept','sign_status','mode','inf','type',
'VA','VI_alt','new_target_satisfaction']

df1_y = df_a['target_satisfaction']
df1_x = df_a[['educ','emp','gender','hispanic','dept','sign_status','mode','inf','type',
'VA','VI_alt','new_target_satisfaction']]

# create a pipeline for all categorical features
categorical_transformer = Pipeline(steps=[
    ('ohe', OneHotEncoder(handle_unknown='ignore'))])

# create the overall column transformer
col_transformer = ColumnTransformer(transformers=[
    ('ohe', OneHotEncoder(handle_unknown='ignore'), categorical_features)],
   # ('num', numeric_transformer, numeric_features)],
                                    remainder='passthrough')

lr = Pipeline(steps = [('preprocessor', col_transformer),
                            ('classifier', LogisticRegression(solver='lbfgs',max_iter=200000))])
#['liblinear', 'newton-cg', 'lbfgs', 'sag', 'saga']

X_train, X_test, y_train, y_test = train_test_split(df1_x, df1_y, test_size=0.2, random_state=101)

lr_model = lr.fit(X_train, y_train)
y_pred = lr_model.predict(X_test)

# to leverage threshold resetting
# THRESHOLD = 0.5
# y_pred = np.where(lr_model.predict_proba(X_test)[:,1] > THRESHOLD, 1, 0)

print(classification_report(y_test, y_pred)) 
```

![](../Images/ddc8a4b7b3f30161f1444ff43462c28b.png)

最常见的解决方案是简单地删除该特征。但也有越来越多的替代解决方案：

1.  应用费斯特修正，最大化一个“惩罚”似然函数。目前，*statsmodels*没有此功能，但R有[11]。

1.  惩罚回归也可以有效，特别是通过测试求解器、惩罚项和学习率的组合[2]。我将*new_target_satisfaction*保留在模型中并尝试了各种组合，但在这个特定的情况下几乎没有变化。

1.  一些从业者会手动交换一些随机选择的观测值，以使其与目标的分离不那么完美，比如将上图中的红圈重新添加回去[8, 10]。对该特征和目标运行交叉表可以帮助你确定需要交换的案例百分比。在此过程中，你可能会问自己*为什么？我为什么要这样重构一个特征，以便LR模型可以接受？* 为了让你更安心，一些研究认为完美分离只是模型的症状，而非数据[8, 11]。

1.  最后，一些反对者实际上并不认为如此高的系数有任何问题[8]。非常高的比值比仅仅表明它强烈暗示了一个关联，并且预测几乎是完美的。警告这些发现，且就此为止。这个论点的基础是高系数是不幸的结果，沃尔德检验和似然比需要评估信息与替代假设。

## 结论

如果你能克服来自现实世界数据集的挑战，逻辑回归绝对是多功能且强大的。我希望这个概述为可能的解决方案提供了一个良好的基础。你觉得哪个建议最有趣？还有其他建议吗？

感谢阅读。在评论区分享你的想法，并告诉我你在逻辑回归中遇到的其他问题。我也很乐意在[LinkedIn](https://www.linkedin.com/in/gabe-verzino-71401137/)上联系并讨论相关问题。

查看我其他的一些文章：

[**使用贝叶斯网络预测医院辅助服务量**](https://medium.com/towards-data-science/using-bayesian-networks-to-forecast-ancillary-service-volume-in-hospitals-48968a978cb5)

[**为何类平衡被过度炒作**](https://medium.com/towards-data-science/why-balancing-classes-is-over-hyped-e382a8a410f7)

[**特征工程 CPT 代码**](https://medium.com/mlearning-ai/working-with-cpt-codes-5a2b04a4d183)

## 引用

1.  Allison, Paul. 逻辑回归中的收敛失败。宾夕法尼亚大学，费城，PA。 [https://www.people.vcu.edu/~dbandyop/BIOS625/Convergence_Logistic.pdf](https://www.people.vcu.edu/~dbandyop/BIOS625/Convergence_Logistic.pdf)

1.  Geron, Aurelien. 《动手学机器学习：使用 Scikit-Learn、Keras 和 Tensorflow》。第二版。出版单位：O’Reilly，2019年。

1.  Sckit-learn 文档，sklearn.linear_model.LogisticRegression。 [https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html](https://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

1.  Google Groups 讨论。“MLE 错误：警告：求逆海森矩阵失败：也许我不能使用‘matrix’容器？” 2014年。 [https://groups.google.com/g/pystatsmodels/c/aELcgNpg5f8](https://groups.google.com/g/pystatsmodels/c/aELcgNpg5f8)

1.  Gill, Jeff & King, Gary. 《当你的海森矩阵不可逆时该怎么办：非线性估计中的模型重规格化替代方案》。社会学方法与研究，第33卷，第1期，2004年8月，第54-87页 DOI: 10.1177/0049124103262681。 [https://gking.harvard.edu/files/help.pdf](https://gking.harvard.edu/files/help.pdf)

1.  “针对二分类问题的变量选择。” StackExchange, CrossValidated。在线阅读: [https://stats.stackexchange.com/questions/64271/variable-selection-for-a-binary-classification-problem/64637#64637](https://stats.stackexchange.com/questions/64271/variable-selection-for-a-binary-classification-problem/64637#64637)

1.  “在监督学习中，为什么特征相关性会带来不利影响。” StackExchange, 数据科学。在线阅读: [https://datascience.stackexchange.com/questions/24452/in-supervised-learning-why-is-it-bad-to-have-correlated-features](https://datascience.stackexchange.com/questions/24452/in-supervised-learning-why-is-it-bad-to-have-correlated-features)

1.  “如何处理逻辑回归中的完美分离。” StackExchange, CrossValidated。在线阅读: [https://stats.stackexchange.com/questions/11109/how-to-deal-with-perfect-separation-in-logistic-regression](https://stats.stackexchange.com/questions/11109/how-to-deal-with-perfect-separation-in-logistic-regression)

1.  Allison, Paul. “何时可以安全地忽略多重共线性”。《统计前沿》，2012年9月10日。在线阅读: [https://statisticalhorizons.com/multicollinearity/](https://statisticalhorizons.com/multicollinearity/)

1.  康奈尔统计咨询单位。逻辑回归中的分离与收敛问题。Statnews #82。出版：2012年2月。在线阅读: [https://cscu.cornell.edu/wp-content/uploads/82_lgsbias.pdf](https://cscu.cornell.edu/wp-content/uploads/82_lgsbias.pdf)

1.  Logistf: Firth 的偏差减少逻辑回归。R 文档。出版日期：2023年8月18日。在线阅读: [https://cran.r-project.org/web/packages/logistf/index.html](https://cran.r-project.org/web/packages/logistf/index.html)

*注意：所有图片均由作者提供，除非另有说明。*
