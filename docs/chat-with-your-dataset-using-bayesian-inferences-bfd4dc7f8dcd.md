# 使用贝叶斯推断与你的数据集对话。

> 原文：[`towardsdatascience.com/chat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd`](https://towardsdatascience.com/chat-with-your-dataset-using-bayesian-inferences-bfd4dc7f8dcd)

## 向数据集提出问题的能力一直是一个令人着迷的前景。你会惊讶于学习一个可以用来询问数据集的局部贝叶斯模型是多么简单。

[](https://erdogant.medium.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)![Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfd4dc7f8dcd--------------------------------) [Erdogan Taskesen](https://erdogant.medium.com/?source=post_page-----bfd4dc7f8dcd--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bfd4dc7f8dcd--------------------------------) ·13 分钟阅读·2023 年 11 月 13 日

--

![](img/cd3cbd949d64dc93de294b51ab286416.png)

图片由 [Vadim Bogulov](https://unsplash.com/pt-br/@franku84?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 提供，来源于 [Unsplash](https://unsplash.com/photos/MfBnqUOz_qY?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)

随着类似 ChatGPT 的模型的兴起，更多的人能够分析自己的数据集，并且可以说是“*提问*”。尽管这很棒，但在将其作为自动化流程中的分析步骤时，这种方法也有其缺点。这尤其适用于模型结果可能产生重大影响的情况。为了保持控制并确保结果准确，我们也可以使用贝叶斯推断与数据集对话。*在这篇博客中，我们将逐步介绍如何学习贝叶斯模型，并在数据科学薪资数据集上应用 do-calculus。我将演示如何创建一个模型，让你“提问”数据集并保持控制。你会惊讶于使用* [*bnlearn 库*](https://erdogant.github.io/bnlearn/pages/html/index.html)*创建这样一个模型的简便性。*

# 介绍

从数据集中提取有价值的洞察对于数据科学家和分析师来说是一项持续的挑战。类似于 ChatGPT 的模型使得互动分析数据集变得更加容易，但与此同时，这种方法可能变得不够透明，甚至不清楚为什么做出某些选择。依赖这样的黑箱方法在自动化分析流程中远非理想。当模型的结果对采取的行动有重大影响时，创建透明模型尤其重要。

> 能够有效地与数据集进行沟通一直是研究人员和从业人员的一个令人着迷的前景。

在接下来的部分中，我将首先介绍 [*bnlearn 库*](https://erdogant.github.io/bnlearn/pages/html/index.html) *[1]*，以及如何学习因果网络。然后，我将演示如何使用混合数据集学习因果网络，以及如何应用 do-calculus 有效查询数据集。*让我们看看贝叶斯推理如何帮助我们与数据集互动！*

*如果你觉得这篇文章有帮助，欢迎* [*关注我*](https://erdogant.medium.com/subscribe) *，因为我写了更多关于贝叶斯学习的内容。如果你考虑加入 Medium 会员，可以通过使用我的推荐链接来支持我的工作。价格和一杯咖啡一样，但这允许你每月无限阅读文章。*

# Bnlearn 库

***Bnlearn*** 是一个强大的 Python 包，它提供了一整套用于贝叶斯网络因果分析的函数。它可以处理离散、混合和连续数据集，并提供了广泛的用户友好功能，用于因果学习，包括*结构学习、参数学习和推理* [1–3]。在进行推理之前，我们需要理解结构学习和参数学习，因为推理依赖于这两种学习***。***

**学习数据集的因果结构** 是 *bnlearn* 的一个重要特性。结构学习消除了对变量之间潜在关系的先验知识或假设的需求。在 *bnlearn* 中，有三种方法可以学习因果模型并捕捉变量之间的依赖关系。结构学习将生成一个所谓的 **有向无环图（DAG）**。虽然这三种技术都会生成因果 DAG，但有些可以处理大量特征，而有些则具有更高的准确性。***有关结构学习的更多细节，请参见*** ***下面的博客***。

+   *基于评分的结构学习：使用评分函数 BIC、BDeu、k2、bds、aic，结合如 exhaustivesearch、hillclimbsearch、chow-liu、树增强朴素贝叶斯（TAN）、朴素贝叶斯等搜索策略。*

+   *基于约束的结构学习（PC）：使用统计方法如卡方检验在建模之前测试边缘强度。*

+   *混合结构学习：（两种技术的结合）*

+   *基于评分、基于约束和混合结构学习*。虽然这三种技术都会生成因果 DAG，但有些可以处理大量特征，而有些则具有更高的准确性。***有关结构学习的更多细节，请参见下面的博客 [2]***。

[](/a-step-by-step-guide-in-detecting-causal-relationships-using-bayesian-structure-learning-in-python-c20c6b31cee5?source=post_page-----bfd4dc7f8dcd--------------------------------) ## 使用贝叶斯结构学习在 Python 中检测因果关系的逐步指南。

### 入门指南，帮助有效确定变量之间的因果关系。

towardsdatascience.com

**参数学习** 是贝叶斯网络分析的第二个重要部分，*bnlearn* 在这一领域也表现出色。通过利用一组数据样本和一个（预先确定的）DAG，我们可以估计条件概率分布或表（CPDs 或 CPTs）。***有关参数学习的更多细节，我推荐*** ***以下博客******:***

[](/a-step-by-step-guide-in-designing-knowledge-driven-models-using-bayesian-theorem-7433f6fd64be?source=post_page-----bfd4dc7f8dcd--------------------------------) ## 设计基于知识驱动的模型的逐步指南，使用贝叶斯定理。

### 如果你没有数据，但有专家知识，那么可以使用计算机辅助来转换这些知识的入门指南。

towardsdatascience.com

*Bnlearn* 还提供了大量的函数和辅助工具，以帮助用户完成整个分析过程。这些包括数据集转换函数、拓扑排序推导、图形比较工具、深刻的交互式绘图功能等。*bnlearn* 库支持加载 bif 文件，将有向图转换为无向图，并执行统计测试以评估变量之间的独立性。如果你想了解 *bnlearn* 相比其他因果库的表现，***这个博客适合你***:

[](/the-power-of-bayesian-causal-inference-a-comparative-analysis-of-libraries-to-reveal-hidden-d91e8306e25e?source=post_page-----bfd4dc7f8dcd--------------------------------) ## 贝叶斯因果推断的力量：对库进行比较分析，以揭示隐藏的…

### 通过使用最合适的贝叶斯因果推断库来揭示数据集中的隐藏因果变量：一个…

towardsdatascience.com

在下一部分，我们将开始使用 do-calculus 进行推断，并通过实际示例进行操作。这使我们能够向数据集提出问题。*如前所述，结构学习和参数学习构成了基础。*

# 查询数据集需要使用 do-calculus 进行推断。

当我们进行**使用 do-calculus 进行推断**时，这基本上意味着我们可以查询数据集并“*提出问题*”。为此，我们需要两个主要成分：**DAG**和**分配给图中每个节点的 CPTs**。CPTs 包含每个变量的概率，并捕捉给定其父节点的因果关系。*让我们继续，创建一个示例，看看它是如何真正工作的。*

# 使用数据科学薪资数据集的应用

为了演示，我们将使用从 ai-jobs.net [5] 派生的[数据科学薪资数据集](https://ai-jobs.net/salaries/download/)。这个薪资数据集是全球收集的，包含了 4134 个样本的 11 个特征。如果我们加载数据，我们可以探索列并将特征设置为连续或类别。请注意，模型复杂性随着类别数量的增加而增加，这意味着需要更多的数据和计算时间来确定因果 DAG。

```py
# Install datazets.
!pip install datazets
```

```py
# Import library
import datazets as dz
# Get the data science salary data set
df = dz.get('ds_salaries')
# The features are as following
df.columns
# 'work_year'          > The year the salary was paid.
# 'experience_level'   > The experience level in the job during the year.
# 'employment_type'    > Type of employment: Part-time, full time, contract or freelance.
# 'job_title'          > Name of the role.
# 'employee_residence' > Primary country of residence.
# 'remote_ratio'       > Remote work: less than 20%, partially, more than 80%
# 'company_location'   > Country of the employer's main office.
# 'company_size'       > Average number of people that worked for the company during the year.
# 'salary'             > Total gross salary amount paid.
# 'salary_currency'    > Currency of the salary paid (ISO 4217 code).
# 'salary_in_usd'      > Converted salary in USD.
```

## 复杂性是一个主要限制

当特征包含许多类别时，复杂性会随着与该表关联的父节点数量的增加而呈指数增长。换句话说，当你增加类别的数量时，需要大量的数据来获得可靠的结果。可以这样考虑：当你将数据划分为类别时，每个类别中的样本数量在每次划分后会变得更少。每个类别的样本数量低直接影响统计能力。在我们的例子中，我们有一个特征`job_title`，它包含 99 个唯一的职称，其中 14 个职称（如*数据科学家*）包含 25 个样本或更多。剩余的 85 个职称要么是唯一的，要么只出现过几次。为了确保这个特征不会因统计能力不足而被模型移除，我们需要将一些职称进行聚合。在下面的代码部分，我们将职称聚合为 7 个主要类别。这会得到足够样本的类别，以用于贝叶斯建模。

```py
# Group similar job titles
titles = [['data scientist', 'data science', 'research', 'applied', 'specialist', 'ai', 'machine learning'],
          ['engineer', 'etl'],
          ['analyst', 'bi', 'business', 'product', 'modeler', 'analytics'],
          ['manager', 'head', 'director'],
          ['architect', 'cloud', 'aws'],
          ['lead/principal', 'lead', 'principal'],
          ]
```

```py
# Aggregate job titles
job_title = df['job_title'].str.lower().copy()
df['job_title'] = 'Other'
# Store the new names
for t in titles:
    for name in t:
        df['job_title'][list(map(lambda x: name in x, job_title))]=t[0]
print(df['job_title'].value_counts())
# engineer          1654
# data scientist    1238
# analyst            902
# manager            158
# architect          118
# lead/principal      55
# Other                9
# Name: job_title, dtype: int64
```

下一个预处理步骤是重命名一些特征名称。此外，我们还将添加一个新特征，描述公司是否位于*美国*或*欧洲*，并删除一些冗余变量，如`salary_currency`和`salary`。

```py
# Rename catagorical variables for better understanding
df['experience_level'] = df['experience_level'].replace({'EN': 'Entry-level', 'MI': 'Junior Mid-level', 'SE': 'Intermediate Senior-level', 'EX': 'Expert Executive-level / Director'}, regex=True)
df['employment_type'] = df['employment_type'].replace({'PT': 'Part-time', 'FT': 'Full-time', 'CT': 'Contract', 'FL': 'Freelance'}, regex=True)
df['company_size'] = df['company_size'].replace({'S': 'Small (less than 50)', 'M': 'Medium (50 to 250)', 'L': 'Large (>250)'}, regex=True)
df['remote_ratio'] = df['remote_ratio'].replace({0: 'No remote', 50: 'Partially remote', 100: '>80% remote'}, regex=True)
```

```py
import numpy as np

# Add new feature
df['country'] = 'USA'
countries_europe = ['SM', 'DE', 'GB', 'ES', 'FR', 'RU', 'IT', 'NL', 'CH', 'CF', 'FI', 'UA', 'IE', 'GR', 'MK', 'RO', 'AL', 'LT', 'BA', 'LV', 'EE', 'AM', 'HR', 'SI', 'PT', 'HU', 'AT', 'SK', 'CZ', 'DK', 'BE', 'MD', 'MT']
df['country'][np.isin(df['company_location'], countries_europe)]='europe'
# Remove redundant variables
salary_in_usd = df['salary_in_usd']
#df.drop(labels=['salary_currency', 'salary'], inplace=True, axis=1)
```

作为最后一步，我们需要离散化`salary_in_usd`，这可以手动完成，也可以使用*bnlearn*中的`discretizer`函数来完成。为了演示目的，我们将两者都做。在后者的情况下，我们假设薪资依赖于`experience_level`和`country`。更多细节请参见这篇博客[6]。基于这些输入变量，薪资然后被划分为不同的区间（参见下面的代码部分）。

```py
# Discretize the salary feature.
discretize_method='manual'
```

```py
import bnlearn as bn

# Discretize Manually
if discretize_method=='manual':
    # Set salary
    df['salary_in_usd'] = None
    df['salary_in_usd'].loc[salary_in_usd<80000]='<80K'
    df['salary_in_usd'].loc[np.logical_and(salary_in_usd>=80000, salary_in_usd<100000)]='80-100K'
    df['salary_in_usd'].loc[np.logical_and(salary_in_usd>=100000, salary_in_usd<160000)]='100-160K'
    df['salary_in_usd'].loc[np.logical_and(salary_in_usd>=160000, salary_in_usd<250000)]='160-250K'
    df['salary_in_usd'].loc[salary_in_usd>=250000]='>250K'
else:
    # Discretize automatically but with prior knowledge.
    tmpdf = df[['experience_level', 'salary_in_usd', 'country']]
    # Create edges
    edges = [('experience_level', 'salary_in_usd'), ('country', 'salary_in_usd')]
    # Create DAG based on edges
    DAG = bn.make_DAG(edges)
    bn.plot(DAG)
    # Discretize the continous columns
    df_disc = bn.discretize(tmpdf, edges, ["salary_in_usd"], max_iterations=1)
    # Store
    df['salary_in_usd'] = df_disc['salary_in_usd']
    # Print
    print(df['salary_in_usd'].value_counts())
```

## 最终的数据框

最终的数据框有 10 个特征和 4134 个样本。每个特征是一个具有两个或多个状态的分类特征。这个数据框将作为学习结构和确定因果 DAG 的输入。

```py
# work_year           experience_level  ... country salary_in_usd
# 0          2023           Junior Mid-level  ...     USA         >250K
# 1          2023  Intermediate Senior-level  ...     USA      160-250K
# 2          2023  Intermediate Senior-level  ...     USA      100-160K
# 3          2023  Intermediate Senior-level  ...     USA      160-250K
# 4          2023  Intermediate Senior-level  ...     USA      100-160K
#     ...                        ...  ...     ...           ...
# 4129       2020  Intermediate Senior-level  ...     USA         >250K
# 4130       2021           Junior Mid-level  ...     USA      100-160K
# 4131       2020                Entry-level  ...     USA      100-160K
# 4132       2020                Entry-level  ...     USA      100-160K
# 4133       2021  Intermediate Senior-level  ...     USA       60-100K
#
# [4134 rows x 10 columns]
```

# 贝叶斯结构学习用于估计 DAG。

目前，我们已经对数据集进行了预处理，准备开始学习因果结构。*bnlearn* 中实现了六种算法来帮助完成这个任务。我们需要选择一种不需要目标变量的方法，并且它需要能够处理多个类别。可用的搜索策略有：

+   ***爬山搜索*** 算法是一种启发式搜索方法。它从一个空网络开始，根据评分指标迭代地添加或移除边。该算法探索不同的网络结构，并选择得分最高的一个。

+   ***穷举搜索*** 在所有可能的网络结构上进行穷举搜索，以找到最佳的贝叶斯网络。它根据指定的评分指标评估和打分每个结构。虽然这种方法能保证找到最佳的网络结构，但由于可能性呈指数级增长，对于大型网络来说计算开销可能很大。

+   ***约束搜索*** 在贝叶斯网络的结构学习过程中结合用户指定的约束或专家知识。它使用这些约束来引导搜索并限制可能的网络结构空间，确保所学习的网络符合指定的约束。

+   ***周-刘*** 算法是一种用于学习树结构贝叶斯网络结构的方法。它计算每对变量之间的互信息，并通过贪婪地选择最大化网络总互信息的边来构建一棵树。该算法高效且广泛用于学习离散贝叶斯网络的结构，但需要设置一个**根节点**。

+   ***朴素贝叶斯*** 算法假设数据集中所有特征在给定类别变量的条件下是条件独立的。它学习给定类别下每个特征的条件概率分布，并使用贝叶斯定理来计算给定特征下类别的后验概率。尽管有其朴素的假设，这种算法在分类任务中常被使用，并且对于大数据集来说效率较高。

+   ***TAN***（树增强朴素贝叶斯）算法是朴素贝叶斯算法的扩展，允许在给定类别变量的条件下特征之间存在依赖关系。它学习一个连接特征的树结构，并使用该结构来建模条件依赖关系。TAN 将朴素贝叶斯的简单性与一定的建模能力相结合，使其成为处理相关特征分类任务的热门选择。此方法需要设置一个**类别节点**。

评分类型 *BIC、K2、BDS、AIC 和 BDEU* 被用来评估和比较不同的网络结构。例如，*BIC* 平衡了模型复杂性和数据拟合，而其他评分类型考虑了不同类型的先验概率。此外，`independence test` 从模型中剪除虚假的边。在我们的用例中，我将使用 `hillclimbsearch` 方法和评分类型 `BIC` 进行结构学习。我们*不*定义目标值，而是让 *bnlearn* 决定数据的整个因果结构。

```py
# Structure learning
model = bn.structure_learning.fit(df, methodtype='hc', scoretype='bic')
```

```py
# independence test
model = bn.independence_test(model, df, prune=False)
# Parameter learning to learn the CPTs. This step is required to make inferences.
model = bn.parameter_learning.fit(model, df, methodtype="bayes")
# Plot
bn.plot(model, title='Salary data set')
bn.plot(model, interactive=True, title='method=tan and score=bic')
```

![](img/6dd42db64479512bf238fd10face2ed9.png)

图 1\. 结构学习后，得到的因果 DAG。

![](img/6ae6eb059c7bad22e2beafc663f1d90d.png)

图 2\. 因果 DAG 的交互式图。

# 与你的数据集聊天。

使用学习到的**DAG**（图 1 和图 2），我们可以估计条件概率分布（CPTs，见下方代码部分），并使用*do-calculus*进行推断。*让我们* *开始提出问题吧。注意，结果可能会（略微）因模型中的随机成分而变化。*

## 问题 1.

> 在大公司工作时，职位的概率是多少？ `*P(job_title | company_size=Large (>250))*`

*在运行下面的代码部分后，我们可以看到工程科学家是最可能的结果* `*(P=0.34)*` *，其次是数据科学家* `*(P=0.26)*`*。*

```py
query = bn.inference.fit(model, variables=['job_title'],
                         evidence={'company_size': 'Large (>250)'})
```

```py
# +----+----------------+-----------+
# |    | job_title      |         p |
# +====+================+===========+
# |  0 | Other          | 0.031616  |
# +----+----------------+-----------+
# |  1 | analyst        | 0.209212  |
# +----+----------------+-----------+
# |  2 | architect      | 0.0510425 |
# +----+----------------+-----------+
# |  3 | data scientist | 0.265006  |
# +----+----------------+-----------+
# |  4 | engineer       | 0.343216  |
# +----+----------------+-----------+
# |  5 | lead/principal | 0.0407967 |
# +----+----------------+-----------+
# |  6 | manager        | 0.0591106 |
# +----+----------------+-----------+
```

## 问题 2.

> 在全职工作类型、部分远程工作、数据科学职能为入门级且居住在德国（DE）的情况下，薪资范围的概率是多少？

*在下面的结果中，我们可以看到我们的五个薪资类别，其中在这些条件下最强的后验概率* `*P=0.7*` *对应的薪资低于 80K。注意，其他薪资也会出现，但发生的频率较低。*

通过改变变量和证据，我们可以提出各种问题。例如，我们现在可以改变经验水平、居住地、职位等，确定概率如何变化。

```py
query = bn.inference.fit(model,
                         variables=['salary_in_usd'],
                         evidence={'employment_type': 'Full-time',
                                   'remote_ratio': 'Partially remote',
                                   'job_title': 'data scientist',
                                   'employee_residence': 'DE',
                                   'experience_level': 'Entry-level'})
```

```py
# +----+-----------------+-----------+
# |    | salary_in_usd   |         p |
# +====+=================+===========+
# |  0 | 100-160K        | 0.0664068 |
# +----+-----------------+-----------+
# |  1 | 160-250K        | 0.0424349 |
# +----+-----------------+-----------+
# |  2 | 80-100K         | 0.117463  |
# +----+-----------------+-----------+
# |  3 | <80K            | 0.707087  |
# +----+-----------------+-----------+
# |  4 | >250K           | 0.0666078 |
# +----+-----------------+-----------+
```

# 结束语。

在这篇博客中，我们学习了如何创建贝叶斯模型，以及如何使用 do-calculus 对混合数据集进行推断。通过使用*bnlearn*，建立这些模型变得简单明了，模型提供了易于理解和解释的结果，这些结果可以轻松嵌入数据科学流程中。

*保持安全。保持冷静。*

***干杯 E.***

*如果你觉得这篇文章有帮助，欢迎* [*关注我*](https://erdogant.medium.com/subscribe) *，因为我写了更多关于贝叶斯学习的内容。如果你考虑加入 Medium 会员，你可以通过使用我的推荐链接来支持我的工作。这和一杯咖啡的价格相同，但这让你每个月可以无限阅读文章。*

# 软件

+   Bnlearn [Colab Notebook 示例](https://erdogant.github.io/distfit/pages/html/Documentation.html#colab-notebook)

+   [比较八个贝叶斯因果关系库的 PDF 文件](https://erdogant.gumroad.com/l/Comparison_python_libraries_bayesian_causality?layout=profile)

# 让我们联系吧！

+   [在 LinkedIn 上联系我](https://www.linkedin.com/in/erdogant/)

+   [在 Github 上关注我](https://github.com/erdogant)

+   [在 Medium 上关注我](https://erdogant.medium.com/)

# 参考文献

1.  Taskesen, E. (2020). [*使用 bnlearn Python 包学习贝叶斯网络*](https://erdogant.github.io/bnlearn)（版本 0.3.22）[计算机软件]。

1.  Taskesen E, *使用贝叶斯结构学习在 Python 中检测因果关系的逐步指南*，Medium，2021

1.  Taskesen E, *使用贝叶斯定理设计知识驱动模型的逐步指南*，Medium，2021

1.  Taskesen, E. (2020). *贝叶斯因果推断的力量：比较分析库以揭示数据集中隐藏的因果关系*，Medium 2023。

1.  [`ai-jobs.net/salaries/download/`](https://ai-jobs.net/salaries/download/) ([CC0: 公开领域](https://creativecommons.org/publicdomain/zero/1.0/))

1.  Kay H. 等人, [*使用贝叶斯结构时间序列模型推断因果影响*](https://research.google/pubs/pub41854/)，2015，《应用统计年鉴》（247–274，第 9 卷）

1.  Taskesen, E (2023), *创建并探索数据科学中的角色和薪资景观**.* Medium。
