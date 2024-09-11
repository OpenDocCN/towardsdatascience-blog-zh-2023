# 当数据集较小时，特征是你的朋友。

> 原文：[https://towardsdatascience.com/when-the-dataset-is-small-features-are-your-friends-6e7f8dcc819e?source=collection_archive---------9-----------------------#2023-04-03](https://towardsdatascience.com/when-the-dataset-is-small-features-are-your-friends-6e7f8dcc819e?source=collection_archive---------9-----------------------#2023-04-03)

## 特征工程可以弥补数据的不足。

[](https://medium.com/@aimagefrombydgoszcz?source=post_page-----6e7f8dcc819e--------------------------------)[![Krzysztof Pałczyński](../Images/0cce629a7cefc6c33e2759bb8a68b123.png)](https://medium.com/@aimagefrombydgoszcz?source=post_page-----6e7f8dcc819e--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6e7f8dcc819e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6e7f8dcc819e--------------------------------) [Krzysztof Pałczyński](https://medium.com/@aimagefrombydgoszcz?source=post_page-----6e7f8dcc819e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F7c74555dd91c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-the-dataset-is-small-features-are-your-friends-6e7f8dcc819e&user=Krzysztof+Pa%C5%82czy%C5%84ski&userId=7c74555dd91c&source=post_page-7c74555dd91c----6e7f8dcc819e---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6e7f8dcc819e--------------------------------) ·7分钟阅读·2023年4月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6e7f8dcc819e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-the-dataset-is-small-features-are-your-friends-6e7f8dcc819e&user=Krzysztof+Pa%C5%82czy%C5%84ski&userId=7c74555dd91c&source=-----6e7f8dcc819e---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6e7f8dcc819e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fwhen-the-dataset-is-small-features-are-your-friends-6e7f8dcc819e&source=-----6e7f8dcc819e---------------------bookmark_footer-----------)![](../Images/d18793d4116fea77e67b9b3e75e2180f.png)

照片由[Thomas T](https://unsplash.com/@pyssling240?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)拍摄，发布在[Unsplash](https://unsplash.com/photos/OPpCbAAKWv8?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)上

在快速发展的人工智能（AI）领域，数据已经成为无数创新应用和解决方案的命脉。确实，大数据集通常被认为是强大且准确的AI模型的基础。然而，当手头的数据集相对较小时会发生什么？在本文中，我们探讨了特征工程在克服小数据集限制中的关键作用。

**玩具数据集**

我们的旅程从创建数据集开始。在这个例子中，我们将进行简单易行的信号分类。数据集有两个类别；频率为1的正弦波属于类别0，频率为2的正弦波属于类别1。信号生成的代码如下。该代码生成正弦波，应用加性高斯噪声，并随机化相位偏移。由于噪声和相位偏移的加入，我们得到多样的信号，分类问题变得不再简单（尽管通过正确的特征工程仍然容易）。

```py
def signal0(samples_per_signal, noise_amplitude):
    x = np.linspace(0, 4.0, samples_per_signal)
    y = np.sin(x * np.pi * 0.5)
    n = np.random.randn(samples_per_signal) * noise_amplitude

    s = y + n

    shift = np.random.randint(low=0, high=int(samples_per_signal / 2))
    s = np.concatenate([s[shift:], s[:shift]])

    return np.asarray(s, dtype=np.float32)

def signal1(samples_per_signal, noise_amplitude):
    x = np.linspace(0, 4.0, samples_per_signal)
    y = np.sin(x * np.pi)
    n = np.random.randn(samples_per_signal) * noise_amplitude

    s = y + n

    shift = np.random.randint(low=0, high=int(samples_per_signal / 2))
    s = np.concatenate([s[shift:], s[:shift]])

    return np.asarray(s, dtype=np.float32) 
```

![](../Images/a6fe3838540861160fcc95370a509e39.png)![](../Images/ec78692e74973b00b2092b8b00433287.png)

类别0中的信号可视化。

![](../Images/00e0f11732157e1356ae7b76c589e818.png)![](../Images/2631abaaeb2b6811dbb49d5f04821b2f.png)

类别1中的信号可视化

**深度学习性能**

最先进的信号处理模型是卷积神经网络（CNN）。所以，让我们创建一个。这一网络包含两个一维卷积层和两个全连接层。代码列在下面。

```py
class Network(nn.Module):

    def __init__(self, signal_size):

        c = int(signal_size / 10)
        if c < 3:
            c = 3

        super().__init__()
        self.cnn = nn.Sequential(
            nn.Conv1d(1, 8, c),
            nn.ReLU(),
            nn.AvgPool1d(2),
            nn.Conv1d(8, 16, c),
            nn.ReLU(),
            nn.AvgPool1d(2),
            nn.ReLU(),
            nn.Flatten()
        )

        l = 0
        with torch.no_grad():
            s = torch.randn((1,1,SAMPLES_PER_SIGNAL))
            o = self.cnn(s)
            l = o.shape[1]

        self.head = nn.Sequential(
            nn.Linear(l, 2 * l),
            nn.ReLU(),
            nn.Linear(2 * l, 2),
            nn.ReLU(),
            nn.Softmax(dim=1)
        )

    def forward(self, x):

        x = self.cnn(x)
        x = self.head(x)

        return x
```

CNNs是可以处理原始信号的模型。然而，由于其参数众多的架构，它们往往需要大量的数据。然而，最开始，让我们假设我们有足够的数据来训练神经网络。我使用信号生成创建了一个包含200个信号的数据集。每个实验重复了十次，以减少随机变量的干扰。代码如下：

```py
SAMPLES_PER_SIGNAL = 100
SIGNALS_IN_DATASET = 20
NOISE_AMPLITUDE = 0.1
REPEAT_EXPERIMENT = 10

X, Y = [], []

stop = int(SIGNALS_IN_DATASET / 2)
for i in range(SIGNALS_IN_DATASET):

    if i < stop:
        x = signal0(SAMPLES_PER_SIGNAL, NOISE_AMPLITUDE)
        y = 0
    else:
        x = signal1(SAMPLES_PER_SIGNAL, NOISE_AMPLITUDE)
        y = 1

    X.append(x.reshape(1,-1))
    Y.append(y)

X = np.concatenate(X)
Y = np.array(Y, dtype=np.int64)

train_x, test_x, train_y, test_y = train_test_split(X, Y, test_size=0.1)

accs = []
train_accs = []

for i in range(REPEAT_EXPERIMENT):

    net = NeuralNetClassifier(
        lambda: Network(SAMPLES_PER_SIGNAL),
        max_epochs=200,
        criterion=nn.CrossEntropyLoss(),
        lr=0.1,
        callbacks=[
            #('lr_scheduler', LRScheduler(policy=ReduceLROnPlateau, monitor="valid_acc", mode="min", verbose=True)),
            ('lr_scheduler', LRScheduler(policy=CyclicLR, base_lr=0.0001, max_lr=0.01, step_size_up=10)),
        ],
        verbose=False,
        batch_size=128
    )

    net = net.fit(train_x.reshape(train_x.shape[0], 1, SAMPLES_PER_SIGNAL), train_y)
    pred = net.predict(test_x.reshape(test_x.shape[0], 1, SAMPLES_PER_SIGNAL))
    acc = accuracy_score(test_y, pred)

    print(f"{i} - {acc}")

    accs.append(acc)

    pred_train = net.predict(train_x.reshape(train_x.shape[0], 1, SAMPLES_PER_SIGNAL))
    train_acc = accuracy_score(train_y, pred_train)
    train_accs.append(train_acc)

    print(f"Train Acc: {train_acc}, Test Acc: {acc}")

accs = np.array(accs)
train_accs = np.array(train_accs)

print(f"Average acc: {accs.mean()}")
print(f"Average train acc: {train_accs.mean()}")
print(f"Average acc where training was successful: {accs[train_accs > 0.6].mean()}")
print(f"Training success rate: {(train_accs > 0.6).mean()}")
```

CNNs的测试准确率达到了99.2%，这是对最先进模型的预期。然而，这一指标是针对这些实验运行得到的，其中训练是成功的。所谓“成功”，是指训练数据集上的准确率超过了60%。在这个例子中，CNNs权重初始化对训练至关重要，并且有时会发生问题，因为CNNs是复杂的模型，容易遇到由于随机权重初始化不幸带来的问题。训练的成功率是70%。

现在，让我们看看当数据集较小时会发生什么。我将数据集中的信号数量减少到20个。因此，CNNs的测试准确率达到了71.4%，准确率下降了27.8个百分点。这是不可接受的。不过，接下来该怎么做呢？数据集需要更长，以便使用最先进的模型。在工业应用中，获取更多数据要么不可行，要么至少非常昂贵。我们是否应该放弃这个项目，另谋他路？

不。当数据集很小的时候，特征是你的朋友。

**特征工程**

这个特定的例子涉及基于频率的信号分类。因此，我们可以应用经典的傅里叶变换。傅里叶变换将信号分解为由频率和幅度参数化的一系列正弦波。结果，我们可以使用傅里叶变换来检查每个频率在形成信号中的重要性。这种数据表示应该简化任务，使得小数据集就足够了。此外，傅里叶变换将数据结构化，以便我们可以使用更简单的模型，如随机森林分类器。

![](../Images/c989e6107d8239d3eeece4087683c300.png)![](../Images/586366bcd10b608ebfbcccea868904e8.png)

信号可视化转换为频谱。在左侧是类别0的信号频谱，右侧是类别1的信号频谱。这些图有对数刻度以便更好地显示。此示例中使用的模型在线性刻度上解释信号。

转换信号和训练随机森林分类器的代码如下：

```py
X, Y = [], []

stop = int(SIGNALS_IN_DATASET / 2)
for i in range(SIGNALS_IN_DATASET):

    if i < stop:
        x = signal0(SAMPLES_PER_SIGNAL, NOISE_AMPLITUDE)
        y = 0
    else:
        x = signal1(SAMPLES_PER_SIGNAL, NOISE_AMPLITUDE)
        y = 1

    # Transforming signal into spectrum
    x = np.abs(fft(x[:int(SAMPLES_PER_SIGNAL /2 )]))    

    X.append(x.reshape(1,-1))
    Y.append(y)

X = np.concatenate(X)
Y = np.array(Y, dtype=np.int64)

train_x, test_x, train_y, test_y = train_test_split(X, Y, test_size=0.1)

accs = []
train_accs = []

for i in range(REPEAT_EXPERIMENT):
    model = RandomForestClassifier()
    model.fit(train_x, train_y)

    pred = model.predict(test_x)
    acc = accuracy_score(test_y, pred)

    print(f"{i} - {acc}")

    accs.append(acc)

    pred_train = model.predict(train_x)
    train_acc = accuracy_score(train_y, pred_train)
    train_accs.append(train_acc)

    print(f"Train Acc: {train_acc}, Test Acc: {acc}")

accs = np.array(accs)
train_accs = np.array(train_accs)

print(f"Average acc: {accs.mean()}")
print(f"Average train acc: {train_accs.mean()}")
print(f"Average acc where training was successful: {accs[train_accs > 0.6].mean()}")
print(f"Training success rate: {(train_accs > 0.6).mean()}")
```

随机森林分类器在20个和200个信号长度的数据集上达到了100%的测试准确率，且每个数据集的训练成功率也为100%。因此，我们在所需数据量较少的情况下获得了比CNN更好的结果——这都归功于特征工程。

**过拟合风险**

尽管特征工程是一个强大的工具，但也必须记得从输入数据中减少不必要的特征。输入向量中的特征越多，过拟合的风险越高——尤其是在小数据集中。每一个不必要的特征都可能引入随机波动，机器学习模型可能将其视为重要模式。数据集中的数据越少，随机波动的风险越高，从而产生在现实世界中不存在的相关性。

可能有助于修剪过大的特征集合的机制之一是搜索启发式方法，如遗传算法。特征修剪可以被表述为一个任务，即找到最小数量的特征，以便成功训练机器学习模型。它可以通过创建一个长度等于特征数据大小的二进制向量来编码。 “0”表示特征不在数据集中，而“1”表示特征在数据集中。然后，这样向量的适应度函数是模型在修剪数据集上达到的准确度与向量中零的计数的总和，经过适当的权重缩放。

这只是移除不必要特征的众多解决方案之一。然而，它非常强大。

**结论**

尽管呈现的示例相对简单，但它展示了在工业中应用人工智能系统时的典型问题。目前，深度神经网络几乎可以实现我们所期望的所有功能，只要提供足够的数据。然而，数据通常稀缺且昂贵。因此，人工智能的工业应用通常涉及进行广泛的特征工程，以简化问题，从而减少训练模型所需的数据量。

感谢阅读。本示例生成的代码可以通过以下链接访问：[https://github.com/aimagefrombydgoszcz/Notebooks/blob/main/when_dataset_is_small_features_are_your_friend.ipynb](https://github.com/aimagefrombydgoszcz/Notebooks/blob/main/when_dataset_is_small_features_are_your_friend.ipynb)

除非另有说明，所有图片均由作者提供。
