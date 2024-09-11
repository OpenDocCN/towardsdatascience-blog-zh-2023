# 揭示偏差调整的力量：在类别不平衡数据集中提升预测精度

> 原文：[https://towardsdatascience.com/unveiling-the-power-of-bias-adjustment-enhancing-predictive-precision-in-imbalanced-datasets-ecad1836fc58?source=collection_archive---------2-----------------------#2023-08-13](https://towardsdatascience.com/unveiling-the-power-of-bias-adjustment-enhancing-predictive-precision-in-imbalanced-datasets-ecad1836fc58?source=collection_archive---------2-----------------------#2023-08-13)

[](https://sirano1004.medium.com/?source=post_page-----ecad1836fc58--------------------------------)[![Hyung Gyu Rho](../Images/ce0248c75a21e0871d8e0b9fdf3c55b6.png)](https://sirano1004.medium.com/?source=post_page-----ecad1836fc58--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ecad1836fc58--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ecad1836fc58--------------------------------) [Hyung Gyu Rho](https://sirano1004.medium.com/?source=post_page-----ecad1836fc58--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F89f7d2237f82&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-power-of-bias-adjustment-enhancing-predictive-precision-in-imbalanced-datasets-ecad1836fc58&user=Hyung+Gyu+Rho&userId=89f7d2237f82&source=post_page-89f7d2237f82----ecad1836fc58---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ecad1836fc58--------------------------------) ·11 分钟阅读·2023年8月13日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fecad1836fc58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-power-of-bias-adjustment-enhancing-predictive-precision-in-imbalanced-datasets-ecad1836fc58&user=Hyung+Gyu+Rho&userId=89f7d2237f82&source=-----ecad1836fc58---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fecad1836fc58&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Funveiling-the-power-of-bias-adjustment-enhancing-predictive-precision-in-imbalanced-datasets-ecad1836fc58&source=-----ecad1836fc58---------------------bookmark_footer-----------)

> 解决类别不平衡问题对于数据科学中的准确预测至关重要。本文介绍了偏差调整，以提高模型在类别不平衡情况下的准确性。探索偏差调整如何优化预测并克服这一挑战。

# **介绍**

在数据科学领域，有效管理不平衡数据集对于准确预测至关重要。不平衡的数据集以显著的类别差异为特征，可能导致模型偏向多数类，并对少数类的表现不佳，尤其是在诸如欺诈检测和疾病诊断等关键领域。

本文介绍了一种实用的解决方案，称为偏差调整。通过微调模型中的偏差项，它可以对抗类别不平衡，增强模型对多数类和少数类的准确预测能力。文章概述了针对二分类和多分类的算法，并探讨了它们的基本原理。值得注意的是，算法解释和基本原理部分严格建立了我的算法、过采样和调整类别权重之间的理论联系，从而加深了读者的理解。

为了证明其有效性和合理性，一项模拟研究考察了偏差调整与过采样之间的关系。此外，还通过实际应用展示了在信用卡欺诈检测中实现偏差调整的实施过程和实际效益。

偏差调整为在面对类别不平衡时优化预测建模结果提供了直接且有影响力的途径。本文提供了对偏差调整机制、原理及实际应用的全面理解，使其成为数据科学家在不平衡数据集中提升模型性能的重要工具。

# **算法**

偏差调整算法引入了一种方法来解决二分类和多分类任务中的类别不平衡问题。通过在每个训练周期重新校准偏差项，该算法提高了模型处理不平衡数据集的能力。通过调整偏差项，该算法使模型对少数类实例更加敏感，从而提高分类准确性。

## 模型 f(x) 及其在预测中的作用

我们的偏差调整算法的核心概念是*f*(*x*) —— 这是指导我们处理类别不平衡问题的关键因素。*f*(*x*) 作为输入特征*x*和最终预测之间的连接。在二分类中，它作为一种映射，将输入转换为实际值，并与用于概率解释的 sigmoid 激活函数对齐。在多分类中，*f*(*x*) 变成一组函数，*f_k*​(*x*)，其中每个类别*k*都有自己的函数，与 softmax 激活函数同步工作。这种区别在我们的偏差调整算法中至关重要，我们使用*f*(*x*) 来调整偏差项并微调对类别不平衡的敏感性。

## 算法概述

算法的概念很简单：计算每个类别 *k* 的 *f_k*​(*x*) 的平均值，并将该平均值表示为 *δk*​。通过从 *f_k*​(*x*) 中减去 *δk*​，我们确保 *f_k*​(*x*) − *δk*​ 的期望值对于每个类别 *k* 都变为0。因此，模型预测每个类别发生的概率是相同的。虽然这提供了对算法原理的简要了解，但重要的是要注意，这种方法有理论和数学基础支撑，后续章节将进一步探讨。

## 二分类算法

![](../Images/e6293ba872ad3b540f9cd23197189f24.png)

作者创作

**预测的利用**：在进行预测时，应用算法中最后计算的 **δ** 值。该 **δ** 值反映了在训练过程中所做的累计调整，并作为预测时 sigmoid 激活函数中最终偏差项的基础。

![](../Images/a4a52dfc7014e6b66e345a243c864b93.png)

## 多类别算法

![](../Images/1799b9446e951ce30b308f0a72cd0f2b.png)

作者创作

**预测的利用**：我们算法训练过程的最终结果是一个关键元素——最后计算的 δk 值。该 δk 值封装了在训练过程中精心调整的累计偏差项。它的重要性在于，它作为预测时 softmax 激活函数中最终偏差项的基础参数。

![](../Images/4d4e7537ab3c519e04fbfa2dfd3eb397.png)

作者创作

# 算法解释与基本原理

> 从过采样到调整类别权重，从调整类别权重到新算法

在本节中，我们将深入探讨算法的解释和基本原理。我们的目标是阐明算法操作的机制和原理，提供关于其在分类任务中解决类别不平衡问题的有效性的见解。

## 损失函数与不平衡

我们从深入探讨算法的核心——损失函数开始。对于初步阐述，我们将研究未直接解决类别不平衡问题的损失函数。假设一个二分类问题，其中类别1占90%的观察值，类别0占剩余的10%。我们将类别1的观察值集合表示为 C1，将类别0的观察值集合表示为 C0，以此作为起点。

在未解决类别不平衡问题的情况下，损失函数的形式为：

![](../Images/b02fec09ab7ad330f7e894557d973f43.png)

作者创作

在模型估计过程中，我们努力最小化这个损失函数：

![](../Images/62429da598d38fdc82aa3a41f804fed5.png)

作者创作

## 缓解不平衡：过采样和调整类别权重

然而，我们工作的核心在于解决类别不平衡问题。为了克服这一挑战，我们探讨了过采样技术。虽然存在各种过采样方法——包括简单过采样、随机过采样、SMOTE等——为了展示的清晰性，我们目前关注简单过采样，并稍微涉及随机过采样。

**简单过采样：** 我们工具箱中的一种基本方法是简单过采样，这是一种将少数类别的实例按八倍因子复制以匹配多数类别大小的技术。在我们的示例中，少数类别占10%，多数类别占90%，我们将少数类别的观察值复制八倍，从而有效地平衡类别分布。将复制的观察值集合记为D0，这一步骤将我们的损失函数转化如下：

![](../Images/4993b018b94f60b98c45f133295481f6.png)

作者创建

这揭示了一个深刻的见解：简单过采样的核心原则与调整类别权重的概念无缝对应。将少数类别复制八倍有效地等同于将少数类别的权重增加到九倍。显著的是，过采样技术与权重调整的机制相似。

**随机过采样：** 对随机过采样的简单思考突显了一个相似的观察。随机过采样，与其更简单的对应方法类似，充当了观察权重随机调整的等效方法。

## **从调整类别权重到调整偏差**

一个关键的启示强调了我们方法的核心：偏差调整、过采样和权重调整之间的本质等价性。这一洞察来源于

> “Prentice和Pyke（1979）……已经表明，当模型包含每个类别的常数（截距）项时，这些常数项是唯一受Y的不平等选择概率影响的系数” Scott & Wild (1986) [2]。此外，Manski和Lerman（1977）在softmax设置中也显示了相同的结果 [1]。

**揭示重要性：** 将这一洞察转化为机器学习领域，常数（截距）项就是偏差项。这一基本观察揭示了，当我们重新校准类别权重或观察权重时，结果变化主要表现为对偏差项的调整。简而言之，偏差项是将我们的策略与解决类别不平衡问题的关键连接起来的枢纽。

## 统一视角

这种理解提供了一个直接的解释，说明我们的算法、过采样和权重调整在本质上是可互换和替代的。这种统一性简化了我们的方法，同时保持了其在缓解类别不平衡问题上的有效性。

# 模拟研究：通过过采样验证偏差项的影响

为了巩固我们的主张，即过采样主要影响偏置项，同时保持模型的功能核心不变，我们深入进行了一项针对性的模拟研究。我们的目标是实证演示过采样技术如何仅影响偏置项，而不改变模型的本质。

## 模拟设置

为了说明这一目的，我们关注一个简化的场景：具有单一特征的逻辑回归。我们的模型定义为：

![](../Images/90ab09648be9ab0c4594e647a59c1e96.png)

作者创建

其中 **1**(.) 表示指示函数，x_i 从标准正态分布中抽取，*e_i* 服从逻辑斯蒂分布。在这种情况下，我们设定 *f*(*x*)=*x*。

## 运行模拟：

使用此设置，我们仔细检查了过采样技术对偏置项的影响，同时保持模型的核心不变。我们进行了三种过采样方法：简单过采样、SMOTE 和随机采样。每种方法都经过仔细应用，结果也被仔细记录。

下面的 Python 代码片段概述了模拟过程：

```py
# Load Packages
import numpy as np
import statsmodels.api as sm
from imblearn.over_sampling import SMOTE, RandomOverSampler

# Set seed
np.random.seed(1)
# Create Simulation Datasets
x = np.random.normal(size = 10000)
y = (2.5 + x + np.random.logistic(size = 10000)) > 0
# Bias term is set to 2.5 and coefficient of x to 1

# The size of class 1 is 9005
print(sum(y == 1))
# The size of class 0 is 995
print(sum(y == 0))

# We want to match the size of class 0 to that of class 1
# Method 0 Don't do anything
x0 = x
y0 = y
method0 = sm.Logit(y0, sm.add_constant(x0)).fit()
print(method0.summary())  # 2.54 bias term and 0.97 x3 coefficient

# Method 1 Simple Oversampling
x1 = np.concatenate((x, np.repeat(x[y == 0], 8)))
y1 = np.concatenate((y, np.array([0] * (len(x1) - len(x)))))
method1 = sm.Logit(y1, sm.add_constant(x1)).fit()
print(method1.summary())  # 0.35 bias term and 0.98 x3 coefficient

# Method 2 SMOTE
smote = SMOTE(random_state = 1)
x2, y2 = smote.fit_resample(x[:, np.newaxis], y)
method2 = sm.Logit(y2, sm.add_constant(x2)).fit()
print(method2.summary())  # 0.35 bias term and 1 x3 coefficient

# Method 3 Random Sampling
random_sampler = RandomOverSampler(random_state=1)
x3, y3 = random_sampler.fit_resample(x[:, np.newaxis], y)
method3 = sm.Logit(y3, sm.add_constant(x3)).fit()
print(method3.summary()) # 0.35 bias term and 0.99 x3 coefficient
```

## 结果：

![](../Images/cf669eaba6a84a8ca92c2ddd8dcffbff.png)

模拟结果；作者创建

## 主要观察

我们的模拟研究结果简洁地验证了我们的主张。尽管应用了各种过采样方法，但核心模型函数 *f*(*x*)=*x* 保持不变。关键见解在于模型组件在所有过采样技术中保持了显著的一致性。相反，偏置项表现出明显的变化，证实了我们的观点，即过采样主要影响偏置项，而不改变模型的基本结构。

## 加强核心概念

我们的模拟研究无疑强调了过采样、权重调整和偏置项修改之间的基本等价性。通过展示过采样仅改变偏置项，我们强化了这些策略作为对抗类别不平衡的可互换工具的原则。

# 将偏置调整算法应用于信用卡欺诈检测

为了展示我们偏置调整算法在解决类别不平衡问题上的有效性，我们使用了一个来自 [Kaggle 竞赛](https://www.kaggle.com/competitions/playground-series-s3e4/overview) 的真实数据集，专注于信用卡欺诈检测。在这种情况下，挑战在于预测一笔信用卡交易是否欺诈（标记为 1）或非欺诈（标记为 0），考虑到欺诈案件的固有稀少性。

我们首先加载必要的包并准备数据集：

```py
import numpy as np
import pandas as pd
import tensorflow as tf
import tensorflow_addons as tfa
from sklearn.model_selection import train_test_split
from imblearn.over_sampling import SMOTE, RandomOverSampler

# Load and preprocess the dataset
df = pd.read_csv("/kaggle/input/playground-series-s3e4/train.csv")
y, x = df.Class, df[df.columns[1:-1]]
x = (x - x.min()) / (x.max() - x.min())
x_train, x_valid, y_train, y_valid = train_test_split(x, y, test_size=0.3, random_state=1)
batch_size = 256
train_dataset = tf.data.Dataset.from_tensor_slices((x_train, y_train)).shuffle(buffer_size=1024).batch(batch_size)
valid_dataset = tf.data.Dataset.from_tensor_slices((x_valid, y_valid)).batch(batch_size)
```

然后，我们定义了一个简单的深度学习模型用于二分类，并设置了优化器、损失函数和评估指标。我遵循了竞赛评估并选择了AUC作为评估指标。此外，模型故意被简化，因为本文的重点是展示如何实现偏差调整算法，而不是在预测中取得最佳成绩。

```py
model = tf.keras.Sequential([
    tf.keras.layers.Normalization(),
    tf.keras.layers.Dense(32, activation='swish'),
    tf.keras.layers.Dense(32, activation='swish'),
    tf.keras.layers.Dense(1)
])
optimizer = tf.keras.optimizers.Adam()
loss = tf.keras.losses.BinaryCrossentropy()
val_metric = tf.keras.metrics.AUC()
```

在我们的偏差调整算法的核心中，训练和验证步骤中，我们细致地解决了类别不平衡问题。为了阐明这个过程，我们深入探讨了平衡模型预测的复杂机制。

## 使用累积Delta值的训练步骤

在训练步骤中，我们开始了提升模型对类别不平衡敏感性的旅程。在这里，我们计算并累积了两个不同集群的模型输出之和：`delta0`和`delta1`。这两个集群具有重要意义，分别代表了与类别0和类别1相关的预测值。

```py
# Define Training Step function
@tf.function
def train_step(x, y):
    delta0, delta1 = tf.constant(0, dtype = tf.float32), tf.constant(0, dtype = tf.float32)
    with tf.GradientTape() as tape:
        logits = model(x, training=True)
        y_pred = tf.keras.activations.sigmoid(logits)
        loss_value = loss(y, y_pred)
        # Calculate new bias term for addressing imbalance class
        if len(logits[y == 1]) == 0:
            delta0 -= (tf.reduce_sum(logits[y == 0]))
        elif len(logits[y == 0]) == 0:
            delta1 -= (tf.reduce_sum(logits[y == 1]))
        else:
            delta0 -= (tf.reduce_sum(logits[y == 0]))
            delta1 -= (tf.reduce_sum(logits[y == 1]))
    grads = tape.gradient(loss_value, model.trainable_weights)
    optimizer.apply_gradients(zip(grads, model.trainable_weights))
    return loss_value, delta0, delta1
```

## 验证步骤：使用Delta解决不平衡问题

从训练过程中得出的归一化Delta值在验证步骤中发挥了核心作用。凭借这些精细化的类别不平衡指标，我们将模型的预测与真实的类别分布更准确地对齐。`test_step`函数将这些Delta值集成到自适应调整预测中，最终导致更精细的评估。

```py
@tf.function
def test_step(x, y, delta):
    logits = model(x, training=False)
    y_pred = tf.keras.activations.sigmoid(logits + delta)  # Adjust predictions with delta
    val_metric.update_state(y, y_pred)
```

## 利用Delta值进行不平衡修正

随着训练的进行，我们收集了 encapsulated 在`delta0`和`delta1`集群总和中的宝贵见解。这些累积值成为我们模型预测中固有偏差的指示器。在每个训练周期结束时，我们执行了一个重要的转换。通过将累积的集群总和除以每个类别的相应观察数，我们得到归一化的Delta值。这种归一化作为关键的平衡器，概括了我们偏差调整方法的本质。

```py
E = 1000
P = 10
B = len(train_dataset)
N_class0, N_class1 = sum(y_train == 0), sum(y_train == 1)
early_stopping_patience = 0
best_metric = 0
for epoch in range(E):
    # init delta
    delta0, delta1 = tf.constant(0, dtype = tf.float32), tf.constant(0, dtype = tf.float32)
    print("\nStart of epoch %d" % (epoch,))
    # Iterate over the batches of the dataset.
    for step, (x_batch_train, y_batch_train) in enumerate(train_dataset):
        loss_value, step_delta0, step_delta1 = train_step(x_batch_train, y_batch_train)

        # Update delta
        delta0 += step_delta0
        delta1 += step_delta1

    # Take average of all delta values
    delta = (delta0/N_class0 + delta1/N_class1)/2

    # Run a validation loop at the end of each epoch.
    for x_batch_val, y_batch_val in valid_dataset:
        test_step(x_batch_val, y_batch_val, delta)

    val_auc = val_metric.result()
    val_metric.reset_states()
    print("Validation AUC: %.4f" % (float(val_auc),))
    if val_auc > best_metric:
        best_metric = val_auc
        early_stopping_patience = 0
    else:
        early_stopping_patience += 1

    if early_stopping_patience > P:
        print("Reach Early Stopping Patience. Training Finished at Validation AUC: %.4f" % (float(best_metric),))
        break;
```

## 结果

在我们应用于信用卡欺诈检测的过程中，我们算法的增强效果得到了体现。通过将偏差调整无缝集成到训练过程中，我们获得了令人印象深刻的AUC得分0.77。这与未进行偏差调整时获得的0.71的AUC得分形成了鲜明对比。预测性能的显著改善证明了该算法在处理类别不平衡的复杂性方面的能力，为更准确和可靠的预测铺平了道路。

# 参考文献

[1] Manski, C. F., & Lerman, S. R. (1977). [从基于选择的样本中估计选择概率](https://www.jstor.org/stable/1914121). *Econometrica: Journal of the Econometric Society*, 1977–1988.

[2] Scott, A. J., & Wild, C. J. (1986). [在病例对照或选择性抽样下拟合逻辑模型](https://www.jstor.org/stable/2345712)。*《皇家统计学会B系列：统计方法》*，*48*(2)，170–182。
