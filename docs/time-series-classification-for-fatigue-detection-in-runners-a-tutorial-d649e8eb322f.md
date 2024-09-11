# 对跑步者疲劳检测的时间序列分类 — 一个教程

> 原文：[https://towardsdatascience.com/time-series-classification-for-fatigue-detection-in-runners-a-tutorial-d649e8eb322f?source=collection_archive---------2-----------------------#2023-12-07](https://towardsdatascience.com/time-series-classification-for-fatigue-detection-in-runners-a-tutorial-d649e8eb322f?source=collection_archive---------2-----------------------#2023-12-07)

## 对跑步者可穿戴传感器数据进行的参与者间和参与者内分类的逐步演示

[](https://medium.com/@k.bahavathy?source=post_page-----d649e8eb322f--------------------------------)[![K Bahavathy](../Images/2c67d5d9cb787a50ef52a9d1b4111f9e.png)](https://medium.com/@k.bahavathy?source=post_page-----d649e8eb322f--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d649e8eb322f--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----d649e8eb322f--------------------------------) [K Bahavathy](https://medium.com/@k.bahavathy?source=post_page-----d649e8eb322f--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8911deab0ef9&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-classification-for-fatigue-detection-in-runners-a-tutorial-d649e8eb322f&user=K+Bahavathy&userId=8911deab0ef9&source=post_page-8911deab0ef9----d649e8eb322f---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d649e8eb322f--------------------------------) · 6 分钟阅读 · 2023年12月7日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fd649e8eb322f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-classification-for-fatigue-detection-in-runners-a-tutorial-d649e8eb322f&user=K+Bahavathy&userId=8911deab0ef9&source=-----d649e8eb322f---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fd649e8eb322f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ftime-series-classification-for-fatigue-detection-in-runners-a-tutorial-d649e8eb322f&source=-----d649e8eb322f---------------------bookmark_footer-----------)![](../Images/f1c145157577341e4a9022bf76d38667.png)

图片由作者提供

使用可穿戴传感器收集的运行数据可以提供有关跑步者表现和整体技术的洞见。这些传感器提供的数据通常具有时间序列的特性。本教程讲解了一个疲劳检测任务，其中时间序列分类方法用于跑步数据集。在本教程中，时间序列数据以原始格式使用，而不是从时间序列中提取特征。这导致数据中增加了一个维度，因此使用传统向量格式的数据的传统机器学习算法效果不佳。因此，需要使用特定的时间序列算法。

数据包含跑步者在正常和疲劳状态下的运动捕捉数据。数据使用位于爱尔兰都柏林大学的惯性测量单元（IMU）收集。本教程中使用的数据可以在[https://zenodo.org/records/7997851](https://zenodo.org/records/7997851) 找到。数据呈现一个二分类任务，我们试图预测‘疲劳’和‘非疲劳’之间的区别。在本教程中，我们使用专门的Python包，[Scikit-learn](https://scikit-learn.org/stable/)；这是一个用于Python的机器学习工具包，以及[sktime](https://github.com/sktime/sktime)；这是一个专门为时间序列机器学习创建的库。

数据集包含多个数据通道。在这里，为了简化问题，我们将问题建模为单变量问题，因此仅使用一个数据通道。我们选择了幅度加速度信号，因为它是性能最佳的信号[[1](http://xkdd2023.isti.cnr.it/papers/223.pdf), [2](https://ieeexplore.ieee.org/document/10331612)]。幅度信号是每个方向分量平方和的平方根。

关于数据收集和处理的更多详细信息可以在以下文献中找到，[[1](http://xkdd2023.isti.cnr.it/papers/223.pdf), [2](https://ieeexplore.ieee.org/document/10331612)]。

总结一下，在本教程中：

+   使用最先进的时间序列分类技术在可穿戴传感器收集的数据上执行时间序列分类任务。

+   对跑步者疲劳检测中使用的参与者间模型（全球化）和参与者内模型（个性化）进行了比较。

## 分类任务的设置

首先，我们需要加载分析所需的数据。对于此评估，我们使用“Accel_mag_all.csv”中的数据。我们使用pandas加载数据。确保你已经从[https://](https://zenodo.org/records/7997851)[10.5281/zenodo.7997850](https://zenodo.org/doi/10.5281/zenodo.7997850) 下载了此文件。

```py
import pandas as pd

filename = "Accel_mag_all.csv"
data = pd.read_csv(filename, header = None)
```

需要从sktime和sklearn包中调用一些函数，因此我们在开始分析之前加载它们：

```py
from sktime.transformations.panel.rocket import Rocket
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import RidgeClassifierCV, LogisticRegression, LogisticRegressionCV
from sklearn.model_selection import LeaveOneGroupOut
```

接下来，我们将标签和参与者编号分开。从这里开始，数据将通过数组来表示。

```py
import numpy as np

X = data.iloc[:,2:].values

y =  data[1].values
participant_no =  data[0].values
```

对于这个任务，我们将使用Rocket变换结合岭回归分类器。Rocket是一种用于时间序列分类的最先进技术[3]。Rocket通过生成随机卷积核，然后沿时间序列进行卷积来生成特征图。然后，在此特征图上使用简单的线性分类器，例如岭回归分类器。可以创建一个管道，首先使用Rocket对数据进行变换，标准化特征，最后使用岭回归分类器进行分类。

```py
rocket_pipeline_ridge = make_pipeline(
    Rocket(random_state=0), 
    StandardScaler(), 
    RidgeClassifierCV(alphas=np.logspace(-3, 3, 10))
)
```

## 全球化分类

在我们有来自多个参与者的数据的应用中，使用所有数据一起意味着一个参与者的数据可能同时出现在训练集和测试集中。为避免这种情况，通常进行留一法（LOSO）分析，即模型在所有参与者的基础上进行训练，但测试时只测试一个被排除的参与者。这对于每个参与者都重复此过程。这种方法可以测试模型在参与者之间泛化的能力。

```py
logo = LeaveOneGroupOut()

logo.get_n_splits(X, y, participant_no)

Rocket_score_glob = []
for i, (train_index, test_index) in enumerate(logo.split(X, y, participant_no)):
    rocket_pipeline_ridge.fit(X[train_index], y[train_index])

    Rocket_score = rocket_pipeline_ridge.score(X[test_index],y[test_index])
    Rocket_score_glob = np.append(Rocket_score_glob, Rocket_score)
```

打印出上述结果的摘要：

```py
print("Global Model Results")
print(f"mean accuracy: {np.mean(Rocket_score_glob)}")
print(f"standard deviation: {np.std(Rocket_score_glob)}")
print(f"minimum accuracy: {np.min(Rocket_score_glob)}")
print(f"maximum accuracy: {np.max(Rocket_score_glob)}")
```

上述代码的输出：

```py
Global Model Results
mean accuracy: 0.5919805636306338
standard deviation: 0.10360659996594646
minimum accuracy: 0.4709480122324159
maximum accuracy: 0.8283582089552238
```

从这次LOSO分析中得到的准确率明显较低，有些数据集的结果甚至和随机猜测一样差。这表明，一个参与者的数据可能无法很好地泛化到另一个参与者。这是处理个人传感数据时常见的问题，因为运动技术和整体生理状况因人而异。此外，在此应用中，一个人如何应对疲劳可能与另一个人不同。让我们看看通过个性化模型是否可以提高性能。

## 个性化分类

在构建个性化模型时，预测是基于个人的数据进行的。在将时间序列数据分为训练集和测试集时，应以数据未被打乱的方式进行。为此，我们将每个类别分为单独的训练集和测试集，以保持训练集和测试集中每个类别的比例，同时保持数据的时间序列特性。使用跑步的前三分之一的数据来训练模型，以预测最后三分之一的数据。

```py
Rocket_score_pers = []
for i, (train_index, test_index) in enumerate(logo.split(X, y, participant_no)):

    #print(f"Participant: {participant_no[test_index][0]}")
    label = y[test_index]
    X_S = X[test_index]

    # Identify the indices for each class
    class_0_indices = np.where(label == 'NF')[0]
    class_1_indices = np.where(label == 'F')[0]

    # Split each class into train and test using indexing
    class_0_split_index = int(0.66 * len(class_0_indices))
    class_1_split_index = int(0.66 * len(class_1_indices))

    X_train = np.concatenate((X_S[class_0_indices[:class_0_split_index]], X_S[class_1_indices[:class_1_split_index]]), axis=0)
    y_train = np.concatenate((label[class_0_indices[:class_0_split_index]], label[class_1_indices[:class_1_split_index]]), axis=0)

    X_test = np.concatenate((X_S[class_0_indices[class_0_split_index:]],X_S[class_1_indices[class_1_split_index:]]), axis=0)
    y_test = np.concatenate((label[class_0_indices[class_0_split_index:]], label[class_1_indices[class_1_split_index:]]), axis=0)

    rocket_pipeline_ridge.fit(X_train, y_train)

    Rocket_score_pers = np.append(Rocket_score_pers, rocket_pipeline_ridge.score(X_test,y_test))
```

打印出上述结果的摘要：

```py
print("Personalised Model Results")
print(f"mean accuracy: {np.mean(Rocket_score_pers)}")
print(f"standard deviation: {np.std(Rocket_score_pers)}")
print(f"minimum accuracy: {np.min(Rocket_score_pers)}")
print(f"maximum accuracy: {np.max(Rocket_score_pers)}")
```

上述代码的输出：

```py
Personalised Model Results
mean accuracy: 0.9517626092184379
standard deviation: 0.07750979452994386
minimum accuracy: 0.7037037037037037
maximum accuracy: 1.0
```

通过个性化模型，性能得到了显著提升。因此，在此应用中，显然从一个人到另一个人的泛化存在困难。

## 结论

为了对来自可穿戴传感器的时间序列数据进行分类，使用了最先进的技术Rocket。这项分析表明，在这一领域中，个性化模型能带来更好的分类模型。

![](../Images/04d298758a397c3a3668a96122ee3054.png)

全球分类与每个参与者的个性化分类所获得的准确率

上图显示了使用个性化模型在性能上的显著提升，对于许多参与者，性能几乎翻倍。这种现象可能与个体间生理和跑步技巧的差异有关。从用户的角度来看，全球模型和个性化模型在不同的应用场景下都有其优点。例如，在需要监测个体用户运动技巧的临床环境中，个性化模型可能会很有用。然而，从单个个体收集足够的数据以进行准确预测可能是困难的，因此对于许多应用来说，全球模型将是理想的选择。

本教程中介绍的代码也可以在 github 上找到: [https://github.com/bahavathyk/TSC_for_Fatigue_Detection](https://github.com/bahavathyk/TSC_for_Fatigue_Detection.git)

## 参考文献：

[1] B. Kathirgamanathan, T. Nguyen, G. Ifrim, B. Caulfield, P. Cunningham. 通过对可穿戴传感器数据进行时间序列分析来解释跑步者的疲劳，XKDD 2023: 第五届国际解释性知识发现数据挖掘研讨会，ECML PKDD，2023，[http://xkdd2023.isti.cnr.it/papers/223.pdf](http://xkdd2023.isti.cnr.it/papers/223.pdf)

[2] B. Kathirgamanathan, B. Caulfield 和 P. Cunningham，“基于惯性测量单元的全球化运动分类模型，”2023 IEEE 第19届国际体感网络会议（BSN），波士顿，马萨诸塞州，美国，2023年，页码 1–4，doi: 10.1109/BSN58485.2023.10331612。

[3] A. Dempster, F. Petitjean 和 G. I. Webb. ROCKET：使用随机卷积核进行极其快速和准确的时间序列分类。《数据挖掘与知识发现》，34(5)：1454–1495，2020。
