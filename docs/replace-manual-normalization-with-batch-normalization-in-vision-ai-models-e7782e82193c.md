# 替换视觉 AI 模型中的手动归一化为批量归一化

> 原文：[`towardsdatascience.com/replace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c?source=collection_archive---------9-----------------------#2023-05-18`](https://towardsdatascience.com/replace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c?source=collection_archive---------9-----------------------#2023-05-18)

## 一个巧妙的技巧是，将批量归一化层作为模型的第一层，以避免在视觉（图像/视频）AI 模型中进行昂贵的手动像素归一化

[](https://medium.com/@dhruvbird?source=post_page-----e7782e82193c--------------------------------)![Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----e7782e82193c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----e7782e82193c--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7782e82193c--------------------------------) [Dhruv Matani](https://medium.com/@dhruvbird?source=post_page-----e7782e82193c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F63f5d5495279&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freplace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c&user=Dhruv+Matani&userId=63f5d5495279&source=post_page-63f5d5495279----e7782e82193c---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----e7782e82193c--------------------------------) ·8 分钟阅读·2023 年 5 月 18 日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fe7782e82193c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freplace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c&user=Dhruv+Matani&userId=63f5d5495279&source=-----e7782e82193c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fe7782e82193c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Freplace-manual-normalization-with-batch-normalization-in-vision-ai-models-e7782e82193c&source=-----e7782e82193c---------------------bookmark_footer-----------)

与 [Naresh](https://medium.com/u/1e659a80cffd?source=post_page-----e7782e82193c--------------------------------) 共同撰写

![](img/4c45a0398260aad3d9333c698c15e9e9.png)

照片由 [Kevin Ku](https://unsplash.com/ko/@ikukevk?utm_source=medium&utm_medium=referral) 拍摄，发布在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral) 上

# 图像预处理中的通道归一化

彩色图像通常有 3 个通道（RGB）。视觉 AI 模型通常会对图像像素进行预处理和归一化，以便将给定通道中的像素归一化到均值为 0.0 和方差为 1.0。由于每个通道可以有自己独特的统计数据，所以归一化是按通道进行的。批量归一化是视觉模型中用于避免被称为协变量转移现象的一种通用最佳实践。

## 什么是协变量转移？

[协变量转移](https://en.wikipedia.org/wiki/Batch_normalization)是一种现象，当输入特征（即协变量）的分布在机器学习模型的训练阶段和测试阶段之间发生变化时，就会发生这种现象。这可能导致模型性能下降，因为在训练期间做出的假设在测试期间可能不再成立。协变量转移可能由于数据收集过程的变化、被采样的人群的变化或模型使用环境的变化而发生。为了解决协变量转移，可以使用[领域适应](https://en.wikipedia.org/wiki/Domain_adaptation)和重要性加权等技术，以根据输入分布的变化调整模型的预测。

然而，这些技术相当复杂，需要对输入数据分布有更深入的理解。

## 批量归一化如何避免协变量转移？

[批量归一化](https://en.wikipedia.org/wiki/Batch_normalization)通过规范化神经网络中每一层的激活值来帮助解决协变量转移。这意味着每一层的激活值的均值和方差保持在一个固定的值上，而不受该层输入分布的影响。通过这样做，批量归一化减少了训练数据集和测试数据集之间的协变量转移影响。批量归一化可以应用于任何领域，并且能够很好地扩展到各种使用案例而无需修改。

更具体地说，在训练过程中，批量归一化根据当前批次计算的激活均值和方差对每一层的激活值进行中心化和缩放。这将激活值归一化为零均值和单位方差，这有助于稳定和加速训练过程。在训练阶段，跟踪一个运行均值和方差。

在测试过程中，使用训练期间计算的均值和方差来归一化激活值。这确保了归一化与训练数据一致，并减少了协变量转移的影响。

通过减少协变量转移的影响，批量归一化可以提高神经网络的泛化性能，使其对输入分布的变化更加稳健。

# 为什么要预处理输入？

这样做是为了让模型更快地收敛，并对输入数据有良好的泛化能力。当输入数据分布在一致且指定良好的范围内时，模型的表现相对更好。具体来说，它有以下好处：

1.  **防止溢出或下溢**：机器学习模型通常涉及诸如加法、乘法和指数运算等数学操作。如果输入值过大或过小，操作可能会导致溢出或下溢，从而产生不准确或未定义的结果。例如，将一个小的浮点数（fp32）加到一个大数上可能会导致[忽略小数](https://stackoverflow.com/questions/22186589/why-does-adding-a-small-float-to-a-large-float-just-drop-the-small-one)。

1.  **确保高效学习**：神经网络通常使用反向传播算法来更新网络中的权重。该算法依赖于计算梯度，如果输入值没有归一化，这些梯度可能会变得非常小或非常大，从而使学习过程变得更慢且效率降低。

1.  **提高泛化能力**：当模型在特定范围的输入值上训练时，它可能无法在该范围之外的输入上表现良好。归一化输入可以帮助模型对新数据和未见过的数据进行泛化。

**批量归一化**可以通过在训练过程中归一化网络每一层的输入来解决一些这些问题。这可以提高模型的稳定性，并使其对输入的尺度不那么敏感。然而，批量归一化并不总是可以完全替代使用硬编码常数手动完成的输入归一化。

# 什么是批量归一化？

关于批量归一化是什么及其如何帮助模型的主题已经在众多文章中进行了广泛的讨论，因此我们将链接到提供最详细见解的文章，并让读者对其操作形成直观理解。我们还提供了一些将批量归一化与其他归一化技术进行比较的链接。

+   三层理解的批量归一化

+   批量归一化解释

+   [深入了解深度学习：批量归一化](https://d2l.ai/chapter_convolutional-modern/batch-norm.html)

+   [层归一化与批量归一化的后果是什么？](https://ai.stackexchange.com/questions/27309/what-are-the-consequences-of-layer-norm-vs-batch-norm)

# 输入归一化通常是如何完成的？

通常，训练模型的人员负责计算整个训练数据集的每个通道的统计数据（均值和方差），并在训练视觉 AI 模型之前进行归一化。这种归一化也应该在推理过程中进行。

使用[torchvision transforms](https://pytorch.org/vision/main/transforms.html)时，这种预处理的代码可能如下所示。

```py
transforms = torch.nn.Sequential(
    transforms.CenterCrop(10),
    transforms.Normalize(
      # Channel means
      #    R,     G,     B
      (0.485, 0.456, 0.406),
      # Channel standard deviation
      (0.229, 0.224, 0.225),
    ),
)
```

你可以在这里阅读更多关于[Normalize 变换的信息](https://pytorch.org/vision/main/generated/torchvision.transforms.Normalize.html#torchvision.transforms.Normalize)。

你会注意到**均值和标准差是预先计算的**，然后**硬编码**到**预处理管道**中。关于如何**正确**和**高效**地做这件事，网上有很多**讨论**。例如，

1.  [计算数据集的均值和标准差](https://discuss.pytorch.org/t/computing-the-mean-and-std-of-dataset/34949)

1.  [Kaggle Notebook: 计算数据集均值和标准差（使用 PyTorch）](https://www.kaggle.com/code/kozodoi/computing-dataset-mean-and-std/notebook)

1.  [如何在 PyTorch 中计算图像的均值和标准差](https://www.binarystudy.com/2021/04/how-to-calculate-mean-standard-deviation-images-pytorch.html)

# 如何减少这项工作的痛苦？

现在我们知道这个过程有多痛苦，我们将看到一个巧妙的技巧来减轻你的痛苦！

只需将一个[BatchNorm2d](https://pytorch.org/docs/stable/generated/torch.nn.BatchNorm2d.html)层作为视觉 AI 模型的第一层，并从预处理步骤中移除[Normalize](https://pytorch.org/vision/main/generated/torchvision.transforms.Normalize.html#torchvision.transforms.Normalize)变换！

通过将批量归一化作为模型的第一层，输入数据将在训练过程中**自动归一化**，你不需要手动归一化图像像素。这可以节省一些编码时间，并减少在归一化过程中引入错误的机会。

# 手动归一化和批量归一化的粗略等价性

在这里，我们将看到一些代码，可以说服我们这两种方法的粗略等价性。

我们将使用 1000 批随机生成的 1x1 图像（具有 3 个通道），看看手动计算的均值和方差是否与使用 PyTorch 的 BatchNorm2d 层计算的结果相似。

```py
torch.manual_seed(21)
num_channels = 3

# Example tensor so that we can use randn_like() below.
y = torch.randn(20, num_channels, 1, 1)

model = nn.BatchNorm2d(num_channels)

# nb is a dict containing the buffers (non-trainable parameters)
# of the BatchNorm2d layer. Since these are non-trainable
# parameters, we don't need to run a backward pass to update
# these values. They will be updated during the forward pass itself.
nb = dict(model.named_buffers())
print(f"Buffers in BatchNorm2d: {nb.keys()}\n")

stacked = torch.tensor([]).reshape(0, num_channels, 1, 1)

for i in range(2000):
    x = torch.randn_like(y)
    y_hat = model(x)
    # Save all the input tensor into 'stacked' so that
    # we can compute the mean and variance later.
    stacked = torch.cat([stacked, x], dim=0)
# end for

print(f"Shape of stackend tensor: {stacked.shape}\n")
smean = stacked.mean(dim=(0, 2, 3))
svar = stacked.var(dim=(0, 2, 3))
print(f"Manually Computed:")
print(f"------------------")
print(f"Mean: {smean}\nVariance: {svar}\n")
print(f"Computed by BatchNorm2d:")
print(f"------------------------")
rm, rv = nb['running_mean'], nb['running_var']
print(f"Mean: {rm}\nVariance: {rv}\n")
print(f"Mean Absolute Differences:")
print(f"--------------------------")
print(f"Mean: {(smean-rm).abs().mean():.4f}, Variance: {(svar-rv).abs().mean():.4f}")
```

你可以查看下面代码单元的输出。

```py
Buffers in BatchNorm2d: dict_keys(['running_mean', 'running_var', 'num_batches_tracked'])

Shape of stackend tensor: torch.Size([40000, 3, 1, 1])

Manually Computed:
------------------
Mean: tensor([0.0039, 0.0015, 0.0095])
Variance: tensor([1.0029, 1.0026, 0.9947])

Computed by BatchNorm2d:
------------------------
Mean: tensor([-0.0628,  0.0649,  0.0600])
Variance: tensor([1.0812, 1.0318, 1.0721])

Mean Absolute Differences:
--------------------------
Mean: 0.0602, Variance: 0.0616
```

我们从一个使用 `torch.randn_like()` 初始化的随机张量开始，因此我们期望在足够大的样本数量（40k）中，均值和方差将趋近于 0.0 和 1.0，因为这正是我们期望[torch.randn_like()](https://pytorch.org/docs/stable/generated/torch.randn_like.html)生成的结果。

我们看到手动计算的均值和方差与使用 BatchNorm2d 的滚动平均法计算的均值和方差之间的差异在所有实际应用中都足够接近。我们可以看到，使用 BatchNorm2d 计算的均值始终高于或低于手动计算的均值（最多 40 倍）。然而，从实际的角度来看，这应该没有问题。

# 注意事项

使用批量归一化代替手动归一化确实有利有弊，这篇文章如果不详细比较这两者在何时适用或不适用的话就不完整了。

## 迁移学习

在使用迁移学习时，通常建议保持[预训练模型](https://pytorch.org/vision/main/models.html)中使用的归一化方法，以避免引入不必要的变化。在这种情况下，用批量归一化替换手动归一化可能不合适。

例如，以下是 torchvision 页面对这个主题的描述。

> “在使用预训练模型之前，必须对图像进行预处理（调整正确的分辨率/插值、应用推理变换、重新缩放值等）。没有标准的方法来做到这一点，因为这取决于给定模型的训练方式。它可以在模型系列、变体甚至权重版本之间有所不同。使用正确的预处理方法至关重要，否则可能导致准确性下降或输出错误。”

## 训练效率

在考虑训练效率时，预先计算数据集的均值和方差并将其硬编码为预训练的归一化步骤可能是有益的。这可以防止在训练过程中重复计算这些统计量。

请注意，使用任何方法在推理期间的计算量相等，因为你会使用计算出的均值和方差对输入进行归一化，无论是在将输入喂入模型之前（手动计算）还是作为模型的第一步（使用批量归一化）。

## 人工效率

基于我们上面的观察，对于人类来说，插入一个批量归一化层比手动计算统计量要容易得多。

## 数据增强

在将输入送入模型之前进行数据增强时，必须小心在所有增强和其他预处理步骤完成后**再**应用归一化，以避免计算出不正确的统计量。例如，如果你使用了[ColorJitter 变换](https://pytorch.org/vision/main/generated/torchvision.transforms.ColorJitter.html#torchvision.transforms.ColorJitter)，它将会显著改变计算出的统计量。

这引出了另一个有趣的问题***“在使用数据增强时，应该什么时候计算数据集的均值和方差？”***

这有点棘手，因为准确地跨增强输入计算均值和方差需要你预先知道哪些图像将使用哪些转换进行增强，然后需要你在模型训练期间以一致的方式计算统计信息并应用增强。这在一般情况下有些困难，因为增强是随机应用在输入图像上的。此外，同一图像在每个训练周期会以不同方式进行增强，以防止模型过度拟合训练数据集。因此，在这种情况下，使用批量标准化也会提高模型的准确性，因为它计算的是增强图像的均值和方差，而不是原始未增强的图像。

## 小批量分布

由于批量标准化在训练期间计算小批量的均值和方差以进行归一化值，因此重要的是随机化输入数据以确保小批量的均值和方差在某种程度上代表整个训练数据集。如果你的小批量数据很小或有偏差，那么你应该考虑消除偏差或使用手动标准化。

这个问题在测试或推理时间不存在。

# 结论

标准化是任何视觉 AI 管道中的重要预处理步骤。通过手动步骤正确并高效地计算它可能会很繁琐且容易出错。在许多情况下，将批量标准化作为视觉 AI 模型的第一层是手动标准化的可行替代品。
