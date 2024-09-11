# 如何通过操作符融合和CPU/GPU代码生成加速深度学习

> 原文：[https://towardsdatascience.com/how-pytorch-2-0-accelerates-deep-learning-with-operator-fusion-and-cpu-gpu-code-generation-35132a85bd26?source=collection_archive---------0-----------------------#2023-04-20](https://towardsdatascience.com/how-pytorch-2-0-accelerates-deep-learning-with-operator-fusion-and-cpu-gpu-code-generation-35132a85bd26?source=collection_archive---------0-----------------------#2023-04-20)

## 介绍了PyTorch中的深度学习编译器技术，包括图捕获、中间表示、操作符融合以及优化的C++和GPU代码生成

[](https://medium.com/@shashankprasanna?source=post_page-----35132a85bd26--------------------------------)[![Shashank Prasanna](../Images/ede96160e770a1db0bb7ab9ece9bdf4f.png)](https://medium.com/@shashankprasanna?source=post_page-----35132a85bd26--------------------------------)[](https://towardsdatascience.com/?source=post_page-----35132a85bd26--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----35132a85bd26--------------------------------) [Shashank Prasanna](https://medium.com/@shashankprasanna?source=post_page-----35132a85bd26--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fe0c596ca35b5&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-pytorch-2-0-accelerates-deep-learning-with-operator-fusion-and-cpu-gpu-code-generation-35132a85bd26&user=Shashank+Prasanna&userId=e0c596ca35b5&source=post_page-e0c596ca35b5----35132a85bd26---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----35132a85bd26--------------------------------) · 17分钟阅读 · 2023年4月20日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F35132a85bd26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-pytorch-2-0-accelerates-deep-learning-with-operator-fusion-and-cpu-gpu-code-generation-35132a85bd26&user=Shashank+Prasanna&userId=e0c596ca35b5&source=-----35132a85bd26---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F35132a85bd26&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-pytorch-2-0-accelerates-deep-learning-with-operator-fusion-and-cpu-gpu-code-generation-35132a85bd26&source=-----35132a85bd26---------------------bookmark_footer-----------)![](../Images/c4b5db3571e5b2492a3e7f9626723155.png)

插图由作者提供

计算机编程是神奇的。我们用人类可读的语言编写代码，然后仿佛魔法一样，它被转换成通过硅晶体管的电流，使其像开关一样工作，并实现复杂的逻辑——只是为了让我们能在互联网上欣赏猫咪视频。在编程语言和运行它的硬件处理器之间，有一个重要的技术组件——编译器。编译器的工作是将我们人类可读的语言代码翻译并简化为处理器能理解的指令。

编译器在深度学习中扮演着非常重要的角色，提升训练和推理性能，提高能源效率，并针对各种AI加速器硬件。在这篇博客文章中，我将讨论支持PyTorch 2.0的深度学习编译器技术。我将带你了解编译过程的不同阶段，并通过代码示例和可视化讨论各种基础技术。

# 什么是深度学习编译器？

深度学习编译器将用深度学习框架编写的高级代码转换为优化的低级硬件特定代码，以加速训练和推理。它通过执行层和操作符融合、更好的内存规划，生成目标特定的优化融合内核来减少函数调用开销，从而在深度学习模型中发现性能优化的机会。

![](../Images/da27c5876c704b425c43a40781423299.png)

插图由作者提供

与传统软件编译器不同，深度学习编译器必须处理高度并行化的代码，这些代码通常在专用的AI加速硬件（如GPU、TPU、AWS Trainium/Inferentia、Intel Habana Gaudi等）上加速运行。为了提高性能，深度学习编译器必须利用硬件特定的功能，如混合精度支持、性能优化的内核，并尽量减少主机（CPU）与AI加速器之间的通信。

虽然深度学习算法正在以迅猛的速度不断进步，但硬件AI加速器也在不断发展，以跟上深度学习算法的性能和效率需求。我在之前的博客文章中讨论了算法与AI加速器的共同演进：

[](/ai-accelerators-machine-learning-algorithms-and-their-co-design-and-evolution-2676efd47179?source=post_page-----35132a85bd26--------------------------------) [## AI加速器、机器学习算法及其共同设计与演进

### 在AI加速器（如NVIDIA GPU、Intel Habana Gaudi和AWS等）中，机器学习的高效算法和方法——

towardsdatascience.com](/ai-accelerators-machine-learning-algorithms-and-their-co-design-and-evolution-2676efd47179?source=post_page-----35132a85bd26--------------------------------)

在本文中，我将专注于软件方面的内容，特别是接近硬件的软件子集 —— 深度学习编译器。首先，让我们来看看深度学习编译器中的不同功能。

# PyTorch 2.0 中的深度学习编译器

PyTorch 2.0 包括新的编译器技术，以提高模型性能和运行时效率，并通过简单的 API 针对不同的硬件后端进行目标化：torch.compile()。虽然[其他博客文章](https://pytorch.org/get-started/pytorch-2.0/)和文章详细讨论了PyTorch 2.0的性能优势，但在这里，我将专注于当您调用PyTorch 2.0编译器时内部发生的情况。如果您想要量化的性能优势，可以在来自huggingface、timm和torchbench的不同模型的[性能仪表板](https://github.com/pytorch/pytorch/issues/93794)找到。

在高层次上，PyTorch 2.0 深度学习编译器的默认选项执行以下关键任务：

1.  **图捕获**：为您的模型和函数表示计算图。PyTorch 技术：TorchDynamo，Torch FX，FX IR

1.  **自动微分**：使用自动微分进行后向图追踪并降低到基本运算符。PyTorch 技术：AOTAutograd，Aten IR

1.  **优化**：前向和后向图级优化以及操作融合。PyTorch 技术：TorchInductor（默认）或其他编译器

1.  **代码生成**：生成硬件特定的 C++/GPU 代码。PyTorch 技术：TorchInductor，OpenAI Triton（默认）和其他编译器

通过这些步骤，编译器将转换您的代码并生成逐渐“降低”的中间表示（IR）。“降低”是编译器词汇中的一个术语，它指的是通过自动转换和重新编写将广泛的操作（例如由PyTorch API支持的操作）映射到狭窄的操作集（例如由硬件支持的操作）。PyTorch 2.0 编译器流程：

![](../Images/55cdb16777e0b865f242827c8d4130fa.png)

如果您对编译器术语不熟悉，请不要被这一切吓到。我也不是编译器工程师。继续阅读，随着我将使用一个简单的示例和可视化来详细解释，一切将变得清晰起来。

# 通过 `torch.compile()` 编译器过程的详细步骤

注意：整个步骤详见[此处的Jupyter Notebook](https://github.com/shashankprasanna/pytorch-examples/blob/main/pytorch-compile-blogpost/torch-compile-under-the-hood.ipynb)

为了简单起见，我将定义一个非常简单的函数并将其通过PyTorch 2.0编译器流程运行。您可以用深度神经网络模型或nn.Module子类替换此函数，但这个例子应该能帮助您更好地理解底层发生的事情，而不是复杂的多参数模型。

![](../Images/0e1df7429cacd7a8c758940431df63f8.png)

该函数的 PyTorch 代码：

```py
def f(x):
  return torch.sin(x)**2 + torch.cos(x)**2
```

如果你在高中三角学课程中认真听讲，你会知道我们的函数值对于所有实值 x 始终为 1。这意味着它的导数是一个常数的导数，必须等于零。这在验证函数及其导数的作用时会很有用。

现在，是时候调用 `torch.compile()`。首先，让我们说服自己编译这个函数不会改变其输出。对于相同的 1x1000 随机向量，函数输出与 1 的向量之间的均方误差应在编译和未编译的函数之间为零（在某些误差容限下）。

![](../Images/e4440c36c8e45317c5e3628d1819dc59.png)

作者截图

我们所做的只是添加了一行额外的代码 `torch.compile()` 来调用我们的编译器。现在让我们看看每个阶段发生了什么。

# 图形捕获：PyTorch 模型或函数的计算图表示

**PyTorch 技术：** TorchDynamo, FX 图, FX IR

编译器的第一步是确定编译内容。进入 TorchDynamo。TorchDynamo 截取你的 Python 代码的执行，并将其转换为 FX 中间表示（IR），并将其存储在一个称为 FX 图的特殊数据结构中。你问这看起来是什么样的？很高兴你问了。下面，我们将查看生成这些的代码，但这是转换和输出的结果：

![](../Images/c6a6411faea674730ef0b417028eda20.png)

作者截图

重要的是要注意，Torch FX 图只是 IR 的容器，并不真正指定它应该包含哪些操作符。在下一部分，我们将看到 FX 图容器再次出现，但带有不同的 IR 集。如果你比较函数代码和 FX IR，它们之间几乎没有差别。实际上，它是你编写的相同 PyTorch 代码，只是以 FX 图数据结构期望的格式进行布局。当执行时，它们将提供相同的结果。

如果你在没有任何参数的情况下调用 `torch.compile()`，它将使用默认设置，这会运行整个编译器栈，包括默认的硬件后端编译器 TorchInductor。但如果我们现在讨论 TorchInductor 会有点超前，因此暂时搁置这个话题，等我们准备好时再回来。首先我们需要讨论图形捕获，我们可以通过截取 `torch.compile()` 的调用来实现这一点。以下是我们将要做的：`torch.compile()` 允许你提供自己的编译器，但由于我不是编译器工程师，也不知道如何编写编译器，我将提供一个虚拟编译器函数来捕获 TorchDynamo 生成的 FX 图 IR。

以下是我们为 `torch.compile()` 函数编写的虚拟编译器后端函数 inspect_backend，在这个函数中我做了两件事：

1.  打印由 TorchDynamo 捕获的 FX IR 代码

1.  保存 FX 图形可视化

上述代码片段的输出是 FX IR 代码和图表，显示了我们的函数 `sin^2(x)+cos^2(x)`

![](../Images/4aceac24aae49fe3d4cc61ae7a54a437.png)

作者的屏幕截图

请注意，我们的虚拟编译器 inspect_backend 函数是在我们使用一些数据调用编译函数时才调用的，即当我们调用 `compiled_model(x)` 时。在上述代码片段中，我们只评估函数，或者用深度学习术语说，“进行前向传播”。在接下来的部分，我们将利用 PyTorch 的自动微分引擎 torch.autograd 来计算导数和“反向传播”图。

# 自动微分：前向和后向计算图

**PyTorch 技术:** AOTAutograd, 核心 Aten IR

TorchDynamo 给出了前向传播函数评估作为 FX 图，但是后向传播呢？为了完整起见，我将偏离我们的主要话题，说一点为什么我们需要评估函数相对于其权重的梯度。如果你已经了解数学优化如何工作，请跳过这节。

## **反向传播和反向图是什么？**

深度学习和机器学习中的“学习”部分是一个数学优化问题，简单地说就是：找到一个变量 w 的值，使得某个关于 w 的函数取得最小值。或者更简洁地说：

![](../Images/89206945634aa1846e7dd6915fcf0efa.png)

在机器学习中，f(w) 是由权重参数化的损失函数。 f(w) 可以更清晰地表示为训练标签与模型根据训练数据对训练标签进行预测之间的错误度量：

![](../Images/73c9ab5ba817313bf304d21987085e3a.png)

结果发现，如果我们能够计算损失相对于权重的“减少速率”，我们就可以更新我们的权重，使其向一个越来越小的损失 f(w) 前进一步。换句话说，我们必须朝着更好地拟合训练数据集的模型前进。我们可以通过计算在给定 w 处损失 f(w) 的最陡斜率，来找到下一个权重值，并扰动 w 朝着那个方向前进。函数关于权重的斜率，就是其相对于权重的导数。由于存在多个权重值，导数变成一个矢量量，称为梯度，是一个针对每个权重的分量的偏导数的矢量。权重 w 在每次迭代时通过梯度的某个函数 g() 进行扰动，如下所示：

![](../Images/e1d0c3208f5555a0af3b2d0396f433cf.png)

其中函数 g(.) 取决于优化器（例如 sgd, sgdm, rmsprop, adam 等）。

对于 SGD 权重更新步骤如下：

![](../Images/c2fba7a1140e7d8e82a8425cbf4e9515.png)

## **PyTorch 2.0 如何跟踪反向传播图？**

首先，让我们计算我们期望反向传递图应该是什么样的，然后与 PyTorch 生成的结果进行比较。对于我们的简单函数，前向图和反向图应该实现以下函数。如果 sin 和 cos 让你感到困扰，你可以想象 f(x) 是应用于神经网络的损失函数。

![](../Images/3f65c215f211e0ed8c73a719d0937eba.png)

PyTorch 使用反向模式的 [自动微分](https://en.wikipedia.org/wiki/Automatic_differentiation) 来计算梯度，PyTorch 的自动微分实现称为 Autograd。PyTorch 2.0 引入了 AOTAutograd，它在执行之前预先跟踪前向和反向图，并生成一个联合的前向和反向图。然后，它将前向图和反向图分成两个独立的图。前向图和反向图都存储在 FX 图数据结构中，可以如下面所示进行可视化。

![](../Images/94722341aa44c88e98820ab72d1f3085.png)

作者截图

你可以通过遍历图上的节点来验证数学计算是否正确。AOTAutograd 生成的反向传递确实计算了我之前分享的方程中的导数，该导数应该等于零，因为原始函数仅生成了单位矩阵。

现在我们将通过扩展我们伪编译器函数 `inspect_backend` 来运行 AOTAutograd，调用 AOTAutograd 并生成我们的反向图。更新后的 `inspect_backend` 定义了一个前向 (fw) 和反向 (bw) 编译器捕获函数，该函数读取 AOTAutograd 中的前向和反向图，打印降低的 ATen IR，并保存前向和反向图的 FX 图。

这将生成以下的前向和反向图。请注意，前向图也与我们在图 x 中看到的略有不同。例如，FX 图 IR 中的 `torch.sin(x)` 和我们原始代码中的已被替换为 `torch.ops.aten.sin.default()`。你可能会问，这个叫做 aten 的奇怪东西是什么，如果你还不熟悉的话。ATen 代表一个张量库，它是一个非常富有创意命名的低级库，具有 C++ 接口，实现了许多在 CPU 和 GPU 上运行的基本操作。

在急切模式操作中，你的 PyTorch 操作会被路由到这个库，然后调用适当的 CPU 或 GPU 实现。AOTAutograd 自动生成代码，将较高级的 PyTorch API 替换为前向和反向图的 ATen IR，你可以在下面的输出中看到：

![](../Images/06ff63252f4aecddd8d7f2010b65c823.png)

作者截图

你还可以看到，除了前向传递的输出，前向图还输出了一些额外的张量 [add, sin, cos, primals_1]。这些张量被保存用于反向传递的梯度计算。你也可以在之前分享的前向和反向传递的计算图中看到这一点。

# PyTorch 中不同类型的 IR 有哪些？

ATen IR 是我们在上一节讨论的 ATen 库所支持的操作符列表，你可以在 [ATen library here](https://pytorch.org/cppdocs/api/namespace_at.html) 查看实现的完整操作列表。PyTorch 中还有两个其他的 IR 概念你应该了解：1/ Core Aten IR 2/ Prims IR。Core Aten IR 是更广泛的 Aten IR 和 Prims IR 的一个子集，而 Prims IR 是 Core Aten IR 的一个更小的子集。假设你正在设计一个处理器并希望在你的硬件上支持 PyTorch 代码加速。要在硬件中支持完整的 PyTorch API 列表几乎是不可能的，因此你可以构建一个仅支持 Core Aten IR 或 Prims IR 中定义的较小基本操作符子集的编译器，并让 AOTAutograd 将复合操作符分解为核心操作符，正如我们将在下一节中看到的那样。

# ATen IR、Core ATen IR 和 Prims IR 之间有什么区别？

![](../Images/d93a56ae8fb97def11f795028aedf795.png)

作者截图

[Core Aten IR](https://pytorch.org/docs/stable/ir.html#core-aten-ir)（以前称为标准 Aten IR）是 Aten IR 的一个子集，可用于组合 Aten IR 中的所有其他操作符。针对特定硬件加速器的编译器可以专注于支持 Core Aten IR 并将其映射到其低级硬件 API。这使得为 PyTorch 添加硬件支持变得更容易，因为他们不必实现对完整 PyTorch API 的支持，而 PyTorch API 将继续随着更多抽象的增加而增长。

[Prims IR](https://pytorch.org/docs/stable/ir.html#prims-ir) 是 Core Aten IR 的一个更小的子集，它将 Core Aten IR 操作进一步分解为基本操作，使得针对特定硬件的编译器更容易支持 PyTorch。但将操作符分解为更低级别的操作将不可避免地导致性能下降，因为会增加额外的内存写入和函数调用开销。预期是硬件编译器可以将这些操作符融合回去，以支持硬件 API 并恢复性能。

尽管我们不需要进一步将我们的函数分解为 Core Aten IR 和 Prims IR，我将下面演示如何操作。

# （可选话题）分解为 Core Aten IR 和 Prims IR

如果你正在设计硬件或硬件编译器，在硬件中支持完整的 PyTorch API 列表几乎是不可能的，尤其是考虑到深度学习和 AI 发展的速度。但硬件设计师的优势在于，大多数深度学习功能可以映射到很少的基本数学操作中，而计算密集型的操作是矩阵-矩阵和矩阵-向量操作。像 PyTorch API 支持的复合操作符可以使用 AOTAutograd 分解为这些基本操作，如我们将在本节中讨论的。如果你不涉及底层硬件，你可以跳过本节。

你可以更新 AOTAutograd 函数以传递一个分解字典，该字典可以将 Aten IR 降级到 Core Aten IR 和 Prims IR。我只会在这里分享相关的代码片段和输出，因为你可以在 GitHub 上找到完整的笔记本。默认情况下，操作符不会被分解为 Core Aten IR 或 Prims IR，但你可以传递一个分解字典。

在下面的代码片段中，我将我们的函数 f 转换为损失函数 f_loss，通过将均方误差 (MSE) 的计算包含到我们的函数中。我这样做是为了演示 AOTAutograd 如何将 MSE 分解为其基本操作符。

![](../Images/d5b809b001a11fa4885d200666cf1ecd.png)

作者提供的截图

分解的输出是 mse_loss 被分解为更基本的操作：减法、幂(2)、均值。

![](../Images/7a98f1b51f979fbc1eaf146e26c068c4.png)

作者提供的截图

这是因为两个向量 x 和 y 之间的 MSE 或均方误差定义为以下公式，只需要这 3 个操作：减法，其中幂是逐元素操作。如果你为你的硬件编写编译器，你可能已经支持这 3 个操作，通过分解，你的 PyTorch 代码可以在不做进一步修改的情况下运行。

![](../Images/59f8eb7f7eb8b29efb52c939d8c8ca99.png)

你还可以在 FX 图形可视化中看到这一点

![](../Images/1f285f38f1e453c6c632bd556c7be40a.png)

作者提供的截图

现在让我们进一步将其分解为 Prims IR，这是 ~250 个操作符的一个更小的子集。同样，我只会在这里分享相关的代码片段和输出，因为你可以在 GitHub 上找到完整的笔记本。

![](../Images/cb738e3595a61790cd419b57d1953abc.png)

作者提供的截图

prim IR 分解的输出如下。所有标记为红色的 aten 操作都被替换或分解为使用绿色的 prim 操作符。

![](../Images/23c2cb563e551a01b80fb2b27af96c2b.png)

作者提供的截图

# 图优化：层和操作符融合以及 C++/GPU 代码生成

**讨论的 PyTorch 技术：** TorchInductor、OpenAI Triton（默认）其他编译器

在博客的最后一部分，我们将讨论使用 TorchInductor 进行操作符融合和自动代码生成。首先是一些基础知识：

## **深度学习优化编译器是什么？**

深度学习的优化编译器擅长发现代码中的性能差距，并通过将代码转换以减少诸如内存访问、内核启动、针对目标后端的数据布局优化等代码属性来解决这些差距。TorchInductor 是 torch.compile() 的默认优化编译器，可以使用 OpenAI Triton 为 GPU 生成优化内核，并使用 OpenMP pragma 指令为 CPU 生成优化内核。

## **深度学习中的操作符融合是什么？**

深度学习由许多基本操作组成，例如矩阵-矩阵和矩阵-向量乘法。在PyTorch的急切执行模式中，每个操作都会导致在硬件上进行单独的函数调用或内核启动。这会导致CPU启动内核的开销，并导致在内核启动之间进行更多的内存读写。像TorchInductor这样的深度学习优化编译器可以将多个操作融合为一个复合运算符，并为其生成低级GPU内核或C++/OpenMP代码。这样可以由于较少的内核启动和较少的内存读/写而实现更快的计算。

![](../Images/88439b04d463737c8ee388ef2abe8689.png)

作者提供的截图

前一节中从AOTAutograd输出的计算图由许多在FX图中表示的Aten运算符组成。TorchInductor优化不会改变图中的底层计算，只是通过运算符和层的融合对其进行重组，并且为其生成CPU或GPU代码。由于TorchInductor可以提前看到完整的前向和反向计算图，因此可以对不依赖彼此的操作进行乱序执行，并最大化硬件资源利用。

在底层，对于GPU目标，TorchInductor使用OpenAI的Triton生成融合的GPU内核。Triton本身是一个独立的基于Python的框架和编译器，用于编写优化的低级GPU代码，否则需使用CUDA C/C++编写。但唯一的区别是，TorchInductor将生成编译为低级PTX代码的Triton代码。

对于多核CPU目标，TorchInductor会生成C++代码，并插入OpenMP指示以生成并行内核。从PyTorch用户级别的角度来看，这是IR转换流程：

![](../Images/b798e58504966c9c4c5fb1ca8428aced.png)

当然，这是高级视图，我在这里省略了一些细节，并鼓励您阅读[TorchInductor论坛帖子](https://dev-discuss.pytorch.org/t/torchinductor-a-pytorch-native-compiler-with-define-by-run-ir-and-symbolic-shapes/747)和[OpenAI的Triton博文](https://openai.com/research/triton)。

现在我们将放弃在前一节中使用的虚拟编译器，并使用使用TorchInductor的完整PyTorch编译器栈。

注意，我已经传递了一个可选参数，启用了两个调试功能：

+   `trace.enabled`：生成中间代码以检查TorchInductor生成的代码

+   `trace.graph_enabled`：生成在运算符融合后的优化计算图可视化

对于我们的简单示例，TorchInductor能够将函数中的所有中间操作融合到一个自定义运算符中，您可以在下面看到这如何简化了前向和反向计算图。

![](../Images/498c56a774d5c1681ee51dc91320325e.png)

作者提供的截图

你一定好奇这个融合运算符在代码中是什么样子。融合运算符的代码是由 TorchInductor 自动根据目标设备 — CPU 或 GPU — 生成的，它不需要明确指定目标设备，可以根据数据和模型设备类型进行推断。

要查看生成的代码，你必须启用调试，使用 trace.enabled=True，这将创建一个名为 torch_compile_debug 的目录，包含调试信息。

前向和反向图代码的完整路径为：

+   `torch_compile_debug/run_<DATE_TIME_PID>/aot_torchinductor/model__XX_forward_XX/output_code.py`

+   `torch_compile_debug/run_<DATE_TIME_PID>/aot_torchinductor/model__XX_backward_XX/output_code.py`

如果你设置 device = ‘cuda’（假设你的计算机有 GPU 设备），那么在 forward 文件夹生成的代码将是 Open AI Triton。

![](../Images/0107bd207a6a3273b9627cf0b36ac5b4.png)

作者截图

如果你设置 device = ‘CPU’，那么生成的代码将是带有 OpenMP pragmas 的 C++ 代码。

![](../Images/9db2f9c42048666128a54202d4db2d0a.png)

作者截图

# 总结

深度学习编译器的内部机制复杂，有如瑞士表般精致。在本博文中，我希望给你提供一个关于这个主题的简明易懂的入门，并介绍这些技术如何驱动 PyTorch 2.0。

![](../Images/bb3bb197c0e1d2f8fe2c007bb6c5e1a0.png)

作者截图

1.  我从一个简单的 PyTorch 函数开始。

1.  我展示了 TorchDynamo 如何捕获图并在 FX IR 中表示它。

1.  我展示了 AOTAutograd 如何生成反向传递图，将 PyTorch 运算符降低为 Aten 运算符，并在 FX 图容器中表示它。

1.  我讨论了如何进一步将 Aten 运算符分解为 Core Aten IR 和 Prims IR，以减少其他编译器支持的运算符数量，而无需支持完整的 PyTorch API 列表。

1.  我展示了 TorchInductor 如何执行运算符融合，并为 CPU 和 GPU 目标生成优化的代码。

如果你跟随我，应该能够对以下问题提供高层次的回答：

+   什么是深度学习编译器？

+   当你调用 torch.compile() 时，PyTorch 2.0 编译器会做什么？

+   为什么我们需要提前进行前向和反向传递图？

+   PyTorch 中有哪些不同的中间表示（IR）？

+   ATen IR、Core ATen IR、Prims IR 之间有何区别？

+   什么是运算符融合，以及它为何重要？

# 感谢你阅读到最后！

如果你觉得这篇文章有趣，请考虑在 medium 上关注我，以便在我发布新文章时收到通知。也请查看我在 [medium](https://medium.com/@shashankprasanna) 上的其他博文，或者关注我的推特 ([@shshnkp](https://twitter.com/shshnkp))，[LinkedIn](https://www.linkedin.com/in/shashankprasanna/) 或在下方留言。如果你希望我写关于特定主题的文章，我很乐意听取你的建议！
