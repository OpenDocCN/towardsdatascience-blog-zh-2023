# 与**Dropout正则化**对抗过拟合

> 原文：[https://towardsdatascience.com/combating-overfitting-with-dropout-regularization-f721e8712fbe?source=collection_archive---------2-----------------------#2023-03-03](https://towardsdatascience.com/combating-overfitting-with-dropout-regularization-f721e8712fbe?source=collection_archive---------2-----------------------#2023-03-03)

## 探索在自己的机器学习模型中实现Dropout的过程

[](https://medium.com/@rohankvij?source=post_page-----f721e8712fbe--------------------------------)[![罗汉·维吉](../Images/6ef53fffb4749e1665360555bf18275f.png)](https://medium.com/@rohankvij?source=post_page-----f721e8712fbe--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f721e8712fbe--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f721e8712fbe--------------------------------) [罗汉·维吉](https://medium.com/@rohankvij?source=post_page-----f721e8712fbe--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe44b36765084&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombating-overfitting-with-dropout-regularization-f721e8712fbe&user=Rohan+Vij&userId=e44b36765084&source=post_page-e44b36765084----f721e8712fbe---------------------post_header-----------) 发表在[数据科学前沿](https://towardsdatascience.com/?source=post_page-----f721e8712fbe--------------------------------) · 7分钟阅读 · 2023年3月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff721e8712fbe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombating-overfitting-with-dropout-regularization-f721e8712fbe&user=Rohan+Vij&userId=e44b36765084&source=-----f721e8712fbe---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff721e8712fbe&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcombating-overfitting-with-dropout-regularization-f721e8712fbe&source=-----f721e8712fbe---------------------bookmark_footer-----------)![](../Images/2cb3dea34c130db7298a48db8ba2f7d2.png)

照片由[皮埃尔·巴敏](https://unsplash.com/@bamin?utm_source=medium&utm_medium=referral)拍摄，发布于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

过拟合是大多数人在训练和使用机器学习模型时都会遇到的常见挑战。自机器学习诞生以来，研究人员一直在尝试解决过拟合问题。其中一种方法是 dropout 正则化，即随机删除模型中的神经元。在本文中，我们将探讨 dropout 正则化的工作原理，如何在自己的模型中实现它，以及与其他方法相比的优缺点。

# I. 介绍

## 什么是过拟合？

过拟合是指模型在训练数据上过度训练，导致其在新数据上的表现不佳。实质上，模型在追求尽可能准确的过程中，过分关注训练数据中的细节和噪声。这些特征在现实世界的数据中往往不存在，因此模型的表现往往不佳。当模型的参数数量相对于数据量过多时，就容易发生过拟合。这会导致模型过度关注与模型需要开发的一般模式无关的小细节。例如，假设一个复杂的模型（参数较多）被训练来识别图片中是否存在马，那么它可能会开始关注天空或环境的细节，而不是马本身。这种情况可能发生在：

1.  模型过于复杂（参数过多），对自己不利。

1.  模型训练时间过长。

1.  模型训练所用的数据集过小。

1.  模型在相同的数据上进行训练和测试。

1.  模型训练的数据集具有重复的特征，容易导致过拟合。

## 为什么过拟合很重要？

过拟合不仅仅是一个简单的烦恼——它可能摧毁整个模型。它给人一种模型表现良好的假象，尽管它可能没有对提供的数据做出正确的概括。

过拟合可能会带来极其严重的后果，尤其是在医疗保健等领域，人工智能越来越普及。由于过拟合而未能经过适当训练或测试的人工智能可能会导致错误的诊断。

# II. 什么是 Dropout？

## Dropout 作为一种正则化技术

理想情况下，对抗过拟合的最佳方法是训练多种不同架构的模型，并对它们的输出进行平均。然而，这种方法的问题在于，它极其耗费资源和时间。虽然相对较小的模型可能还算负担得起，但训练大型模型可能需要大量时间，这很容易超出任何人的资源承受能力。

Dropout 的工作原理是通过“丢弃”输入层或隐藏层中的神经元来实现的。网络中会移除多个神经元，这意味着它们实际上不存在——它们的输入和输出连接也会被破坏。这会人为地创建出多个较小的、更简单的网络。这迫使模型不依赖于一个单独的神经元，也就是说，它必须多样化其方法，并开发出多种方法来实现相同的结果。例如，回到马的例子，如果一个神经元主要负责马的树部分，那么它被丢弃会迫使模型更多地关注图像的其他特征。Dropout 也可以直接应用于输入神经元，这意味着整个特征会从模型中消失。

## 将 Dropout 应用于神经网络

Dropout 是通过在每一层（包括输入层）随机丢弃神经元来应用于神经网络的。预定义的 dropout 率决定了每个神经元被丢弃的概率。例如，dropout 率为 0.25 意味着神经元被丢弃的概率是 25%。Dropout 在模型训练的每个 epoch 中应用。

请记住，没有理想的 dropout 值——这在很大程度上依赖于模型的超参数和最终目标。

## Dropout 和有性繁殖

回想一下你的大一生物课——你可能学过减数分裂或有性繁殖。在减数分裂的过程中，基因会发生随机突变。这意味着结果后代可能具有父母双方基因中都没有的特征。这种随机性随着时间的推移，使生物群体更适应其环境。这个过程被称为进化，没有它，我们今天也不会存在。

Dropout 和有性繁殖都旨在增加多样性，防止系统依赖于一组参数，而没有改进的空间。

# III. 将 Dropout 应用于您的模型

## 数据集

让我们从一个可能容易过拟合的数据集开始：

```py
# Columns: has tail, has face, has green grass, tree in background, has blue sky, 3 columns of noise | is a horse image (1) or not (0)
survey = np.array([
 [1, 1, 1, 1, 1, 1], # tail, face, green grass, tree, blue sky | is a horse image
 [1, 1, 1, 1, 1, 1], # tail, face, green grass, tree blue sky | is a horse image
 [0, 0, 0, 0, 0, 0], # no tail, no face, no green grass, no tree, no blue sky | is not a horse image
 [0, 0, 0, 0, 0, 0], # no tail, no face, no green grass, no tree, no blue sky | is not a horse image
])
```

这些数据回到了我们关于马和它的环境的例子。我们已将图像的特性抽象成一个易于解释的简单格式。可以清楚地看到，数据并不理想，因为包含马的图像也可能包含树、绿色草地或蓝色天空——它们可能在同一张图片中，但一个不会影响另一个。

## MLP 模型

让我们用 Keras 快速创建一个简单的 MLP：

```py
# Imports
from keras.models import Sequential
from keras.layers import Dense, Dropout
import numpy as np

# Columns: has tail, has face, has green grass, tree in background, has blue sky, 3 columns of noise | is a horse image (1) or not (0)
survey = np.array([
 [1, 1, 1, 1, 1, 1], # tail, face, green grass, tree, blue sky | is a horse image
 [1, 1, 1, 1, 1, 1], # tail, face, green grass, tree blue sky | is a horse image
 [0, 0, 0, 0, 0, 0], # no tail, no face, no green grass, no tree, no blue sky | is not a horse image
 [0, 0, 0, 0, 0, 0], # no tail, no face, no green grass, no tree, no blue sky | is not a horse image
])

# Define the model
model = Sequential([
    Dense(16, input_dim=5, activation='relu'),
    Dense(8, activation='relu'),
    Dense(1, activation='sigmoid')
])

# Compile the model
model.compile(loss='binary_crossentropy', optimizer='adam', metrics=['accuracy'])

# Train the model
X = survey[:, :-1]
y = survey[:, -1]
model.fit(X, y, epochs=1000, batch_size=1)

# Test the model on a new example
test_example = np.array([[1, 1, 0, 0, 0]])
prediction = model.predict(test_example)
print(prediction)
```

我强烈推荐使用像 Jupyter Notebook 这样的 Python 笔记本来组织你的代码，以便你可以快速重新运行单元，而无需重新训练模型。沿着每个注释拆分代码。

让我们进一步分析一下我们正在测试模型的数据：

```py
test_example = np.array([[1, 1, 0, 0, 0]])
```

本质上，我们有一张包含所有马的属性的图像，但没有我们在数据中包含的任何环境因素（绿色草地、蓝色天空、树木等）。模型输出：

```py
0.02694458
```

哎呀！即使模型有脸和尾巴——我们用来识别马的特征——它对图像是马的判断信心只有 2.7%。

## 在 MLP 中实现 Dropout

Keras 使得实现 dropout（以及其他防止过拟合的方法）变得极其简单。我们只需返回到包含模型层的列表：

```py
# Define the model
model = Sequential([
    Dense(16, input_dim=5, activation='relu'),
    Dense(8, activation='relu'),
    Dense(1, activation='sigmoid')
])
```

并添加一些 dropout 层！

```py
# Define the model
model = Sequential([
    Dense(16, input_dim=5, activation='relu'),
    Dropout(0.5),
    Dense(8, activation='relu'),
    Dropout(0.5),
    Dense(1, activation='sigmoid')
])
```

现在模型输出：

```py
0.98883545
```

尽管马图像中不包含环境变量，它的判断信心为 99% 这是一张马的图片！

`Dropout(0.5)` 这一行表示，上层中的任何神经元都有 50% 的机会被“丢弃”或从存在中移除，以便对后续层进行参考。通过实现 dropout，我们实际上以资源高效的方式在数百个模型上训练了 MLP。

## 选择一个 Dropout 率

找到适合你模型的理想 dropout 率的最佳方法是通过反复试验——没有一种放之四海而皆准的方法。可以从较低的 dropout 率开始，大约 0.1 或 0.2，然后逐渐增加，直到达到你期望的准确性。以我们的马匹 MLP 为例，dropout 率为 0.05 会导致模型对图像是马的判断信心为 16.5%。另一方面，dropout 率为 0.95 会导致模型丧失过多的神经元以至于无法正常工作——但仍然能够达到 54.1% 的信心。这些值对该模型并不合适，但可能对其他模型适用。

# IV. 结论

让我们回顾一下——dropout 是一种在机器学习中用于防止过拟合并整体提升模型性能的强大技术。它通过随机“丢弃”输入层和隐藏层中的神经元来实现这一点。这使得分类器在一次训练过程中能够训练出数百到数千个独特的模型，从而避免过度关注某些特征。

在接下来的文章中，我们将探索在机器学习领域中用于替代或补充 dropout 的新技术。敬请期待更多内容！
