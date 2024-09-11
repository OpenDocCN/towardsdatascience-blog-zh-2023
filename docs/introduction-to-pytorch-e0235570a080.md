# PyTorch 介绍

> 原文：[https://towardsdatascience.com/introduction-to-pytorch-e0235570a080?source=collection_archive---------11-----------------------#2023-03-01](https://towardsdatascience.com/introduction-to-pytorch-e0235570a080?source=collection_archive---------11-----------------------#2023-03-01)

## 了解 PyTorch 项目的工作流程

[](https://medium.com/@pumaline?source=post_page-----e0235570a080--------------------------------)[![Datamapu](../Images/63b0c7f9a3d160c5bb039bbebd791f7e.png)](https://medium.com/@pumaline?source=post_page-----e0235570a080--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e0235570a080--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----e0235570a080--------------------------------) [Datamapu](https://medium.com/@pumaline?source=post_page-----e0235570a080--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffcd72d75ae6e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pytorch-e0235570a080&user=Datamapu&userId=fcd72d75ae6e&source=post_page-fcd72d75ae6e----e0235570a080---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e0235570a080--------------------------------) · 13 分钟阅读 · 2023年3月1日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe0235570a080&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pytorch-e0235570a080&user=Datamapu&userId=fcd72d75ae6e&source=-----e0235570a080---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe0235570a080&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-pytorch-e0235570a080&source=-----e0235570a080---------------------bookmark_footer-----------)![](../Images/1e2d7992b69d15a3657c1c6969a38233.png)

图片来源：Igor Lepilin 在 Unsplash

在本文中，我们将通过使用PyTorch的深度学习项目生命周期。我们假设你已经对神经网络有一定了解，因此不会详细解释它们，只会关注PyTorch特有的方面。我们将主要遵循[官方文档](https://pytorch.org/tutorials/beginner/basics/intro.html)中展示的步骤，但考虑一个不同的例子。在文档中展示的是图像分类的例子，而这里我们将考虑存储在.csv文件中的表格数据。这意味着一些更改是必要的，特别是在准备数据集时。拥有两个不同的例子应该有助于更好地理解PyTorch项目的一般工作流程。除了这篇文章，你还可以查看包含完整代码和工作流程结构的[colab笔记本](https://drive.google.com/file/d/1p31TH09BExMYyo-cm2DfcacoTdxONgwe/view?usp=sharing)，或者在[GitHub](https://github.com/froukje/articles/blob/main/06_pytorch_introduction.ipynb)上找到该笔记本。

# 数据

使用的数据从[kaggle](https://www.kaggle.com/datasets/adityakadiwal/water-potability) [1]下载，且免费提供。这些数据描述了在城市环境中确定水质所需的不同特征。目标是预测水质是好还是坏。也就是说，我们正在考虑一个二分类问题。总共有9个特征（均为数值型）和标签。

![](../Images/0657b63353cb716606c32939eca8dcdb.png)

给定数据集的特征和标签。

# 预处理

与每个数据科学项目一样，首先需要进行一些预处理。这与我们使用的深度学习框架无关。因此，我们在这里不详细讨论。有关更多信息，请参见[colab笔记本](https://colab.research.google.com/drive/1p31TH09BExMYyo-cm2DfcacoTdxONgwe)或[GitHub](https://github.com/froukje/articles/blob/main/06_pytorch_introduction.ipynb)。

我们很幸运，数据不需要太多准备。所有特征都是数值型和浮点型。然而，有一些缺失值，我们用相应特征的均值进行了填补。还需考虑的是，目标变量并不均匀分布，0（差的水质）的数量远多于1（良好的水质）。为了简化，我们对数据进行了上采样，使得0的数量与1的数量相等，通过从1的子集随机抽样直到达到相同数量的样本。最后，我们将数据集划分为训练集（80%）、验证集（10%）和测试集（10%），并对数据进行了缩放。

# 创建一个PyTorch数据集

为了在 PyTorch 模型中使用我们的数据，我们需要将其转化为特定的形式：*PyTorch 数据集*。该数据集的构建与模型解耦。数据集对象存储样本及其对应的标签。此时，这个例子略微偏离了 PyTorch 文档页面。文档中使用的示例数据是 [FashionMNIST](https://www.kaggle.com/datasets/zalando-research/fashionmnist)。对于这个（和其他几个）数据集，PyTorch 提供了预加载的数据集。要了解如何加载这些数据集，可以查看他们的 [PyTorch 教程](https://pytorch.org/tutorials/beginner/basics/data_tutorial.html)。然而，如果你想用 PyTorch 处理自己的数据，你很可能需要编写自己的自定义数据集类。

要创建自定义的数据集类，我们可以从 PyTorch 提供的 Dataset 类继承。我们需要调整以下三个主要方法：

**__init__** 方法在实例化数据集对象时运行一次。在这个简单的例子中，仅将输入和标签作为张量存储。

**__len__** 方法返回数据集中样本的数量。

**__getitem__** 方法加载并返回数据集中给定索引处的样本。

数据集也是进行变换的地方，例如处理图像数据时。在我们的表格数据中，这一点不相关，因此在这里不涉及。对于这里考虑的水质问题，自定义数据集类如下所示：

```py
class WaterDataset(Dataset):
    def __init__(self, X, y):

        # The __init__ method is run once when instantiating the Dataset object
        self.X = torch.tensor(X)
        self.y = torch.tensor(np.array(y).astype(float))

    def __len__(self):

        # The __len__ method returns the number of samples in our dataset.
        return len(self.y)

    def __getitem__(self, idx):

        # The __getitem__ method loads and returns a sample from the dataset
        # at the given index idx.
        return self.X[idx], self.y[idx]
```

使用这个类，我们定义训练、验证和测试数据集。

```py
train_dataset = WaterDataset(X_train, y_train)
val_dataset = WaterDataset(X_val, y_val)
test_dataset = WaterDataset(X_test, y_test)
```

# 定义 DataLoader

创建数据集后，我们使用 PyTorch 的 DataLoader 将可迭代对象包装起来，以便在训练和验证期间轻松访问数据。数据集一次获取一个样本的特征和标签。在训练模型时，我们通常希望以批次的形式传递样本，并在每个 epoch 重新洗牌数据。在这个例子中，迭代 DataLoader 时，每次迭代返回一个包含 32 个样本的迷你批次。可以进一步配置 DataLoader。有关所有可能的配置，请参阅 [文档](https://pytorch.org/docs/stable/data.html)。

```py
train_dataloader = DataLoader(train_dataset, batch_size=32, shuffle=True)
val_dataloader = DataLoader(val_dataset, batch_size=32)
test_dataloader = DataLoader(test_dataset, batch_size=32)
```

# 定义一个模型

现在，数据已经准备好，我们可以定义模型了。我们假设你对神经网络的基本结构有所了解。在 PyTorch 中，**torch.nn** 命名空间提供了创建神经网络的所有构建模块。我们在这个例子中使用的模型非常简单，仅包含 [线性层](https://pytorch.org/docs/stable/generated/torch.nn.Linear.html#torch.nn.Linear)、[ReLu 激活函数](https://pytorch.org/docs/stable/generated/torch.nn.ReLU.html#torch.nn.ReLU) 和 [Dropout 层](https://pytorch.org/docs/stable/generated/torch.nn.Dropout.html)。有关 PyTorch 中所有预定义层的概述，请参阅 [文档](https://pytorch.org/docs/stable/nn.html)。

我们可以通过继承**nn.Module**来构建自己的模型。一个 PyTorch 模型至少包含两个方法。**__init__** 方法，其中实例化了所有需要的层，和**forward** 方法，其中定义了最终模型。以下是一个示例模型，能够为我们的示例数据提供足够好的结果。

```py
class WaterNet(nn.Module):
    def __init__(self):
        super().__init__()

        # define 4 linear layers
        # the first input dimension is the number of features
        # the output layer is 1 
        # the other in and output parameters are hyperparamters and can be changed 
        self.fc1 = nn.Linear(in_features=9, out_features=64)
        self.fc2 = nn.Linear(in_features=64, out_features=32)
        self.fc3 = nn.Linear(in_features=32, out_features=16)
        self.fc4 = nn.Linear(in_features=16, out_features=1)
        self.relu = nn.ReLU()
        self.dropout = nn.Dropout()

    def forward(self, x):

        # apply the linear layers with a relu activation
        x = self.fc1(x)
        x = self.relu(x)
        x = self.dropout(x)
        x = self.fc2(x)
        x = self.relu(x)
        x = self.fc3(x)
        x = self.relu(x)
        x = self.fc4(x)

        return x.squeeze()
```

该模型由四个线性层组成。输入和输出特征的数量已定义，这决定了输入和输出样本的大小。我们的数据包含 9 个特征，因此第一层的输入特征数量为 9。输出特征的大小可以更改，但必须与下一个输入特征的大小匹配。由于我们最终需要一个 1 维的输出（0 或 1），因此最后的输出特征大小为 1。注意，这里没有应用最终的 sigmoid 层。我们将在下一节中解释这一点。

# 训练模型

接下来，我们需要训练模型。在 PyTorch 中训练模型包括四个主要步骤：

1.  应用模型

1.  计算损失

1.  反向传播

1.  更新权重

要训练一个轮次，这些步骤需要在所有批次的*train_dataloader*上完成。然后需要另一个循环遍历所需的轮次数。伪代码中一个轮次的训练如下：

```py
for batch in train_dataloader:
  # apply model
  y_hat = model(x)
  # calculate loss
  loss = loss_function(y_hat, y)
  # backpropagation
  loss.backward
  # update weights
  optimizer.step()
```

优化器和损失函数仍需定义。我们将在下一节中完成这部分。以下是包含这个训练循环的函数。此外，还计算了一些指标（准确率、召回率和精度）。注意，我们将模型设置为训练模式（`model.train()`），而不是评估模式（`model.eval()`）。这会影响 dropout 或批归一化层，这些层在训练和验证时的处理方式不同。该函数的输入是

1\. **模型**。这是上面定义的模型。

2\. **设备**。这可以是 GPU 或 CPU，将在下一节中设置。

3\. **train_dataloader**。上述定义的用于训练数据的数据加载器。

4\. **优化器**。优化器用于最小化误差。我们将在下一节中具体说明。

5\. **损失函数（标准）**。我们将在下一节中具体说明。

6\. **训练轮次**。当前的训练轮次。

```py
def train(model, device, train_dataloader, optimizer, criterion, epoch, print_every):
    '''
    parameters:
    model - the model used for training
    device - the device we work on (cpu or gpu)
    train_dataloader - the training data wrapped in a dataloader
    optimizer - the optimizer used for optimizing the parameters
    criterion - loss function
    epoch - current epoch
    print_every - integer after how many batches results should be printed
    '''

    # create empty list to store the train losses
    train_loss = []
    # variable to count correct classified samples
    correct = 0
    # variable to count true positive, false positive and false negative samples
    TP = 0
    FP = 0
    FN = 0
    # create an empty list to store the predictions
    predictions = []

    # set model to training mode, i.e. the model is used for training
    # this effects layers like BatchNorm() and Dropout
    # in our simple example we don't use these layers
    # for the sake of completeness 'model.train()' is included 
    model.train()

    # loop over batches
    for batch_idx, (x, y) in enumerate(train_dataloader):

        # set data to device
        x, y = x.to(device), y.to(device)

        # set optimizer to zero
        optimizer.zero_grad()

        # apply model 
        y_hat = model(x.float())

        # calculate loss
        loss = criterion(y_hat, y.float())
        train_loss.append(loss.item())

        # backpropagation
        loss.backward()

        # update the weights
        optimizer.step()

        # print the loss every x batches
        if batch_idx % print_every == 0:
            percent = 100\. * batch_idx / len(train_dataloader)
            print(f'Train Epoch {epoch} \
            [{batch_idx * len(train_dataloader)}/{len(train_dataloader.dataset)} \
            ({percent:.0f}%)] \tLoss: {loss.item():.6f}')

        # calculate some metrics

        # to get the predictions, we need to apply the sigmoid layer
        # this layer maps the data to the range [0,1]
        # we set all predictions > 0.5 to 1 and the rest to 0
        y_pred = torch.sigmoid(y_hat) > 0.5
        predictions.append(y_pred)
        correct += (y_pred == y).sum().item()
        TP += torch.logical_and(y_pred == 1, y == 1).sum()
        FP += torch.logical_and(y_pred == 1, y == 0).sum()
        FN += torch.logical_and(y_pred == 0, y == 1).sum()

    # total training loss over all batches
    train_loss = torch.mean(torch.tensor(train_loss))
    epoch_accuracy = correct/len(train_dataloader.dataset)
    # recall = TP/(TP+FN)
    epoch_recall = TP/(TP+FN)
    # precision = TP/(TP+FP)
    epoch_precision = TP/(TP+FP)

    return epoch_accuracy, train_loss, epoch_recall, epoch_precision
```

模型的验证类似，但没有反向传播，也没有更新权重。伪代码中一个轮次的验证如下。

```py
for batch in val_dataloader:
  # apply model
  y_hat = model(x)
  # calculate loss
  loss = loss_function(y_hat, y)
```

以下是一个用于验证一个轮次的函数。它与前面用于训练的函数非常相似。注意，模型设置为评估模式（`model.eval()`），并且由于在验证过程中不需要计算梯度，我们设置了 `with torch.no_grad()`。这将减少计算时的内存消耗。

```py
def valid(model, device, val_dataloader, criterion):
    '''
    parameters:
    model - the model used for training
    device - the device we work on (cpu or gpu)
    val_dataloader - the validation data wrapped in a dataloader
    criterion - loss function
    '''

    # create an empty list to store the loss
    val_loss = []
    # variable to count correct classified samples
    correct = 0
    # variable to count true positive, false positive and false negative samples
    TP = 0
    FP = 0
    FN = 0
    # create an empty list to store the predictions
    predictions = []

    # set model to evaluation mode, i.e. 
    # the model is only used for inference, this has effects on
    # dropout-layers, which are ignored in this mode and batchnorm-layers, which use running statistics
    model.eval()

    # disable gradient calculation 
    # this is useful for inference, when we are sure that we will not call Tensor.backward(). 
    # It will reduce memory consumption for computations that would otherwise have requires_grad=True.
    with torch.no_grad():
        # loop over batches
        for x, y in val_dataloader:

            # set data to device
            x, y = x.to(device), y.to(device)

            # apply model
            y_hat = model(x.float())

            # append current loss
            loss = criterion(y_hat, y.float())
            val_loss.append(loss.item())

            # calculate some metrics

            # to get the predictions, we need to apply the sigmoid layer
            # this layer maps the data to the range [0,1]
            # we set all predictions > 0.5 to 1 and the rest to 0
            y_pred = torch.sigmoid(y_hat) > 0.5 
            predictions.append(y_pred)
            correct += (y_pred == y).sum().item()#y_pred.eq(y.view_as(y_pred)).sum().item()
            TP += torch.logical_and(y_pred == 1, y == 1).sum()
            FP += torch.logical_and(y_pred == 1, y == 0).sum()
            FN += torch.logical_and(y_pred == 0, y == 1).sum()

        # total validation loss over all batches
        val_loss = torch.mean(torch.tensor(val_loss))
        epoch_accuracy = correct/len(val_dataloader.dataset)
        # recall = TP/(TP+FN)
        epoch_recall = TP/(TP+FN)
        # precision = TP/(TP+FP)
        epoch_precision = TP/(TP+FP)

        print(f'Validation: Average loss: {val_loss.item():.4f}, \
                Accuracy: {epoch_accuracy:.4f} \
               ({100\. * correct/len(val_dataloader.dataset):.0f}%)')

    return predictions, epoch_accuracy, val_loss, epoch_recall, epoch_precision
```

# 综合起来

要使用 PyTorch 训练神经网络，我们需要做以下几步：

1.  从数据中**生成数据集**并将其包装成**数据加载器**

+   我们在前面的部分已经做过这些，然而为了完整性，我们将再次生成它们。

2\. **定义模型**

我们使用上述定义的模型`WaterNet`。

3. **定义优化器**

+   在PyTorch中有不同的[优化器](https://pytorch.org/docs/stable/optim.html)。我们使用Adam优化器，这是一种非常常见的优化器。不过你也可以尝试不同的优化器。Adam优化器是随机梯度下降的扩展。简单来说，区别在于随机梯度下降在训练过程中保持学习率不变，而Adam则对其进行调整。关于Adam优化器的介绍可以在[这里](https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/)找到。

4. **定义损失函数**

+   我们正在考虑一个1维输出的二分类问题，这种问题的默认选择是[二元交叉熵](/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a)，我们将使用它。

+   请注意，我们没有在模型中应用最终的[sigmoid层](https://pytorch.org/docs/stable/generated/torch.nn.Sigmoid.html)。这是故意的，因为PyTorch提供了`[nn.BCEWithLogitsLoss()](https://pytorch.org/docs/stable/generated/torch.nn.BCEWithLogitsLoss.html)`方法，它将最终的sigmoid层和二元交叉熵结合起来。我们也可以单独应用这两种方法，即将`[nn.Sigmoid](https://pytorch.org/docs/stable/generated/torch.nn.Sigmoid.html)`层作为模型的最后一步，然后使用`[nn.BCE()](https://pytorch.org/docs/stable/generated/torch.nn.BCELoss.html)`损失。不过，使用`nn.BCEWithLogitsLoss()`是处理二分类问题的推荐方式，因为它在数值上更稳定。

在我们开始训练模型之前，我们设置超参数。超参数是可调节的参数，允许你控制模型优化过程。不同的超参数值可以影响模型的训练和收敛速度。在我们的例子中，我们有三个超参数需要设置。请注意，模型层的输入和输出特征也是超参数。我们将它们设置为固定值，不过你可以尝试不同的值。

+   `batch_size`：训练和验证批次大小

+   `epochs`：训练的周期数

+   `learning_rate`：学习率

我们还设置了变量`print_every`。这不是一个超参数，而只是决定在训练和验证过程中多频繁打印损失。请注意，如果有GPU可用，我们还需要手动将设备设置为“cuda”。

```py
# hyperparamters 
batch_size = 32
epochs = 150
learning_rate = 1e-3
print_every = 200

# set device to GPU, if available
device = torch.device("cuda" if torch.cuda.is_available() else "cpu")

# create datasets
train_dataset = WaterDataset(X_train, y_train)
valid_dataset = WaterDataset(X_val, y_val)

# wrap into dataloader
train_dataloader = DataLoader(train_dataset, batch_size=batch_size, shuffle=True, drop_last=True)
val_dataloader = DataLoader(valid_dataset, batch_size=batch_size)

# define the model and move it to the available device
model = WaterNet().to(device)

# define the optimizer
optimizer = torch.optim.Adam(model.parameters(), lr=learning_rate)#, weight_decay=1e-4)

# define loss
criterion = nn.BCEWithLogitsLoss()
```

现在，我们准备开始训练。下面是最终的训练循环。此外，计算得到的指标会为每个周期保存。

```py
# create empty lists to store the accuracy and loss per validation epoch 
train_epoch_accuracy = []
train_epoch_loss = []
train_epoch_recall = []
train_epoch_precision = []
val_epoch_accuracy = []
val_epoch_loss = []
val_epoch_recall = []
val_epoch_precision = []

# loop over number of epochs
for epoch in range(epochs):

    # train model for each epoch
    train_accuracy, train_loss, train_recall, train_precision = \
        train(model, device, train_dataloader, optimizer, criterion, epoch, print_every)
    # save training loss and accuracy for each epoch
    train_epoch_accuracy.append(train_accuracy)
    train_epoch_loss.append(train_loss)
    train_epoch_recall.append(train_recall)
    train_epoch_precision.append(train_precision)

    # validate model for each epoch
    predictions, val_accuracy, val_loss, val_recall, val_precision = \
        valid(model, device, val_dataloader, criterion)
    # save validation loss and accuracy for each epoch
    val_epoch_accuracy.append(val_accuracy)
```

# 评估结果

为了评估结果，我们可以查看训练和评估过程中的损失和指标。

![](../Images/ed6db4f285385f8aa7d567e2e4d7bc7a.png)

训练和验证的损失为150个周期。

![](../Images/140d4c2d9d8d9d601fb2f02dde90be62.png)

训练和验证的回顾持续150个周期。

你可以在[笔记本](https://colab.research.google.com/drive/1p31TH09BExMYyo-cm2DfcacoTdxONgwe)中找到其他计算指标的图表。

# 保存模型

如果我们以后想使用我们的模型，我们需要保存它。我们可以通过保存它的`state_dict()`来做到这一点。

```py
torch.save(model.state_dict(), 'water_model_weights.pth')
```

我们可以用以下方式加载它

```py
model = WaterNet()
model.load_state_dict(torch.load('water_model_weights.pth'))
model.eval()
```

不要忘记使用`model.eval()`将模型设置为评估模式，以将丢弃和批量归一化层设置为评估模式。

# 将模型应用于测试集

现在我们将训练好的模型应用于测试数据。

```py
# create a dataset
test_dataset = WaterDataset(X_test, y_test)

# wrap into dataloader
test_dataloader = DataLoader(test_dataset, batch_size=batch_size)

predictions, test_accuracy, test_loss, test_recall, test_precision = \
        valid(model, device, val_dataloader, criterion)
```

# 结论

本文展示了如何使用PyTorch进行深度学习项目的详细示例。讨论并应用了深度学习工作流中的各个步骤到一个具体的数据集。在训练模型之前，一个重要的步骤是将数据转换为正确的形式，并为特定应用定义一个定制的数据集。在训练模型时，四个主要步骤是（1）应用模型，（2）计算损失，（3）进行反向传播，（4）更新权重。定义并应用了一个包含所有这些步骤的示例训练函数。最后，要使用模型，了解如何存储和重新加载模型是很重要的。

# 资源

[1] Aditya Kadiwal, 2021, 水质 — 水质分类数据集, [https://www.kaggle.com/datasets/mssmartypants/water-quality](https://www.kaggle.com/datasets/mssmartypants/water-quality), 下载于2023年1月, 许可证：CC0: 公共领域

[2] PyTorch, 2022, [https://pytorch.org/tutorials/beginner/basics/intro.html](https://pytorch.org/tutorials/beginner/basics/intro.html)

除非另有说明，所有图片均为作者提供。

在这里查看更多数据科学和机器学习的帖子：

[## 更多

### 数据科学和机器学习博客

[datamapu.com](https://datamapu.com/?source=post_page-----e0235570a080--------------------------------) [medium.com](https://medium.com/@pumaline/subscribe?source=post_page-----e0235570a080--------------------------------) [## 每当Pumaline发布时获得电子邮件。

### 每当Pumaline发布时获得电子邮件。通过注册，如果你还没有Medium账户，将会创建一个…

[medium.com](https://medium.com/@pumaline/subscribe?source=post_page-----e0235570a080--------------------------------) [www.buymeacoffee.com](https://www.buymeacoffee.com/pumaline?source=post_page-----e0235570a080--------------------------------) [## Pumaline

### 嗨，我喜欢学习和分享有关数据科学和机器学习的知识。

[www.buymeacoffee.com](https://www.buymeacoffee.com/pumaline?source=post_page-----e0235570a080--------------------------------)
