# 如何在不重新采样的情况下应对类别不平衡

> 原文：[`towardsdatascience.com/how-to-tackle-class-imbalance-without-resampling-47bbeb2180aa`](https://towardsdatascience.com/how-to-tackle-class-imbalance-without-resampling-47bbeb2180aa)

## 超越重新采样、阈值调整或成本敏感模型的类别不平衡分类

[](https://vcerq.medium.com/?source=post_page-----47bbeb2180aa--------------------------------)![Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----47bbeb2180aa--------------------------------)[](https://towardsdatascience.com/?source=post_page-----47bbeb2180aa--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----47bbeb2180aa--------------------------------) [Vitor Cerqueira](https://vcerq.medium.com/?source=post_page-----47bbeb2180aa--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----47bbeb2180aa--------------------------------) ·阅读时间 7 分钟·2023 年 3 月 1 日

--

![](img/f9ebb8f645bb4e67587a6f3ae607c680.png)

[Denise Johnson](https://unsplash.com/@auntneecey?utm_source=medium&utm_medium=referral) 的照片，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

不平衡分类是一个相关的机器学习任务。这个问题通常通过三种方法中的一种来处理：重新采样、成本敏感模型或阈值调整。

在本文中，你将学习一种不同的方法。我们将探讨如何使用聚类分析来解决不平衡分类问题。

# 引言

许多现实世界的问题涉及不平衡的数据集。在这些问题中，某一类是稀少的，通常对用户来说更重要。

以欺诈检测为例。欺诈案件在大量常规活动中是稀有的实例。准确检测稀少但欺诈性的活动在许多领域都是至关重要的。其他涉及不平衡数据集的常见例子包括客户流失或信用违约预测。

不平衡的分布是机器学习算法面临的一项挑战。关于少数类的信息相对较少。这阻碍了算法训练出良好模型的能力，因为算法往往偏向于多数类。

# 如何处理类别不平衡

处理类别不平衡有三种标准方法：

1.  重新采样方法；

1.  成本敏感模型；

1.  阈值调整。

![](img/a058a1d7e62d2e8adbb47f9d68630b5d.png)

[Viktor Talashuk](https://unsplash.com/@viktortalashuk?utm_source=medium&utm_medium=referral) 的照片，发布于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

重新采样可以说是处理不平衡分类任务中最受欢迎的策略。这种方法通过转换训练集来提高少数类的相关性。

重采样可以用来为少数类创建新案例（过采样）、丢弃多数类案例（欠采样），或两者结合。

下面是使用*imblearn*库的重采样方法工作的示例：

```py
from sklearn.datasets import make_classification
from imblearn.over_sampling import SMOTE

X_train, y_train = make_classification(n_samples=500, n_features=5, n_informative=3)
X_res, y_res = SMOTE().fit_resample(X_train, y_train)
```

重采样方法是多用途的，易于与任何学习算法结合。但它们也有一些限制。

对多数类进行欠采样可能导致重要信息的丢失。过采样可能增加过拟合的风险。这种情况发生在重采样传播了来自少数类的噪声时。

有一些替代方案用于重采样训练数据。这些包括调整决策阈值或使用成本敏感模型。不同的阈值会导致不同的精准率和召回率得分。因此，调整决策阈值可以提高模型的性能。

成本敏感模型通过对误分类错误分配不同的成本来工作。少数类的错误通常更为昂贵。这种方法需要领域专业知识来定义每种类型错误的成本。

# 利用聚类处理类别不平衡

大多数重采样方法通过寻找接近决策边界的实例来工作——即分割多数类和少数类实例的前沿。边界情况原则上是最难分类的。因此，它们用于驱动重采样过程。

![](img/37a847f56433fb33dee5290d8804725e.png)

支持向量机模型的决策边界。原始图片：Alisneaky Vector: Zirguezi, CC BY-SA 4.0\. 图片[来源](https://commons.wikimedia.org/w/index.php?curid=47868867)。

例如，ADASYN 是一种流行的过采样技术。它使用来自少数类的样本创建人工实例，这些样本的最近邻来自多数类。

## 使用聚类分析寻找边界情况

我们还可以使用聚类分析来捕捉哪些观察点接近决策边界。

假设有一个聚类，其观察点全部属于多数类。这可能意味着该聚类距离决策边界较远，位于多数类的一侧。通常，这些观察点容易建模。

另一方面，如果一个实例属于一个同时包含两类的聚类，则可以认为它是边界实例。

我们可以利用这些信息构建一个层次化模型来处理不平衡分类。

## 如何构建一个层次化模型以应对不平衡分类

我们基于两个层级构建一个层次化模型。

在第一层级，建立一个模型以将简单实例与边界实例分开。因此，目标是预测输入实例是否属于至少包含一个少数类观察点的聚类。

在第二层级，我们丢弃简单的案例。然后，我们建立一个模型来解决原始分类任务，使用剩余的数据。第一层级通过从训练集中移除简单的实例来影响第二层级。

在两个层次中，不平衡问题被减少，这使得建模任务更简单。

## Python 实现

上述方法称为 ICLL（层次学习的不平衡分类）。以下是其实现：

```py
from collections import Counter
from typing import List

import numpy as np
import pandas as pd
from scipy.cluster.hierarchy import linkage, fcluster
from scipy.spatial.distance import pdist

class ICLL:
    """
    Imbalanced Classification via Layered Learning
    """

    def __init__(self, model_l1, model_l2):
        """
        :param model_l1: Predictive model for the first layer
        :param model_l2: Predictive model for the second layer
        """
        self.model_l1 = model_l1
        self.model_l2 = model_l2
        self.clusters = []
        self.mixed_arr = np.array([])

    def fit(self, X: pd.DataFrame, y: np.ndarray):
        """
        :param X: Explanatory variables
        :param y: binary target variable
        """
        assert isinstance(X, pd.DataFrame)
        X = X.reset_index(drop=True)

        if isinstance(y, pd.Series):
            y = y.values

        self.clusters = self.clustering(X=X)

        self.mixed_arr = self.cluster_to_layers(clusters=self.clusters, y=y)

        y_l1 = y.copy()
        y_l1[self.mixed_arr] = 1

        X_l2 = X.loc[self.mixed_arr, :]
        y_l2 = y[self.mixed_arr]

        self.model_l1.fit(X, y_l1)
        self.model_l2.fit(X_l2, y_l2)

    def predict(self, X):
        """
        Predicting new instances
        """

        yh_l1, yh_l2 = self.model_l1.predict(X), self.model_l2.predict(X)

        yh_f = np.asarray([x1 * x2 for x1, x2 in zip(yh_l1, yh_l2)])

        return yh_f

    def predict_proba(self, X):
        """
        Probabilistic predictions
        """

        yh_l1_p = self.model_l1.predict_proba(X)
        try:
            yh_l1_p = np.array([x[1] for x in yh_l1_p])
        except IndexError:
            yh_l1_p = yh_l1_p.flatten()

        yh_l2_p = self.model_l2.predict_proba(X)
        yh_l2_p = np.array([x[1] for x in yh_l2_p])

        yh_fp = np.asarray([x1 * x2 for x1, x2 in zip(yh_l1_p, yh_l2_p)])

        return yh_fp

    @classmethod
    def cluster_to_layers(cls, clusters: List[np.ndarray], y: np.ndarray) -> np.ndarray:
        """
        Defining the layers from clusters
        """

        maj_cls, min_cls, both_cls = [], [], []
        for clst in clusters:
            y_clt = y[np.asarray(clst)]

            if len(Counter(y_clt)) == 1:
                if y_clt[0] == 0:
                    maj_cls.append(clst)
                else:
                    min_cls.append(clst)
            else:
                both_cls.append(clst)

        both_cls_ind = np.array(sorted(np.concatenate(both_cls).ravel()))
        both_cls_ind = np.unique(both_cls_ind)

        if len(min_cls) > 0:
            min_cls_ind = np.array(sorted(np.concatenate(min_cls).ravel()))
        else:
            min_cls_ind = np.array([])

        both_cls_ind = np.unique(np.concatenate([both_cls_ind, min_cls_ind])).astype(int)

        return both_cls_ind

    @classmethod
    def clustering(cls, X, method='ward'):
        """
        Hierarchical clustering analysis
        """

        d = pdist(X)

        Z = linkage(d, method)
        Z[:, 2] = np.log(1 + Z[:, 2])
        sZ = np.std(Z[:, 2])
        mZ = np.mean(Z[:, 2])

        clust_labs = fcluster(Z, mZ + sZ, criterion='distance')

        clusters = []
        for lab in np.unique(clust_labs):
            clusters.append(np.where(clust_labs == lab)[0])

        return clusters
```

聚类部分自动完成，无需用户输入。因此，你唯一需要定义的是每个层次的学习算法。

以下是如何使用该方法的示例。在这个例子中，每个层次的模型是一个随机森林。

```py
import pandas as pd
from sklearn.datasets import make_classification
from sklearn.ensemble import RandomForestClassifier as RFC

# https://github.com/vcerqueira/blog/blob/main/src/icll.py
from src.icll import ICLL

# creating a dummy data set
X, y = make_classification(n_samples=500, n_features=5, n_informative=3)
X = pd.DataFrame(X)

# creating a instance of the model
icll = ICLL(model_l1=RFC(), model_l2=RFC())
# training
icll.fit(X, y)
# probabilistic predictions
probs = icll.predict_proba(X)
```

## 一个更严重的例子

层次方法与重采样的比较如何？

下面是基于糖尿病相关数据集的比较。你可以查看参考文献 [1] 获取详细信息。以下是如何将这两种方法应用于该数据集：

```py
from sklearn.model_selection import train_test_split
from sklearn.metrics import roc_curve, roc_auc_score
from imblearn.over_sampling import SMOTE

# loading diabetes dataset https://github.com/vcerqueira/blog/tree/main/data
data = pd.read_csv('data/pima.csv')

X, y = data.drop('target', axis=1), data['target']
X = X.fillna(X.mean())

# train test split
X_train, X_test, y_train, y_test = train_test_split(X, y, test_size=0.33, random_state=42)

# resampling with SMOTE
X_res, y_res = SMOTE().fit_resample(X_train, y_train)

# creating the models
smote = RFC()
icll = ICLL(model_l1=RFC(), model_l2=RFC())

# training 
smote.fit(X_res, y_res)
icll.fit(X_train, y_train)

# inference
smote_probs = smote.predict_proba(X_test)
icll_probs = icll.predict_proba(X_test)
```

下面是每种方法的 ROC 曲线：

![](img/359d27737414908fd10f5516e15b22b1.png)

每种方法的 ROC 曲线。图片由作者提供。

ICLL 的曲线更接近左上角，这表明它是更好的模型。

在参考文献 [2] 的论文中进行了更多实验，其中介绍了 ICLL。结果表明 ICLL 在不平衡分类问题中表现具有竞争力。你可以在 [Github](https://github.com/vcerqueira/icll) 上查看实验代码。

# 关键要点

+   不平衡分类是数据科学中的一项重要任务；

+   对训练集进行重采样是处理这些问题的常见方法。但是，这可能导致信息丢失或噪声传播。常见的替代方法包括阈值调整或成本敏感模型；

+   你还可以使用层次方法来处理不平衡问题；

+   ICLL 是一种用于不平衡分类的层次方法。除了学习算法外，它不需要任何用户参数。ICLL 提供了与重采样方法相媲美的性能。

希望你觉得这个方法有用。感谢阅读，下次见！

## 参考文献

[1] [皮马印第安人糖尿病数据集](https://sci2s.ugr.es/keel/dataset.php?cod=21#sub1)（GPL-3 许可证）

[2] [Cerqueira, V., Torgo, L., Branco, P., & Bellinger, C. (2022). 通过层次学习的自动不平衡分类。*机器学习*, 1–22.](https://link.springer.com/article/10.1007/s10994-022-06282-w)

[3] Branco, Paula, Luís Torgo, 和 Rita P. Ribeiro. “对不平衡领域预测建模的综述。” *ACM 计算调查（CSUR）* 49.2 (2016): 1–50。
