# RAFT中的光流：第2部分

> 原文：[https://towardsdatascience.com/optical-flow-with-raft-part-2-f0376a972c25?source=collection_archive---------8-----------------------#2023-10-03](https://towardsdatascience.com/optical-flow-with-raft-part-2-f0376a972c25?source=collection_archive---------8-----------------------#2023-10-03)

## 解密深度光流模型

[](https://medium.com/@itberrios6?source=post_page-----f0376a972c25--------------------------------)[![Isaac Berrios](../Images/e36cdbfc91b71ceb91c6e4191e1e0833.png)](https://medium.com/@itberrios6?source=post_page-----f0376a972c25--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f0376a972c25--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f0376a972c25--------------------------------) [Isaac Berrios](https://medium.com/@itberrios6?source=post_page-----f0376a972c25--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Ffbadc8e8ee44&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptical-flow-with-raft-part-2-f0376a972c25&user=Isaac+Berrios&userId=fbadc8e8ee44&source=post_page-fbadc8e8ee44----f0376a972c25---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----f0376a972c25--------------------------------) ·10分钟阅读·2023年10月3日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff0376a972c25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptical-flow-with-raft-part-2-f0376a972c25&user=Isaac+Berrios&userId=fbadc8e8ee44&source=-----f0376a972c25---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff0376a972c25&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptical-flow-with-raft-part-2-f0376a972c25&source=-----f0376a972c25---------------------bookmark_footer-----------)![](../Images/5575917b4ef6fceae3fe047ad5c9c16c.png)

图片由[Kevin Hansen](https://unsplash.com/@kevinhansenfoto?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在这篇文章中，我们将以另一种方式来看待[RAFT](https://arxiv.org/pdf/2003.12039.pdf)。第1部分中的直接方法能够详细解析网络的细节，但在这里我们将可视化这些细节并建立有价值的直觉。[在第1部分](/optical-flow-with-raft-part-1-f984b4a33993)中，我们的目标是理解RAFT，以便我们可以直接使用它；而在第2部分，我们将旨在以一种方式理解RAFT，使我们能够利用其架构的不同部分来构建自己的模型。

这里是本文的概览：

+   **动机**

+   **RAFT架构**

+   **查找操作符**

+   **迭代更新**

+   **结论**

# **动机**

RAFT 的概念被许多后续工作利用，理解 RAFT 是理解这些新方法的关键。你如何知道 RAFT 的哪些部分可以或应该被利用？为什么许多后续工作使用相关体？这些问题的答案来自于掌握 RAFT 的内部工作原理，仅仅了解论文表面内容通常是不够的，有时我们需要深入探讨，RAFT 也不例外。

# RAFT 架构

首先，快速回顾一下 RAFT，其架构可以分解为三个基本模块，如下所示。

![](../Images/ba7083f8d850dff02dfa034d097a49cb.png)

图 1\. RAFT 的架构。修改自 [来源](https://arxiv.org/pdf/2003.12039.pdf)。

## 特征提取

**特征编码器**是一个卷积神经网络（CNN），利用共享权重从图像 *I₁* 和 *I₂* 中提取特征。**上下文编码器**提取上下文和隐藏特征，这些特征都被输入到迭代更新模块中。

![](../Images/b271f2ab7872d61354a8e84e538bbcbe.png)

特征网络 f 和上下文网络 g 的函数映射。来源：作者。

![](../Images/464cbe1c3c517286214c66e6d7dcf357.png)

4D 相关体的计算。修改自 [来源](https://arxiv.org/pdf/2003.12039.pdf)。

## 视觉相似性

两个特征图的点积形成一个 4D **全对相关体**，其中 *g¹* 的每个像素映射到 *g²* 的所有像素，这些映射被称为 **2D 响应图**。*其中 g¹ 和 g² 分别是从 I₁ 和 I₂ 中提取的特征图张量。* 在相关体的最后两个维度上执行平均池化，卷积核大小为：1、2、4、8。

![](../Images/4a6d33f0bc89d459238bf26e1345bbe9.png)

图 2\. 左：*I₁* 中单个像素与 *I₂* 中所有像素的关系。右：相关金字塔中各种相关体的 2D 响应图。[来源](https://arxiv.org/pdf/2003.12039.pdf)。

我们将这些相关体积堆叠成一个 5D **相关金字塔**，每一层将 *g¹* 的精细像素与 *g²* 的逐渐粗糙的像素特征相关联。这使我们能够捕获关于大规模和小规模像素位移的信息。**查找操作符** 从相关体积中提取相关特征。它获取 *g¹* 的一个精细特征像素及其对应的光流场，并计算其新的明显位置，这称为其 **对应关系**。然后，它在预定义半径 *r* 周围形成一个 2D 网格，并随后沿网格执行 [亚像素](https://dsp.stackexchange.com/questions/34103/subpixel-what-is-it) [双线性重采样](https://en.wikipedia.org/wiki/Bilinear_interpolation#Application_in_image_processing) 以获得新的网格值。这个重采样的特征网格包含流（下降）方向信息，查找操作符对金字塔中每层的每个像素执行此操作。这些逐像素特征网格称为 **相关特征**，然后被重新塑形并输入到 **迭代更新块**。

## 迭代更新

迭代更新块接受四个输入：上下文特征、相关特征、当前流估计和隐藏特征。流和相关特征被一起编码为运动特征，因为它们都描述了特征像素的相对运动。上下文特征保持不变，作为更新块的稳定参考。该块本身由一个 [ConvGRU](https://arxiv.org/pdf/1511.06432.pdf) 组成，该网络计算一定数量的递归更新，随后由一个包含卷积层的流头将隐藏状态转换为原始输入分辨率的 1/8 的流估计。

![](../Images/9cd5d3879b2b09b1701845575fe6e931.png)

图 3\. RAFT 更新块用于大型架构，突出显示了不同的子块。蓝色—特征提取块，红色—递归更新块，绿色—流头。修改自 [Source](https://arxiv.org/pdf/2003.12039.pdf)。

更新操作符的功能类似于优化算法，这意味着它从初始流 ***f₀*** 开始，并迭代计算新的流值 *Δfₖ*，这些值被添加到先前的流估计中：*fₖ₊₁ = fₖ + Δfₖ*，直到收敛到一个固定值：***fₖ → f****。在执行迭代更新时，流估计和相关特征（不是相关金字塔）会持续被精炼。一旦迭代耗尽，流估计将从 1/8 上采样到原始分辨率。

## 凸上采样

**凸上采样** 通过将每个精细像素估计为其邻近的 3x3 粗像素网格的 [凸组合](https://www.math.ucla.edu/~baker/149/handouts/cc_convex/node4.html)。权重由一个神经网络参数化，该网络能够为每个精细像素学习最佳权重。下方显示了一个示例。

![](../Images/85c37d6ee2a54440ef1ef992fc8238d0.png)

图 4\. 上：双线性 VS 凸采样的比较。下：单个全分辨率像素（紫色）的凸采样示例。 [来源](https://arxiv.org/pdf/2003.12039.pdf)。

## 学习光流偏移量

重要的是要记住，RAFT 不一定估计光流，它估计的是从起始点的 *光流偏移量*，输出的是这些光流偏移量的累积。第一次估计是对 *I₁* 像素位置上 **0** 的先前光流的更新。*I₁* 的信息来自初始隐藏状态和上下文特征，这为更新块提供了持续的反馈，指导学习，使 RAFT 能够估计光流偏移量，而不是重新估计光流。

> RAFT 不直接估计光流，它的更新块从起始点估计光流偏移量，模型输出的是光流偏移量的累积。

在训练过程中，网络的递归更新模拟了优化算法的步骤，其中每个新的光流估计 *fₖ₊₁ = fₖ + Δfₖ* 被目标函数越来越严格地审视，迫使网络随着迭代次数的增加学习更保守的 *Δfₖ* 估计。目标函数捕捉所有光流更新，是光流预测与真实值之间的加权 *l1* 距离的总和，权重呈指数增长。

![](../Images/383fe7364b5d7933ecc3cd10277d536c.png)

RAFT 的损失，γ = 0.8。 [来源](https://arxiv.org/pdf/2003.12039.pdf)。

这种优化算法的模仿以及查找操作符的半径协同作用，限制了每次更新的搜索空间，从而减少了过拟合的风险，加快了训练速度，并且提高了泛化能力。

# 解读 RAFT

现在我们可以开始解读 RAFT，并深入了解它如何进行预测。这个教程的代码位于 [GitHub](https://github.com/itberrios/CV_projects/blob/main/RAFT/RAFT_dive.ipynb)，在这里 RAFT 代码已被修改，以在每个主要块处输出其中间特征图。测试图像是逆时针旋转的吊扇，这会产生几乎所有方向的光流。为了获得较大的光流位移，我们将跳过序列中的几个图像，这将使光流特征更加明显，便于研究。

![](../Images/4adb00bf6d078b255e1f8c66f077a6d0.png)

图 5\. 预测的光流。来源：作者。

现在让我们检查来自特征编码器块的图。

![](../Images/b503bd5f9ee96b974c97d463bf3d6220.png)

图 6\. 来自特征编码器块的特征图（0–127）。来源：作者。

特征编码器生成了*fmap 1*和*fmap 2*，它们看起来像噪声，但实际上包含了对流动估计至关重要的信息。隐藏的特征图类似于输入图像*I₁*，它们直接初始化了 ConvGRU 更新块的隐藏状态，提供了关于*I₁*中像素的宝贵信息。上下文特征图与输入图像有些相似，突出显示了诸如角落和边缘等强特征。原始论文建议，上下文特征提高了网络准确识别空间运动边界的能力。

## 相关性

相关金字塔对于计算准确的光流至关重要，因为它能够捕捉从*I₁*到*I₂*的像素对应关系。正如我们将看到的，**相关体积是 RAFT 的核心**，我们将可视化其出色的像素位移捕捉能力。我们将通过检查几个测试像素来接近这一点，并查看它们的二维响应图如何捕捉相对位移。我们只能看到特征，但像素位移的信息仍然会显现。

> **相关金字塔**捕捉了图像序列中像素在多个分辨率级别上的对应关系。

下图展示了一个测试图像的估计流动，并标注了一些测试像素。

![](../Images/f902c4b6f07dd5992db526543609a8fc.png)

图 7\. 带注释测试像素的流动图像。来源：作者。

每个像素的水平和垂直流场分量的估计值是：

+   像素 0: (-49.4, -4.3)

+   像素 1: (-5.8, -26.4)

+   像素 2: (23.5, -9.3)

## 访问相关特征

要访问给定测试像素的相关图，我们将以下函数添加到 corr.py 脚本中，以获得任何金字塔级别下给定测试像素的整数索引。

```py
def get_corr_idx(loc, lvl, w=71, h=40):
    """ Obtains index of test pixel location in correlation volume.
        loc - test pixel location 
        lvl - Pyramid level
        w - 1/8 of padded horizontal image width
        h - 1/8 of padded vertical image height
        """
    u = np.clip(np.round(loc[2]/(lvl*8)), 0, (w-1)) 
    v = np.clip(np.round(loc[1]/(lvl*8)), 0, (h-1))
    return int(u + w*v)
```

一旦我们获得了测试像素索引，我们可以获取其二维响应图、重新采样的相关特征和每个金字塔级别的对应关系。

```py
test_pixel_idx = get_corr_idx(test_pixel, lvl=(2**i))

# get the 2D response map
corr_response = corr[test_pixel_idx, 0, :, :].detach().cpu().numpy()

# get the resampled correlation feature grids
resampled_corr = bilinear_sampler(corr, coords_lvl)
resampled_corr_response = resampled_corr[test_pixel_idx, 0, :, :].detach().cpu().numpy()

# get the correspondence
pixel_loc = centroid_lvl[test_pixel_idx, :, :, :].cpu().numpy().squeeze()
```

## 可视化相关特征

对于每个像素，图 8-10 中展示了前 15 次更新的第一个金字塔级别的二维响应图 GIF。对应关系由红色方框标记。大的相关值（亮点）指示了*I₂*中的相对像素位置。注意，随着网络学习流动偏移，对应关系如何围绕高相关值收敛。尽管这些是大位移，但在第一个金字塔级别上的所有对相关性仍能捕捉到它们。

![](../Images/9b9f909bb8dc4e9c80e17e81a37ad73d.png)

图 8\. 金字塔级别 1，像素 0 的二维相关响应图。来源：作者。

![](../Images/844f559361369f87bbcce34eeacc2c90.png)

图 9\. 金字塔级别 1，像素 1 的二维相关响应图。来源：作者。

![](../Images/c448c644aebe747cf566e489f19037d3.png)

图 10\. 金字塔级别 1，像素 2 的二维相关响应图。来源：作者。

相关金字塔能够捕捉所有层级的相关性，但这并不总是显而易见的。随着金字塔层级的提升，事物开始变得更加抽象，确定RAFT实际在做什么变得越来越困难，此外，我们看到的是提取特征的相关性，使得事情更加模糊。

## 检索下降方向

匹配点通过查找操作符来提出下降方向。相关查找操作符在新的匹配点位置周围放置一个半径为*r*的网格：*x’ = (u + f¹(u) + v + f²(v))*，其中*(****f¹****,* ***f²****)*是当前的流场估计。围绕x’的网格用于从相关体中进行双线性重采样。这些重采样网格是输入到更新操作符中的相关特征，以预测下一个流估计。下图显示了第一个金字塔层级2D相关响应在像素1的情况以及其对应的双线性重采样网格；上排是第一次迭代，流初始化为零；下排是第二次迭代。

![](../Images/f9308199d73b8c7e95ff70a223754e6d.png)

图 11\. 像素 1 放大后的相关响应和双线性重采样网格。上排：迭代 0，下排：迭代 1。匹配点在(行，列)。来源：作者。

请注意上排中，左侧的相关响应与右侧的重采样网格相同，这是由于零流初始化造成的。我们还注意到关于右上角的双线性重采样网格的一个非常重要的点，最大值直接位于中心的左侧。如果我们向右移动三像素并向上移动一像素，即(3, -1)，那么我们就会落在这个大值上。这是从相关体中检索出的***建议***下降方向。在迭代更新块中，网络利用这些信息来制定实际的下降方向*Δfₖ*。

在下排中，我们可以看到匹配点大致从(39, 34)移动到(41.93, 33.3)，这是一个(2.93, -0.7)的位移，显示网络确实利用了建议的下降方向。在右下角的重采样网格中，我们看到最大值位于中心并与匹配点对齐，这表明网络已经有一个接近收敛的流预测。

## 运动特征

运动特征是相关特征和当前流估计的卷积编码。它们提供了需要由更新块细化的像素流信息。以下展示了每次迭代的一些运动特征图。

![](../Images/740100ab4278f039ce2ce90d8cf04c36.png)

图 12\. 不同迭代下的运动特征图。是的，我挑选了有趣的特征。来源：作者。

看起来，运动特征与具有大幅度移动的像素对应，不同的特征图似乎对应于不同的像素流，这在图12右侧的特征126和127中表现得尤为明显。它们都以类似的方式收敛到实际流动预测。

# **结论**

在这篇文章中，我们学习了RAFT及其内部工作原理。我们看到提取的隐藏特征提供了关于 *I₁* 的有用信息，而提取的上下文特征则提供了关于 *I₁* 强特征的参考信息。我们已经可视化了相关体如何捕捉到小范围和大范围像素位移的信息。事实证明，RAFT中的相关概念被许多后续工作所采用，从可视化中建立的直觉加强了这种模式。如果你已经读到这里，恭喜你！你现在对RAFT有了比表面层次更深入的理解。

# 参考文献

[1] Teed, Z., & Deng, J. (2020). Raft: Recurrent all-pairs field transforms for optical flow. *计算机视觉 — ECCV 2020*, 402–419\. [https://doi.org/10.1007/978-3-030-58536-5_24](https://doi.org/10.1007/978-3-030-58536-5_24)
