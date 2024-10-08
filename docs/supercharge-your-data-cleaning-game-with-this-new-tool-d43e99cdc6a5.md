# 用这个新工具提升你的数据清洗技能

> 原文：[`towardsdatascience.com/supercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5`](https://towardsdatascience.com/supercharge-your-data-cleaning-game-with-this-new-tool-d43e99cdc6a5)

## PYTHON | 数据 | 分析

## 一份利用 pandas_dq 轻松进行数据清洗的指南

[](https://david-farrugia.medium.com/?source=post_page-----d43e99cdc6a5--------------------------------)![David Farrugia](https://david-farrugia.medium.com/?source=post_page-----d43e99cdc6a5--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d43e99cdc6a5--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d43e99cdc6a5--------------------------------) [David Farrugia](https://david-farrugia.medium.com/?source=post_page-----d43e99cdc6a5--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d43e99cdc6a5--------------------------------) ·阅读时间 5 分钟·2023 年 4 月 19 日

--

![](img/7062261f0283364355b203a6d4dcad89.png)

照片由 [JESHOOTS.COM](https://unsplash.com/@jeshoots?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果这篇文章的标题引起了你的兴趣，那么你肯定意识到数据清洗和预处理步骤对整体分析项目的重要性。

如果你准备训练一个机器学习模型，或者你只是想进行一些探索性数据分析，脏数据无疑是你面前的障碍。你可能听过这样一句话：准备数据是 80%的工作。

数据清洗过程可能是数据分析过程中最耗时、最繁琐、最令人沮丧的部分之一。查找重复项、多重共线性、缺失值或无限值等，都是从理解数据和提取可操作洞察中浪费的宝贵时间。

在这篇文章中，我们将探讨令人惊叹的 Python 包`pandas_dq`，以及它如何提升你下一个数据清洗任务的速度和质量。

[](https://github.com/AutoViML/pandas_dq?source=post_page-----d43e99cdc6a5--------------------------------) [## GitHub - AutoViML/pandas_dq: 用一行代码发现数据质量问题并清理数据…

### 用兼容 Scikit-Learn 的转换器用一行代码发现数据质量问题并清理数据。…

[github.com](https://github.com/AutoViML/pandas_dq?source=post_page-----d43e99cdc6a5--------------------------------)

# 首先…

我们需要安装这个包。`pandas_dq` 可以通过以下方式获得：

```py
pip install pandas_dq
```

或者，你可以从源代码安装：

```py
# download from https://github.com/AutoViML/pandas_dq/archive/master.zip
cd <pandas_dq_Destination>
git clone git@github.com:AutoViML/pandas_dq.git
```

# 它是如何工作的？

给定一个数据集，开始使用这个工具非常简单。它目前有 3 个主要功能：

+   dq_report

+   Fix_DQ

+   DataSchemaChecker

## dq_report

这个函数的目的是生成包含数据集所有数据质量问题的报告。

假设我们有 iris 数据集

```py
import pandas as pd
df = pd.read_csv('https://raw.githubusercontent.com/mwaskom/seaborn-data/master/iris.csv')
```

我们可以如下运行 `dq_report`：

```py
from pandas_dq import dq_report

dq_report(df, target=None, verbose=1)
```

这将给我们带来以下结果：

![](img/8ef127357eef9f4d9cbc774378c8ea35.png)

作者提供的图像

该包迅速告诉我们特征`sepal_width`有 4 个异常值，并建议我们要么对其进行修剪（将任何异常值设置为最大值），要么将其移除。它还可以识别是否存在多重共线性特征（高度相关的特征），例如`petal_length`和`petal_width`。

我们还有选项运行针对目标的数据质量检查。在有监督学习任务中（每当我们有目标/标签需要预测时），我们还需要检查特征与目标之间的关系。`dq_report`通过允许我们指定目标列，使这一过程变得非常简单。

***注意****：目标列不能是非数值型值*

```py
# map string target to numeric
df['species'] = pd.factorize(df['species'])[0]

dq_report(df, target='species', verbose=1)
```

![](img/727c6720745184bdc7dbd74a2b617750.png)

作者提供的图像

这个函数执行所有这些检查：

1.  *它检测 ID 列*

1.  *它检测零方差列*

1.  *它识别稀有类别（少于列中 5%的类别）*

1.  *它查找列中的无限值*

1.  *它检测混合数据类型（即具有多于一种数据类型的列）*

1.  *它检测异常值（即超出四分位范围的浮点列）*

1.  *它检测高基数特征（即具有超过 100 个类别的特征）*

1.  *它检测高度相关的特征（即两个特征的绝对相关性高于 0.8）*

1.  *它检测重复行（即数据集中同一行出现多次）*

1.  *它检测重复列（即数据集中同一列出现两次或更多次）*

1.  *它检测偏斜分布（即偏斜度超过 1.0 的特征）*

1.  *它检测不平衡的类别（即目标变量中的某一类别显著多于其他类别）*

1.  *它检测特征泄漏（即与目标高度相关的特征，相关性 > 0.8）*

## Fix_DQ

这个函数执行`dq_report`所做的所有检查，但也在一行代码中进行处理。这通常在准备建模时对一组特征（排除目标列）进行操作。

```py
from pandas_dq import Fix_DQ

fdq = Fix_DQ()
result = fdq.fit_transform(df.drop('species', axis=1))
```

我们得到以下输出：

```py
Alert: Detecting 1 duplicate rows...
    Dropping petal_length which has a high correlation with ['sepal_length']
    Dropping petal_width which has a high correlation with ['sepal_length', 'petal_length']
Alert: Dropping 1 duplicate rows can sometimes cause column data types to change to object. Double-check!
```

以及结果数据框：

![](img/d72a22d428c4105803f451673175a48e.png)

作者提供的图像

## DataSchemaChecker

这个函数接受一个数据模式，并确保我们的数据框遵循该模式。这在我们想确保列的数据类型时很有用，无论是因为我们想在使用预训练模型时确保一致的数据类型，执行某种类型验证、序列化，甚至将其导入到数据库中。

这个函数的主要特点（可能是一个 bug）是当没有数据类型问题时，它会输出 AttributeError。

```py
from pandas_dq import DataSchemaChecker

wrong_schema = dict(zip(df.columns, ['float64', 'float64', 'float64', 'float64', 'float64']))
ds = DataSchemaChecker(schema=wrong_schema)
ds.fit_transform(df)
```

![](img/a9867db2459e339221a642f26a0343ce.png)

作者提供的图片

# 总结

`pandas_dq` 仍然是一个相对较新的工具，它极大地改善了我的数据清洗过程。除了自动化整个流程，它还以惊人的速度执行这些步骤。你绝对需要检查它的结果并深入挖掘——但它在缩小焦点方面非常有帮助——在这个过程中节省了宝贵的时间。

**你喜欢这篇文章吗？只需每月 $5，你就可以成为会员，解锁对 Medium 的无限访问权限。你将直接支持我以及你在 Medium 上的其他喜爱作家。非常感谢！**

[](https://david-farrugia.medium.com/membership?source=post_page-----d43e99cdc6a5--------------------------------) [## 使用我的推荐链接加入 Medium - David Farrugia

### 获取对我所有⚡高级⚡内容的独家访问权限，并在 Medium 上畅享无限制的阅读。通过购买我提供的服务来支持我的工作…

[david-farrugia.medium.com](https://david-farrugia.medium.com/membership?source=post_page-----d43e99cdc6a5--------------------------------)

## 想要联系我吗？

我很想听听你对这个话题的看法，或者对 AI 和数据的任何想法。

如果你希望联系我，请发邮件到 ***davidfarrugia53@gmail.com***。

[Linkedin](https://www.linkedin.com/in/david-farrugia/)
