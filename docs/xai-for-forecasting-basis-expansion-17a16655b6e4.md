# XAI 预测：基础扩展

> 原文：[https://towardsdatascience.com/xai-for-forecasting-basis-expansion-17a16655b6e4?source=collection_archive---------6-----------------------#2023-03-24](https://towardsdatascience.com/xai-for-forecasting-basis-expansion-17a16655b6e4?source=collection_archive---------6-----------------------#2023-03-24)

![](../Images/fba0ad0a63160f4102e075123ef170a5.png)

图片来源：[理查德·霍尔瓦斯](https://unsplash.com/@orwhat?utm_source=medium&utm_medium=referral) 在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## NBEATS 和其他可解释的深度预测模型

[](https://medium.com/@upadhyan?source=post_page-----17a16655b6e4--------------------------------)[![Nakul Upadhya](../Images/336cb21272e9b1f098177adbde50e92e.png)](https://medium.com/@upadhyan?source=post_page-----17a16655b6e4--------------------------------)[](https://towardsdatascience.com/?source=post_page-----17a16655b6e4--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----17a16655b6e4--------------------------------) [Nakul Upadhya](https://medium.com/@upadhyan?source=post_page-----17a16655b6e4--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F4d9dddc62a80&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxai-for-forecasting-basis-expansion-17a16655b6e4&user=Nakul+Upadhya&userId=4d9dddc62a80&source=post_page-4d9dddc62a80----17a16655b6e4---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----17a16655b6e4--------------------------------) ·15分钟阅读·2023年3月24日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F17a16655b6e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxai-for-forecasting-basis-expansion-17a16655b6e4&user=Nakul+Upadhya&userId=4d9dddc62a80&source=-----17a16655b6e4---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F17a16655b6e4&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fxai-for-forecasting-basis-expansion-17a16655b6e4&source=-----17a16655b6e4---------------------bookmark_footer-----------)

预测是许多行业中的关键方面，从金融到供应链管理。多年来，研究人员探索了各种预测技术，从传统的时间序列方法到基于机器学习的模型。

近年来，预测人员转向深度学习，并在如长短期记忆 (LSTM) 网络和时间卷积网络 (CNNs) 等模型中获得了有希望的结果。2019 年之前，解决预测问题的主要方法是将传统统计方法（如 ARIMA）与深度学习相结合 [1]。然而，预测文献在 2020 年突破了这一点，并朝两个不同的方向发展。

第一个方向涉及开发利用注意力机制和变换器架构进行预测的方法。这一方法由 Li 等人首次提出的 LogSparse Transformer 开创，该方法调整了传统的自注意力机制，使其在 NLP 任务中对局部性更为敏感且使用更少的内存 [5]。此工作后来由 Autoformer [6]、Informer [7]、FEDFormer [8] 等模型进一步扩展和改进。这是一个蓬勃发展的领域，但最近的工作对这一方法提出了质疑。Zeng 等人最近质疑了自注意力机制在预测任务中的有效性，并进行了大量实验，显示注意力机制可能对时间表示不太有用 [9]（这些实验的总结可以在[这篇文章](https://medium.com/towards-data-science/transformers-lose-to-linear-models-902164ca5974)中找到）。此外，从可解释性 AI (XAI) 角度来看，这一方法也稍显不足。尽管这些模型都有可视化的注意力机制，但许多学者认为这可能无法解释，这也是一个活跃的辩论领域。Denis Vorotyntsev 发表了一篇[总结辩论的优秀文章](/is-attention-explanation-b609a7b0925c)，我强烈建议查看他的文章 [10]。

与基于注意力的变换器方法相比，解决预测问题的另一主要方向是由 Oreshkin 等人 [1] 在 2020 年提出的神经基础扩展分析方法，该方法使用 N-BEATS 架构。该方法涉及使用多个堆叠的深度全连接网络来迭代构建预测。网络不是直接预测时间序列，而是预测基础的权重。这种架构允许用户指定他们想要提取的时间序列组件，并且预测的重建性质增加了额外的可解释性。

在本文中，我旨在总结基础分析架构的完整机制，并展示自原始 N-BEATS 论文以来基础扩展方法的改进。具体而言，我将涵盖 N-BEATS 的工作原理 [1] 以及对 NBEATS 的两个改进：N-HiTS [2] 和 DEPTS [4]。

# N-BEATS

![](../Images/28208184fe703ee750bb312da4c19fdc.png)

图 1：Oreshkin 等人 2020 年的 N-BEATS 架构 [1]

N-BEATS 代表**神经基础扩展分析**（Neural Basis Expansion Analysis）用于**时间序列**（Time Series），是基础扩展架构的原始模型。因此，我们将花更多时间解释这一架构，相较于其他模型。

在开发 N-BEATS 时，作者的目标是通过创建一个不依赖于任何统计概念（如最大似然估计或传统自回归模型（例如 ARIMA））的预测模型来展示深度学习的力量，同时仍提供一些传统时间序列方法所提供的可解释性[1]。在追求这些特性的过程中，Oreshkin 等人不仅达成了他们的目标，还证明了他们的模型比当时的最先进技术更优秀。使 N-BEATS 成为强大模型的一些特性包括：

1.  优异的性能：在 M4 数据集（包含 100,000 个不同时间序列的数据集）上，N-BEATS 能够击败当时的顶尖表现者 ES-RNN [11]，平均超出 3%。

1.  大规模和多重预测：N-BEATS 用于生成预测的方法可以直接进行多步预测，避免了由于迭代预测生成（将预测结果回传到模型中）所带来的预测漂移问题。

1.  相对较快的训练：与 Transformer 对应模型相比，N-BEATS 仅包含 MLP 堆栈，并且在其架构中没有任何递归网络，使得训练过程更为简单。

1.  可解释性：当使用 N-BEATS 的可解释版本（N-BEATSi）时，用户可以清楚地看到时间序列成分的分解，如趋势和季节性。其他时间序列数据集也取得了令人惊叹的成功。

为了实现这些目标，N-BEATS 使用了一些巧妙的架构组件和技巧。

## 块与基础

块可能是 N-BEATS 架构中最重要的部分。

如图 1 中蓝色部分所示，块包含 1 个全连接网络堆栈，该堆栈将其输出传递到 2 个不同的线性层。我们可以将全连接层视为生成输入的编码。这两个线性层随后接收这个编码并将其投射到 2 组权重中：一组用于预测基础，一组用于回溯基础。在数学上，这组操作可以描述如下（这有一个 4 层全连接网络）：

![](../Images/a7851cca3f7db3cc8771c5b63cb1f937.png)

图片来源：Oreshkin 等人 2022

在这里，*x_l* 是输入到 *l-th* 块中的数据，而 thetas 是基础权重。

## **基础**

现在需要注意的是，当我们提到“基础”时，我们指的是线性代数中的意义。换句话说，基础是一组线性独立的向量，我们可以将其线性组合形成向量空间中的任何其他向量。在 N-BEATS 的情况下，线性组合的权重由神经网络定义。

通用架构遵循这一确切定义：

![](../Images/b15bb2d679bc5ccfb89d3fd4c5cf4398.png)

图片来源于 Oreshkin 等人 2022

其中 y 是由块 1 预测的时间序列部分。最终输出只是由用户定义的或作为可训练参数设置的一组基向量的加权和。然而，这种基向量版本不可解释，也不试图显式捕捉时间序列的特定成分。不过，通过更改使用的基方程，N-BEATS 可以非常容易地修改成更具解释性的版本。作者将这种新配置称为 N-BEATSi [1]。

第一个可解释的基向量是趋势基向量。这将基向量更改为一系列低次幂的多项式（例如 at+bt²+ct³），我们的网络预测每个多项式项的权重。

![](../Images/0dbc76c64300d01a08c880344e5d9b0f.png)

图片来源于 Oreshkin 等人 2022

第二个可解释的基向量是季节性基向量。与趋势基向量类似，网络预测一组方程的权重，但这一次它预测的是傅里叶级数的权重。

![](../Images/5ba2adb957fea254d43eee653c464e89.png)

图片来源于 Oreshkin 等人 2022

最终，网络实际上为我们的时间序列提供了一个回归方程，使用户能够理解时间序列的具体工作原理 [1]。

## **回溯预测与预测**

每个块不仅产生预测结果，还会产生“回溯预测”——对输入窗口的预测。回溯预测可以被视为一种过滤机制。当一个块产生预测结果时，它产生的回溯预测是时间序列中已经被分析的部分，后续的块不应该再捕捉这些信息，因为当前块已经捕捉到了这些信息 [1]。

在通用架构中，预测和回溯预测将由不同的网络产生不同的权重，我们依赖训练过程使它们相互对齐。这是不可避免的，因为预测和回溯预测的基向量维度不同。然而，由于可解释版本使用的方程依赖于时间步，因此在使用可解释基向量时，强烈建议让回溯预测和预测共享基向量权重 [1]。

## 双重残差堆叠

N-BEATS 不只是由 1 个块组成，而是由多个串联在一起的块组成，每个块解释时间序列的一个信号。允许这种行为的关键机制是双重残差堆叠。**简单来说：当前块的输入等同于前一个块的输入减去前一个块的回溯预测。此外，最终预测是所有块预测的总和 [1]。**

![](../Images/fecd120caa6fe309e5261a7b0044995a.png)

图片来源于 Oreshkin 等人 2022

在通用情况下，这种双重残差堆叠机制允许更平滑的梯度流动和更快的训练。此外，“有意义的部分预测的聚合”也提供了较高的可解释性，因为它帮助用户识别他们尝试预测的时间序列中的关键信号[1]。此外，通过检查所有块的回溯，用户可以查看是否有任何信号或模式被忽略或是否有过拟合，这大大有助于调试过程。

## 堆栈

为了更好地组织网络，架构还将各个块组织成堆栈。在每个堆栈中，所有块都共享相同类型的基础（通用、趋势或季节性）。这种组织不仅有助于信号分解过程，还可以提供更多的可解释性，因为现在我们可以直接查看堆栈级别的输出，而无需检查每个块。

要了解这些解释的具体样子，请查看 [PyTorch Forecasting 文档中的精彩示例](https://pytorch-forecasting.readthedocs.io/en/stable/tutorials/ar.html#Interpret-model)。

## 协变量

虽然原始的 N-BEATS 架构仅适用于单变量时间序列，但 Challu 等人[3] 引入了允许协变量的方法。

第一种方法是将协变量简单地展平并附加到每个块中的全连接层的输入上。每个回溯和预测仍然仅针对目标时间序列，残差堆叠机制仅适用于目标，而不适用于协变量。

第二种方法是首先生成协变量的编码，然后将其作为块的基础：

![](../Images/6e86dde1aab893c7c647b9f329b525bc.png)

图片来源于 Challu 等人 2023

# DEPTS

![](../Images/f7eccce5889922e30d548f5445b6bdfb.png)

DEPTS 架构来源于 Fan 等人 2022[4]

DEPTS 代表 **D**eep **E**xpansion **L**earning for **P**eriodic **T**ime **S**eries **F**orecasting，顾名思义，这是一种对 NBEATS 的改进，专注于提升原始模型的周期性/季节性预测能力 [4]。具体来说，DEPTS 在两个方面改变了 NBEATS 的功能：

1.  虽然 NBEATS/NBEATSi 可以通过傅里叶基来处理周期性，但这种方法难以考虑超出回溯窗口的大周期行为，而 DEPTS 更好地处理这种情况 [4]。

1.  NBEATS/NBEATSi 只能考虑加法季节性（因为它是逐步构建预测的）。DEPTS 也能处理乘法季节性 [4]。

## 周期性模块与周期性上下文

DEPTS 用来实现上述两个特性的第一个机制是其专用的周期性模块，该模块接受回溯窗口和预测范围的时间步长，并生成由余弦序列定义的季节性上下文向量 [4]：

![](../Images/26df5a1df036784e47802810236fa19e.png)

图片来源于 Fan 等人 2022[4]

在这个模块中，*A*、*F* 和 *P* 都是可训练的参数。**通过允许网络学习这些参数并在预测中共享它们，模型可以直接学习全局周期模式，而无需像 NBEATS 模型那样基于输入数据重新推断模式** [4]。

然而，这一学习过程中的一个棘手之处在于，在尝试学习 *A*、*F* 和 *P* 时，模型很容易陷入局部最小值。为了解决这个问题，会进行预学习优化以初始化参数值。注意：在这个方程中，*phi* 代表可学习参数的集合，*M* 是一个与生成函数中的每一项相乘的二进制向量（如果选择该项则为 1，否则为 0） [4]。

![](../Images/b00eb1396b0f913808f26b915e971ace.png)

图片来源于 Fan 等人 2022 [4]

简单来说，在训练网络之前，先在训练集上进行参数的预拟合，然后选择 *J* 个最佳频率（用户选择 *J*）。为了避免解决整个优化问题，作者建议进行离散余弦变换，并选择具有最高幅度的 *J* 项 [4]。

## 周期性块和周期性残差

周期性模块的全部目的是生成初始周期性上下文向量 *z*。这些上下文向量被传递到周期性块 [4]。

![](../Images/30e265fb31cfea590f09e645433525b3.png)

DEPTS 周期性块架构来自 Fan 等人 2022[4]

简而言之，周期性块接受周期性上下文向量（实际上是时间序列的季节性成分），并通过一个非常小的子网络，生成类似于传统 NBEATS 块的回溯和预测。**这处理了周期性模块处理的加性季节性，并将其处理为应对非线性季节性** [4]**。**

这些周期性回溯和预测是如何使用的？深入探讨 DEPTS 的一层可以给我们更多的见解。注意在这个图中，*x* 是目标序列 [4]。

![](../Images/a6325dca35ace00df886ec9ec3216812.png)

DEPTS 层架构来自 Fan 等人 2022[4]

一旦模型生成了预测向量回溯和预测，回溯将从局部块的输入中减去，预测则添加到整体模型预测中。局部块与 NBEATS 中的块相同，执行相同的操作，并使用相同的双重残差堆叠机制。DEPTS 还使用双重残差堆叠机制来处理上下文向量，我们从周期性上下文向量中去除解释的季节性，并将局部块输出和周期性上下文向量传递到下一层 [4]。

所有这些机制结合在一起使 DEPTS 成为一个周期性预测的强大工具 [4]。

## 可解释性

像NBEATS一样，DEPTS保持了NBEATS的许多解释优势，即预测的聚合。然而，DEPTS还提供了对全球周期模式的观察，这是NBEATS所没有的。通过运行周期性模块，可以轻松可视化全球周期模式。此外，通过查看“剩余”模式，可以检查最终的周期上下文向量，以了解局部预测如何可能偏离全球模式。

# N-HiTS

![](../Images/e19bf2949152a6fdb6359ca12abb44cc.png)

N-HiTS架构来自Challu等人2022[2]

NBEATS的第二大改进是N-HiTS或**N**eural **H**ierarchical **I**nterpolation for **T**ime Series **F**orecasting [2]。虽然DEPTS引入了专门的周期性预测机制，N-HiTS则专注于更好地构建预测聚合过程，并提供了两个主要优势：

1.  通过层次化的预测聚合方法，N-HiTS在识别和提取信号方面比N-BEATS在大多数情况下更为出色[2]。

1.  通过插值机制，N-HiTS的内存使用量比NBEATS小得多，使得训练变得更加容易[2]。

这一切都得益于层次插值机制，它由两个部分组成：多速率信号采样和插值[2]。

## 多速率信号采样

![](../Images/8304f5648b9160236c9d42ce723cad26.png)

N-HiTS块架构来自Challu等人2022[2]

第一个主要机制是多速率信号采样。这非常简单（这使得它更加美丽）：在每个块的开头添加一个MaxPool层，该层预处理块输入并有效地平滑它[2]。这为什么有用？好吧，**我们可以在每个块中改变MaxPool层的内核大小，有效捕捉不同大小的信号[2]。** 此外，MaxPool还通过提供平滑处理，使模型对噪声的敏感性降低，本身就是一个强大但简单的工具[2]。

## 插值

多速率采样非常强大，但与插值机制结合使用时效果更佳。

回到原始的通用N-BEATS块架构，每个块生成两组权重：回溯权重集和预测权重集。通常，生成的权重数量等于窗口*L*的长度（用于回溯）或预测*H*的长度[1]。然而，这种方法存在一些问题。首先，预测所有权重可能导致生成的预测更具波动性和噪声[2]。此外，对于较大的预测范围，这可能导致极高的内存使用[2]。

N-HiTS 选择应用插值，其中预测的参数数量等于 *r*H*（以及回溯的 *r*L*），其中 r 是表现比率 [2]。更高的表现比率会导致更多的参数被预测。然后可以通过任何插值方法填补缺失的权重。作者测试了线性插值、最近邻插值和三次插值，但也可以使用任何自定义方法。

通过仅预测少量的权重，与 N-BEATS 相比，N-HiTS 的内存使用极其少，使其成为一个更轻量级的模型。

> 注意：据我所知，作者尚未提供可解释基函数的插值机制，因此 N-HiTS 限于通用基函数。

## 分层插值

然后，作者将这种权重插值与上述多速率信号采样结合起来，创建了分层插值机制。**在分层插值中，块的表现比率和 MaxPool 核心大小在各个块之间是反向变化的** [2]**。** 任何具有高表现比率的块也使用小的核心大小。任何具有低表现比率的块使用大的核心大小。这两个参数的同步正是使 N-HiTS 能够捕捉不同幅度信号的原因，如下图所示 [2]：

![](../Images/2433c91bab87ade4a1f294f5f3708dcc.png)

线性插值（左）与非插值（右）。图源自 Challu 等人 2022 [2]

直观上，这也有意义：如果对函数应用了强烈平滑（即大核心大小 MaxPool），则需要更少的参数来捕捉输入的行为，反之亦然。

分层插值还为 NBEATS 的通用形式提供了更多的可解释性。用户可以查看堆叠输出，并理解哪个在建模更大的模式，哪个在捕捉小的噪声。虽然 N-HiTS 无法使用可解释基方程，但可以认为 N-HiTS 不需要这些方程仍然可以具有可解释性。

# 结论

当 N-BEATS 出现时，它改变了深度预测领域，并催生了一个新的预测分支。突然间，我们可以从一个相对简单的模型中获得高度准确的预测，该模型可以端到端地训练。N-HiTS 和 DEPTS 然后能够在 NBEATS 的基础上进行扩展，增加了更多功能，增强了基扩展方法的特定方面。值得注意的是，N-HiTS 的分层插值与 DEPTS 的周期性模块/块/上下文向量也可以一起使用，创建一个更强大的模型。

还有一些工作将注意力机制应用于这些基础扩展方法，主要是 Jiang 等人提出的 DeepFS 模型 [12]，该模型使用了一个注意力层（改编自 [7]）来生成编码，然后将该编码传递到类似周期性 N-BEATS 块的模块中，以及传递到一个前馈堆栈中。尽管这可以被视为一种改进，但我在本文中没有详细描述此模型，因为正如 [9] 所示，时间序列预测中注意力机制的益处仍然存在争议。

关于基础扩展是否可解释也存在一些争论。如果最终我们的模型产生了40个不同的方程，这是否能被解释？此外，尽管我们可以识别这些模型所识别的所有信号，但确定这些信号的机制仍然相对不透明，我们可能不知道为什么模型选择接收或忽略某个信号。当将这些模型适应到使用协变量时间序列时，可解释性也会降低。

尽管存在这些缺陷，但本文讨论的所有模型在表现上始终表现出强大的性能，并为预测领域的更多创新铺平了道路。

# 资源和参考文献

# 资源和参考文献

1.  N-BEATS 和 N-HiTS 可以通过 [Darts](https://unit8co.github.io/darts/index.html) 和 [Pytorch Forecasting](https://pytorch-forecasting.readthedocs.io/en/stable/models.html) 这两个高质量的 Python 预测包来使用。

1.  [DEPTS 的源代码](https://github.com/weifantt/DEPTS)

1.  [N-HiTS 的源代码](https://github.com/cchallu/n-hits)

1.  [NBEATSx 的源代码](https://github.com/cchallu/nbeatsx)

1.  [NBEATS 的源代码](https://github.com/ServiceNow/N-BEATS)

## 参考文献

[1] B.N. Oreshkin, D. Carpow, N. Chapados, Y. Bengio. [N-BEATS: 神经基础扩展分析用于可解释的时间序列预测](https://arxiv.org/abs/1905.10437) (2020). 第八届国际学习表征会议。

[2] C. Challu, K.G. Olivares, B.N. Oreshkin, F. Garza, M. Mergenthaler-Canseco, A. Dubrawski. [N-HiTS: 神经层次插值用于时间序列预测](https://arxiv.org/abs/2201.12886) (2022). 第三十七届 AAAI 人工智能会议。

[3] K.G. Olivares, C. Challu, G. Marcjasz, R. Weron, A. Dubrawski. [带外生变量的神经基础扩展分析：使用 NBEATSx 预测电力价格](https://www.sciencedirect.com/science/article/pii/S0169207022000413) (2023). 国际预测期刊。

[4] W. Fan, S. Zheng, X. Yi, W. Cao, Y. Fu, J. Bian, T-Y. Liu. [DEPTS: 深度扩展学习用于周期时间序列预测](https://arxiv.org/abs/2203.07681) (2022). 第十届国际学习表征会议。

[5] S. Li, X. Jin, Y. Xuan, X. Zhou, W. Chen, Y. Wang, X. Yan. [增强变换器在时间序列预测中的局部性并突破记忆瓶颈](https://proceedings.neurips.cc/paper/2019/hash/6775a0635c302542da2c32aa19d86be0-Abstract.html) (2019). 神经信息处理系统大会第32届

[6] H. Wu, J. Xu, J. Wang, M. Long. [Autoformer：具有自相关的分解变换器用于长期序列预测](https://arxiv.org/abs/2106.13008) (2021). 神经信息处理系统大会第34届

[7] H. Zhou, S. Zhang, J. Peng, S. Zhang, J. Li, H. Xiong, W. Zhang. [Informer：超越高效变换器用于长序列时间序列预测](https://arxiv.org/abs/2012.07436) (2021). 第三十五届AAAI人工智能大会，虚拟会议

[8] T. Zhou, Z. Ma, Q. Wen, X. Wang, L. Sun, R. Jin. [EDformer：用于长期序列预测的频率增强分解变换器](https://arxiv.org/abs/2201.12740) (2022). 第39届国际机器学习会议

[9] A. Zeng, M. Chen, L. Zhang, Q. Xu. [变换器对时间序列预测是否有效？](https://arxiv.org/abs/2205.13504) (2022). 第三十七届AAAI人工智能大会

[10] D. Vorotyntsev. [注意力是解释吗？](/is-attention-explanation-b609a7b0925c) (2022). 数据科学前沿

[12] S. Jiang, T. Syed, X. Zhy, J. Levy, B. Aronchik, Y. Sun. [桥接自注意力和时间序列分解以进行周期性预测](https://www.amazon.science/publications/bridging-self-attention-and-time-series-decomposition-for-periodic-forecasting) (2022). 第31届ACM国际信息与知识管理会议

[11]S. Smyl, J. Ranganathan, A Pasqua. [M4预测竞赛：介绍一种新的混合ES-RNN模型](https://eng.uber.com/m4-forecasting-competition/) (2018). 优步研究
