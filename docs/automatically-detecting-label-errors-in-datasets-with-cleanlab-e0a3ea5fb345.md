# 使用 CleanLab 自动检测数据集中的标签错误

> 原文：[https://towardsdatascience.com/automatically-detecting-label-errors-in-datasets-with-cleanlab-e0a3ea5fb345?source=collection_archive---------3-----------------------#2023-07-22](https://towardsdatascience.com/automatically-detecting-label-errors-in-datasets-with-cleanlab-e0a3ea5fb345?source=collection_archive---------3-----------------------#2023-07-22)

## 一则关于人工智能和错误分类的巴西联邦法律的故事

[](https://joaopedro214.medium.com/?source=post_page-----e0a3ea5fb345--------------------------------)[![João Pedro](../Images/64a0e14527be213e5fde0a02439fbfa7.png)](https://joaopedro214.medium.com/?source=post_page-----e0a3ea5fb345--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e0a3ea5fb345--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e0a3ea5fb345--------------------------------) [João Pedro](https://joaopedro214.medium.com/?source=post_page-----e0a3ea5fb345--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb111eee95c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomatically-detecting-label-errors-in-datasets-with-cleanlab-e0a3ea5fb345&user=Jo%C3%A3o+Pedro&userId=b111eee95c&source=post_page-b111eee95c----e0a3ea5fb345---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0a3ea5fb345--------------------------------) ·10 分钟阅读·2023年7月22日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe0a3ea5fb345&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomatically-detecting-label-errors-in-datasets-with-cleanlab-e0a3ea5fb345&user=Jo%C3%A3o+Pedro&userId=b111eee95c&source=-----e0a3ea5fb345---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe0a3ea5fb345&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fautomatically-detecting-label-errors-in-datasets-with-cleanlab-e0a3ea5fb345&source=-----e0a3ea5fb345---------------------bookmark_footer-----------)![](../Images/36477c5670bfa2ec58e04ee972390139.png)

照片由 [Gustavo Leighton](https://unsplash.com/@g_leighton?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 引言

几周前，当我在进行个人项目的数据集搜索时，我偶然发现了巴西众议院开放数据门户网站，这里包含了大量的数据——包括议员的费用、政党元数据等——所有这些数据都可以通过一个不错的 [API](https://dadosabertos.camara.leg.br/swagger/api.html#staticfile) 获取。

几小时的搜索和检查后，有一些非常有趣的事情引起了我的注意：所有由议员提出的法律的汇编，包括它们的‘*ementas*’（简要摘要）、作者、年份，更重要的是，它们的**主题**（健康、安全、金融等）——由议会文献和信息中心（Centro de Documentação e Informação da Câmara）分类。

我的脑海中闪现出一个灵光——“*我将创建一个监督分类管道来* ***预测法律的主题******使用其摘要****，探索机器学习的一些基础设施方面，如使用DVC的数据版本控制或类似的东西*。”我迅速编写了一个脚本，收集了一个包含超过60,000条法律的大型数据集，时间跨度从1990年到2022年。

我已经稍微涉猎过司法和立法数据，所以我有一种感觉这个任务不会很难。但是，为了更轻松些，我选择仅分类法律提案（LP）是否涉及“*纪念和纪念日期*”（二分类）。理论上，这应该很简单，因为文本非常简单：

![](../Images/a26315bf71eaec67b082f036007bfbbb.png)

原始摘要和ChatGPT的字面翻译 - I。图片由作者提供。

但是，无论我尝试了什么，我的性能在f1分数上始终未能超过~0.80，且（正类的）召回率相对较低，为0.5–0.7。

当然，我的数据集高度不平衡，这一类别占数据集总量的不到5%，但还有更多因素。

经过一些调查，使用基于正则表达式的查询检查数据，查看错误分类记录后，我发现了几个错误标记的例子。通过我粗略的方法，我发现了~200个假阴性，占“真实”正例的~7.5%和我数据集的0.33%，还没有提到假阳性。见下图：

![](../Images/86340626c166a22cac095a5946da6dbd.png)

错误分类的例子。图片由作者提供。

这些例子影响了我的验证指标——“*它们可能有多少个？我需要手动搜索错误吗？*”

然而，**Confident Learning** 作为**Clean Lab** Python包的形式出现，来拯救我了。

![](../Images/1e88f7af7141237d5d19f6b5f3c4f018.png)

Clean Lab标志。图片来自[GitHub](https://github.com/cleanlab/cleanlab)。

# 什么是Confident Learning？

正确标记数据是任何监督机器学习项目中最耗时且最昂贵的步骤之一。像众包、半监督学习、微调等技术尝试减少收集标签的成本或减少模型训练中对这些标签的需求。

幸运的是，我们已经在这个问题上领先一步。我们有专业人士提供的标签，可能是具备足够*专业知识*的政府工作人员。但我以粗略正则表达式处理的非专业视角一旦超出我的性能预期，就能发现错误。

关键点是：数据中还有多少错误？

检查每一条法律是不现实的——需要一种 **自动** 检测 **错误标签** 的方法，这就是 Confident Learning 的作用。

总结一下，它使用从模型概率预测中收集的统计数据来估计数据集中的错误。它可以检测噪声、离群值，以及——本篇文章的主要内容——**标签错误**。

我不会详细介绍 CL，但有一篇[很好的文章](/confident-learning-err-did-you-say-your-data-is-clean-ef2597903328)涵盖了它的主要要点，还有一个来自 **CleanLab** 创始人的[YT 视频](https://youtu.be/BnOTv0f9Msk)谈论它在该领域的研究。

让我们看看它在实践中是如何工作的。

# 数据

数据来自巴西众议院开放数据门户，包含 1990 年至 2022 年的法律提案（LP）。最终的数据集包含约 60K LPs。

单个 LP 可能与多个主题相关，例如健康和金融，这些信息也可以在开放数据门户中找到。为了更方便处理，我通过将每个单独的主题二值化到单独的列中来编码主题信息。

如前所述，本篇文章使用的主题是“*致敬和纪念日期*”。我选择它是因为它的 *ementas* 非常简短且简单，因此标签错误很容易识别。

数据和代码可以在[项目的 GitHub 仓库](https://github.com/jaumpedro214/data-analysis-camara-federal-pls)中找到。

# 实现

我们的目标是自动修复“*致敬和纪念日期*”中的每一个标签错误，并使这个帖子以一个干净、整洁的数据集作为结束，准备好用于机器学习问题。

## 设置环境

运行这个项目所需的是经典的 ML/Data Science Python 包（Pandas, Numpy 和 Scikit-Learn）+ CleanLab 包。

```py
cleanlab==2.4.0
scikit-learn==1.2.2
pandas>=2.0.1
numpy>=1.20.3
```

只需安装这些要求，我们就可以开始了。

## 使用 CL 检测标签错误

CleanLab 包自带识别许多数据集问题的能力，如离群值和重复/接近重复的条目，但我们只对 **标签错误** 感兴趣。

**CleanLab** 使用由机器学习模型生成的概率，这些概率代表了条目被标记为某个标签的信心。如果数据集有 *n* 个条目和 *m* 个类别，那么这将由一个 *n* x *m* 的矩阵 P 表示，其中 P[i, j] 代表第 *i* 行属于类别 *j* 的概率。

这些概率和“真实”标签被用在 CleanLab 的内部以估计错误。

让我们练习一下：

*导入包*

```py
import numpy as np
import pandas as pd

from sklearn.feature_extraction.text import TfidfVectorizer
from sklearn.ensemble import RandomForestClassifier

from sklearn.model_selection import train_test_split, cross_val_score, cross_val_predict
from sklearn.model_selection import GridSearchCV, StratifiedKFold
from sklearn.pipeline import Pipeline

from sklearn.metrics import accuracy_score, precision_score, recall_score, f1_score
from sklearn.metrics import confusion_matrix, classification_report

from cleanlab import Datalab

RANDOM_SEED = 214
np.random.seed(RANDOM_SEED)
```

*加载数据中…*

```py
df_pls_theme = pd.read_parquet(
    '../../data/proposicoes_temas_one_hot_encoding.parquet'
)

#              "Tributes and commemorative dates"
BINARY_CLASS = "Homenagens e Datas Comemorativas"
IN_BINARY_CLASS = "in_" + BINARY_CLASS.lower().replace(" ", "_")

df_pls_theme = df_pls_theme.drop_duplicates(subset=["ementa"])
df_pls_theme = df_pls_theme[["ementa", BINARY_CLASS]]
df_pls_theme = df_pls_theme.rename(
    columns={BINARY_CLASS: IN_BINARY_CLASS}
)
```

首先，让我们生成概率。

正如 CleanLab 文档中提到的，为了实现更好的性能，生成的概率必须 [基于样本外记录](https://docs.cleanlab.ai/stable/tutorials/pred_probs_cross_val.html#pred-probs-cross-val)（‘非训练’数据）。这是很重要的，因为模型在预测训练数据上的概率时，通常会过于自信。生成样本外概率的最常用方法是使用 K-Fold 策略，如下所示：

```py
y_proba = cross_val_predict(
    clean_pipeline, 
    df_pls_theme['ementa'], 
    df_pls_theme[IN_BINARY_CLASS],
    cv=StratifiedKFold(n_splits=5, shuffle=True, random_state=RANDOM_SEED), 
    method='predict_proba', 
    verbose=2,
    n_jobs=-1
)
```

> 注意：重要的是要了解类别分布 —— 因此使用了 StratifiedKFold 对象。所选类别在数据集中所占比例不足 5%，一种天真的采样方法可能会导致模型在不正确平衡的数据集上生成低质量的概率。

CleanLab 使用一个名为 Datalab 的类来处理其错误检测任务。它接收包含我们数据的 DataFrame 和标签列的名称。

```py
lab = Datalab(
    data=df_pls_theme,
    label_name=IN_BINARY_CLASS,
)
```

现在，我们只需将先前计算的概率传递给它……

```py
lab.find_issues(pred_probs=y_proba)
```

……开始查找问题

```py
lab.get_issue_summary("label")
```

![](../Images/18b86c0c2d30b30543c431a2c24802a7.png)

就这么简单。

*get_issues*(”label”) 函数返回一个 DataFrame，包含 CleanLab 为每条记录计算的指标和指标。最重要的列是 ‘*is_label_issue*’ 和 ‘*predicted_label*’，分别表示记录是否存在标签问题以及可能的正确标签。

```py
lab.get_issues("label")
```

我们可以将这些信息合并到原始 DataFrame 中，以检查哪些示例是有问题的。

```py
# Getting the predicted errors
y_clean_labels = lab.get_issues("label")[['predicted_label', 'is_label_issue']]

# adding them to the original dataset
df_ples_theme_clean = df_pls_theme.copy().reset_index(drop=True)
df_ples_theme_clean['predicted_label'] = y_clean_labels['predicted_label']
df_ples_theme_clean['is_label_issue'] = y_clean_labels['is_label_issue']
```

我们来看几个例子：

![](../Images/3860e3258b8a00fd1fdbe455d23c18ab.png)![](../Images/78d445cc8ecdad5f1e1925259f2b4547.png)

对我来说，这些法律显然与*致敬和纪念日期*相关；然而，它们并没有被适当地分类为这些。

很好！—— CleanLab 能够在我们的数据集中找到 312 个标签错误，但现在该怎么办？

这些错误可以是手动检查以进行修正的对象（以主动学习的方式）或立即修正（假设 CleanLab 做得对）。前者更耗时，但可能会导致更好的结果，而后者更快，但可能会导致更多错误。

无论选择哪条路径，CleanLab 都将记录的劳动量从 60K 减少到几百条 —— 在最坏的情况下。

*但有一个陷阱。*

我们怎么能确保 CleanLab 找到了数据集中的**所有**错误？

实际上，如果我们运行上述管道，但将错误修正为真实标签，CleanLab 将会发现更多错误……

错误更多，但*希望*比第一次运行的错误少。

我们可以重复这个逻辑任意次数：找出错误，修正错误，用新假定的更高质量标签重新训练模型，再次找出错误……

![](../Images/10bf4f2f521ce9afa9cba9dd8aabecb2.png)

迭代修正错误。图片由作者提供。

希望在一些交互之后，错误的数量将会是零。

## 使用 CleanLab 迭代修正错误

要实现这个想法，我们只需在循环中重复上述过程，下面的代码正是这样做的：

让我们来回顾一下。

在每次迭代中，OOS 概率是按照之前展示的方式生成的：使用*cross_val_predict*方法和StratifiedKFold。当前的概率集（每次迭代中）用于构建一个新的Datalab对象并发现新的标签问题。

发现的问题与当前数据集合并并修复。

我选择了将修复后的标签作为新列附加的策略，而不是替换原始列。

![](../Images/8de06e839952d37d0a01ab1a3d2f79db.png)

附加修复后的标签。图片由作者提供。

> LABEL_COLUMN_0 是原始标签，LABEL_COLUMN_1 是修复1次的标签列，LABEL_COLUMN_2 是修复2次的标签列，以此类推……

除了这个过程之外，还计算并存储了常规分类指标，以备后续检查。

经过8次互动（约16分钟），过程完成了。

# 结果

下表显示了在过程中的性能指标。

![](../Images/4703adfb83c3b6e815df7719203e2aef.png)

在8次迭代中，数据集中发现了总计**393个标签错误**。正如预期的那样，发现的错误数量随着每次迭代的增加而减少。

有趣的是，这个过程在仅仅6次迭代中就能够“收敛”到一个“解决方案”——在最后2次迭代中错误数保持为0。这是一个很好的迹象，说明在这种情况下，CleanLab的实现是稳健的，并且没有发现任何可能导致振荡的‘偶然’错误。

尽管错误的数量仅占数据集的0.6%，f1分数从0.81提高到了0.90，约11%。这可能是由于类别高度不平衡，因为新的322个正标签总共占原始正样本的约12%。

但CleanLab真的能够发现有意义的错误吗？让我们检查几个例子看看它们是否有意义。

**假阴性已修复**

![](../Images/3cd56b85d5a050f44f03bbfaa5af56e4.png)

原始总结和ChatGPT的逐字翻译 — II. 图片由作者提供。

上述文本确实类似于“致敬和纪念日期”，这表明它们应该被适当地分类为此类 — 指向CleanLab

**假阳性已修复**

![](../Images/b6e491d0925dc75bee1219f14afc62bb.png)

原始总结和ChatGPT的逐字翻译 — III. 图片由作者提供。

在这种情况下我们有一些错误，第2和第4条规则不是假阳性。虽然不是很好，但还算可以。

我重复进行了这个检查采样新‘固定’规则的操作几次，总的来说，CleanLab在检测假阴性方面表现几乎完美，但对假阳性有点困惑。

现在，即使我们可能没有一个*完美*标记的数据集，我也更**自信**现在用它来训练机器**学习**模型了。

# 结论

机器学习领域长期以来受困于模型质量差和计算能力不足，但这种情况已经过去。现在，大多数机器学习应用的真正瓶颈是数据。不是原始数据，而是经过精炼的数据——具有良好标签、格式规范、噪音或异常值较少的数据。

因为无论模型多么庞大和强大，无论你在管道中混入多少统计和数学，这些都无法拯救你免于计算机科学的最基本法则：垃圾进，垃圾出。

这个项目见证了这一原则——我测试了几个模型、深度学习架构、采样技术和向量化方法，最终发现问题出在基础上：我的数据是错误的。

在这种情况下，投资于数据质量技术成为创建成功机器学习项目的关键方面。

在这篇文章中，我们探讨了 CleanLab，这个包帮助我们检测和修复数据集中的错误标签。它不仅显著提高了数据集的质量，而且以自动、可重复且廉价的方式完成——无需人工干预。

我希望这个项目能帮助你更好地理解自信学习和 CleanLab 包。正如往常一样，我并不是这些主题的专家，我强烈建议你进一步阅读，以下是一些参考文献。

感谢您的阅读！😉

# 参考文献

> 所有代码都可以在[这个 GitHub 仓库](https://github.com/jaumpedro214/data-analysis-camara-federal-pls)中找到。
> 
> 使用的数据 — [开放数据门户联邦议会](https://dadosabertos.camara.leg.br/faq/faq-home.html#r14). [开放数据 — 法律 nº 12.527]
> 
> 除非另有说明，所有图像均由作者创建。

[1] Cleanlab. (无日期). *GitHub — cleanlab/cleanlab: 标准的数据中心 AI 包，用于处理混乱的真实世界数据和标签的数据质量和机器学习。* [GitHub](https://github.com/cleanlab/cleanlab).

[2][*计算样本外预测概率的交叉验证*](https://docs.cleanlab.ai/stable/tutorials/pred_probs_cross_val.html#pred-probs-cross-val) *— cleanlab*. (无日期).

[3] Databricks. (2022年7月19日). *CleanLab: 用于发现和修复 ML 数据集中的错误的 AI* [视频]. [YouTube](https://www.youtube.com/watch?v=BnOTv0f9Msk).

[4][*常见问题*](https://docs.cleanlab.ai/stable/tutorials/faq.html) *— cleanlab*. (无日期).

[5] Mall, S. (2023年5月25日). 标签错误是否不可避免？自信学习是否有用？[*Medium*](/confident-learning-err-did-you-say-your-data-is-clean-ef2597903328).

[6] Northcutt, C. G. (2021). *自信学习：估计数据集标签的不确定性*. [arXiv.org](https://arxiv.org/abs/1911.00068).
