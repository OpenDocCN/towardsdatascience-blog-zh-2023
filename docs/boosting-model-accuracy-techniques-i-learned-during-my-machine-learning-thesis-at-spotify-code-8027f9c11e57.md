# 提升模型准确性：我在 Spotify 机器学习论文中学到的技术（+代码片段）

> 原文：[`towardsdatascience.com/boosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57`](https://towardsdatascience.com/boosting-model-accuracy-techniques-i-learned-during-my-machine-learning-thesis-at-spotify-code-8027f9c11e57)

## 改善顽固 ML 模型的技术数据科学家工具栈

[](https://medium.com/@elalamik?source=post_page-----8027f9c11e57--------------------------------)![Khouloud El Alami](https://medium.com/@elalamik?source=post_page-----8027f9c11e57--------------------------------)[](https://towardsdatascience.com/?source=post_page-----8027f9c11e57--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----8027f9c11e57--------------------------------) [Khouloud El Alami](https://medium.com/@elalamik?source=post_page-----8027f9c11e57--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----8027f9c11e57--------------------------------) ·12 分钟阅读·2023 年 8 月 24 日

--

*这篇文章是记录我在 Spotify 机器学习论文中学习内容的两部分之一。请务必查看* *第二篇关于我如何在这项研究中实现特征重要性**.*

[](/feature-importance-analysis-with-shap-i-learned-at-spotify-aacd769831b4?source=post_page-----8027f9c11e57--------------------------------) ## SHAP 中的特征重要性分析，我在 Spotify 学习到的（在复仇者的帮助下）

### 使用 SHAP 识别关键特征并了解它们如何影响机器学习模型的预测结果

towardsdatascience.com

在 2021 年，我花了 8 个月时间构建一个预测模型，以测量*用户满意度*，这是我在 Spotify 论文的一部分。

![](img/2106c3fd9d7bd76cadf0157aecf85277.png)

图片由作者提供

我的目标是理解是什么使用户对他们的音乐体验感到满意。为此，我构建了一个 LightGBM 分类器，其输出是一个二元响应：

*y = 1 → 用户似乎满意

y = 0 → 不怎么满意*

预测人类满意度是一个挑战，因为人类本质上是不满足的。即使是机器也不适合解读人类心理的奥秘。所以，自然地，我的模型也陷入了困惑。

## 从人类预测者到占卜师

我的准确率约为 0.5，这在分类器中是最糟糕的结果。这意味着算法有 50%的概率预测“是”或“否”，这与人类的猜测一样随机。

所以我花了 2 个月尝试和结合不同的技术来提高模型的预测能力。最后，我终于将 ROC 分数从 0.5 提高到 0.73，这是一个巨大的成功！

在这篇文章中，我将与您分享我用来显著提高模型准确性的技术。当你的模型无法合作时，这篇文章可能会很有用。

*由于这项研究的保密性，我不能分享敏感信息，但我会尽力确保内容不令人困惑。*

# 但首先，确保订阅我的通讯！

点击下面的链接，我会发送更多**个性化内容和内幕技巧**，帮助你成为数据科学家！

[## 加入+1k 读者 💌 关注我在科技+Spotify 的数据科学之旅，别错过！](https://medium.com/@elalamik/subscribe?source=post_page-----8027f9c11e57--------------------------------)

### 加入+1k 读者 💌 关注我在科技+Spotify 的数据科学之旅，别错过！通过注册，你…

[medium.com](https://medium.com/@elalamik/subscribe?source=post_page-----8027f9c11e57--------------------------------)

# #0\. 数据准备

在深入探讨我使用的方法之前，我想确保你首先掌握基础知识。其中一些方法依赖于对变量的编码以及数据的相应准备，以便它们能够正常工作。我包含的一些代码片段也引用了我在数据准备部分创建的用户定义函数，因此请务必检查它们。

![](img/bba31f366c84e7be4e7fca230a55f625.png)

这是我实现步骤中的管道顺序

## 1\. 编码变量

确保你的变量已编码：

+   *序数特征，* 以便模型保留序数信息

+   *类别特征，* 以便模型能够解释名义数据

所以首先，让我们将变量存储在某个地方。同样，因为研究是保密的，我不能透露使用的数据，所以我们先用这些数据：

```py
region = ['APAC', 'EU', 'NORTHAM', 'MENA', 'AFRICA']
user_type = ['premium', 'free']

ordinal_list = ['region', 'user_type']
```

然后，确保构建编码变量的函数：

```py
from sklearn.preprocessing import OneHotEncoder, OrdinalEncoder

def var_encoding(X, cols, ordinal_list, encoding):

    #Function to encode ordinal variables
    if encoding == 'ordinal_ordered': 
        encoder = OrdinalEncoder(categories=ordinal_list) 
        encoder.fit(X.loc[:, cols])
        X.loc[:, cols] = encoder.transform(X.loc[:, cols])

    #Function to encode categorical variables    
    elif encoding == 'ordinal_unordered':
        encoder = OrdinalEncoder()
        encoder.fit(X.loc[:, cols])
        X.loc[:, cols] = encoder.transform(X.loc[:, cols])

    else:     
        encoder = OneHotEncoder(handle_unknown='ignore')
        encoder.fit(df.loc[:, cols])
        df.loc[:, cols] = encoder.transform(df.loc[:, cols])

    return X
```

然后将该函数应用于你的变量列表。这意味着你需要创建包含变量名称的字符串的列表，即为*序数*变量、*类别*变量和*数值*变量分别创建一个列表。

```py
def encoding_vars(X, ordinal_cols, ordinal_list, preprocessing_categoricals=False):

    #Encode ordinal variables
    df = var_encoding(df, ordinal_cols, ordinal_list, 'ordinal_ordered')

    #Encode categorical variables
    if preprocessing_categoricals: 
       df = var_encoding(df, categorical_cols, 'ordinal_unordered')    

    #Else set your categorical variables as 'category' if needed
    else:
        for cat in categorical_cols: 
            X[cat] = X[cat].astype('category')

    #Rename your variables as such if needed to keep track of the order
    #An encoded feature such as region will no longer show female or male, but 0 or 1
    df.rename(columns={'user_type': 'free_0_premium_1'},   
    df.reset_index(drop=True, inplace=True)   

    return df
```

## 2\. 划分数据

划分你的数据框以获得*训练集*、*验证集*和*测试集*：

+   **训练集** — 用于在你选择的算法上训练模型，例如 LightGBM

+   **验证集** — 用于超调参数和优化预测结果

+   **测试集** — 用于进行最终预测

## 🔊 记住

在我的研究中，我将数据划分了两次以满足不同的目的。第一次划分在一开始进行，以基于用户级别的划分来创建训练集、验证集和测试集。另一种划分则在进行交叉验证和超参数调整时发生。

初始划分允许数据的更灵活和随机分割，确保每个集合中用户的多样性。测试集留作最终模型评估，而训练集和验证集则用于模型开发和超参数调优。

在我的研究中，我使用了`**GroupShuffleSplit**`，如下：

```py
from sklearn.model_selection import GroupShuffleSplit

def split_df(df, ordinal_cols, ordinal_list, target):
    #splitting train and test
    splitter = GroupShuffleSplit(test_size=.13, n_splits=2, random_state=7)
    split = splitter.split(df, groups=df['user_id'])
    train_inds, test_inds = next(split)

    train = df.iloc[train_inds]
    test = df.iloc[test_inds]

    #splitting validation and test
    splitter2 = GroupShuffleSplit(test_size=.5, n_splits=2, random_state=7)
    split = splitter2.split(test, groups=test['user_id'])
    val_inds, test_inds = next(split)

    val = test.iloc[val_inds]
    test = test.iloc[test_inds]

    #defining X and y
    X_train = train.drop(['target_variable'], axis=1)
    y_train = train.target_variable

    X_val = val.drop(['target_variable'], axis=1)
    y_val = val.target_variable

    X_test = test.drop(['target_variable'], axis=1)
    y_test = test.target_variable

    #encoding the variables in the sets based on a pre-defined encoding function
    X_train = encoding_vars(X_train, ordinal_cols, ordinal_list)
    X_val = encoding_vars(X_val, ordinal_cols, ordinal_list)
    X_test = encoding_vars(X_test, ordinal_cols, ordinal_list)

    return X_train, y_train, X_val, y_val, X_test, y_test
```

# #1\. 特征工程

特征工程在提高模型准确性方面产生了巨大差异。

当涉及到用户收听满意度时，我想知道它是更依赖于用户、他们的流媒体行为，还是其他因素。虽然我拥有的初步用户数据有意义，但缺乏足够的信息增益和预测能力。

我优化过程中的最重要步骤变成了创建能够更好地捕捉用户满意度的新特征。

正如名字所示，创建新特征是一个*创造性*的过程，这意味着你需要坐下来发挥你的领域知识，思考捕捉重要信息的新颖方法。

我在这个过程中使用的两个主要方法是：

1.  **特征交互。** 我所做的最重要的变换是将已经存在的特征组合在一起，以创建比率。

    *例如：假设我有一个衡量总播放分钟数的特征，还有一个跟踪新发行曲目的总播放分钟数的特征。我可以在这里做的事情是提取来自新发行的播放分钟数，然后将其除以总播放分钟数，以创建“新音乐播放比率”。这捕捉了完全新的信息。*

1.  **特征聚合。** 我做的另一件事是对数据进行时间和组的聚合，以创建汇总特征，如均值或标准差。这意味着你可以在不同的时间组上创建相同的特征，但覆盖不同的聚合。

    *例如：计算过去 7 天、14 天和 30 天每个播放列表每日播放的曲目数量的平均值。瞧，你刚刚解锁了新信息。*

## 🔊 请记住

特征工程也是一个迭代的过程。你可能需要尝试不同的特征组合、变换和技术，以找到适合你特定问题的最佳特征集。

总是用新的特征在单独的验证集上验证模型的表现，以确保改进不是由于过拟合。

# #2\. 特征选择

因此，我向模型输入了许多特征，但并不真正知道哪些是相关的。我们可能认为变量越多，模型学习得越好，但如果模型从所有内容中学习，包括垃圾，这最终会比任何东西都更有害。

特征过多意味着其中一些可能会给模型引入噪声，这很糟糕，因为它：

1.  隐藏数据中的潜在模式或关系。

1.  导致过拟合，因为模型从噪声中学习而不是从真实关系中学习。

1.  增加复杂性并减慢训练速度。

为了避免所有这些问题，我们可以使用诸如皮尔逊相关系数、递归特征消除或卡方检验等方法追踪罪魁祸首。

在我的案例中，我使用了前两种方法。

## 皮尔逊相关系数

该系数衡量两个或更多变量之间的**线性**关系。

它是两个特征协方差与它们标准差乘积的比率。最终输出在-1 到 1 之间，其中 1 表示变量之间的正*线性*关系，而-1 表示负关系。

皮尔逊相关系数在特征选择中有两个用途：

1.  **过滤掉最不重要的特征**，这些特征往往与目标变量的相关性较低。

1.  **限制变量之间的多重共线性**以避免因数据冗余而产生的过拟合。

**为什么使用？** 这是一个计算便宜的统计方法，用于捕捉因变量的内在属性。

**如何使用？** 相关热图指出了变量之间的线性关系。

```py
def corr_matrix(df):
    # Select upper triangle of correlation matrix
    upper = df.corr().abs().where(np.triu(np.ones(df.corr().abs().shape), k=1).astype(np.bool))

    return upper

def cols_todrop(corr_matrix, threshold):
    # Find features with correlation greater than x (you pick your threshold)
    to_drop = [col for col in corr_matrix.columns if any(corr_matrix[col] > threshold)]

    return to_drop
```

```py
# Get a ranking of top 10 features with the highest correlation based on your threshold
upper = corr_matrix(data)
upper.returned_1d.sort_values(ascending=False)[:10]
```

```py
# Plot the correlation heatmap
plt.figure(figsize=(16, 6))

heatmap = sns.heatmap(data.corr(), vmin=-1, vmax=1, annot=True, cmap='BrBG')

heatmap.set_title('Correlation Heatmap', fontdict={'fontsize':15}, pad=12)
plt.savefig('heatmap_pre.png', dpi=300, bbox_inches='tight')

plt.show()
```

## 🚨 小心非线性关系！

有时变量之间也可能存在非线性关系，这意味着在过滤多重共线性特征时需要小心。

识别非线性关系可以提供更细致和准确的数据洞察，这意味着你可能想保留这些特征。为此，你可以使用诸如斯皮尔曼等级相关、肯德尔秩相关、散点图等替代方法。

## 递归特征消除

它通过使用重要性算法对特征进行加权和排序，递归地缩小特征范围。从所有特征开始，它适配所选的机器学习模型，对特征进行排序，并用更小的子集迭代，直到达到所需的特征数量（你最初设定的数量）。

**为什么使用？** 结果是按重要性排序的特征，这使我们能够将预测能力最差的特征踢出局。

## 🚨 小心编码！

RFE 需要对分类变量进行先前的数值编码才能工作，所以请参考初始部分进行变量编码。

```py
from sklearn.feature_selection import RFE
selector = RFE(model, n_features_to_select=30, step=1)
selector = selector.fit(X_train, y_train)

rfe_vars_keys = list(X_train.columns)
rfe_vars_values = list(selector.ranking_)
rfe_vars = dict(zip(rfe_vars_keys, rfe_vars_values))

sorted(rfe_vars.items(), key=lambda x: x[1])
```

我在筛选最不重要的特征时，结合了这两种方法的结果：

+   使用皮尔逊相关系数，我没有发现因变量和目标变量之间有强线性关系。因此，我保留了所有特征 *(我也害怕删除非线性关系)。*

+   使用递归特征消除，我移除了最低排名的特征 *(因为为什么不呢)*。

# #3\. 超参数调整

超参数调整是优化机器学习模型时的强制步骤。基本上，这是寻找能够为你的模型提供最佳性能的参数组合的过程。

在我的研究中，我使用了一种结合`**GroupKFold**`交叉验证和`**RandomizedSearchCV**`的两步策略进行超参数调优，这是一种最佳组合，因为：

1.  示例数据量非常大*(30 万用户)*。

1.  用户数据需要适当地拆分*(我们不想在所有分割中都找到 K 的流数据，不不)*。

## **第 1 步：** 使用 GroupKFold 防止数据泄漏

我的数据包含了多个用户的记录。由于数据会被用于超参数调优，我需要防止数据泄漏，确保同一用户的信息不会在训练集和验证集中被分开。

最佳的方法是`**GroupKFold**`，它通过在每次迭代中使用数据集的不同部分，随机将数据分为训练集和验证集。这会创建具有不同且不重叠用户的独立集。

这对于实现可靠的性能评估至关重要，因为你希望模型在完全未见过的用户上进行测试，而不仅仅是训练过程中见过的用户的新数据。

## **第 2 步：** 使用 RandomizedSearchCV 进行高效的超参数调优

我的样本数据大约有 30 万用户，这是在我的计算能力下不会引发系统崩溃的最大规模。使用`**RandomizedSearchCV**`在这种大样本情况下要高效得多。效果显著。

它不会像传统的网格搜索那样搜索所有可能的超参数组合，而是随机抽取超参数空间的一个子集。然后使用交叉验证评估所选组合的性能。

## ✨结果

通过结合这两者，我对多个数据子集进行了超参数调优，这些子集的用户不重叠。这样我能够：

1.  解决数据泄漏问题

1.  确保计算效率

1.  实施稳健的超参数选择基础

```py
def grid_search(X, y, groups):
    gkf = GroupKFold(n_splits=5).split(X, y, groups)
    model = lgb.LGBMClassifier(objective='binary', verbose=-1, max_depth=-1, random_state=314, metric='None', n_estimators=5000)

    grid = RandomizedSearchCV(
        model, param_grid, scoring='roc_auc', random_state=314,
        n_iter=100, cv=gkf, verbose=10, return_train_score=True, n_jobs=-1)

    return grid

grid = grid_search(X, y, groups)

%%time
grid.fit(X, y)

#printing the best hyperparameters
best_params = grid.best_params_
```

在通过`**RandomizedSearchCV**`和`**GroupKFold**`确定最佳超参数后，我们使用`**GroupShuffleSplit**`的初始训练集和验证集来训练最终模型。

还记得我们在这篇文章一开始创建的`split_df()`函数吗？我们在这一步中使用它来拆分数据。

```py
# We split the data using our initial function
X_train, y_train, X_val, y_val, X_test, y_test = split_df(df, ordinal_cols, ordinal_dfs, target='target_variable')
```

然后，我们插入通过超参数调优找到的最佳参数。

```py
# We train the model using the best_params that we got from HP Tuning 
clf = lgb.LGBMClassifier(objective='binary', max_depth=-1, random_state=314, metric='roc_auc', n_estimators=5000, num_threads=16, verbose=-1,
                           **best_params)
%%time
clf.fit(X_train, y_train, eval_set=(X_val, y_val), eval_metric='roc_auc')

# Test set evaluates the final performance of the model on unseen users
roc_auc_score(y_test, clf.predict(X_test))
```

## 🔊 请记住

我提到这一点是因为在研究过程中这让我很困惑。`eval_set`用于在训练过程中监控模型在特定验证集上的表现。这与交叉验证不同，交叉验证评估模型在多个训练-验证分割上的泛化能力。

# #4. 数据生成

在实施所有之前的步骤后，我的模型仍然需要额外的提升。由于我的数据中某些组的代表性较弱，我的模型在这些组上的泛化能力有些吃力。

所以我确保为所有代表性不足的用户组生成了一个更大的随机样本。这一步骤正好给了我的模型所需的内容，使其能够正确地泛化数据中的所有美好智慧，并做出可靠的预测。

# 最后的一句话

请记住，优化模型的过程是一个迭代的过程，这意味着你可能需要结合和重复一些方法，直到达到令人满意的性能。

## 优化方法回顾

1.  **特征工程** — 使用不同的方法创建新特征，例如特征聚合、转换、时间数据编码、标准化等，可以为数据引入新的信息。

1.  **特征选择** — 在创建新特征后，评估它们的重要性，并移除那些对模型性能没有贡献的无关或冗余特征。一些方法包括皮尔逊相关系数、递归特征消除或卡方检验。

1.  **超参数调优** — 使用 GroupKFold 防止数据泄漏，然后以计算上高效的方式使用 RandomisedSearchCV 寻找最佳参数。

1.  **数据生成** — 确保样本中的各组得到均等代表，如果需要且可能的话，增加样本大小以涵盖更多的数据点。

# 我为你准备了礼物 🎁！

注册我的 [**新闻通讯**](https://levelupwithk.substack.com/) **K 的 DataLadder**，你将自动获得我的 **终极 SQL 备忘单**，其中包含我每天在大科技公司工作中使用的所有查询+另一个秘密礼物！

我每周分享在科技行业作为数据科学家的经历，包括实用技巧、技能和故事，所有这些都是为了帮助你提升水平——因为没有人真正知道，直到他们亲身体验！

## 如果你还没有这样做的话

+   订阅我的 [**YouTube**](https://rebrand.ly/tdf62uv)频道。新视频即将上线！

+   关注我的 [**Instagram**](https://www.instagram.com/elalamikhouloud/)、[**LinkedIn**](https://www.linkedin.com/in/elalamik/)、[**X**](https://twitter.com/elalamik)，随你喜欢。

不久见！
