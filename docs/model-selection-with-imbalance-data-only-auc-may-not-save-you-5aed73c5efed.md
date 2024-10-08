# 不平衡数据模型选择：仅凭 AUC 可能无法拯救你

> 原文：[`towardsdatascience.com/model-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed`](https://towardsdatascience.com/model-selection-with-imbalance-data-only-auc-may-not-save-you-5aed73c5efed)

## 你是否在有效地搜索参数？

[](https://medium.com/@cerlymarco?source=post_page-----5aed73c5efed--------------------------------)![Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----5aed73c5efed--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5aed73c5efed--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----5aed73c5efed--------------------------------) [Marco Cerliani](https://medium.com/@cerlymarco?source=post_page-----5aed73c5efed--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----5aed73c5efed--------------------------------) ·6 min read·2023 年 2 月 22 日

[]

![](img/0fd9065c148b0219514a0be4d198178b.png)

由[Mpho Mojapelo](https://unsplash.com/@mpho_mojapelo?utm_source=medium&utm_medium=referral)拍摄，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

大多数数据科学家在参加会议向业务相关者展示机器学习结果时，通常会回答以下问题：

> AUC？那是什么？你能详细说明一下吗？

**数据科学日常工作中的术语和概念可能对大多数人来说并不熟悉**。这在开发人工智能产品以解决现实世界问题时经常发生。在这种情况下，数据科学家与领域专家共同工作，了解领域动态，并将其相应地纳入自动化解决方案中。

**对人工智能在解决商业问题中所提供的附加值保持批判性的看法是至关重要的**。在很多情况下，采用机器学习可能是没用的，因为这些任务可以通过简单的自动化规则解决，或者在现有数据中没有证据证明需要使用人工智能技术。话虽如此，**选择最合适的指标来评估提出的解决方案的有效性是一个非常重要的步骤**。

**选择合适的指标是与领域相关的，并且会根据需求变化**。将 AUC 作为指标来向业务相关者展示所采用的机器学习方法的优点/强项可能存在风险。首先，因为 AUC 的定义可能不是所有人都清楚。其次，**AUC 的经济意义不容易给出**。商业人士以金钱为导向。如果他们不理解所提议的解决方案如何帮助他们节省时间或金钱，他们可能会拒绝这些方案。

在这篇文章中，我们不建议选择正确的业务度量方法。相反，我们关注的是与度量定义紧密相关的更技术性问题。我们指的是模型选择。我们希望**在不平衡的二分类背景下测试模型选择的有效性**。目标是调查简单的决策（如度量选择或阈值调整）如何影响最终结果以及这些结果如何与业务目标相关联。

# 实验设置

我们从模拟一个不平衡的表格数据集开始，该数据集包含 90%的负样本和 10%的正样本。

![](img/313f7fec937b48e91d68b228a18088ee.png)

模拟数据的目标分布 [图像由作者提供]

我们可以将少数类（在我们案例中为 10%的样本）想象为在固定时间范围内流失的客户、发动机系统中发生的故障，或发生的欺诈数量。**对于数据科学家而言，处理现实世界中的不平衡数据是常态**。

处理不平衡问题是困难的。与其与极端不平衡作斗争，不如采用一种简单且在大多数情况下有效的方法，即在学习阶段利用这种不平衡。换句话说，**最好不要尝试过采样方法，而是直接对多数类进行欠采样或保持其不变**。通过应用合理的欠采样比例，可以使模型从数据中学习。此外，这种不平衡现象在推理时得到保留和再现。

![](img/c58909b178b150978d7d87a088198efc.png)

处理目标不平衡的技术比较 [图像由作者提供]

牢记这一简单的建模策略后，我们准备深入探讨模型选择。

# 模型选择

![](img/715a2cbb1411667d96ff80ec13419c93.png)

机器学习用例生命周期 [图像由作者提供]

**在不平衡场景中寻找最佳模型或参数集时，合适的度量选择依赖于基于评分的度量**。我们指的是所有使用预测概率来评估拟合优度的度量。在二分类的背景下，最常见的评分度量包括[AUC](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.roc_auc_score.html#sklearn.metrics.roc_auc_score)、[平均精度](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.average_precision_score.html#sklearn.metrics.average_precision_score)或[交叉熵](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.log_loss.html)。

在这种情况下使用评分指标似乎是一个合理的解决方案。我们独立于像[准确率](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.accuracy_score.html)、[精确率](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.precision_score.html#sklearn.metrics.precision_score)、[召回率](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.recall_score.html#sklearn.metrics.recall_score)或[Fbeta](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.fbeta_score.html#sklearn.metrics.fbeta_score)这样的硬阈值来评估拟合的好坏。

回到我们的模拟用例… 假设业务利益相关者定义的一个要求是获得对少数类的高精确度。我们如何进行模型选择和参数调整以满足这一要求？

```py
model = RandomizedSearchCV(
    XGBRFClassifier(random_state=1234), 
    dict(n_estimators=stats.randint(50,300)), 
    n_iter=20, random_state=1234,
    cv=5, n_jobs=-1, 
    refit=False, error_score='raise',
    scoring={
        'fbeta': make_scorer(fbeta_score, beta=0.1), 
        'roc_auc':'roc_auc', 
        'average_precision':'average_precision'
    },
).fit(X, y)
```

我们设置了一个随机森林的随机搜索，寻找最佳的树木数量。我们注册了 AUC、平均精确度和 Fbeta 的交叉验证评分。我们选择**Fbeta 低 beta 值（0.1）作为精确度的近似值**（我们试图优化的目标）。试验结果在下面的图中报告。

![](img/29a56bd68514c5cec13ee05be0493aa6.png)

Fbeta 作为 AUC（左侧）和平均精确度（右侧）的函数 [作者提供的图片]

正如预期的那样，AUC/平均精确度与 Fbeta 之间没有明确的关系。**选择具有最佳 AUC 的模型并不能保证选择具有最佳 Fbeta 的模型**。

此时，使用根据 AUC 选择的“最佳”参数配置，我们需要在更新的数据集上进行额外的微调，以选择一个硬阈值来最大化精确度并使我们的利益相关者满意。

这样做没有什么不好的，但是否有更有效的方法？**我们能否将阈值调整与参数选择相关联？**

将阈值搜索嵌入到模型训练中是直接的。**使用 *ThresholdClassifier* 估算器，可以在优化定义的评分函数（在我们例子中是 Fbeta）的同时调整二分类阈值**。这会在通过拆分接收到的训练数据得到的验证集上自动完成。通过根据调整后的阈值离散化概率来获得预测类别。

```py
from sklearn.metrics import fbeta_score
from sklearn.model_selection import train_test_split
from sklearn.base import clone, BaseEstimator, ClassifierMixin

class ThresholdClassifier(BaseEstimator, ClassifierMixin):

    def __init__(self, estimator, refit=True, val_size=0.3):
        self.estimator = estimator
        self.refit = refit
        self.val_size = val_size

    def fit(self, X, y):

        def scoring(th, y, prob):
            pred = (prob > th).astype(int)
            return 0 if not pred.any() else \
                -fbeta_score(y, pred, beta=0.1) 

        X_train, X_val, y_train, y_val = train_test_split(
            X, y, stratify=y, test_size=self.val_size, 
            shuffle=True, random_state=1234
        )

        self.estimator_ = clone(self.estimator)
        self.estimator_.fit(X_train, y_train)

        prob_val = self.estimator_.predict_proba(X_val)[:,1]
        thresholds = np.linspace(0,1, 200)[1:-1]
        scores = [scoring(th, y_val, prob_val) 
                    for th in thresholds]
        self.score_ = np.min(scores)
        self.th_ = thresholds[np.argmin(scores)]

        if self.refit:
            self.estimator_.fit(X, y)
        if hasattr(self.estimator_, 'classes_'):
            self.classes_ = self.estimator_.classes_

        return self

    def predict(self, X):
        proba = self.estimator_.predict_proba(X)[:,1]
        return (proba > self.th_).astype(int)

    def predict_proba(self, X):
        return self.estimator_.predict_proba(X)
```

***ThresholdClassifier*** 估算器与模型无关，可以与任何输出概率的二分类器一起使用。在我们的例子中，我们将其应用于我们的随机森林，允许像以前一样寻找最佳参数。

![](img/43f99d171f0503304442cf37fde39d4a.png)

Fbeta 作为 AUC（左侧）和平均精确度（右侧）的函数 [作者提供的图片]

不出意外的是，AUC/平均精度与 Fbeta 之间没有关系。比较原始随机森林和使用阈值调整的随机森林所得到的分数，我们观察到 Fbeta 值的差异。

![](img/60f6dbe9023c9a6bba28742436fa1f39.png)

对于相同参数集，使用（红色）和不使用阈值调整（蓝色）得到的 Fbeta [作者提供的图片]

**寻找分类阈值的最优值可以在相同参数集下提供更好的少数类精度**。结果不会影响产生的概率。评分指标，如 AUC 或平均精度，保持不变。

![](img/927330c1514cb30dbb1ece538c378612.png)

Fbeta，作为 AUC（左）和平均精度（右）的函数，使用（红色）和不使用阈值调整（蓝色）得到的 [作者提供的图片]

我们并不是为了通过观察验证指标的微小改进来宣称最优性能的模型。我们必须追求业务目标。在我们的模拟场景中，显而易见的是，通过简单的技巧**我们可以获得更好的精度**。值得注意的是，我们可以**在没有额外验证数据和将参数搜索与阈值调整结合的情况下得到这些发现**。

# 总结

在这篇文章中，我们概述了评分指标和基于准确性的指标之间的主要区别。我们观察了它们在不平衡的二分类上下文中的表现，以解决实际业务问题。如果我们的目标是衡量我们在检测流失客户、识别欺诈或发现故障引擎部件方面的能力，仅使用 AUC 可能会产生不完整或次优的解决方案。像往常一样，我们必须从一开始就深入了解业务逻辑，并尽力满足这些需求。

[**查看我的 GitHub 仓库**](https://github.com/cerlymarco/MEDIUM_NoteBook)

保持联系：[Linkedin](https://www.linkedin.com/in/marco-cerliani-b0bba714b/)
