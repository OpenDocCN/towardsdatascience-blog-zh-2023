# 深入探讨模型可解释性的PFI

> 原文：[https://towardsdatascience.com/deep-dive-into-pfi-for-model-interpretability-f12f0c64226c?source=collection_archive---------11-----------------------#2023-07-20](https://towardsdatascience.com/deep-dive-into-pfi-for-model-interpretability-f12f0c64226c?source=collection_archive---------11-----------------------#2023-07-20)

## 另一个可供选择的可解释性工具

[](https://medium.com/@tiagotoledojr?source=post_page-----f12f0c64226c--------------------------------)[![Tiago Toledo Jr.](../Images/577748ae15ec9eb7ead9355f94287a9d.png)](https://medium.com/@tiagotoledojr?source=post_page-----f12f0c64226c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f12f0c64226c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f12f0c64226c--------------------------------) [Tiago Toledo Jr.](https://medium.com/@tiagotoledojr?source=post_page-----f12f0c64226c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ff4eeaf479b0c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-pfi-for-model-interpretability-f12f0c64226c&user=Tiago+Toledo+Jr.&userId=f4eeaf479b0c&source=post_page-f4eeaf479b0c----f12f0c64226c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f12f0c64226c--------------------------------) ·6分钟阅读·2023年7月20日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff12f0c64226c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-pfi-for-model-interpretability-f12f0c64226c&user=Tiago+Toledo+Jr.&userId=f4eeaf479b0c&source=-----f12f0c64226c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff12f0c64226c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdeep-dive-into-pfi-for-model-interpretability-f12f0c64226c&source=-----f12f0c64226c---------------------bookmark_footer-----------)![](../Images/7eb50e79f4113ff3f3d5a63159eb633f.png)

图片由 [fabio](https://unsplash.com/@fabioha?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

了解如何评估你的模型对于数据科学家的工作至关重要。如果你不能完全理解并向利益相关者传达你的解决方案，没有人会批准它。这就是为什么了解可解释性方法如此重要的原因。

缺乏可解释性可能会毁掉一个非常好的模型。我还没有开发过一个我的利益相关者不关心理解预测如何产生的模型。因此，知道如何解释模型并将其传达给业务是数据科学家的核心能力。

在这篇文章中，我们将深入探讨置换特征重要性（PFI），这是一种与模型无关的方法，可以帮助我们识别模型中最重要的特征，从而更好地沟通模型在进行预测时的考虑因素。

# 置换特征重要性是什么

PFI 方法尝试估计一个特征对模型结果的重要性，基于我们改变与目标变量相关的特征时模型的表现。

为了做到这一点，对于每个特征，我们要分析其重要性，我们将其随机打乱，同时保持其他特征和目标不变。

这使得特征在预测目标时变得无用，因为我们通过改变它们的联合分布打破了它们之间的关系。

然后，我们可以使用模型来预测我们打乱的数据集。模型性能的减少量将指示该特征的重要性。

算法大致如下：

+   我们在训练数据集上训练一个模型，然后在训练集和测试集上评估其表现。

+   对于每个特征，我们创建一个新的数据集，其中该特征被打乱。

+   然后我们使用训练好的模型来预测新数据集的输出。

+   新的性能指标与旧指标的比值给出了特征的重要性。

请注意，如果一个特征不重要，模型的表现不应有太大变化。如果它重要，那么表现应该会有很大变化。

# 解释 PFI

现在我们知道如何计算 PFI，我们如何解释它呢？

这取决于我们将 PFI 应用到哪个折叠。我们通常有两个选项：将其应用于训练数据集或测试数据集。

# 训练解释

在训练过程中，我们的模型学习数据的模式并尝试表示它。当然，在训练过程中，我们无法知道我们的模型对未见数据的泛化效果如何。

因此，通过将 PFI 应用于训练数据集，我们将看到哪些特征对模型学习数据表示最为相关。

从业务角度来看，这表明哪些特征对模型构建最为重要。

# 测试解释

现在，如果我们将方法应用于测试数据集，我们将看到特征对模型泛化的影响。

让我们考虑一下。如果我们在打乱某个特征后看到模型在测试集上的表现下降，这意味着该特征对该数据集的表现很重要。由于测试集是我们用来测试泛化的（如果你做得对的话），那么我们可以说它对泛化很重要。

# PFI 的问题

PFI 分析了特征对模型性能的影响，因此，它并没有说明原始数据的任何信息。如果模型性能较差，那么你通过 PFI 找到的任何关系都是没有意义的。

这对两种情况都适用，如果你的模型出现欠拟合（训练集上的预测能力低）或过拟合（测试集上的预测能力低），那么你不能从这个方法中获得有用的见解。

此外，当两个特征高度相关时，PFI 可能会误导你的解释。如果你打乱一个特征，但所需的信息编码在另一个特征中，那么性能可能不会受到影响，这可能会让你认为这个特征是无用的，但实际上可能并非如此。

# 在 Python 中实现 PFI

要在 Python 中实现 PFI，我们首先必须导入所需的库。为此，我们主要使用 numpy、pandas、tqdm 和 sklearn 这些库：

```py
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt

from tqdm import tqdm
from sklearn.model_selection import train_test_split
from sklearn.datasets import load_diabetes, load_iris
from sklearn.ensemble import RandomForestRegressor, RandomForestClassifier
from sklearn.metrics import accuracy_score, r2_score
```

现在，我们必须加载数据集，将使用 Iris 数据集。然后，我们将对数据进行随机森林拟合。

```py
X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
  X, y, test_size=0.3, random_state=12, shuffle=True
)

rf = RandomForestClassifier(
  n_estimators=3, random_state=32
).fit(X_train, y_train)
```

在模型拟合完成后，让我们分析其性能，以确定是否可以安全地应用 PFI 来查看特征如何影响我们的模型：

```py
print(accuracy_score(rf.predict(X_train), y_train))
print(accuracy_score(rf.predict(X_test), y_test))
```

我们可以看到，在训练集上我们达到了 99% 的准确率，在测试集上达到了 95.5% 的准确率。目前看来不错。让我们获取原始错误评分以便后续比较：

```py
original_error_train = 1 - accuracy_score(rf.predict(X_train), y_train)
original_error_test = 1 - accuracy_score(rf.predict(X_test), y_test)
```

现在让我们计算置换得分。为此，通常需要对每个特征进行多次打乱，以获得特征得分的统计数据，从而避免任何巧合。在我们的案例中，让我们对每个特征进行 10 次重复：

```py
n_steps = 10

feature_values = {}
for feature in range(X.shape[1]):
  # We will save each new performance point for each feature
    errors_permuted_train = []
    errors_permuted_test = []

    for step in range(n_steps):
        # We grab the data again because the np.random.shuffle function shuffles in place
        X, y = load_iris(return_X_y=True)
        X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.3, random_state=12, shuffle=True)
        np.random.shuffle(X_train[:, feature])
        np.random.shuffle(X_test[:, feature])

    # Apply our previously fitted model on the new data to get the performance
        errors_permuted_train.append(1 - accuracy_score(rf.predict(X_train), y_train))
        errors_permuted_test.append(1 - accuracy_score(rf.predict(X_test), y_test))

    feature_values[f'{feature}_train'] = errors_permuted_train
    feature_values[f'{feature}_test'] = errors_permuted_test
```

现在我们有了一个包含每次打乱性能的字典。接下来，让我们生成一个表格，该表格对每个折叠中的每个特征显示其性能的平均值和标准差，并与模型的原始性能进行比较：

```py
PFI = pd.DataFrame()
for feature in feature_values:
    if 'train' in feature:
        aux = feature_values[feature] / original_error_train
        fold = 'train'
    elif 'test' in feature:
        aux = feature_values[feature] / original_error_test
        fold = 'test'

    PFI = PFI.append({
        'feature': feature.replace(f'_{fold}', ''),
        'pfold': fold,
        'mean':np.mean(aux),
        'std':np.std(aux),
    }, ignore_index=True)

PFI = PFI.pivot(index='feature', columns='fold', values=['mean', 'std']).reset_index().sort_values(('mean', 'test'), ascending=False)
```

我们将得到如下结果：

我们可以看到，特征 2 似乎是数据集中最重要的特征，其次是特征 3。由于我们没有固定 numpy 打乱函数的随机种子，我们可以预期这个数字会有所变化。

然后我们可以绘制一个图表，以更好地可视化重要性：

# 结论

PFI 是一种简单的方法论，可以帮助你快速识别最重要的特征。继续尝试将它应用到你正在开发的模型中，看看它的表现如何。

但也要注意方法的局限性。如果不了解方法的不足之处，将会导致错误的解释。

另外，注意 PFI 显示的是特征的重要性，但并没有说明它对模型输出的影响方向。

那么，告诉我，你打算如何在下一个模型中使用这个方法？

敬请关注更多关于可解释性方法的帖子，这些方法可以提高你对模型的整体理解。
