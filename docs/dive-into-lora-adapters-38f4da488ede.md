# 深入了解LoRA适配器

> 原文：[https://towardsdatascience.com/dive-into-lora-adapters-38f4da488ede?source=collection_archive---------0-----------------------#2023-08-25](https://towardsdatascience.com/dive-into-lora-adapters-38f4da488ede?source=collection_archive---------0-----------------------#2023-08-25)

## 探索参数高效微调（PEFT）：直观理解使用LoRA的微调

[](https://medium.com/@mkamp?source=post_page-----38f4da488ede--------------------------------)[![Mariano Kamp](../Images/d58d3321564409fba27c7c644fe5d813.png)](https://medium.com/@mkamp?source=post_page-----38f4da488ede--------------------------------)[](https://towardsdatascience.com/?source=post_page-----38f4da488ede--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----38f4da488ede--------------------------------) [Mariano Kamp](https://medium.com/@mkamp?source=post_page-----38f4da488ede--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F1ed8ca6eb79f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdive-into-lora-adapters-38f4da488ede&user=Mariano+Kamp&userId=1ed8ca6eb79f&source=post_page-1ed8ca6eb79f----38f4da488ede---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----38f4da488ede--------------------------------) · 14分钟阅读 · 2023年8月25日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F38f4da488ede&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdive-into-lora-adapters-38f4da488ede&user=Mariano+Kamp&userId=1ed8ca6eb79f&source=-----38f4da488ede---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F38f4da488ede&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fdive-into-lora-adapters-38f4da488ede&source=-----38f4da488ede---------------------bookmark_footer-----------)![](../Images/7ff47e06d3e0b926b8f949e6efa3e6e8.png)

大型语言模型（LLMs）已经在全球掀起了风暴。在过去的一年里，我们见证了它们的巨大进步，从非常狭窄和受限的应用，到现在能够进行流畅的多轮对话。

难道这些模型从提取性摘要——逐字复制源文本——到现在提供抽象性摘要的转变不令人惊叹吗？它们现在完全重写摘要，以匹配读者的风格偏好和现有知识。更令人惊奇的是，这些新模型不仅能够生成新代码，还能解释你现有的代码。真是令人着迷。

这些大型模型通常非常强大，即使在**零样本**或**少样本**的情况下查询，也能产生令人印象深刻的结果。尽管这允许快速实验并立即看到结果，但对于许多任务，通常之后会**微调**模型以实现最佳性能和效率。然而，**微调每一个参数**变得**不切实际**且效率低下。此外，考虑到模型的规模，我们是否有足够的标注数据来训练如此庞大的模型而不导致过拟合？

**参数高效微调（PEFT）**来拯救你：你现在可以在**仅调整少量权重**的情况下实现出色的性能。不必在多个机器上调整数十亿个参数，使得微调过程变得更加实际和经济可行。使用PEFT和量化可以让具有数十亿参数的大型模型在单个GPU上进行微调。

这个迷你系列适合那些希望探索PEFT和具体LoRA的经验丰富的机器学习从业者[2]：

+   在**第一篇文章**中，我们探讨了参数高效微调（PEFT）的动机。我们回顾了微调的原理及其作用，以及我们现有实践中可以保留、概括并以改进的方式应用的方面。我们将**亲自动手**，从零开始实现必要的内容，以创造一个**直观的理解**并展示我们选择探索的方法LoRA的简单性。

+   在[**第二篇文章**](/a-winding-road-to-parameter-efficiency-12448e64524d)中，我们现在深入寻找合适的超参数值，即我们回顾应用LoRA时相关的设计决策。在此过程中，我们建立了性能比较的基线，然后回顾可以使用LoRA调整的组件、它们的影响以及如何适当调整它们。

+   基于对单一任务进行训练和调整的模型，在**第三篇文章**中，我们将视角扩展到多个任务的调优。此外，部署方面呢？我们如何利用为单一任务训练的适配器相对小的占用空间，并实现热插拔机制，以便使用单一模型端点进行多个任务的推理。

+   在前三篇文章中，我们对使用PEFT进行训练、调优和部署有了直观的把握。过渡到**第四篇文章**时，我们将变得非常实际。我们将离开我们的教育模型，问道“到目前为止我们学到了什么，如何将其应用于实际场景？”然后使用Hugging Face提供的实现来实现我们的目标。这将包括使用QLoRA，它结合了LoRA和量化，以实现高效的GPU内存使用。

准备好深入了解了吗？今天，让我们从为什么这些方法有效开始。

# 关于预训练和微调的有效性

在他们的研究中，Aghajanyan等人[1]展示了神经网络层在预训练期间如何变化的两个有趣观察，使得微调更容易。这是广泛适用的，而不仅仅是针对特定的微调任务。

他们具体展示了预训练如何最小化表示的内在维度（ID）。以下两个图——取自他们的工作——说明了这一效果：

![](../Images/0db30d959f0e4397ebc0f3a3e01f4154.png)

内在维度在预训练期间逐渐减少（图像由Aghajanyan等人提供）

![](../Images/5761bc5ad66fc6757fc596b2c53efe85.png)

内在维度随着模型容量的增加而减少（图像由Aghajanyan等人提供）

作者没有对所有参数进行微调，而是用一个较小的、随机选择的参数子集来训练相应的模型。参数数量被选择以匹配完整微调的90%性能。这个实现90%性能所需的维度在上面的图中用两个y轴表示为`d90`。

第一张图显示，随着**预训练时长的增加**（x轴），`d90`下降，即在随后的微调中实现90%完整微调性能所需的参数数量减少。这本身就显示了预训练作为一种**压缩知识**的方法的有效性。

在第二张图中，我们还可以看到，随着**容量的增加**，达到`d90`所需的参数数量也下降了。这很有趣。这表明，更大的模型可以学习训练数据的更好表示——模型所见的世界——并创建在**任何**下游任务中易于使用的层次特征。

作者指出的一个具体例子是，RoBERTa Large（354M）的`d90`大约是207个参数。太棒了！

请在上图中找到这个例子，然后也查看一下**较小的**RoBERTa Base（123M）需要**更多**的参数才能达到90%的性能，这里是896。很有趣。

从我在这个话题上的讨论中，我了解到有几点值得明确指出：

+   我们在微调过程中利用了ID的效果，但上面的图表和数字都是关于预训练的。我们只是使用微调的数据来使最终的下游影响更具可感知性。

+   使用更大的模型不仅相对其大小具有更低的ID，而且**绝对**也是如此。当我们转向PEFT时，会看到类似的效果。

在[1]中，你会找到上述图示作为图2、图3，并且引用的结果取自表1。

总结来说，我们可以看到，在预训练过程中学到的表示压缩了模型学习的知识，使得使用这些更具语义的表示来微调下游模型变得更加容易。我们将在此基础上使用PEFT。只不过，我们不会随机选择要调整的参数并目标为90%的性能，而是使用更有针对性的方法来选择要训练的参数，并力求几乎匹配全微调的性能。令人兴奋！

# 什么需要调整？

我们已经确定可以使用非常少量的参数。但是哪一些？在模型的哪个位置？

我们将在下一篇文章中深入讨论更多细节。但为了开始我们的思考并框定问题，让我们现在反思两种一般方法：

![](../Images/e25a07dbccdde5316c7573a6b1923001.png)

基于**任务**的分配：基于对任务的理解，调整哪些参数最具影响力？

**基于任务：** 使用微调时，我们希望保留预训练中的知识，并避免“[灾难性遗忘](https://en.wikipedia.org/wiki/Catastrophic_interference)”。我们认识到，下游任务特定的学习应发生在微调模型的任务头部（这里是分类器）及其下方的直接层（如图中绿色所示），而在较低层和嵌入中我们希望保留我们关于语言使用的一般知识（如图中红色所示）。通常我们通过每层学习率来引导模型，或完全冻结下层。

这都基于我们对**模型学习**下游任务所需的关键知识的位置的理解，以及**预训练**中现有知识应保留的位置。

![](../Images/c44955b30610f89d94e3978ddd89335f.png)

基于**架构**元素的分配：哪些参数在微调中最有效、最有力？

**基于架构：** 相对而言，我们也可以审视我们架构的组件、它们的参数及其可能的影响。在上面的插图中，例如可以看到**LayerNorm**和**Biases**，这些容量较小，但遍布整个模型。这些位于中央位置以影响模型，但参数相对较少。

另一方面，我们还有**嵌入**的参数。这些参数虽然离任务较远，但靠近输入。而且嵌入中有大量参数。因此，如果我们想要高效，这些参数不会是我们进行任何形式的微调，包括PEFT的首选。

最后但同样重要的是，我们还有与transformers架构一起出现的大型线性模块，即**attention vectors**和**feed forward layers**。这些模块具有大量参数，我们可以决定在哪一层调整它们。

我们将在下一篇文章中更详细地回顾选择正确参数的过程。在本文中，无论我们如何切割和拆分问题，我们最终都会得到一组我们想要调整的参数。本文的其余部分将涉及一些线性模块。

# 使用适配器提高效率

我们希望更高效地调整线性模块，而不是调整所有参数。我们使用的方法是注入适配器。这些新模块相对较小，将放置在我们想要适配的模块之后。适配器可以修改线性模块的输出，即它们可以以有利于下游任务的方式细化预训练输出。

![](../Images/114fff28022f5f20e8f14430b66f0a8b.png)

可训练适配器和冻结模块

但这种方法存在一个问题。你能发现吗？这与待适配模块和适配器的相对大小有关。如果你看下面的插图，你会看到GPU内存。为了提高效率，我们将模型大小调整到**尽可能紧密地适配可用的GPU内存**。由于每一层宽度相同，Transformer架构特别容易实现这种调整，即使是降维后的头部也会再次加起来达到整个宽度。因此，我们可以根据Transformer组件的统一宽度选择批次大小。

但如果我们现在在较大的线性层之后注入非常小的适配器，就会出现问题。正如下图所示，我们的内存使用变得低效。

批次大小适合线性层的宽度，但现在我们有一个更小的适配器。因此，大部分GPU必须等待小适配器执行。这降低了GPU的利用率。而且，这比插图中显示的情况更糟，考虑到插图中的适配器区域应该约为1%，而插图中看起来接近20%。

![](../Images/26332bbca0928aa57bc861180739c3a3.png)

GPU利用效率低

解决这个问题的一种方法是并行化适配，并通过加法将它们连接起来，使两个路径都能贡献输出。这样，我们就没有了内存瓶颈，可以并行执行原始线性模块和适配器，避免了之前看到的间隙。

![](../Images/343ef4c27f22a382619945d0f4736aee.png)

更好

但即使并行执行也是一种额外的负担，与完全没有适配器相比。这对于训练来说是正确的，对于推理也是如此。这并不理想。

那么，这种适配器应该有多大？

![](../Images/1e8a2c58cea3b04859df80c5b4324ca5.png)

缺少什么？

我们将在第三篇文章中处理推理过程中的低效率问题。抢先看：一切都会好起来的——我们将把模块的权重与低秩矩阵的乘积合并。

回到这篇文章——让我们解决适配器的大小问题。

# 低秩矩阵作为适配器

让我们放大来看。

下面，你可以看到左侧灰色的原始线性模块和右侧橙色的适配器。为了使它们兼容，输入和输出必须匹配，以便我们可以使用相同的输入并行调用它们，然后将输出相加，类似于使用残差连接。因此，两个侧面的输入和输出维度必须匹配。

![](../Images/fc42e774c09626f35b3190062df1504f.png)

Adaptee与Adapter，每个都是全秩的

线性模块和适配器转换为两个矩阵。由于它们的维度匹配，**机械地**，我们现在有了兼容性。但由于适配器的大小与我们正在调整的模块一样大，我们没有变得更高效。我们需要一个**小而兼容**的适配器。

两个低秩矩阵的乘积符合我们的要求：

![](../Images/c757a4fd74076016954bc5b0fb3ec2a4.png)

适配器被分解为两个更低秩的矩阵

大矩阵被分解为两个低秩矩阵。但这些矩阵本身要小得多，`d_in x r`和`r x d_out`，特别是`r`远小于`d_in`和`d_out`。我们通常会看到`r`的值如1、2、4、16，而`d_in`和`d_out`则如768、1024、3072、4096。

让我们把这些全部结合起来：

![](../Images/bb0ce9840e7ca7a2f29d96c9b0f7a555.png)

在前向传播过程中应用LoRA

我们可以看到输入是一个单一的`x`。`x`随后与原始权重`W0`相乘。`W0`是预训练的权重。然后`x`与`A`和`B`相乘，最终将两个结果相加形成调整后的输出，这里称为`x'`。

存在不同的适配器实现，但在LoRA中，我们将其视为一个优化问题，并为特定的下游任务学习两个低秩矩阵`A`和`B`。学习这些较少的参数比学习`W0`中的所有参数更有效。

# 初始化

让我们快速转到一个相关的话题。你会如何初始化`A`和`B`？如果你随机初始化，考虑一下训练开始时会发生什么？

在每次前向传播中，我们会向适配模块的输出添加随机噪声，我们将不得不等待优化器一步步修正错误的初始化，这会导致微调开始时的不稳定。

为了减轻问题，我们通常使用较低的学习率、更小的初始化值或加热期，以限制这些错误参数的影响，从而避免过度不稳定权重。在LLAMA适配器[3]的论文中，作者引入了零门控：他们将适配器的门的值（与实际权重相乘的值）初始化为0，并在训练过程中逐渐增加其值。

一种替代方法是将`A`和`B`初始化为0。但这样你将无法打破对称性，在学习过程中所有参数可能会被视为一个参数。

LoRA实际做的事情非常优雅。一个矩阵`A`是随机初始化的，而另一个矩阵`B`是用0初始化的。因此，两个矩阵的乘积为0，但每个参数在反向传播过程中仍然可以单独求导。从0开始意味着归纳偏差是不做任何事情，除非改变权重会导致损失减少。因此，在训练开始时不会有不稳定性。不错！

![](../Images/021d30bc91f37166f876e6efd001eb3d.png)

LoRA适配器的初始化——什么都不做

# 代码中可能会是什么样子？

让我们查看一些代码摘录，以了解我们的小示例。你可以在[随附的笔记本](https://github.com/marianokamp/peft_lora/blob/main/1_lora_from_scratch.ipynb)中找到完整的代码，而[更完整的实现](https://github.com/marianokamp/peft_lora/blob/fd46890711dc9edd6219018186d8d2211dda43ee/src/lora.py#L33C2-L33C2)则在同一个仓库中，供后续文章使用。

我们首先设置一个适配器。我们传入一个对要调整的模块的引用，现在我们称之为`adaptee`。我们存储了对其原始`forward`方法的引用，并让`adaptee`的`forward`方法现在指向适配器的`forward`方法的实现。

```py
class LoRAAdapter(nn.Module):
    def __init__(self, 
                 adaptee, # <- module to be adapted
                 r):
        super().__init__()

        self.r = r
        self.adaptee = adaptee

        # Store a pointer to the original forward implementation 
        # of the module to be adapted.
        # Then point its forward method to this adapter module.
        self.orig_forward = adaptee.forward
        adaptee.forward = self.forward
        [..]
```

现在我们已经设置好了集成的机制，我们还初始化了低秩矩阵的参数。注意，我们初始化了一个矩阵为0，另一个矩阵为随机值：

```py
 [..]
        # Adding the weight matrices directly to the adaptee,
        # which makes it more practical to report the parameters,
        # and to remove it later.
        adaptee.lora_A = (nn.Parameter(torch.randn(adaptee.in_features, r)/
                          math.sqrt(adaptee.in_features)))
        adaptee.lora_B = nn.Parameter(torch.zeros(r, adaptee.out_features))
```

最后，在`LoRAAdapter`类中，我们有一个`forward`方法，它首先用输入`x`调用`adaptee`的`forward`方法。这是原始模块中执行的原始路径。但我们还将这个结果与我们调整过的分支中的结果相加，在那里我们将输入`x`与`A`和`B`进行矩阵乘法。

```py
def forward(self, x, *args, **kwargs):
  return (
    self.orig_forward(x, *args, **kwargs) +
    x @ self.adaptee.lora_A @ self.adaptee.lora_B
  )
```

这种简洁对我来说看起来很优雅。

还有更多可能有趣的细节，但最好是和代码一起解释。你可以在[随附的笔记本](https://github.com/marianokamp/peft_lora/blob/main/1_lora_from_scratch.ipynb)中找到这些：

+   如何首先冻结整个模型

+   如何解冻分类器。因为它是特定于我们的下游任务的，我们需要对其进行完全训练。

+   如何添加适配器；这些适配器都是活跃的，未被冻结。

+   审查模块矩阵的维度如何与两个低秩矩阵`A`和`B`相关。

+   当使用小的`r`值时，参数数量会减少多少？

下面的小摘录展示了原始模块`output.dense`的参数没有被训练（标记为`0`），但其LoRA矩阵是可训练的（标记为`1`），当然，模型的整体分类器（也标记为可训练的`1`）：

```py
[..]
roberta.encoder.layer.11.attention.output.LayerNorm.bias       0         768
roberta.encoder.layer.11.intermediate.dense.weight             0     2359296
roberta.encoder.layer.11.intermediate.dense.bias               0        3072
roberta.encoder.layer.11.output.dense.weight                   0     2359296
roberta.encoder.layer.11.output.dense.bias                     0         768
roberta.encoder.layer.11.output.dense.lora_A                   1       12288
roberta.encoder.layer.11.output.dense.lora_B                   1        3072
roberta.encoder.layer.11.output.LayerNorm.weight               0         768
roberta.encoder.layer.11.output.LayerNorm.bias                 0         768
classifier.dense.weight                                        1      589824
classifier.dense.bias                                          1         768
classifier.out_proj.weight                                     1        1536
classifier.out_proj.bias                                       1           2
[..]
Total parameters: 124,978,946, thereof learnable: 923,906 (0.7392%)
```

查看更多内容，请查看[笔记本](https://github.com/marianokamp/peft_lora/blob/main/1_lora_from_scratch.ipynb)。

# 来试试吧？

此外，你将看到一些在[笔记本](https://github.com/marianokamp/peft_lora/blob/main/1_lora_from_scratch.ipynb)中进行的测试，这些测试显示整个设置在机械层面上是有效的。

然后我们进行第一次实验并提交训练作业到SageMaker。我们对原始模型进行完整微调，然后如这里所述启用LoRA进行训练。

对我们的测试，我们在[sst-2数据集](https://huggingface.co/datasets/sst2) [5] 上训练RoBERTa Large [4]，`r`=2，调整所有层的`query`和`output`参数。我们使用`5e-5`和`4e-4`作为全微调和LoRA微调的学习率。

这是结果（更多内容请见[笔记本](https://github.com/marianokamp/peft_lora/blob/main/1_lora_from_scratch.ipynb)）：

```py
full-finetuning accuracy: 0.944
lora-finetuning accuracy: 0.933
```

所以这是……好，还是不好？是什么？首先，这清楚地表明整个设置在机械层面上是有效的——这很好。90%以上的准确度表明它工作得很好。

但效果如何？我们将这些数字与什么进行比较？这两个单独训练运行的代表性如何？我们只是运气好还是不好？LoRA的结果比传统方法更好吗？这不是很奇怪吗？我们调优传统方法的效果如何？

上述结果都不可靠。我们不知道在第二次运行时使用我们的超参数是否会产生类似的结果。此外，我们使用了通过半教育猜测选择的超参数。

当然，还有更好的方法。在[下一篇文章](/a-winding-road-to-parameter-efficiency-12448e64524d)中，我们将采用更严谨的方法来选择超参数，并将更系统地评估性能。

+   建立比较基准。

+   搜索基准和实验的良好超参数。

+   最重要的是：加深我们对LoRA方法和设计决策影响的理解，使我们的直觉以数据驱动的方式对齐。

在那之前，希望你阅读这篇文章时感到愉快。

感谢 [Constantin Gonzalez](https://www.linkedin.com/in/constantingonzalez/), [Ümit Yoldas](https://www.linkedin.com/in/%C3%BCmit-yoldas-23a908177/), [Valerio Perrone](https://www.linkedin.com/in/valerio-perrone/) 和 [Elina Lesyk](https://www.linkedin.com/in/elina-lesyk/) 在撰写本文期间提供的宝贵反馈。

所有图像均由作者提供，除非另有说明。

[1] [Armen Aghajanyan, Luke Zettlemoyer, Sonal Gupta. 内在维度解释了语言模型微调的有效性, 2020](https://aclanthology.org/2021.acl-long.568/)

[2] [Edward J. Hu, Yelong Shen, Phillip Wallis, Zeyuan Allen-Zhu, Yuanzhi Li, Shean Wang, Lu Wang, Weizhu Chen. LoRA: 大型语言模型的低秩适应, 2021](https://arxiv.org/abs/2106.09685)

[3] [Renrui Zhang, Jiaming Han, Chris Liu, Peng Gao, Aojun Zhou, Xiangfei Hu, Shilin Yan, Pan Lu, Hongsheng Li, Yu Qiao. LLaMA-Adapter: 高效微调语言模型的零初始化注意力, 2023](https://arxiv.org/abs/2303.16199)

[4] [尹汉·刘、迈尔·奥特、纳曼·戈亚尔、景飞·杜、曼达尔·乔希、丹琪·陈、奥梅尔·列维、迈克·刘易斯、卢克·泽特尔莫耶、维塞林·斯托扬诺夫。《RoBERTa：一种强健优化的BERT预训练方法》，2019](https://arxiv.org/abs/1907.11692)

[5] [理查德·索彻、亚历克斯·佩雷尔金、简·吴、杰森·庄、克里斯托弗·D·曼宁、安德鲁·吴和克里斯托弗·波茨。《用于情感树库的递归深度模型的语义组合性研究》，2013](https://aclanthology.org/D13-1170/)
