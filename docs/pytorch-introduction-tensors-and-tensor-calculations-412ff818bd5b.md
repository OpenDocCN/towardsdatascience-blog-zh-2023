# PyTorch 简介——张量与张量计算

> 原文：[`towardsdatascience.com/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b`](https://towardsdatascience.com/pytorch-introduction-tensors-and-tensor-calculations-412ff818bd5b)

## 了解张量以及如何在最著名的机器学习库之一 `pytorch` 中使用它们

[](https://ivopbernardo.medium.com/?source=post_page-----412ff818bd5b--------------------------------)![Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----412ff818bd5b--------------------------------)[](https://towardsdatascience.com/?source=post_page-----412ff818bd5b--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----412ff818bd5b--------------------------------) [Ivo Bernardo](https://ivopbernardo.medium.com/?source=post_page-----412ff818bd5b--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----412ff818bd5b--------------------------------) ·8 分钟阅读·2023 年 11 月 30 日

--

![](img/b8413e710aba7c20bbd31cf22008112b.png)

数学魔法——由 AI 生成的图像

在深度学习领域（包括 ChatGPT 的构建基础）中最重要的库之一是 `pytorch`。与 Tensorflow 框架一起，`pytorch` 是可供软件开发人员和数据科学家使用的最著名的神经网络训练框架之一。除了其可用性和简单的 API 外，它在灵活性和内存使用方面表现出色，使其在多维微积分中极其快速（这是反向传播的一个主要组成部分，反向传播是优化神经网络权重的重要技术）——这些细节使其成为公司在构建深度学习模型时最受欢迎的库之一。

在这篇博客文章中，我们将检查一些使用`pytorch`的基本操作，并了解如何处理`tensor`对象！张量是数据的数学表示，通常有不同的名称：

+   1 维张量：通常称为标量，由一个数学值组成。

+   1 维张量：由 *n* 个示例组成，通常称为 1-D 向量，能够在单一维度中存储不同的数学元素。

+   2 维张量：通常称为矩阵，能够在两个维度中存储数据。可以想象成一个普通的 SQL 表或一个 Excel 电子表格。

+   3 维张量及以上：具有这种维度的数据通常更难以可视化，通常称为 *n-维* 张量*。

在对数学概念进行简单介绍后，让我们探索如何在 Python 中使用`pytorch`！

# 张量对象

如我们所述，张量对象是 *n 维* 对象的数学泛化，可以扩展到几乎任何维度。尽管在深度学习的上下文中，`tensors` 通常是多维的，但我们也可以使用 `torch` 创建单元素张量（通常称为标量）（尽管名为 `pytorch`，我们使用 `torch` 来操作 Python 中的库）。

如果张量是 `torch`（或 `pytorch`）中的核心对象，我们如何在库中创建它们？

超简单！让我们创建第一个单元素张量：

```py
import torch
scalar = torch.tensor(5)
```

我们的 `scalar` 对象包含一个数字 — 5。让我们通过在 Python 控制台中调用它来可视化我们的张量：

![](img/7a3593223add0044338cb8aa311c1030.png)

标量对象 — 作者图像

> 事实 1: `torch.tensor` 用于创建张量对象

当然，我们不仅限于单元素张量 — 我们还可以创建具有多个元素的 1 维对象。让我们将一个列表传递到 `torch.tensor` 中，看看结果如何：

```py
vector = torch.tensor([7, 7])
vector
```

![](img/05ff4daefab1efc39855aa600251e521.png)

vector 对象 — 作者图像

我们的对象 `vector` 现在包含一个维度上的两个元素。可以将这些数据视为 1 行或 1 列的数据。

拥有“维度”允许我们访问张量中的有趣属性 — 例如 `ndim`：

```py
vector.ndim
```

![](img/20a86d131a2a7168049f72deffaae26e.png)

vector 对象的 ndim — 作者图像

> 事实 2: `*tensor.ndim*` 用于获取张量对象的维度数量

在我们的例子中，`vector` 对象只有一个维度。我们如何知道张量对象包含多少元素？通过使用另一个属性 - `shape`！

```py
vector.shape
```

![](img/7ebc201398505131cf4c64559f22d02e.png)

vector 对象的 shape — 作者图像

> 事实 3: `*tensor.shape*` 用于获取张量对象的形状

我们的张量对象在一个维度中包含两个元素。我们将看看这个输出与多维对象的比较。

`torch` 张量还包含一个附加的数据类型。要了解是哪个，我们可以使用：

```py
vector.dtype
```

![](img/5d04609ab803207f60e42d4a13744a7f.png)

vector 对象的 dtype — 作者图像

> 事实 4: `*tensor.dtype*` *输出张量对象的类型。*

我们的张量包含 `int64` 格式的数据。

现在让我们将对象扩展为一个 2-D 张量：

```py
matrix = torch.tensor([[10.0, 20.0], 
                       [30.0, 40.0]])

matrix
```

![](img/038f1a20195e8e9e6ef374f846b57495.png)

matrix 对象 — 作者图像

让我们看看关于我们的 `matrix` 对象的一些属性：

```py
print(matrix.ndim)
print(matrix.shape)
print(matrix.dtype)
```

![](img/9f0f9f370b1cb20d6d6d0aacb4c1cfa3.png)

ndim、shape 和 dtype 的矩阵对象 — 作者图像

我们的 `matrix` 对象包含两个维度中每个有 2 个元素的数据，dtype 为 `float32`。

为了完成我们对创建张量的探索，让我们看看如何使用 `torch.rand` 生成随机张量：

```py
torch.rand(size=(4, 4))
```

![](img/042af5da88558fef9bd1c0bc403ca822.png)

随机张量 — 作者图像

例如，在上面的张量中，我们使用`tensor.rand`生成一个 4x4 的矩阵。这是在深度学习中非常常见的操作（例如，生成随机的神经网络层权重以便后续优化）。

# 张量操作

现在我们来看看如何对张量进行操作。如果你已经熟悉`numpy`，这应该很简单！从一个简单的加法操作开始：

```py
tensor = torch.tensor([1, 2, 3])
tensor + 20
```

![](img/c011825b3678c3959acd6e54f5b2f475.png)

张量 + 10 计算 — 作者提供的图片

向张量添加标量很简单 — 只需使用普通的数学操作即可！你能猜到如何将张量与标量相乘吗？

简单！

```py
tensor * 10
```

![](img/231ec78f551782b2a8faff7083a77d8f.png)

张量 * 10 计算 — 作者提供的图片

你也可以使用抽象的`torch.multiply`：

```py
torch.multiply(tensor, 10)
```

![](img/231ec78f551782b2a8faff7083a77d8f.png)

张量 * 10 计算 — 作者提供的图片

张量的两个最常见的操作是[Hadamard](https://en.wikipedia.org/wiki/Hadamard_product_(matrices))和[点积](https://en.wikipedia.org/wiki/Dot_product)，后者是[注意力](https://en.wikipedia.org/wiki/Attention_(machine_learning))机制中广泛使用的著名计算之一。

让我们创建两个 2 维张量来检查这些操作：

```py
tensor_1 = torch.tensor([[1,2,3],[2,3,4]])
tensor_2 = torch.tensor([[1,2],[2,3],[3,4]])
```

![](img/23e8ad1876d981a1c6674d7b19405994.png)

张量 _1，一个 2x3 张量 — 作者提供的图片

![](img/1c65872340eb8eac160c223c3be8b78f.png)

张量 _2，一个 3x2 张量 — 作者提供的图片

要执行 Hadamard 积，张量的形状必须匹配。让我们计算`tensor_1`与其自身的 Hadamard 积：

```py
# Hadamard product
tensor_1 * tensor_1
```

![](img/c65d2b0f5ef2237e2545847722fbaa8d.png)

张量 _1 乘以张量 _1 — 作者提供的图片

对于点积操作，张量的内维度必须匹配。让我们将张量 _1（一个 2x3 张量）与张量 _2（一个 3x2 张量）相乘：

```py
torch.matmul(tensor_1, tensor_2)
```

![](img/b9e45a13c9d19b0bf13817ec7972f819.png)

张量 _1 与张量 _2 的点积 — 作者提供的图片

我们也可以使用优雅的@操作，它执行相同的操作：

```py
tensor_1 @ tensor_2
```

![](img/1f27284e552a677066a8d19e212c1566.png)

张量 _1 与张量 _2 的点积 — 作者提供的图片

# 张量索引

在最后的示例中，让我们看看如何从张量中提取某些元素。对于这些示例，我们将使用：

```py
indexing_example = torch.tensor([[10,20,30],[40,50,60],[70,80,90]])
indexing_example
```

![](img/b0502c943daafd92ef874f03bd991e87.png)

2-D 张量示例 — 作者提供的图片

`pytorch`中的索引与其他 Python 对象类似 — 让我们尝试索引第一列：

```py
indexing_example[0,:]
```

![](img/437768eee86f939199df84b0eba6f16a.png)

第 1 行示例 — 作者提供的图片

使用`[]`中的 0 索引将使我们能够提取对象的第一行。`:`符号使我们能够从某一维度提取所有元素。在我们的例子中，我们想要从列（第 2 维）中提取所有元素。

你能猜到如何提取第一列吗？只需交换索引的位置！

```py
indexing_example[:,0]
```

![](img/1fd97f007ac58f2e361d693efded311c.png)

第 1 列示例 — 作者提供的图片

对于更复杂的对象，我们也可以使用相同的逻辑。让我们尝试从一个 3D `tensor` 中索引一个元素：

```py
indexing_example_3d = torch.tensor([[[10,20,30],[40,50,60],[70,80,90]], [[100,200,300],[400,500,600],[700,800,900]]])
indexing_example_3d
```

![](img/2089ea114dfb6cfa3186935b3d1400d8.png)

3D 张量 — 图片由作者提供

我们如何从这个张量中提取元素“100”？让我们来看看，我们需要：

+   第一行

+   第一列

+   第二个矩阵

使用索引逻辑，我们可以轻松实现这一点：

```py
indexing_example_3d[1,0,0]
```

![](img/dfaa73d9d3a30a542e7eb6b80d6d1684.png)

从 indexing_example_3d 提取的 100 元素 — 图片由作者提供

在`torch`中，3D 对象的索引顺序如下：矩阵、行、列。

你能尝试索引一个 4D 对象吗？

# 额外内容 — 张量存储在哪里？

使用`torch`相对于其他数组库（如`numpy`）的一个优点是能够将张量保存在`gpu`中——这在我们需要加速神经网络计算时特别有用。

默认情况下，你的张量存储在`cpu`上（而大多数计算机仅有一个 cpu 可用），但你可以通过以下操作将张量发送到`gpu`：

```py
device = "cuda" if torch.cuda.is_available() else "cpu"
```

如果`torch.cuda.is_available()`在你的计算机上找到特定的 NVIDIA gpu，它会让你将张量发送到该 gpu。

想象一下你有一个存储在`tensor`命名对象中的张量，你可以使用`.to`方法将其发送到设备：

```py
tensor_on_gpu = tensor.to(device)
```

# 结论

感谢你抽时间阅读这篇文章！处理张量非常有趣，确实能为你提供一个坚实的基础，来使用高级神经网络。

`torch` API 非常优雅且易于可视化。之后，你可以使用这些张量来训练神经网络（这是我将在本系列的下一篇博客中展示的内容）。此外，在学习过程中稍微掌握一些线性代数将对学习其他数据科学和机器学习算法极有帮助。

这篇文章的灵感来源于[`www.learnpytorch.io/`](https://www.learnpytorch.io/)——这是一个关于 Pytorch 的优秀免费课程，我强烈推荐。在[DareData](https://www.daredata.engineering/)我们参与了许多深度学习项目，我不能强调这门课程对我们培训人员学习这种机器学习范式及其相关框架的重要性。

在下一篇文章中，我们将探讨如何使用`torch`训练线性回归——敬请期待！

*如果你想参加我的 Python 课程，随时加入* ***我的 16 小时 Python 课程*** *(*[*初学者完整 Python 训练营*](https://www.udemy.com/course/the-python-for-absolute-beginners-bootcamp/?couponCode=MEDIUMJULY)*）。我的 Python 课程适合初学者/中级开发者，我非常希望你能加入我的课堂！*

![](img/9244c5c0a938984506d56f766b6e234a.png)

Python 初学者训练营 — 图片由作者提供
