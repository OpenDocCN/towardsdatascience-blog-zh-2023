# 监控机器学习模型的生产：为什么和如何？

> 原文：[https://towardsdatascience.com/monitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6?source=collection_archive---------7-----------------------#2023-09-05](https://towardsdatascience.com/monitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6?source=collection_archive---------7-----------------------#2023-09-05)

## 我们的模型在不断发展的世界中受到什么影响？分析重点是漂移示例，以及实施基于Python的监控策略

[](https://medium.com/@johnleungTJ?source=post_page-----13d07a5ff0c6--------------------------------)[![John Leung](../Images/ef45063e759e3450fa7f3c32b2f292c3.png)](https://medium.com/@johnleungTJ?source=post_page-----13d07a5ff0c6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----13d07a5ff0c6--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----13d07a5ff0c6--------------------------------) [John Leung](https://medium.com/@johnleungTJ?source=post_page-----13d07a5ff0c6--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6125e8835d3b&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6&user=John+Leung&userId=6125e8835d3b&source=post_page-6125e8835d3b----13d07a5ff0c6---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----13d07a5ff0c6--------------------------------) · 12分钟阅读 · 2023年9月5日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F13d07a5ff0c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6&user=John+Leung&userId=6125e8835d3b&source=-----13d07a5ff0c6---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F13d07a5ff0c6&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmonitoring-machine-learning-models-in-production-why-and-how-13d07a5ff0c6&source=-----13d07a5ff0c6---------------------bookmark_footer-----------)

机器学习（ML）模型开发通常需要时间和技术专长。作为数据科学爱好者，当我们获取一个数据集进行探索和分析时，我们迫不及待地使用各种[最先进的模型](https://medium.com/geekculture/whats-the-public-sentiment-under-inflationary-economic-environment-5edc899efa29)或[以数据为中心的策略](https://medium.com/mlearning-ai/how-to-apply-data-centric-ai-mindset-to-text-classification-problems-b41656c70c16)进行训练和验证。当我们优化模型性能时，感觉所有的任务仿佛都已完成，令人非常满足。

然而，将模型部署到生产环境后，有许多原因可能导致模型性能下降或退化。

![](../Images/295fd5ece6d0106a67d735d40b80bb4a.png)

照片由[Adrien Delforge](https://unsplash.com/@adriendlf?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**#1 训练数据是通过模拟生成的**

数据科学家经常[面临限制](https://www.cio.com/article/465251/how-can-cios-protect-personal-identifiable-information-pii-for-a-new-class-of-data-consumers.html)访问生产数据，这导致模型使用模拟或样本数据进行训练。尽管数据工程师负责确保训练数据在规模和复杂性方面的代表性，但训练数据仍然在某种程度上与生产数据有所偏差。上游数据处理中的系统性缺陷，如数据收集和标记，也存在风险。这些因素可能影响额外有用输入特征的提取或阻碍模型的良好泛化能力。

> **例如：** 财务行业中的投资者数据或医疗行业中的患者信息通常由于安全和隐私问题而被模拟。

**#2 新的生产数据展示了新的数据分布**

随着时间的推移，输入特征的特征也可能发生变化，例如年龄组、收入范围或其他客户人口统计数据的变化。数据源本身甚至可能由于各种情况完全被替代。在模型开发过程中，优化依赖于学习和捕捉训练数据中多数群体的模式。然而，随着时间的推移，之前的多数群体可能会转变为生产数据中的少数群体，从而使原有的静态模型无法满足最新的生产需求。

> **例如：** 模型最初使用亚洲地区的客户数据进行了良好的训练。最近，由于业务扩展到美国，相同的模型直接根据具有变化的输入特征进行预测。

![](../Images/2143289b7b56e32bb729f36ba6d960ca.png)

数据漂移（图片由作者提供）

**#3 我们预测的模式在不断演变**

除了输入特征分布的变化外，[特征与目标变量之间的关系](https://en.wikipedia.org/wiki/Concept_drift)在不断发展的环境中也可能发生变化。这些变化可能会以意想不到的方式发生，导致原始模型逐渐失效。

+   突发的概念变化

这些变化可能会突然发生，有时在几周内，由于不可预见的情况。

> **示例：** 公共 COVID-19 封锁期间对虚拟会议服务需求的激增。

+   渐进的概念变化

这种变化需要较长时间才能显现，通常是自然进程。

> **示例：** 由于长期通货膨胀，乳制品价格的逐步上涨。

+   反复出现的概念变化

这些变化可能会周期性发生，通常在一年中的特定时间。

> **示例：** 在诸如黑色星期五和圣诞节前的星期六等特殊日子，电子商务销售的快速增长。

![](../Images/c4b2cc04ce61055adc9758b8f7dd5a57.png)

概念漂移（图片由作者提供）

## 模型开发只是生产就绪的机器学习系统中的一小部分

许多公司通过使用机器学习应用程序强调数据驱动的决策制定。假设您的机器学习模型用于关键应用，例如医疗诊断，以帮助医疗专业人员识别疾病和病症。任何模型退化都意味着影响诊断的准确性，可能导致错误的治疗方案和患者结果的妥协。现实世界中有大量重要的案例，因此，实施客观和持续的监控以检测任何可能的变化变得至关重要。

在接下来的部分中，我们将深入探讨各种监控层级，并提供示例 Python 代码来演示其实现。

![](../Images/f2c3baf922fa9c1ad596a1db57177a2f.png)

图片来源于 [Nathy dog](https://unsplash.com/@nathdah?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## #1 监控模型性能指标

为了检测模型退化的任何迹象，直接且有效的方法之一是跟踪性能指标的变化。

+   [回归模型的性能指标](/what-are-the-best-metrics-to-evaluate-your-regression-model-418ca481755b)：决定系数（R-squared）、均方根误差（RMSE）和平均绝对误差（MAE）

+   [分类模型的性能指标](/the-5-classification-evaluation-metrics-you-must-know-aa97784ff226)：精确度、召回率和 F1 值

从初始部署中收集的这些性能指标作为持续监控和评估的基准。在收集新的地面真实数据时，例如在营销活动结束后，定期重新评估这些指标至关重要。如果误差指标超过预定义的阈值，或者诸如R-squared这样的指标低于阈值，则需要考虑重新执行数据工程过程和重新训练模型。

尽管这种监控方法提供了关于漂移的有价值的见解，但它往往滞后。我们可以采取更积极的方法来监控最新的输入数据。

## #2 检测数据分布的变化

我们可以应用统计方法来比较两个数据集的数据分布，而不是等待足够的输入数据来可靠地评估模型性能。在我们的情况下，我们可以确定训练数据集的分布是否与最新的生产数据集相同。如果不能统计上自信这两个分布相同，则表明模型出现了漂移。这作为性能变化的代理。

+   Kolmogorov-Smirnov检验（[K-S Test](/kolmogorov-smirnov-test-84c92fb4158d)）：非参数检验（即对基础数据分布没有假设）**用于数值特征**，其在分布中心比在尾部更敏感。

> **解释：** 当p值< 0.05时，触发表示存在漂移的警报。

+   人口稳定指数（[PSI](/psi-and-csi-top-2-model-monitoring-metrics-924a2540bed8d)）：一种可以应用于**数值变量和分类变量**的统计检验。它是一个指标，显示每个变量如何与基准值独立地偏离。当评估特征分布而不是目标变量时，有时被称为特征稳定指数（CSI）。

> **解释：** 值为0~0.1表示没有显著的分布变化；值为0.1~0.2表示中等分布变化；值大于0.2则解释为显著的分布变化。

其他著名的统计检验包括 [Kullback-Leibler散度](/understanding-kl-divergence-f3ddc8dff254)、[Jensen-Shannon散度](/how-to-understand-and-use-jensen-shannon-divergence-b10e11b03fd6) 和 [Wasserstein距离](/the-gromov-wasserstein-distance-835c39d4751d)。

## #3 使用滑动窗口方法监控漂移

检测数据或模式漂移的任何延迟都会导致时间差，这可能导致地面真实数据与模型预测之间的差异。为了弥补这一差距，是否有进一步的进展？一种有前景的想法是利用流数据而不是批数据。

自适应窗口（[ADWIN](https://scikit-multiflow.readthedocs.io/en/stable/api/generated/skmultiflow.drift_detection.ADWIN.html)）算法利用滑动窗口方法有效检测概念漂移。与传统的固定大小窗口不同，ADWIN通过在不同点截断统计窗口来决定窗口的大小。每当新数据到来时，ADWIN分析统计数据并识别出两个子窗口在均值上显著不同的点。

> **解释：** 当两个均值之间的绝对差异超过预定义的阈值时，会触发一个警报，表示存在漂移。

## **示例演示**

让我们探讨一个示例，展示上述监控策略的实现。

我们将利用从Kaggle获得的 [数据集](https://www.kaggle.com/datasets/parisrohan/credit-score-classification?datasetId=2289007&sortBy=voteCount&select=train.csv)。该数据集包含10万条记录，涵盖28个描述客户人口统计和信用相关历史的特征。

我们的目标是将全球金融公司中的客户按信用评分区间进行分类。目标变量是**Credit_Score**，这是一个分类测量，分为“差”、“标准”和“好”。

特征的示例包括：

+   *Occupation*: 客户的职业（例如科学家、教师、工程师等）

+   *Annual_Income*: 客户的年收入

+   *Credit_History_Age*: 客户信用历史的年龄

+   *Payment_Behaviour*: 基于支出频率（低/高）和支付金额（小/中/大）的6组支付行为

![](../Images/89da1d42d08937e934255688405067cd.png)

图片由 [CardMapr.nl](https://unsplash.com/@cardmapr?utm_source=medium&utm_medium=referral) 提供，来自 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**模型性能指标**

我们开始应用数据清洗和转换技术，包括但不限于：

+   修正/删除缺失值或不正确数据（例如年龄 < 0）或重复记录

+   检测和处理数据异常值

+   对数值变量进行最小-最大缩放

+   对分类变量应用标签编码

*这一部分需要深入的探索性数据分析。然而，由于重点在于分享监控策略，这里未详细讨论所执行的转换。*

随后，我们拆分数据集并开发了高级梯度提升算法 [LightGBM](https://lightgbm.readthedocs.io/en/stable/)。

```py
import numpy as np
import pandas as pd
from sklearn import metrics
from sklearn.metrics import f1_score, precision_score, recall_score
from sklearn.model_selection import train_test_split
from sklearn.preprocessing import LabelEncoder
from lightgbm import LGBMClassifier

# Convert target variable to numeric
df['Credit_Score'] = df['Credit_Score'].str.replace('Good', '3', n=-1)
df['Credit_Score'] = df['Credit_Score'].str.replace('Standard', '2', n=-1)
df['Credit_Score'] = df['Credit_Score'].str.replace('Poor', '1', n=-1)
df['Credit_Score'] = df[['Credit_Score']].apply(pd.to_numeric)

# Split the dataset
X=df.loc[:, df.columns != 'Credit_Score']
Y=df['Credit_Score']
x_train, x_test, y_train, y_test = train_test_split(X, Y, test_size=0.25)

# Train the LightGBM model
lgbm = LGBMClassifier()
lgbm.fit(x_train, y_train)
y_pred = lgbm.predict(x_test)

# Print performance metrics
print('F1 score: %.3f' % f1_score(y_test, y_pred, average='weighted'))
print('Precision: %.3f' % precision_score(y_test, y_pred, average='weighted'))
print('Recall: %.3f' % recall_score(y_test, y_pred, average='weighted'))
```

在我们的模型开发过程中，我们收集了多个性能指标，包括0.810的F1分数、0.818的精确度和0.807的召回率。后续监控可以类似地进行评估。例如，如果F1分数低于0.75，则会触发警报，提示我们立即采取措施来缓解问题。

**K-S检验**

为了展示K-S检验的工作原理，我选择了数值特征*Credit_History_Age*。我们将通过创建三个不同的数据集来检查这个统计检验的敏感性：一个包含1000个样本，另一个包含5000个样本，还有一个包含大约64000个样本，这对应于清理后的训练样本的总量。*Credit_History_Age*特征中的每个数据点都是从训练数据中随机挑选，并通过随机浮点数进行了改变。

![](../Images/04df626e2dd072d349f5c829070d2589.png)

‘Credit_History_Age’在原始数据和漂移数据中的数据分布（图像来源：作者）

我们获得了以下结果：

+   对于1000个样本的数据集，K-S检验的p值为0.093。

+   对于5000个样本的数据集，K-S检验的p值为0.002。

+   对于大约64000个样本的数据集，K-S检验的p值为0.000。

当使用1000个样本检查数据漂移场景时，p值超过了0.05。因此，我们可以得出结论，两种分布仍然相似。然而，随着样本量增加到5000及以上，K-S检验表现出卓越的性能，p值明显低于0.05。这清楚地表明了数据分布的漂移，作为明确的警报。

```py
from scipy import stats

# Create new datasets with different no. of samples
original_df = x_train[['Credit_History_Age', 'Payment_Behaviour']].reset_index(drop=True)
new_df = x_train[['Credit_History_Age', 'Payment_Behaviour']].reset_index(drop=True)
new_df1 = new_df.sample(n = 1000).reset_index(drop=True)
new_df2 = new_df.sample(n = 5000).reset_index(drop=True)
new_df3 = new_df.sample(n = len(x_train)).reset_index(drop=True)

# Prepare drifted data for numeric feature
def drift_numeric_col(df, numeric_col, drift_range):
    df[numeric_col] = df[numeric_col] + np.random.uniform(0, drift_range, size=(df.shape[0], ))

drift_numeric_col(new_df1, 'Credit_History_Age', 2)
drift_numeric_col(new_df2, 'Credit_History_Age', 2)
drift_numeric_col(new_df3, 'Credit_History_Age', 2)

# K-S Test
def ks_test(original_df, new_df, numeric_col):
    test = stats.ks_2samp(original_df[numeric_col], new_df[numeric_col])
    print("Column : %s , p-value : %1.3f" % (numeric_col, test[1]))

# Conduct K-S Test for numeric feature
ks_test(original_df, new_df1, 'Credit_History_Age')
ks_test(original_df, new_df2, 'Credit_History_Age')
ks_test(original_df, new_df3, 'Credit_History_Age')
```

**PSI**

除了使用K-S检验外，我们还将利用PSI（人口稳定性指数）来评估数值特征*Credit_History_Age*以及类别特征*Payment_Behaviour*。为了表示漂移效果，我们随机将*Payment_Behaviour*特征的80%数据替换为特定标签值。

```py
# Prepare drifted data for categorical column
def drift_cat_col(df, cat_col, drift_ratio):
    no_of_drift = round(len(df)*drift_ratio)
    random_numbers = [random.randint(0, 1) for _ in range(no_of_drift)]
    indices = random.sample(range(len(df[cat_col])), no_of_drift)
    df.loc[indices, cat_col] = random_numbers

drift_cat_col(new_df1, 'Payment_Behaviour', 0.8)
drift_cat_col(new_df2, 'Payment_Behaviour', 0.8)
drift_cat_col(new_df3, 'Payment_Behaviour', 0.8)
```

![](../Images/8b316011e2d1c1c0f9c4f96cac6b8772.png)

‘Payment_Behaviour’在原始数据和漂移数据中的数据分布（图像来源：作者）

+   数值特征*Credit_History_Age*

在1000个样本的情况下，PSI值为0.023。

在5000个样本的情况下，PSI值为0.015。

在大约64000个样本的情况下，PSI值为0.021。

+   类别特征*Payment_Behaviour*

在1000个样本的情况下，PSI值为0.108。

在5000个样本的情况下，PSI值为0.111。

在大约64000个样本的情况下，PSI值为0.112。

所有*Credit_History_Age*特征的PSI值明显低于0.1，这表明没有显著的分布变化。通过将这些结果与K-S检验的结果进行比较，我们观察到K-S检验在检测分布变化方面比PSI具有更高的敏感性。

另一方面，*Payment_Behaviour*特征的PSI值大约为0.11，表示中等程度的分布变化。有趣的是，三个PSI值保持相对一致，暗示着PSI的有效性不太依赖于样本量。此外，PSI具有监测各种特征类型的灵活性，因此在漂移检测中仍然是一种有价值的方法。

以下是PSI的实现代码：

```py
def psi(data_base, data_new, num_bins = 10):
    # Sort the data
    data_base = sorted(data_base)
    data_new = sorted(data_new)

    # Prepare the bins
    min_val = min(data_base[0], data_new[0])
    max_val = max(data_base[-1], data_new[-1])
    bins = [min_val + (max_val - min_val)*(i)/num_bins for i in range(num_bins+1)]
    bins[0] = min_val - 0.0001
    bins[-1] = max_val + 0.0001

    # Bucketize the baseline data and count the samples
    bins_base = pd.cut(data_base, bins = bins, labels = range(1,num_bins+1))
    df_base = pd.DataFrame({'base': data_base, 'bin': bins_base})
    grp_base = df_base.groupby('bin').count()
    grp_base['percent_base'] = grp_base['base'] / grp_base['base'].sum() 

    # Bucketize the new data and count the samples
    bins_new = pd.cut(data_new, bins = bins, labels = range(1,num_bins+1))
    df_new = pd.DataFrame({'new': data_new, 'bin': bins_new})
    grp_new = df_new.groupby('bin').count()
    grp_new['percent_new'] = grp_new['new'] / grp_new['new'].sum()

    # Compare the bins
    psi_df = grp_base.join(grp_new, on = "bin", how = "inner")

    # Calculate the PSI
    psi_df['percent_base'] = psi_df['percent_base'].replace(0, 0.0001)
    psi_df['percent_new'] = psi_df['percent_new'].replace(0, 0.0001)
    psi_df['psi'] = (psi_df['percent_base'] - psi_df['percent_new']) * np.log(psi_df['percent_base'] / psi_df['percent_new'])

    # Return the total PSI value
    return np.sum(psi_df['psi'].values)

# Conduct K-S Test for numeric feature
psi(original_df['Credit_History_Age'], new_df1['Credit_History_Age'])
psi(original_df['Credit_History_Age'], new_df2['Credit_History_Age'])
psi(original_df['Credit_History_Age'], new_df3['Credit_History_Age'])

# Conduct K-S Test for categorical feature
psi(original_df['Payment_Behaviour'], new_df1['Payment_Behaviour'])
psi(original_df['Payment_Behaviour'], new_df2['Payment_Behaviour'])
psi(original_df['Payment_Behaviour'], new_df3['Payment_Behaviour'])
```

**ADWIN 算法**

最后，我们测试了 ADWIN 在检测数值特征 *Credit_History_Age* 变化方面的能力。[数据流](https://en.wikipedia.org/wiki/Streaming_data) 包含训练数据，随后是漂移数据。我们期望算法在检查完原始数据后，能够迅速识别出漂移。

为了直观地展示情况，创建了一个散点图来展示原始数据的最后 500 个点（以蓝色表示），以及漂移数据的前 500 个点（以绿色表示）。漂移数据显示出略高的平均值。

![](../Images/42d6513244576dea35f68f17a17b8272.png)

散点图（作者提供的图像）

通过不断添加流元素，ADWIN 在索引 64457 处识别出了变化，这是漂移数据中的第 637 个数据点。相比之下，K-S 测试和 PSI 需要更多的数据点来自信地得出漂移的存在。ADWIN 更好的性能证明了其在速度和简便性方面监控多样特征的能力。

以下是 ADWIN 的实现代码：

```py
from skmultiflow.drift_detection import ADWIN

adwin = ADWIN()
data_stream=[]
data_stream = np.concatenate((original_df['Credit_History_Age'],new_df3['Credit_History_Age']))

# Add stream elements to ADWIN and verify if drift occurred
for i in range(len(data_stream)):
  adwin.add_element(data_stream[i])
  if adwin.detected_change():
  print('Change detected at index {}'.format(i))
  adwin.reset()
```

## 总结一下

我们深入探讨了数据漂移和模型漂移这两个关键概念，它们可能导致生产中的模型衰退。我们可以通过模型性能指标、统计测试和自适应窗口技术来主动监测和检测漂移条件。与参与一次性的 Kaggle 比赛不同，构建一个生产就绪的 ML 系统是一个迭代的过程。这需要一种将全面的模型监控整合进来以确保强大且持续高性能服务空间的心态转变。

## 在你离开之前

如果你喜欢这篇文章，我邀请你关注 [我的 Medium 页面](https://medium.com/@johnleungTJ)。通过这样做，你可以保持更新，了解与数据科学副项目、机器学习操作（MLOps）演示和项目管理方法相关的精彩内容。

[](/optimizing-your-strategies-with-approaches-beyond-a-b-testing-bf11508f8930?source=post_page-----13d07a5ff0c6--------------------------------) [## 超越 A/B 测试的策略优化

### 用通俗易懂的方式深入解释经典 A/B 测试的优化：Epsilon-贪婪、汤普森采样、上下文……

towardsdatascience.com](/optimizing-your-strategies-with-approaches-beyond-a-b-testing-bf11508f8930?source=post_page-----13d07a5ff0c6--------------------------------) [](https://medium.com/mlearning-ai/how-to-apply-data-centric-ai-mindset-to-text-classification-problems-b41656c70c16?source=post_page-----13d07a5ff0c6--------------------------------) [## 如何将数据中心化的 AI 思维方式应用于文本分类问题？

### 使用各种以数据为中心的 Python 包和 AI 技巧掌握客户投诉分类

[medium.com](https://medium.com/mlearning-ai/how-to-apply-data-centric-ai-mindset-to-text-classification-problems-b41656c70c16?source=post_page-----13d07a5ff0c6--------------------------------)
