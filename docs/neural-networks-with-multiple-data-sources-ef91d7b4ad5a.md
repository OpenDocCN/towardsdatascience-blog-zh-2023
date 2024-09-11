# 具有多个数据源的神经网络

> 原文：[https://towardsdatascience.com/neural-networks-with-multiple-data-sources-ef91d7b4ad5a?source=collection_archive---------4-----------------------#2023-01-06](https://towardsdatascience.com/neural-networks-with-multiple-data-sources-ef91d7b4ad5a?source=collection_archive---------4-----------------------#2023-01-06)

## 如何使用 Tensorflow 设计一个具有多个数据源输入的神经网络

[](https://medium.com/@morgan_lynch?source=post_page-----ef91d7b4ad5a--------------------------------)[![Morgan Lynch](../Images/731f65c99b8bedc3e07ca6f281596f09.png)](https://medium.com/@morgan_lynch?source=post_page-----ef91d7b4ad5a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ef91d7b4ad5a--------------------------------)[![数据科学前沿](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ef91d7b4ad5a--------------------------------) [Morgan Lynch](https://medium.com/@morgan_lynch?source=post_page-----ef91d7b4ad5a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4b5483121384&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-networks-with-multiple-data-sources-ef91d7b4ad5a&user=Morgan+Lynch&userId=4b5483121384&source=post_page-4b5483121384----ef91d7b4ad5a---------------------post_header-----------) 发表在 [数据科学前沿](https://towardsdatascience.com/?source=post_page-----ef91d7b4ad5a--------------------------------) ·5 分钟阅读·2023年1月6日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fef91d7b4ad5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-networks-with-multiple-data-sources-ef91d7b4ad5a&user=Morgan+Lynch&userId=4b5483121384&source=-----ef91d7b4ad5a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fef91d7b4ad5a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fneural-networks-with-multiple-data-sources-ef91d7b4ad5a&source=-----ef91d7b4ad5a---------------------bookmark_footer-----------)![](../Images/b642d0d8398604490bcb3b4dd45695fc.png)

具有多个数据源的卷积神经网络。图片来源：作者。

在许多使用场景中，神经网络需要并行训练多个数据源。这些包括医学应用场景，其中可能会有一张或多张图像与结构化的患者数据一起使用，或者多图像应用场景，其中不同对象的图像贡献到单一输出。例如，使用个人房屋和汽车的独立照片来预测他们的收入。

集体数据不能一体处理，因为每个数据源都有其独特的属性和形状。为了成功设计一个网络，每个输入流需要单独处理和训练。

使用具有多个独立输入的 CNN 已被证明比单一图像输入提高了准确性。在一项研究中[1]，处理了三个不同的图像输入分支，并将它们合并，结果比单独处理图像提高了 8% 的准确性。

此外，还显示出在 CNN 设计中，晚期合并网络分支也能产生更好的准确性[2]。这种晚期合并意味着在实际操作中，输入分支应该在合并到最终模型并生成预测之前，几乎完全作为独立网络进行处理。

我们将详细讨论如何设计这种类型的卷积神经网络（CNN），通过一个理论上的患者数据示例，其中包含一个数据 CSV 文件和一张图像。我们将只考虑一个图像输入，但这种方法也可以用于每个患者的多个图像。

首先，必须加载源文件并将其处理为 Pandas 数据框。下方示例中，加载了一个简单的数据集，包括患者 ID、患者年龄和一个标志，表示是否已诊断出癌症。

需要注意数据框的形状，因为这将影响后续网络的设计。

接下来，我们必须为每个患者加载一张图像。这是通过对患者数据框进行迭代来完成的，以保持记录的顺序。

图像数据也被转换为 numpy 数组，以保持与从文件中加载的患者数据的一致性。

![](../Images/973fb21675363cb370788210ba1674fd.png)

加载数据的形状。图片由作者提供

我们现在需要考虑已加载数据的形状。对于图像，如果每个图像的尺寸为 512x512 像素，并且我们有*n*张图像，那么数据的形状为(n, 512, 512)。对于具有多个通道的图像，可能会添加进一步的维度，但我们将保持这个示例简单。

对于结构化的患者数据，我们的文件中有三列和*n*条记录。这将导致数据形状为(n, 3)。患者 ID 列在训练中是不需要的，因此这列可能会被删除，从而得到最终训练数据的形状为(n, 2)。

数据的进一步预处理，如缩放，不在本讨论范围之内。对于本示例，我们将直接使用原始数据。

然而，在设计神经网络之前，还需要一步。这一步是将数据分割成训练集和测试集。这需要在一个步骤中完成，以保持数据集的顺序和分割。下面的示例演示了如何使用 scikit-learn 来完成这一步骤：

一旦分割完成，我们就可以从两个数据集中提取目标特征作为我们的‘y’数据集。检查两个结果训练数据集的形状应该产生类似于以下的输出：

*(1200, 512, 512)*

*(1200, 3)*

记录数为 1,200。两个数据集需要具有相同数量的记录，以便它们可以在神经网络的输出中合并。

现在我们可以使用 Keras 函数式 API 开始设计神经网络。首先，我们将从结构化的患者数据开始：

网络的设计可以有所不同，但最好包括一个归一化层。归一化层仅适应于训练数据。

重要的是，输入层的形状设置为数据中的列数（在此示例中为3）。

输出层的形状也很关键，因为这是将与图像处理分支合并的形状。这由最后的 Dense 层决定。在此示例中，输出层的形状将是：

*(无, 64)*

其中‘None’是 Keras 对记录数量的解释，未指定。

数据分支现在已经完成，我们可以查看图像处理分支。虽然可以设计自己的网络，但在实践中使用预设计的模型更为方便。在此示例中，我们将使用 Keras Applications 中的 Resnet-50。

如上所示，输入形状是每个图像的大小，加上一个额外的维度用于图像通道（在此案例中为1）。

在 Resnet 模型的末尾添加一个全连接的 Dense 层，以使输出与数据分支的形状相同：

*(无, 64)*

因为我们在整个过程中都注意了数据的形状，所以现在能够合并两个分支的输出：

两个分支被连接在一起，最后添加一个全连接的 Dense 层以将模型减少到最终的预测。这里使用的激活函数可以有所不同。在此示例中，使用线性激活函数来输出实际的类别概率。

CNN 的最终设计总结如下：

![](../Images/dd2da001a98f1ee9fd06d02b4297ad05.png)

最终 CNN 设计。图片由作者提供

如上所示，如果仔细考虑数据的形状，可以成功地合并多个分支。然后，可以使用这个合并后的模型从多个数据源生成单一的预测。

感谢阅读。

**参考文献：**

[1] Yu Sun, Lin Zhu, Guan Wang, Fang Zhao, “Multi-Input Convolutional Neural Network for Flower Grading”, *Journal of Electrical and Computer Engineering*, vol. 2017, Article ID 9240407, 8 pages, 2017\. [https://doi.org/10.1155/2017/9240407](https://doi.org/10.1155/2017/9240407)

[2] Seeland M, Mäder P (2021) Multi-view classification with convolutional neural networks. PLoS ONE 16(1): e0245230\. [https://doi.org/10.1371/journal.pone.0245230](https://doi.org/10.1371/journal.pone.0245230)
