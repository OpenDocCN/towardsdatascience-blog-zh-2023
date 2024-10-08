# PyTorch 简介：从训练循环到预测

> 原文：[`towardsdatascience.com/introduction-to-pytorch-from-training-loop-to-prediction-a70372764432`](https://towardsdatascience.com/introduction-to-pytorch-from-training-loop-to-prediction-a70372764432)

## *对 PyTorch 训练循环和应对库陡峭初学曲线的一般方法的介绍*

[](https://medium.com/@theDrewDag?source=post_page-----a70372764432--------------------------------)![Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a70372764432--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a70372764432--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----a70372764432--------------------------------) [Andrea D'Agostino](https://medium.com/@theDrewDag?source=post_page-----a70372764432--------------------------------)

·发布在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----a70372764432--------------------------------) ·14 分钟阅读·2023 年 3 月 28 日

--

![](img/39bd895d764577c1f195119868a023ec.png)

作者提供的图片。

在这篇文章中，我们将讨论如何在 Python 中使用 PyTorch 实现逻辑回归模型。

**PyTorch 是全球数据科学家和机器学习工程师社区中最著名和最常用的深度学习框架之一**，因此，如果你希望在应用 AI 领域建立职业生涯，学习这个工具是你学习路径中的一个关键步骤。

它与 TensorFlow 并列，TensorFlow 是由 Google 开发的另一个非常著名的深度学习框架。

除了 API 的结构和组织可能非常不同外，基本原理没有显著的差别。

虽然这两个框架都允许我们创建非常复杂的神经网络，但 PyTorch 通常更受欢迎，因为它的风格更加*python 化*，并且允许开发者将自定义逻辑集成到软件中。

我们将使用**Sklearn 乳腺癌数据集**，这是一个开源数据集，之前在我一些文章中已被使用，用于训练一个二分类模型。

目标是解释如何：

+   从 pandas 数据框转换到 PyTorch 的 Datasets 和 DataLoaders

+   在 PyTorch 中创建一个用于二分类的神经网络

+   创建预测

+   使用实用函数和 matplotlib 评估我们的模型表现

+   使用这个网络进行预测

到文章末尾，**我们将对如何在 PyTorch 中创建神经网络以及训练循环的工作原理有清晰的了解**。

开始吧！

# 安装 PyTorch 及其他依赖

我们通过在专用文件夹中创建虚拟环境来开始我们的项目。

访问此链接以了解如何使用 Conda 创建虚拟环境。

[](/how-to-set-up-a-development-environment-for-machine-learning-b015a91bda8a?source=post_page-----a70372764432--------------------------------) ## 如何为机器学习设置开发环境

### 如何安装、激活和使用用于机器学习和数据科学相关任务的虚拟环境

[towardsdatascience.com

一旦我们的虚拟环境创建完成，我们可以运行以下命令

```py
$ pip install torch -U
```

在终端中。该命令将安装 PyTorch 的最新版本，截至撰写本文时为 2.0 版。

启动一个笔记本，我们可以在执行`import torch`之后使用`torch.__version__`检查库的版本。

我们可以通过导入并运行一个小测试脚本来验证 PyTorch 是否在环境中正确安装，如官方指南所示。

```py
import torch

x = torch.rand(5, 3)
print(x)

>>> tensor([[0.3890, 0.6087, 0.2300],
        [0.1866, 0.4871, 0.9468],
        [0.2254, 0.7217, 0.4173],
        [0.1243, 0.1482, 0.6797],
        [0.2430, 0.4608, 0.8886]])
```

如果脚本正确执行，则我们可以继续进行项目。否则，我建议读者参考这里的官方指南 [`pytorch.org/get-started/locally/`](https://pytorch.org/get-started/locally/)。

让我们继续安装其他依赖项：

+   Sklearn; `pip install scikit-learn`

+   Pandas; `pip install pandas`

+   Matplotlib; `pip install matplotlib`

像 Numpy 这样的库在安装 PyTorch 时会自动安装。

# 导入和探索数据集

让我们从导入已安装的库和 Sklearn 的乳腺癌数据集开始，使用以下代码片段

```py
import torch
import pandas as pd
import numpy as np

from sklearn.datasets import load_breast_cancer

import matplotlib.pyplot as plt

breast_cancer_dataset = load_breast_cancer(as_frame=True, return_X_y=True)
```

让我们创建一个数据框来专门保存我们的 X 和 y，如下所示

```py
df = breast_cancer_dataset[0]
df['target'] = breast_cancer_dataset[1]
df
```

![](img/6de5745961fab2ab6db54bbca2a26f6a.png)

数据框的示例。图片由作者提供。

我们的目标是创建一个可以根据其他列的特征预测目标列的模型。

让我们进行最低限度的探索性分析，以了解数据集。我们将使用 sweetviz 库自动创建分析报告。

我们可以使用命令`pip install sweetviz`安装 sweetviz，并使用这段代码创建一个 EDA（探索性数据分析）报告

```py
import sweetviz as sv

eda_report = sv.analyze(df)
eda_report.show_notebook()
```

![](img/6fad2c75b0b924be07485ffb418ebf66.png)

Sweetviz 正在分析我们的数据集。图片由作者提供。

Sweetviz 将直接在我们的笔记本中创建报告供我们探索。

![](img/321ed6120d3081ba0f5b948dc8335335.png)

Sweetviz 中的“关联”标签。图片由作者提供。

我们看到多个列与目标列的值 0 或 1 高度相关。

由于这是一个多维数据集，且具有不同分布的变量，神经网络是建模该数据的一个有效选项。也就是说，该数据集也可以通过更简单的模型进行建模，如决策树。

现在，我们将导入另外两个库，以便可视化数据集。**我们** **将使用 Sklearn 和 Seaborn 中的 PCA（主成分分析）**来可视化多维数据集。

PCA 将帮助我们将大量变量压缩成两个变量，我们将把这两个变量用作 Seaborn 散点图中的 X 轴和 Y 轴。Seaborn 使用一个额外的参数*色调*来根据附加变量为点上色。我们将使用我们的目标。

```py
import seaborn as sns
from sklearn import decomposition

pca = decomposition.PCA(n_components=2)

X = df.drop("target", axis=1).values
y = df['target'].values

vecs = pca.fit_transform(X)
x0 = vecs[:, 0]
x1 = vecs[:, 1]

sns.set_style("whitegrid")
sns.scatterplot(x=x0, y=x1, hue=y)
plt.title("Proiezione PCA")
plt.xlabel("PCA 1")
plt.ylabel("PCA 2")
plt.xticks([])
plt.yticks([])
plt.show()
```

![](img/49fd8751c34d14f60d7717deed2fa64a.png)

乳腺癌数据集的 PCA 投影。图像由作者提供。

我们看到类别 1 的数据点基于共同特征分组。我们的神经网络的目标是将行分类为目标 0 或 1。

# 创建数据集和数据加载器类

PyTorch 提供了`Dataset`和`DataLoader`对象，允许我们高效地组织和加载数据到神经网络中。

可以直接使用 pandas，但这样做会有缺点，因为它会使我们的代码效率降低。

`Dataset`类允许我们指定数据的正确格式，并应用常常是基础的检索和转换逻辑（例如图像的数据增强）。

让我们看看如何创建一个 PyTorch `Dataset`对象。

```py
from torch.utils.data import Dataset

class BreastCancerDataset(Dataset):
    def __init__(self, X, y):
        # create feature tensors
        self.features = torch.tensor(X, dtype=torch.float32)
        # create label tensors
        self.labels = torch.tensor(y, dtype=torch.long) 

    def __len__(self):
        # we define a method to retrieve the length of the dataset
        return self.features.shape[0]

    def __getitem__(self, idx):
        # necessary override of the __getitem__ method which helps to index our data
        x = self.features[idx]
        y = self.labels[idx]
        return x, y
```

这是一个继承自`Dataset`的类，并允许我们将要创建的`DataLoader`高效地检索数据批次。

该类以 X 和 y 作为输入。

# 训练、验证和测试数据集

在继续以下步骤之前，创建训练、验证和测试集是很重要的。

这些将帮助我们评估模型的性能，并理解预测的质量。

对于感兴趣的读者，我建议阅读文章[训练模型前你应该做的 6 件事](https://medium.com/towards-data-science/6-things-you-should-do-before-training-your-model-51703ab5e125)和[机器学习中的交叉验证是什么](https://medium.com/towards-data-science/what-is-cross-validation-in-machine-learning-14d2a509d6a5)，以更好地理解为什么将数据拆分为三部分是一种有效的性能评估方法。

使用 Sklearn，这变得很简单，使用`train_test_split`方法即可。

```py
from sklearn import model_selection

train_ratio = 0.50
validation_ratio = 0.20
test_ratio = 0.20

x_train, x_test, y_train, y_test = model_selection.train_test_split(X, y, test_size=1 - train_ratio)
x_val, x_test, y_val, y_test = model_selection.train_test_split(x_test, y_test, test_size=test_ratio/(test_ratio + validation_ratio)) 

print(x_train.shape, x_val.shape, x_test.shape)

>>> (284, 30) (142, 30) (143, 30)
```

通过这段小代码，我们根据可控的分割创建了我们的训练、验证和测试集。

# 数据规范化

在进行深度学习时，即使是像二分类这样简单的任务，也总是需要规范化我们的数据。

规范化意味着将数据集中各个列的所有值统一到相同的数值尺度。这有助于神经网络更有效地收敛，从而更快地做出准确的预测。

我们将使用 Sklearn 的`StandardScaler`。

```py
from sklearn import preprocessing

scaler = preprocessing.StandardScaler()

x_train_scaled = scaler.fit_transform(x_train)
x_val_scaled = scaler.transform(x_val)
x_test_scaled = scaler.transform(x_test)
```

注意`fit_transform`仅应用于训练集，而`transform`应用于其他两个数据集。这是为了避免*数据泄露*——即验证集或测试集中的信息无意中泄漏到训练集中。我们希望训练集是唯一的学习来源，不受测试数据影响。

这些数据现在已经准备好输入到`BreastCancerDataset`类中。

```py
train_dataset = BreastCancerDataset(x_train_scaled, y_train)
val_dataset = BreastCancerDataset(x_val_scaled, y_val)
test_dataset = BreastCancerDataset(x_test_scaled, y_test)
```

我们导入 dataloader 并初始化对象。

```py
from torch.utils.data import DataLoader

train_loader = DataLoader(
    dataset=train_dataset,
    batch_size=16,
    shuffle=True,
    drop_last=True
)

val_loader = DataLoader(
    dataset=val_dataset,
    batch_size=16,
    shuffle=False,
    drop_last=True
)

test_loader = DataLoader(
    dataset=test_dataset,
    batch_size=16,
    shuffle=False,
    drop_last=True
)
```

`DataLoader` 的强大之处在于它允许我们指定是否对数据进行洗牌以及数据应该以多少批次提供给模型。**批次大小应被视为模型的超参数**，因此可能会影响推断结果。

# 神经网络在 PyTorch 中的实现

在 PyTorch 中创建模型可能听起来很复杂，但实际上只需要理解一些基本概念。

1.  在 PyTorch 中编写模型时，我们将使用**面向对象的方法**，就像处理数据集一样。这意味着我们将创建一个像 `MyModel` 这样的类，它继承自 PyTorch 的 `nn.Module` 类。

1.  PyTorch 是一个自动微分软件。这意味着当我们基于反向传播算法编写神经网络时，计算导数来计算损失的过程是自动进行的。这需要编写一些专门的代码，第一次遇到时可能会感到困惑。

我建议想了解神经网络工作原理基础的读者查阅文章 [神经网络介绍——权重、偏差和激活](https://medium.com/mlearning-ai/introduction-to-neural-networks-weights-biases-and-activation-270ebf2545aa?source=post_page-----a70372764432--------------------------------)。

[## 神经网络介绍——权重、偏差和激活](https://medium.com/mlearning-ai/introduction-to-neural-networks-weights-biases-and-activation-270ebf2545aa?source=post_page-----a70372764432--------------------------------)

### 神经网络如何通过权重、偏差和激活函数学习

[medium.com](https://medium.com/mlearning-ai/introduction-to-neural-networks-weights-biases-and-activation-270ebf2545aa?source=post_page-----a70372764432--------------------------------)

话虽如此，让我们看看编写逻辑回归模型的代码是怎样的。

```py
class LogisticRegression(nn.Module):
    """
    Our neural network accepts num_features and num_classes.

    num_features - number of features to learn from
    num_classes: number of classes in output to expect (in this case, 1 or 2, since the output is binary (0 or 1))
    """

    def __init__(self, num_features, num_classes):
        super().__init__() # initialize the init method of nn.Module

        self.num_features = num_features
        self.num_classes = num_classes

        # create a single layer of neurons on which to apply the log reg
        self.linear1 = nn.Linear(in_features=num_features, out_features=num_classes) 

    def forward(self, x):
        logits = self.linear1(x) # pass our data through the layer
        probs = torch.sigmoid(logits) # we apply a sigmoid function to obtain the probabilities of belonging to a class (0 or 1)
        return probs # return probabilities
```

我们的类继承自 `nn.Module`。这个类提供了让模型正常工作的后台方法。

## __init__ 方法

类的 `__init__` 方法包含了在 Python 中实例化类时运行的逻辑。在这里我们传递两个参数：特征的数量和要预测的类别数量。

`num_features` 对应于组成数据集的列数减去目标变量，而 `num_classes` 对应于神经网络必须返回的结果数量。

除了这两个参数及其类变量，我们还看到 `super().__init__()` 这一行。super 函数初始化了父类的 init 方法。这使得我们能够在模型中拥有 `nn.Module` 的功能。

在 init 块中，我们实现了一个线性层，称为 `self.linear1`，它的参数是特征的数量和返回结果的数量。

## forward() 方法

通过编写 `forward` 方法，我们告诉 Python 重写 PyTorch 的 `nn.Module` 父类中的相同方法。实际上，这个方法在进行前向传播时被调用——也就是当我们的数据从一层流向另一层时。

`forward` 接受输入 *x*，其中包含模型将根据其性能进行校准的特征。

输入通过第一层，创建了`logits`变量。logits 是神经网络的计算结果，还未通过最终激活函数（在此情况下为 sigmoid）转换为概率。实际上，它们是神经网络在映射到可以解释的函数之前的内部表示。

在这种情况下，sigmoid 函数会将 logits 映射到 0 和 1 之间的概率。如果输出小于 0，则类别为 0，否则为 1。这发生在`self.probs = torch.sigmoid(x)`这一行。

# 用于绘图和准确率计算的实用函数

让我们创建实用函数以在即将看到的训练循环中使用。这两个函数用于计算每个 epoch 结束时的准确率，并在训练结束时显示性能曲线。

```py
def compute_accuracy(model, dataloader):
    """
    This function puts the model in evaluation mode (model.eval()) and calculates the accuracy with respect to the input dataloader
    """
    model = model.eval()
    correct = 0
    total_examples = 0
    for idx, (features, labels) in enumerate(dataloader):
        with torch.no_grad():
            logits = model(features)
        predictions = torch.where(logits > 0.5, 1, 0)
        lab = labels.view(predictions.shape)
        comparison = lab == predictions

        correct += torch.sum(comparison)
        total_examples += len(comparison)
    return correct / total_examples

def plot_results(train_loss, val_loss, train_acc, val_acc):
    """
    This function takes lists of values and creates side-by-side graphs to show training and validation performance
    """
    fig, ax = plt.subplots(1, 2, figsize=(15, 5))
    ax[0].plot(
        train_loss, label="train", color="red", linestyle="--", linewidth=2, alpha=0.5
    )
    ax[0].plot(
        val_loss, label="val", color="blue", linestyle="--", linewidth=2, alpha=0.5
    )
    ax[0].set_xlabel("Epoch")
    ax[0].set_ylabel("Loss")
    ax[0].legend()
    ax[1].plot(
        train_acc, label="train", color="red", linestyle="--", linewidth=2, alpha=0.5
    )
    ax[1].plot(
        val_acc, label="val", color="blue", linestyle="--", linewidth=2, alpha=0.5
    )
    ax[1].set_xlabel("Epoch")
    ax[1].set_ylabel("Accuracy")
    ax[1].legend()
    plt.show()
```

# 模型训练

现在我们进入大多数深度学习新手 struggle 的部分：PyTorch 训练循环。

让我们查看代码，然后进行注释。

```py
import torch.nn.functional as F

model = LogisticRegression(num_features=x_train_scaled.shape[1], num_classes=1)
optimizer = torch.optim.SGD(model.parameters(), lr=0.01)

num_epochs = 10

train_losses, val_losses = [], []
train_accs, val_accs = [], []

for epoch in range(num_epochs):

    model = model.train()
    t_loss_list, v_loss_list = [], []
    for batch_idx, (features, labels) in enumerate(train_loader):

        train_probs = model(features)
        train_loss = F.binary_cross_entropy(train_probs, labels.view(train_probs.shape))

        optimizer.zero_grad()
        train_loss.backward()
        optimizer.step()

        if batch_idx % 10 == 0:
            print(
                f"Epoch {epoch+1:02d}/{num_epochs:02d}"
                f" | Batch {batch_idx:02d}/{len(train_loader):02d}"
                f" | Train Loss {train_loss:.3f}"
            )

        t_loss_list.append(train_loss.item())

    model = model.eval()
    for batch_idx, (features, labels) in enumerate(val_loader):
        with torch.no_grad():
            val_probs = model(features)
            val_loss = F.binary_cross_entropy(val_probs, labels.view(val_probs.shape))
            v_loss_list.append(val_loss.item())

    train_losses.append(np.mean(t_loss_list))
    val_losses.append(np.mean(v_loss_list))

    train_acc = compute_accuracy(model, train_loader)
    val_acc = compute_accuracy(model, val_loader)

    train_accs.append(train_acc)
    val_accs.append(val_acc)

    print(
        f"Train accuracy: {train_acc:.2f}"
        f" | Val accuracy: {val_acc:.2f}"
    )
```

与 TensorFlow 不同，PyTorch 要求我们用纯 Python 编写训练循环。

让我们逐步查看过程：

1.  我们实例化模型和优化器

1.  我们决定一个 epoch 数量。

1.  我们创建一个 for 循环来遍历 epochs。

1.  对于每个 epoch，我们使用`model.train()`将模型设置为训练模式，并循环遍历`train_loader`。

1.  对于每个批次的`train_loader`，计算损失，使用`optimizer.zero_grad()`将梯度计算归零，并使用`optimizer.step()`更新网络的权重。

此时训练循环已经完成，如果需要，可以将相同的逻辑集成到验证数据加载器中，如代码所示。

这是运行此代码后的训练结果。

![](img/8b039bdc7a62f8aa7aebf2d91394de93.png)

训练中。图片由作者提供。

# 神经网络性能评估

我们使用之前创建的实用函数来绘制训练和验证中的损失。

```py
plot_results(train_losses, val_losses, train_accs, val_accs)
```

![](img/da44d373ae986d30b40f3f83af8bb26f.png)

神经网络的性能。图片由作者提供。

我们的二分类模型很快收敛到高准确率，我们可以看到每个 epoch 结束时损失如何下降。

数据集模型简单，样本数量少并没有帮助网络更逐步地收敛到高性能。

我强调，可以将 *TensorBoard* 软件集成到 PyTorch 中，以便在各种实验之间自动记录性能指标。

# 创建预测

我们已经到达了本指南的末尾。让我们查看创建整个数据集预测的代码。

```py
# we transform all our features with the scaler
X_scaled_all = scaler.transform(X)

# transform in tensors
X_scaled_all_tensors = torch.tensor(X_scaled_all, dtype=torch.float32)

# we set the model in inference mode and create the predictions
with torch.inference_mode():
    logits = model(X_scaled_all_tensors)
    predictions = torch.where(logits > 0.5, 1, 0)

df['predictions'] = predictions.numpy().flatten()
```

现在让我们导入 Sklearn 的`metrics` 包，它允许我们直接在 pandas 数据框上快速计算混淆矩阵和分类报告。

```py
from sklearn import metrics
from pprint import pprint

pprint(metrics.classification_report(y_pred=df.predictions, y_true=df.target))
```

![](img/ba65cf7b416913d92c62633c77ee824a.png)

对整个数据集的性能总结，附有分类报告。图片由作者提供。

以及混淆矩阵，它显示了对角线上的正确答案数量。

```py
metrics.confusion_matrix(y_pred=df.predictions, y_true=df.target)

>>> array([[197,  15],
       [ 13, 344]])
```

这里有一个小函数，用于创建一个分类线，将 PCA 图中的类别分开。

```py
def plot_boundary(model):

    w1 = model.linear1.weight[0][0].detach()
    w2 = model.linear1.weight[0][1].detach()
    b = model.linear1.bias[0].detach()

    x1_min = -1000
    x2_min = (-(w1 * x1_min) - b) / w2

    x1_max = 1000
    x2_max = (-(w1 * x1_max) - b) / w2

    return x1_min, x1_max, x2_min, x2_max

sns.scatterplot(x=x0, y=x1, hue=y)
plt.title("PCA Projection")
plt.xlabel("PCA 1")
plt.ylabel("PCA 2")
plt.xticks([])
plt.yticks([])
plt.plot([x1_min, x1_max], [x2_min, x2_max], color="k", label="Classification", linestyle="--")
plt.legend()
plt.show()
```

这是模型如何将良性细胞与恶性细胞区分开来

![](img/c5978be235283290009ada45213ed782.png)

分类边界可视化。图片由作者提供。

# 结论

在本文中，我们已经看到如何使用 PyTorch 从 Pandas 数据框创建一个二分类模型。

我们已经了解了训练循环的样子，如何评估模型，以及如何创建预测和可视化以帮助解释。

使用 PyTorch 可以创建非常复杂的神经网络……只需想到特斯拉，这家基于 AI 的电动车制造商，就使用 PyTorch 创建其模型。

对于那些想要开始深度学习之旅的人来说，尽早学习 PyTorch 成为一个高优先级任务，因为它允许你构建重要的技术，解决复杂的数据驱动问题。

**如果你想支持我的内容创作活动，可以通过下面的推荐链接加入 Medium 会员计划**。我将获得你投资的一部分，你将能够无缝访问 Medium 上大量的数据科学及更多领域的文章。

[](https://medium.com/@theDrewDag/membership?source=post_page-----a70372764432--------------------------------) [## 使用我的推荐链接加入 Medium - Andrea D'Agostino]

### 阅读 Andrea D'Agostino 的每个故事（以及 Medium 上的其他成千上万的作家）。你的会员费直接……

medium.com](https://medium.com/@theDrewDag/membership?source=post_page-----a70372764432--------------------------------)

# 推荐阅读

对于感兴趣的人，这里有一系列我推荐的与 ML 相关的书籍。这些书籍在我看来是**必读**的，并且对我的职业生涯产生了重大影响。

*免责声明：这些是亚马逊附属链接。我将因推荐这些商品而获得亚马逊的小额佣金。你的体验不会改变，你不会被额外收费，但这将帮助我扩展业务，并制作更多关于 AI 的内容。*

+   **机器学习入门：** [*自信的数据技能：掌握数据工作的基础，提升你的职业生涯*](https://amzn.to/3ZzKTz6) 作者：Kirill Eremenko

+   **Sklearn / TensorFlow：** [*动手学机器学习：使用 Scikit-Learn、Keras 和 TensorFlow*](https://amzn.to/433F4Nm) 作者：Aurelien Géron

+   **NLP：** [*文本作为数据：机器学习和社会科学的新框架*](https://amzn.to/3zvH43j) 作者：Justin Grimmer

+   **Sklearn / PyTorch：** [*用 PyTorch 和 Scikit-Learn 学习机器学习：使用 Python 开发机器学习和深度学习模型*](https://amzn.to/3Gcavve) 作者：Sebastian Raschka

+   **数据可视化：** [*数据讲故事：商业专业人士的数据可视化指南*](https://amzn.to/3HUtGtB) 作者：科尔·克纳夫利克

# 有用的链接（由我撰写）

+   **学习如何在 Python 中执行顶级探索性数据分析：** *Python 中的探索性数据分析——逐步过程*

+   **学习 TensorFlow 的基础知识：** [*入门 TensorFlow 2.0——深度学习介绍*](https://medium.com/towards-data-science/a-comprehensive-introduction-to-tensorflows-sequential-api-and-model-for-deep-learning-c5e31aee49fa)

+   **使用 TF-IDF 在 Python 中进行文本聚类：** [*Python 中的 TF-IDF 文本聚类*](https://medium.com/mlearning-ai/text-clustering-with-tf-idf-in-python-c94cd26a31e7)
