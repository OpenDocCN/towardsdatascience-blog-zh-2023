# PyTorch 介绍 — 构建你的第一个线性模型

> 原文：[`towardsdatascience.com/pytorch-introduction-building-your-first-linear-model-d868a8681a41`](https://towardsdatascience.com/pytorch-introduction-building-your-first-linear-model-d868a8681a41)

## 学习如何通过使用“神奇”的 Linear 层来构建你的第一个 PyTorch 模型。

[](https://ivopbernardo.medium.com/?source=post_page-----d868a8681a41--------------------------------)![Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----d868a8681a41--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d868a8681a41--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d868a8681a41--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----d868a8681a41--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d868a8681a41--------------------------------) ·阅读时长 8 分钟·2023 年 12 月 12 日

--

![](img/48de4c355c69f4666a6e07d34fb89e52.png)

回归模型 — AI 生成的图像

在我上一篇博客中，我们学习了[如何使用 PyTorch 张量](https://medium.com/towards-data-science/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b?sk=2cf4d44549664fc647baa3455e9d78e8)，这是 PyTorch 库中最重要的对象。张量是深度学习模型的骨架，因此我们可以利用它们来将更简单的机器学习模型拟合到我们的数据集上。

尽管 PyTorch 以其深度学习能力而闻名，但我们也可以使用该框架来拟合简单的线性模型——这实际上是熟悉`torch` API 的最佳方式之一！

在这篇博客中，我们将继续 PyTorch 介绍系列，查看如何使用 `torch` 库开发一个简单的线性回归模型。在这个过程中，我们将了解 `torch` 的优化器、权重和其他学习模型参数，这对于更复杂的架构将非常有用。

让我们开始吧！

# **加载和处理数据**

在这篇博客中，我们将使用歌曲流行度数据集，我们希望根据一些歌曲特征来预测某首歌曲的流行度。让我们先看一下数据集的前几行：

```py
songPopularity = pd.read_csv(‘./data/song_data.csv’)
```

![](img/d4eab71b89cb0b1002c96e186c73e9d9.png)

歌曲流行度特征列 — 作者提供的图像

这个数据集的一些特征包括关于每首歌曲的有趣指标，例如：

+   歌曲的“能量”级别。

+   对歌曲的关键（例如 A、B、C、D 等）进行标签编码

+   歌曲响度

+   歌曲节奏。

我们的目标是利用这些特征来预测歌曲流行度，这是一个从 0 到 100 的指数。在我们上面展示的示例中，我们旨在预测以下歌曲流行度：

![](img/f50d07927df9b45944546fbada9d76d4.png)

歌曲受欢迎程度目标列— 图片由作者提供

我们将使用 PyTorch 模块来预测这个连续变量，而不是使用 `sklearn`。学习如何在 `pytorch` 中拟合线性回归的好处是什么？我们将获得的知识可以应用于其他复杂模型，如深层神经网络！

让我们从准备数据集开始，首先对特征和目标进行子集划分：

```py
features = ['song_duration_ms', 
            'acousticness', 'danceability', 
            'energy', 'instrumentalness', 
            'key', 'liveness', 'loudness', 
            'audio_mode', 'speechiness', 
            'tempo', 'time_signature', 'audio_valence']

target = 'song_popularity'

songPopularityFeatures = songPopularity[features]
songPopularityTarget = songPopularity[target]
```

我们将使用 `train_test_split` 将数据划分为训练集和测试集。我们将在将数据转换为 `tensors` 之前执行此转换，因为 `sklearn` 的方法会自动将数据转换为 `pandas` 或 `numpy` 格式：

```py
X_train, X_test, y_train, y_test = train_test_split(songPopularityFeatures, songPopularityTarget, test_size = 0.2)
```

创建了 `X_train` 、 `X_test` 、 `y_train` 和 `y_test` 后，我们现在可以将数据转换为 `torch.tensor` — 通过将数据传递给 `torch.Tensor` 函数来完成这个过程很简单：

```py
import torch

def dataframe_to_tensor(df):
    return torch.tensor(df.values, dtype=torch.float32)

# Transform DataFrames into PyTorch tensors using the function
X_train = dataframe_to_tensor(X_train)
X_test = dataframe_to_tensor(X_test)
y_train = dataframe_to_tensor(y_train)
y_test = dataframe_to_tensor(y_test)
```

我们的对象现在是 `torch.Tensor` 格式，这是 `nn.Module` 期望的格式。下面是 `X_train`：

![](img/404cbdaf0a5afb1ec7400d029ee70130.png)

X_train 张量 — 图片由作者提供

很棒——我们已经将训练和测试数据转换为 `tensor` 格式。我们准备好创建我们的第一个 `torch` 模型了，接下来我们将做这件事！

# 构建我们的线性模型

我们将使用一个继承自 `nn.Module` 父类的 `LinearRegressionModel class` 来训练我们的模型。`nn.Module` 类是 Pytorch 所有神经网络的基础类。

```py
from torch import nn

class LinearRegressionModel(nn.Module):
    '''
    Torch Module class.
    Initializes weight randomly and gets trained via train method.
    '''
    def __init__(self, optimizer):
        super().__init__()
        self.optimizer = optimizer

        # Initialize Weights and Bias
        self.weights = nn.Parameter(
            torch.randn(1, 5, dtype=torch.float),
            requires_grad=True)

        self.bias = nn.Parameter(
            torch.randn(1, 5, dtype=torch.float),
            requires_grad=True
            )
```

在这个类中，我们创建对象时只需要一个参数——`optimizer`。我们这样做是因为我们希望在训练过程中测试不同的优化器。在上面的代码中，让我们聚焦于 `# Initialize Weights and Bias` 之后的权重初始化：

```py
self.weights = nn.Parameter(
 torch.randn(1, 13, dtype=torch.float),
 requires_grad=True)

self.bias = nn.Parameter(
            torch.randn(1, dtype=torch.float),
            requires_grad=True
            )
```

线性回归是一个非常简单的函数，公式为 `y = b0 + b1x1 + ... bnxn` 其中：

+   y 等于我们想要预测的目标

+   b0 等于偏差项。

+   b1, …, bn 等于模型的权重（每个变量在最终决策中的权重以及它是负面还是正面贡献）。

+   x1, …, xn 是特征的值。

`nn.Parameter` 的理念是初始化 *b0*（在 `self.bias` 中初始化的偏差）和 *b1, … , bn*（在 `self.weights` 中初始化的权重）。我们正在初始化 13 个权重，因为我们的训练数据集中有 13 个特征。

由于我们处理的是线性回归，因此只有一个偏差值，所以我们只需初始化一个随机标量（[如果这个名字对你来说很陌生，可以查看我的第一篇文章！](https://medium.com/towards-data-science/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b?sk=2cf4d44549664fc647baa3455e9d78e8)）。此外，请注意，我们正在使用 `torch.randn` 随机初始化这些参数。

现在，我们的目标是通过反向传播优化这些权重——为此，我们需要设置我们的线性层，包括回归公式：

```py
def forward(self, x: torch.Tensor) -> torch.Tensor:
        return (self.weights * x + self.bias).sum(axis=1)
```

`trainModel` 方法将帮助我们执行反向传播和权重调整：

```py
 def trainModel(
            self,
            epochs: int,
            X_train: torch.Tensor,
            X_test: torch.Tensor,
            y_train: torch.Tensor,
            y_test: torch.Tensor,
            lr: float
            ):
        '''
        Trains linear model using pytorch.
        Evaluates the model against test set for every epoch.
        '''
        torch.manual_seed(42)
        # Create empty loss lists to track values
        self.train_loss_values = []
        self.test_loss_values = []

        loss_fn = nn.L1Loss()

        if self.optimizer == 'SGD':
            optimizer = torch.optim.SGD(
                params=self.parameters(),
                lr=lr
                )
        elif self.optimizer == 'Adam':
            optimizer = torch.optim.Adam(
                params=self.parameters(),
                lr=lr
                )

        for epoch in range(epochs):
            self.train()
            y_pred = self(X_train)
            loss = loss_fn(y_pred, y_train)
            optimizer.zero_grad()
            loss.backward()
            optimizer.step()

            # Set the model in evaluation mode
            self.eval()
            with torch.inference_mode():
                self.evaluate(X_test, y_test, epoch, loss_fn, loss)
```

在这个方法中，我们可以选择使用随机梯度下降（SGD）或自适应矩估计（*Adam*）优化器。更重要的是，让我们深入了解每个训练周期（对整个数据集的一次遍历）之间发生了什么：

```py
self.train()
y_pred = self(X_train)
loss = loss_fn(y_pred, y_train)
optimizer.zero_grad()
loss.backward()
optimizer.step()
```

这段代码在神经网络的背景下极为重要。它包括了典型的`torch`模型的训练过程：

+   我们使用`self.train()`将模型设置为训练模式

+   接下来，我们使用`self(X_train)`将数据传递给模型 — 这将把数据传递通过前向层。

+   `loss_fn`计算训练数据上的损失。我们的损失函数是`torch.L1Loss`，由平均绝对误差组成。

+   `optimizer.zero_grad()`将梯度设置为零（它们会在每个周期累积，因此我们希望在每次遍历时从零开始）。

+   `loss.backward()`计算每个权重相对于损失函数的梯度。这是优化权重的步骤。

+   最后，我们使用`optimizer.step()`更新模型的参数

最后的步骤是揭示如何使用`evaluate`方法评估我们的模型：

```py
def evaluate(self, X_test, y_test, epoch_nb, loss_fn, train_loss):
        '''
        Evaluates current epoch performance on the test set.
        '''
        test_pred = self(X_test)
        test_loss = loss_fn(test_pred, y_test.type(torch.float))
        if epoch_nb % 10 == 0:
            self.train_loss_values.append(train_loss.detach().numpy())
            self.test_loss_values.append(test_loss.detach().numpy())
            print(f"Epoch: {epoch_nb} - MAE Train Loss: {train_loss} - MAE Test Loss: {test_loss} ")
```

这段代码计算测试集中的损失。此外，我们将使用这种方法在每 10 个周期中打印训练和测试集的损失。

模型准备好后，让我们在数据上训练它，并可视化训练和测试的学习曲线！

# 拟合模型

让我们使用构建的代码来训练模型并观察训练过程 — 首先，我们将使用`Adam`优化器和`0.001`学习率训练模型 500 个周期：

```py
adam_model = LinearRegressionModel('Adam')

adam_model.trainModel(200, X_train, X_test, y_train, y_test, 0.001)
```

这里我使用`adam`优化器训练模型 200 个周期。训练和测试损失的概述如下：

![](img/170b4bd07e394c19b1882ff88d07f4eb.png)

前 150 个周期的训练和测试演变 — 作者提供的图片

我们还可以绘制整个周期的训练和测试损失：

![](img/02396ed220643923d7f33fb675b4ad9f.png)

训练和测试损失 — 作者提供的图片

我们的损失仍然有点高（在最后一个周期，MAE 约为 21），因为线性回归可能无法解决这个问题。

最后，让我们只用`SGD`拟合模型：

```py
sgd_model = LinearRegressionModel(‘SGD’)
sgd_model.trainModel(500, X_train, X_test, y_train, y_test, 0.001)
```

![](img/e636b24a94e0bdea6de6243f45ff609c.png)

SGD 模型的训练和测试损失 — 作者提供的图片

有趣的是 — 训练和测试损失没有改善！这发生是因为 SGD 对特征缩放非常敏感，可能在处理不同尺度的特征时难以计算梯度。作为挑战，尝试对特征进行缩放，并用`SGD`检查结果。缩放后，你还会注意到`Adam`优化器模型的行为更稳定！

# 结论

感谢您抽出时间阅读这篇文章！在这篇博客文章中，我们检查了如何使用`torch`训练一个简单的线性回归模型。虽然 PyTorch 因其深度学习（更多层和复杂功能）而闻名，但学习简单模型是玩转这个框架的好方法。此外，这也是熟悉“损失”函数和梯度概念的绝佳用例。

我们还看到了 `SGD` 和 `Adam` 优化器的工作原理，特别是它们对未缩放特征的敏感程度。

最后，我希望你保留这个过程，它可以扩展到其他类型的模型、函数和过程：

+   `train()` 将模型设置为训练模式。

+   使用 `torch.model` 将数据传递给模型。

+   使用 `nn.L1Loss()` 进行回归问题。其他损失函数可以在[这里](https://pytorch.org/docs/stable/nn.html#loss-functions)找到。

+   `optimizer.zero_grad()` 将梯度设置为零。

+   `loss.backward()` 计算每个权重相对于损失函数的梯度。

+   使用 `optimizer.step()` 更新模型的权重。

下次 PyTorch 的帖子见！我还推荐你访问 [PyTorch 从零到精通课程](https://www.learnpytorch.io/01_pytorch_workflow/)，这是一个精彩的免费资源，激发了这篇文章的方法论。

欢迎加入我新创建的 YouTube 频道——[数据之旅](https://www.youtube.com/@TheDataJourney42)。

*本博客文章中使用的数据集可以在 Kaggle 平台上获得，并通过 Spotify 官方 APP 提取（*[`www.kaggle.com/datasets/yasserh/song-popularity-dataset/data`](https://www.kaggle.com/datasets/yasserh/song-popularity-dataset/data)*）。* 数据集的许可证为 CC0：公有领域*
