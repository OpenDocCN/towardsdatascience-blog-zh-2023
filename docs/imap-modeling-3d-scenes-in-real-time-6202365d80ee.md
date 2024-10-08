# iMAP：实时建模 3D 场景

> 原文：[`towardsdatascience.com/imap-modeling-3d-scenes-in-real-time-6202365d80ee`](https://towardsdatascience.com/imap-modeling-3d-scenes-in-real-time-6202365d80ee)

## 使用手持 RGB-D 相机学习 3D 环境

[](https://wolfecameron.medium.com/?source=post_page-----6202365d80ee--------------------------------)![Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6202365d80ee--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6202365d80ee--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----6202365d80ee--------------------------------) [Cameron R. Wolfe, Ph.D.](https://wolfecameron.medium.com/?source=post_page-----6202365d80ee--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----6202365d80ee--------------------------------) ·15 分钟阅读·2023 年 5 月 12 日

--

![](img/75c66b8fbe2eb3a7bc26faba5f1ea272.png)

（照片由[Brett Zeck](https://unsplash.com/@iambrettzeck?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)提供，来源于[Unsplash](https://unsplash.com/s/photos/map?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText)）

到目前为止，我们只见过用于建模 3D 场景的离线方法（例如，[NeRF](https://cameronrwolfe.substack.com/p/understanding-nerfs)、[SRNs](https://cameronrwolfe.substack.com/p/scene-representation-networks)、[DeepSDF](https://cameronrwolfe.substack.com/p/3d-generative-modeling-with-deepsdf) [2, 3, 4]）。尽管这些方法的表现令人印象深刻，但它们需要数天甚至数周的计算时间来训练底层的神经网络。例如，NeRFs 仅用于表示单个场景的训练时间接近两天。而且，使用神经网络评估新的场景视角也可能相当昂贵！鉴于此，我们可能会想知道是否有可能更快地学习场景表示。

[1] 中探讨了这个问题，提出了 iMAP，一个用于实时表示场景和定位（即跟踪设备姿态）设备的系统。为了理解这意味着什么，考虑一个在场景中移动并捕捉周围环境的相机。iMAP 的任务是 *(i)* 获取这些数据，*(ii)* 建立被观察场景的 3D 表示，并且 (iii) 推断相机（即设备）在捕捉场景时的位置和方向！

iMAP 采用了一种与 NeRF [2] 非常相似的方案，但有一些不同之处：

1.  它基于 RGB-D 数据。

1.  假设有一个流媒体设置。

因此，模型接收深度和颜色信息作为输入。此外，学习过程从完全随机的初始化开始，iMAP 必须实时学习新的 RGB-D 图像。鉴于这种设置，iMAP 被期望*(i)* 模拟场景和 *(ii)* 预测每个输入图像的 RGB-D 相机姿态（即，先前的方法将姿态信息作为输入！）。尽管训练设置非常困难，iMAP 仍然可以实时学习整个房间的 3D 表示！

![](img/9a4ab5b3928c2404d5db5e25b4f3a239.png)

（来自 [1]）

**为什么这篇论文很重要？** 这篇文章是我关于 3D 形状和场景深度学习系列的一部分。该领域最近被 NeRF [2] 的提议所革新。通过 NeRF 表示，我们可以生成场景的任意数量的合成视点，甚至生成相关对象的 3D 表示；见下文。

iMAP 在 NeRF 之后稍晚提出，它能够在不需要几天训练时间的情况下产生高质量的场景表示。与 NeRF 相比，iMAP 以一种廉价、即刻的方式进行学习，并且表现相对较好。

# 背景

我们在本系列的先前概述中看到了一些重要的背景概念，这些概念将在此处相关：

+   前馈神经网络 [[link](https://cameronrwolfe.substack.com/i/94634004/feed-forward-neural-networks)]

+   表示 3D 形状 [[link](https://cameronrwolfe.substack.com/i/94634004/representing-d-shapes)]

+   相机姿态 [[link](https://cameronrwolfe.substack.com/i/97472888/background)]（滚动到“相机视点”子标题）

+   视频中的“帧”是什么？ [[link](https://cameronrwolfe.substack.com/i/73746328/how-is-video-data-structured)]

为了了解 iMAP 所需的所有背景，我们需要快速介绍 SLAM 系统和在线学习的概念。

# 什么是 SLAM？

我们见过的先前场景表示方法使用深度神经网络通过从 3D 空间的可用观察数据（例如，点云数据、图像等）中学习来形成对底层形状或场景的隐式表示。iMAP 有点不同，因为它是一个*同时定位与映射*（SLAM）系统。这意味着 iMAP 执行两个任务：

1.  *定位*：跟踪捕捉底层场景的相机的位置和姿态。

1.  *映射*：形成对底层场景的表示。

我们见过的大多数先前技术仅执行映射，但 iMAP 通过同时执行定位而超越了这些技术。即，底层场景的视点实时传递给 iMAP 系统，该系统既映射底层场景，又预测相机在场景中的轨迹。

![](img/f89852bd7df7ba16c93485b2b9318a50.png)

（来自 [1]）

SLAM 系统的输出，如上所示，有两个组成部分。生成了场景的 3D 表示。在这个表示中，我们可以看到相机在捕捉场景时的轨迹，通过黄色线（相机位置）和相关的 3D 边界框（相机姿态）表示。

**这有什么不同？** 除了对输入数据预测相机姿态之外，SLAM 系统与我们目前见到的其他方法不同，主要在于它们接收数据的方式。也就是说，以前的方法中我们 *(i)* 获取一组场景的图像，并 *(ii)* 在这些图像上训练一个神经网络来建模场景。而 SLAM 系统则以流式的方式接收数据。当系统接收到新的图像时，它必须处理这些图像，预测一个姿态，并实时更新其基础场景表示。

# 在线学习

![](img/a3556729b4c82efa4e65678dc2753caa.png)

离线训练过程（由作者创建）

通常，深度神经网络是以离线方式进行训练的。我们有一个大的、静态的训练数据集，并允许我们的神经网络对这个数据集进行多次训练（或训练周期）；见上文。但，*当我们的数据集不是静态的* 时会发生什么？例如，我们可能会实时接收新数据或纠正应用于现有数据的标签。

![](img/d894978fbc726e88dcf7d7c195e78c15.png)

在线和离线训练设置的术语（由作者创建）

我们通常将这种设置称为“在线学习”，这指的是我们按顺序接收新的数据以训练神经网络。

存在许多不同类型的在线学习；见上文。例如，增量学习假设神经网络按顺序接收新的数据批次，而流式学习则要求网络一次接收一个样本。所有形式的在线学习都假设数据是在一次通过中学习的——我们不能在接收到新数据后“回顾”旧数据。

**灾难性遗忘。** 在线学习被认为比离线学习更困难，因为在训练过程中我们从未能够访问完整的数据集。相反，我们必须在顺序提供的小数据子集上进行学习。如果接收到的数据是非[独立同分布的](https://en.wikipedia.org/wiki/Independent_and_identically_distributed_random_variables)，我们的神经网络可能会遭受[灾难性遗忘](https://cameronrwolfe.substack.com/i/73746325/catastrophic-forgetting)。如[6]中所讨论的，灾难性遗忘指的是神经网络在从新数据中学习时，完全遗忘了较旧的数据。

![](img/934f35816d31dceebcdce8012a461a10.png)

灾难性遗忘的描述（由作者创建）

例如，如果我们正在学习分类猫和狗，也许我们会依次收到大量只有猫的图片的数据（例如，这可能发生在某人的 IoT 门铃摄像头上！）。在这种情况下，神经网络会长时间只学习猫的图片，导致它灾难性地忘记狗的概念。如果输入的数据在猫和狗之间均匀分布，这将不是问题。遗忘发生是因为输入数据是非独立同分布的，而在线学习技术旨在避免这种遗忘。

**重放缓冲区。** 避免灾难性遗忘的一种流行方法是通过重放缓冲区。从高层次来看，重放缓冲区只存储过去观察到的数据缓存。然后，当网络在新数据上更新时，我们也可以从重放缓冲区中抽取一些数据。这样，我们就能获得神经网络曾见过的数据的均等采样；见下文。

![](img/30a4a16366bce6c5def67d01d3beefd7.png)

重放机制的基本示意图（由作者创建）

所有在线学习技术的共同目标是通过最小化灾难性遗忘的影响来最大化神经网络的性能。虽然重放缓冲区被广泛使用、简单且非常有效，但仍然存在许多其他技术——在线学习是深度学习社区中的一个活跃研究领域。关于一些现有技术的概述，我建议阅读我下面的总结！

+   在线学习技术概述 [[link](https://cameronrwolfe.substack.com/p/a-broad-and-practical-exposition-of-online-learning-techniques-a4cbc300dcd4)]

+   流式学习技术概述 [[link](https://cameronrwolfe.substack.com/p/how-to-train-deep-neural-networks-over-data-streams-fdab15704e66)]

**我们为什么要关注在线学习？** iMAP 是一个 SLAM 系统。根本上，SLAM 系统与在线学习有很大关系，因为它们需要接收场景图像流并提供跟踪和映射结果。值得注意的是，iMAP 是一种基于在线学习的技术，依靠重放缓冲区实时表示和跟踪基础场景！

# iMAP 如何运作？

在首次了解 iMAP 后，它可能看起来好得难以置信。首先，iMAP 解决了比之前的工作更难的问题——相机位姿信息是从 RGB-D 数据中预测的，而不是给定的。然后，我们还必须实时学习整个场景表示，*而不是训练几天*？这简直不可能……

![](img/2d86eaadd9f577107934fcf2f66e70cb.png)

(来源于 [1])

但确实是这样！iMAP 通过包括两个部分的处理管道完成所有这些：

1.  *跟踪*：预测相机在场景中移动时的位置和姿态。

1.  *映射*：学习 3D 场景表示。

这两个组件并行运行，因为 RGB-D 相机从不同视角捕捉底层场景。值得注意的是，跟踪必须运行得非常快，因为我们尝试在新数据到来时主动定位相机。映射组件也是并行进行的，但它只在对表示底层场景真正重要的关键帧上操作，这使得 iMAP 能够实时学习。让我们深入了解一些细节吧！

![](img/f502f9fb75ba9e9b63b5b350b43f7b3c.png)

iMAP 网络架构（作者创建）

**网络。** 理解 iMAP 的第一步是了解其基于的神经网络。与大多数先前的工作一样，iMAP 使用前馈网络架构。该网络将 3D 坐标作为输入，并产生 RGB 颜色和体积密度（即，捕捉不透明度）作为输出；见上文。

值得注意的是，iMAP 的网络不将 [视角](https://cameronrwolfe.substack.com/i/97915766/modeling-nerfs)作为输入，正如 NeRF [2] 中那样，因为它不尝试建模视角相关效应（例如反射）。与 NeRF [2] 相似，iMAP 在将坐标作为输入之前将其转换为更高维的位置信息嵌入，遵循 [5] 的方法；见下文。

![](img/444726a8874e6203d1e9b3e393eadce1.png)

iMAP 网络架构与位置信息嵌入（作者创建）

**关于渲染的快速说明。** 假设可以访问相机姿态（我们稍后会学习 iMAP 如何预测这些姿态），我们可以在许多不同的空间位置上评估这个网络，然后使用类似于 [Ray Marching](https://michaelwalczyk.com/blog-ray-marching.html) 的方法将颜色和深度信息聚合成底层场景的渲染图像。 [1] 的渲染方法类似于 [NeRF](https://www.heavy.ai/technical-glossary/volume-rendering) 的方法，但我们希望在我们的渲染中包括深度信息（即，渲染 RGB-D 图像）。这使得渲染过程略有不同，但基本思路是相同的，整个过程仍然是可微分的。

**我们如何优化这个？** iMAP 正在尝试学习：

+   前馈场景网络的参数。

+   来自 RGB-D 相机的输入帧的相机姿态。

为了训练 iMAP 系统以准确预测这些信息，我们依赖两种类型的损失度量：

1.  *光度学*：RGB 像素值中的误差。

1.  *几何学*：深度信息中的误差。

这些误差是通过使用 iMAP 渲染底层场景的 RGB-D 视图计算的，然后将此渲染结果与从相机捕获的地面真实样本进行比较。为了使这个比较更高效，我们仅考虑 RGB-D 图像中的一部分像素；见下文。

![](img/6df94e85f80eb6fd2e4e975db1ad92ad.png)

（来自 [1]）

为了共同优化光度和几何误差，我们只是通过加权和将它们结合起来；见下文。

![](img/18c21e536405bda35532af40eda086b3.png)

（来自 [1]）

要用 iMAP 渲染图像，我们必须：

1.  使用场景网络来获取颜色和几何信息。

1.  推断正确的相机姿态。

1.  基于这些综合信息进行渲染和 RGB-D 视点的生成。

所有这些步骤都是可微的，因此我们可以使用诸如[随机梯度下降](https://en.wikipedia.org/wiki/Stochastic_gradient_descent)之类的技术来优化光度和几何损失，从而训练系统生成与真实 RGB-D 图像紧密匹配的渲染结果。

**跟踪和映射。** 在 iMAP 中，跟踪的目标是学习正在主动捕捉底层场景的相机姿态。由于我们必须对来自相机的每个输入 RGB-D 帧进行此操作，跟踪必须高效。iMAP 通过*(i)* 冻结场景网络和*(ii)* 在固定网络的基础上求解当前 RGB-D 帧的最佳姿态来处理跟踪。这只是我们尽可能高效生成的相机姿态的初步估计——我们可能会在后续进行姿态精细化。

映射的目标是联合优化场景网络和相机姿态，从而隐式地创建场景的准确表示。尝试在所有输入的 RGB-D 相机帧上进行这一操作将会非常昂贵。因此，我们基于重要性（即是否捕捉到场景的“新”部分）维护一组关键帧。在这组关键帧中，我们精细化估计的相机姿态，并训练场景网络以生成与所选关键帧紧密匹配的渲染图像。

![](img/17243f9e7e07aaf6506bf64cbbf7f770.png)

(来源于 [1])

**综合所有内容。** 上图展示了完整的 iMAP 框架。对每个输入帧进行跟踪，以预测相机姿态信息。在此过程中，前馈网络保持冻结，以提高跟踪效率。如果一个帧被选为关键帧，它会被添加到关键帧集合中，并对这些帧的姿态进行精细化，并用于训练场景网络。

![](img/195bcd1deec2a61750e5790438cfcbbf.png)

(来源于 [1])

为了提高效率，映射过程与跟踪过程并行运行，并仅从关键帧集合中采样训练数据，该集合比总的输入图像数量要小得多。此外，损失只在一个小的像素子集上计算，这些像素子集通过层次化策略（即主动采样）来选择，该策略识别图像中具有最高损失值的区域（即场景中的密集或详细区域），并优先在这些区域进行采样；见上文。

**在线学习**。请记住，iMAP 初始时是随机初始化的。当 RGB-D 相机在场景中移动时，iMAP *(i)* 通过跟踪模块跟踪相机，并 *(ii)* 开始选择关键帧以更新映射模块。这组关键帧作为 iMAP 的重播缓冲区。也就是说，前馈场景网络（及相关相机姿态）通过聚合相关的关键帧并在跟踪过程中并行更新这些数据，以在线方式进行训练。映射过程中的每次更新考虑五个帧：三个随机关键帧、最新的关键帧以及当前的 RGB-D 帧。

![](img/f4165fe3b76b2a4adb41ad27d8c4eb5a.png)

（来自 [1]）

前馈场景网络不会遭受灾难性遗忘，因为它从多样的关键帧集合中采样训练数据，而不是仅仅在接收到的数据流上进行训练。然而，与普通的重播机制不同，iMAP 遵循特定的策略，从重播缓冲区中采样训练数据。特别是，具有较高损失值的关键帧被优先考虑（即，关键帧主动采样）；见上文。

# 它真的有效吗？

iMAP 在真实世界（即来自手持 RGB-D 相机）和合成场景数据集（例如，[Replica](https://github.com/facebookresearch/Replica-Dataset) 数据集）上进行了评估和与几个传统 SLAM 基线的比较。值得注意的是，iMAP 以 10 Hz 的频率处理每个接收的帧。为了评估，可以通过在 [体素网格](https://cameronrwolfe.substack.com/i/94634004/representing-d-shapes) 上查询神经网络并运行 [marching cubes](https://graphics.stanford.edu/~mdfisher/MarchingCubes.html#:~:text=Marching%20cubes%20is%20a%20simple,a%20region%20of%20the%20function.) 算法来恢复网格重建（如果有地面真值则可以进行比较）。

![](img/5ab976d095717c5cdbe5caf6642110de.png)

（来自 [1]）

在合成场景中，我们看到 iMAP 在共同创建一致的场景重建（见下文）和准确的相机轨迹（见上文）方面非常强大。实际上，iMAP 被发现比基线更能准确填补场景中未观察到的部分。这种好处得益于深度学习利用先前数据和 [归纳偏差](https://en.wikipedia.org/wiki/Inductive_bias) 以合理/可预测的方式处理模糊性的能力。

![](img/01dd5ea05afea29e7d8612bc6b6fcec7.png)

（来自 [1]）

iMAP 凭借其在每个帧中对 3D 场景表示和相机姿态的联合优化，能够准确地同时进行跟踪和映射。尽管学习过程从完全随机的初始化开始，但 iMAP 很快学会了准确的场景表示和跟踪信息。最值得注意的是，iMAP 框架可以用于任何规模，从小物体到包含各种详细物体的整个房间；见下文。

![](img/32e3f6ed20e90d5c48985449098b8490.png)

（来自 [1]）

尽管制作了相对准确的场景表示，iMAP 的内存使用量与 SLAM 基准相比仍然很低。iMAP 只需要足够的内存来存储关键帧（包括相关数据）和神经网络的参数；见下文。

![](img/05bae1fa7682fb0c5657736e5d73085b.png)

（来自 [1]）

在来自手持 RGB-D 摄像头的真实数据上，iMAP 继续超越 SLAM 基准。特别是，iMAP 在准确渲染深度摄像头读数不准确的场景区域方面意外有效（例如，这种情况通常发生在黑色、反射性或透明表面上）；见下文。

![](img/882b52d6e135989f9e8bb90d978d8020.png)

（来自 [1]）

# 主要结论

与我们目前见过的大多数技术相比，iMAP 非常不同。特别是，它是一个 SLAM 系统，这意味着它除了构建场景表示外，还执行定位。此外，它依赖于在线学习技术，实时学习有关场景的所有内容。iMAP 从随机初始化开始，从零开始学习表示基础场景！一些主要结论如下。

![](img/9afd8e20d0dad0ada703d6afc515bbb3.png)

（来自 [1]）

**超级快！** 我们见过的大多数 3D 场景表示技术都相当昂贵。例如，NeRFs [需要 2 天](https://cameronrwolfe.substack.com/i/97915766/takeaways)才能在单个 GPU 上训练，而 LLFF [在生成新场景视图之前需要约 10 分钟的预处理](https://cameronrwolfe.substack.com/i/98707360/takeaways)。iMAP 可以实时学习所有内容，只要 RGB-D 摄像头提供新的图像。这在创建场景表示的计算成本上是一个巨大的变化。上面展示了 iMAP 跟踪和映射管道的一些时间数据。iMAP 在 PyTorch 中实现，并且可以在桌面 CPU/GPU 系统上运行。

**为什么使用深度学习？** 当我们将 iMAP 系统与其他 SLAM 基准进行比较时，我们会发现它能够“填补”其他系统留下的空白区域。简而言之，iMAP 可以合理地推测在场景中未被明确观察到的区域的内容。这种能力得益于深度前馈神经网络的使用，该网络利用数据/架构中的先验信息，从有限的数据中估计几何形状。

**动态学习**。当开始学习一个场景时，iMAP 并没有任何信息。实际上，它从完全随机的初始化开始，然后使用在线学习技术从输入数据中动态学习场景的表示。考虑到之前的方法（例如，[NeRF](https://cameronrwolfe.substack.com/p/understanding-nerfs)）需要几天的训练时间来表示一个场景，这种方法能够很好地工作确实非常令人惊讶。iMAP 向我们展示了，可能存在一些捷径或更轻量的技术，使我们更容易获得高质量的场景表示。

**位置嵌入**。虽然这是一个较小的点，但我们通过 iMAP 看到，[位置编码](https://cameronrwolfe.substack.com/i/97915766/position-encodings)，最初在 NeRFs 中出现，现在已经成为标准。请记住，将输入坐标转换为更高维的位置信息嵌入，然后作为输入传递给前馈网络，可以更容易地学习高频特征。在这里，我们再次看到位置嵌入的应用，表明这种方法正在成为标准。

# 相关帖子

+   理解 NeRFs [[link](https://cameronrwolfe.substack.com/p/understanding-nerfs)]

+   本地光场融合 [[link](https://cameronrwolfe.substack.com/p/local-light-field-fusion)]

+   场景表示网络 [[link](https://cameronrwolfe.substack.com/p/scene-representation-networks)]

+   使用 ONets 的形状重建 [[link](https://cameronrwolfe.substack.com/p/shape-reconstruction-with-onets)]

+   深度 SDF 的 3D 生成建模 [[link](https://cameronrwolfe.substack.com/p/3d-generative-modeling-with-deepsdf)]

# 结束语

非常感谢阅读这篇文章。我是 [Cameron R. Wolfe](https://cameronrwolfe.me/)，[Rebuy](https://www.rebuyengine.com/) 的人工智能总监以及莱斯大学的博士生。我研究深度学习的实证和理论基础。你也可以查看我在 medium 上的 [其他著作](https://medium.com/@wolfecameron)！如果你喜欢这篇文章，请在 [twitter](https://twitter.com/cwolferesearch) 上关注我或订阅我的 [Deep (Learning) Focus newsletter](https://cameronrwolfe.substack.com/)，在这里我通过易于理解的热门论文概述，帮助读者深入了解深度学习研究中的主题。

# 参考文献

[1] Sucar, Edgar, 等. “iMAP: Implicit mapping and positioning in real-time.” *Proceedings of the IEEE/CVF International Conference on Computer Vision*. 2021.

[2] Mildenhall, Ben, 等. “Nerf: Representing scenes as neural radiance fields for view synthesis.” *Communications of the ACM* 65.1 (2021): 99–106.

[3] Sitzmann, Vincent, Michael Zollhöfer, 和 Gordon Wetzstein. “Scene representation networks: Continuous 3d-structure-aware neural scene representations.” *Advances in Neural Information Processing Systems* 32 (2019).

[4] Park, Jeong Joon 等人。“Deepsdf: 学习用于形状表示的连续符号距离函数。” *IEEE/CVF 计算机视觉与模式识别会议论文集*。2019 年。

[5] Tancik, Matthew 等人。“傅里叶特征使网络能够在低维领域学习高频函数。” *神经信息处理系统进展* 33 (2020): 7537–7547。

[6] Kemker, Ronald 等人。“测量神经网络中的灾难性遗忘。” *美国人工智能协会会议论文集*。第 32 卷，第 1 期，2018 年。
