# 高斯头像：总结

> 原文：[https://towardsdatascience.com/gaussian-head-avatars-a-summary-2bd17bd48500?source=collection_archive---------0-----------------------#2023-12-19](https://towardsdatascience.com/gaussian-head-avatars-a-summary-2bd17bd48500?source=collection_archive---------0-----------------------#2023-12-19)

## 最近高斯点溅射的论文出现了爆炸性增长，头像领域也不例外。这些技术是如何工作的，它们会彻底改变这一领域吗？

[](https://medium.com/@jacksaunders909?source=post_page-----2bd17bd48500--------------------------------)[![Jack Saunders](../Images/00c752fe1c5bc03f52943238cb034ee4.png)](https://medium.com/@jacksaunders909?source=post_page-----2bd17bd48500--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2bd17bd48500--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2bd17bd48500--------------------------------) [Jack Saunders](https://medium.com/@jacksaunders909?source=post_page-----2bd17bd48500--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F24c6b2ceeccc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgaussian-head-avatars-a-summary-2bd17bd48500&user=Jack+Saunders&userId=24c6b2ceeccc&source=post_page-24c6b2ceeccc----2bd17bd48500---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2bd17bd48500--------------------------------) · 15分钟阅读 · 2023年12月19日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2bd17bd48500&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgaussian-head-avatars-a-summary-2bd17bd48500&user=Jack+Saunders&userId=24c6b2ceeccc&source=-----2bd17bd48500---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2bd17bd48500&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgaussian-head-avatars-a-summary-2bd17bd48500&source=-----2bd17bd48500---------------------bookmark_footer-----------)![](../Images/5f471a64d0ba8b95c526decafc9fd6d3.png)

高斯点溅射在训练初期的表现。图像作者提供。

放松一下……如果你对数字人类的研究感兴趣，并且有任何形式的社交媒体，你几乎肯定已经被大量将高斯喷溅应用于该领域的论文轰炸过了。[正如伟大的贾宾·黄](https://twitter.com/jbhuang0604/status/1714280734016540860)所说：2023年确实是用高斯喷溅取代所有NeRF的年份。[GaussianAvatars](https://shenhanqian.github.io/gaussian-avatars)，[FlashAvatar](https://ustc3dv.github.io/FlashAvatar/)，[可重光照高斯编解码头像](https://shunsukesaito.github.io/rgca/)，[MonoGaussianAvatar](https://yufan1012.github.io/MonoGaussianAvatar)：这些论文仅代表了涵盖面部的论文中的一小部分！

如果你像我一样，你会被这些令人惊叹的工作完全压倒。我写这篇文章是为了比较和对比这些论文，并试图提炼出支撑这些工作的关键组件。我几乎肯定会遗漏一些论文，并且在我完成这篇文章时，我预计会有更多论文出现！我将首先简要回顾高斯喷溅作为一种方法，然后讨论一些关键论文。

![](../Images/36ffc18d89ac9eabb337c21048454a02.png)

一系列高斯头像论文中的预览图。左上： [MonoGaussianAvatar](https://yufan1012.github.io/MonoGaussianAvatar)，左下： [GaussianAvatars](https://shenhanqian.github.io/gaussian-avatars)，右上： [可重光照高斯编解码头像](https://shunsukesaito.github.io/rgca/)，右中： [高斯头](https://github.com/chiehwangs/gaussian-head/tree/main)，右下： [高斯头头像](https://yuelangx.github.io/gaussianheadavatar/)。

# 高斯喷溅——总体思路

高斯喷溅现在无处不在。你很可能已经了解了基础知识，如果不了解，还有很多资源可以比我更好地解释它们。如果你感兴趣，这里有一些例子（[1](https://huggingface.co/blog/gaussian-splatting)，[2](https://aras-p.info/blog/2023/09/05/Gaussian-Splatting-is-pretty-cool/)，[3](https://www.reshot.ai/3d-gaussian-splatting)）。尽管如此，我会尽力给出一个快速、一般的概述。

![](../Images/2a81c13cdcdd0fa4926486ca0c2365d6.png)

栅格化三角形（左）与高斯喷溅（右）。灵感来自于[这篇 HuggingFace 文章](https://huggingface.co/blog/gaussian-splatting)。

简而言之，高斯喷溅是一种栅格化形式。它将场景的某种表示转换为屏幕上的图像。这类似于大多数图形引擎的三角形渲染。与绘制三角形不同，高斯喷溅（毫不奇怪）将高斯分布“喷溅”到屏幕上。每个高斯分布由一组参数表示：

+   3D空间中的一个位置（场景中）。**μ**

+   每个轴的缩放（高斯的倾斜）。**s**

+   一种颜色（可以是 RGB 或更复杂的，取决于视角）。**c**

+   一种不透明度（透明度的相反）。**α**

高斯溅射本身并不新鲜，它至少从 90 年代就存在了。新的在于能够以***可微分***的方式渲染它们。这使我们能够使用一组图像将它们拟合到场景中。结合创建更多高斯和删除无用点的方法，我们得到了一种极其强大的 3D 场景表示模型。

我们为什么要这样做？有几个原因：

+   其质量不言而喻。它在视觉效果上甚至超越了 NeRFs。

+   使用高斯溅射渲染的场景可以以数百 fps 运行，而且我们甚至还没有深入探讨硬件/软件优化。

+   由于它们是离散点，可以轻松编辑/组合，这与神经表示不同。

# 使高斯能够动画化

高斯溅射显然很酷。人们寻求将其应用于面孔并不令人惊讶。[你可能见过苹果的角色](https://www.theverge.com/2023/6/5/23750096/apple-vision-pro-headset-persona-facetime)，它们引起了一些关注。本文中的论文完全超越了这些角色。想象一下，完全可控的数字化身可以在消费级 VR 头显上原生运行，具有 6 自由度的相机运动，并且双眼帧率超过 100 fps。这将使“元宇宙”最终成为现实。我个人敢打赌，这种情况将在未来 2–3 年内实现。高斯溅射（或某种变体）很可能是实现这一目标所需的技术。

当然，渲染静态场景的能力还不够。我们需要能够用高斯实现我们目前在三角网格中能够做到的事情。动态高斯（例如 [1](https://dynamic3dgaussians.github.io/)，[2](https://ingra14m.github.io/Deformable-Gaussians/)）的出现并不久。这些方法允许捕捉和重播“高斯视频”。

![](../Images/2e547a3ed88ed0c7d12194135173f57a.png)

动态高斯的示例，最佳查看源头：[这里](https://dynamic3dgaussians.github.io/)。图片来源：JonathonLuiten，[MIT 许可证](https://github.com/JonathonLuiten/Dynamic3DGaussians?tab=License-1-ov-file#readme)

再次，这确实很酷，但不是我们所追求的。理想情况下，我们想要一个可以通过动作捕捉、音频或其他信号进行控制的表现形式。幸运的是，我们有大量的研究专门针对这一点。引入我们老朋友3DMM。[我在上一篇文章中讲解了这些如何工作](https://medium.com/@jacksaunders909/person-specific-deepfakes-with-3d-morphable-models-1df9618b9f6a)。但从本质上讲，它们是通过一小组参数来表示的3D模型。这些参数决定了面部的几何形状，面部表情与面部形状分离。这允许通过只改变少量表情参数来控制面部表情。***大多数试图通过高斯点阵动画的论文以3DMM为核心。***

现在我们将讨论一些最近的高斯头部动画论文（顺序无关）。我在每篇文章的末尾添加了一些TLDR摘要。

# 高斯头像：带有装配3D高斯的逼真头部头像

![](../Images/8a5e32731f274b1269eb9aa3010a4a6f.png)

高斯头像的方法图解。图像直接从[arxiv论文](https://arxiv.org/abs/2312.02069)中复制。

> 高斯头像：带有装配3D高斯的逼真头部头像。[Shenhan Qian](https://arxiv.org/search/cs?searchtype=author&query=Qian%2C+S)、[Tobias Kirschstein](https://arxiv.org/search/cs?searchtype=author&query=Kirschstein%2C+T)、[Liam Schoneveld](https://arxiv.org/search/cs?searchtype=author&query=Schoneveld%2C+L)、[Davide Davoli](https://arxiv.org/search/cs?searchtype=author&query=Davoli%2C+D)、[Simon Giebenhain](https://arxiv.org/search/cs?searchtype=author&query=Giebenhain%2C+S)、[Matthias Nießner](https://arxiv.org/search/cs?searchtype=author&query=Nie%C3%9Fner%2C+M)。Arxiv预印本，2023年12月4日。[**链接**](https://arxiv.org/abs/2312.02069)

我们将首先查看一篇有趣的论文，它是慕尼黑工业大学与丰田之间的合作（我对丰田的参与非常好奇）。该团队正在使用FLAME，这是一种非常流行的3DMM。这种方法旨在利用多视角视频获得一个可以通过FLAME参数控制的模型。它由几个独立的阶段组成。

## FLAME拟合

流水线的第一阶段旨在使用 FLAME 网格重建面部几何的粗略近似。这是通过可微分渲染来完成的。具体来说，他们使用 NVDiffrast 以允许反向传播的方式渲染 FLAME 网格。再次，[我之前已经介绍过这如何工作](https://medium.com/@jacksaunders909/person-specific-deepfakes-with-3d-morphable-models-1df9618b9f6a)。他们的方法与现有跟踪器的不同之处有三点。**1)** 它是多视角的，这意味着他们在多个相机上同时进行优化，而不是单目重建。**2)** 他们还在 FLAME 顶点中添加了一个额外的常数偏移，允许更好的形状重建。这是可能的，因为多视角设置中不存在深度模糊问题。**3)** 他们包括一个拉普拉斯网格正则化器，鼓励网格光滑。下面可以看到 FLAME 跟踪的示例。

![](../Images/ce9dd83723d2ded7b1d1261a39285db3.png)

给定帧的 FLAME 网格重建示例。图像直接转载自 [arxiv 论文](https://arxiv.org/abs/2312.02069)。

## 拟合高斯

接下来的目标是以 FLAME 网格控制的方式拟合高斯。在本文中，该方法类似于 [INSTA](https://zielon.github.io/insta/)，其中 3D 空间中的每个点都“绑定”到 FLAME 网格上的一个三角形。随着网格的移动，点也随之移动。将这个想法扩展到高斯是相当简单的，只需将每个高斯分配到一个三角形。我们在父三角形定义的局部框架中定义每个高斯的参数，并根据 FLAME 网格相对于中性 FLAME 网格定义的变换来调整其参数。

![](../Images/dcc2840d77ae06fb457a166566b2333c.png)

高斯在每个三角形的局部坐标框架中初始化。图像直接转载自 [arxiv 论文](https://arxiv.org/abs/2312.02069)。

例如，假设我们张嘴，一个位于下巴的三角形向下移动 1cm 并旋转 5 度，我们会对任何绑定的高斯应用相同的变换，并以相同的方式旋转高斯的旋转。

从这里开始，过程与原始高斯论文非常相似，前向传递现在获取高斯参数，根据跟踪的网格对其进行变换，并进行点滴。参数使用反向传播进行优化。使用附加损失来防止高斯离父三角形过远。最后，密集化过程稍有变化，以确保任何生成的高斯都绑定到与其父级相同的三角形上。

***TLDR: 将高斯分配到 FLAME 的三角形上，并通过网格进行变换。***

# FlashAvatar: 高保真数字头像渲染，300FPS

![](../Images/b46d7501ee076e0d0327eef0b928d28f.png)

FlashAvatars的方法示意图。图像直接来自于[arxiv论文](https://arxiv.org/abs/2312.02214)。

> FlashAvatar: 高保真数字化身渲染，帧率达到300FPS。[项俊](https://arxiv.org/search/cs?searchtype=author&query=Xiang%2C+J)，[高轩](https://arxiv.org/search/cs?searchtype=author&query=Gao%2C+X)，[郭宇东](https://arxiv.org/search/cs?searchtype=author&query=Guo%2C+Y)，[张聚勇](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C+J)。Arxiv预印本，2023年12月3日。[**链接**](https://arxiv.org/abs/2312.02214)

在我看来，这是最容易理解的论文。这是一篇关注速度的单目（单摄像头）高斯头部化身论文。它可以以300fps（!!）运行，并且训练仅需几分钟。再次强调，该模型分为几个阶段。

## FLAME拟合

由于这是一个单摄像头方法，因此采用了单目重建。这个方法基于可微渲染和Pytorch3D。[该方法在GitHub上开源，来自MICA](https://github.com/Zielon/metrical-tracker)。另外，我在之前的博客文章中也介绍了这个方法的工作原理。

## 高斯分布拟合

本文在uv空间中建模高斯分布。使用预定义的uv地图来获得2D图像和3D网格之间的对应关系。每个高斯分布由其在uv空间的位置而不是3D空间中的位置来定义。通过在uv空间中采样一个像素，可以通过获取在姿态3D网格上的对应点来获得高斯分布的3D位置。为了捕捉口腔内部，本文在口腔内部添加了一些额外的面片。

![](../Images/e8b212f8b6e35bd08648d8d6081f9148.png)

本文将口腔内部建模为一个平面。图像直接来自于[arxiv论文](https://arxiv.org/abs/2312.02214)。

然而，这种uv对应关系将高斯分布的位置限制在了网格表面。这是不理想的，因为粗略的FLAME网格并不能完美重建几何形状。为了解决这个问题，学习了一个小型MLP来将高斯分布相对于其在网格上的位置进行偏移。通过将该MLP基于FLAME模型的表情参数进行条件化，结果的质量得到了改善，这可以看作是一种神经校正方法。

该模型使用L1和LPIPS损失进行重建训练。口腔区域的遮罩被提高，以增加在重建更困难的区域的保真度。

***TLDR; 在uv空间中建模高斯分布，并使用基于表情的MLP对其在网格上的相对位置进行偏移。***

# 可重光高斯编解码化身

![](../Images/6798fbb65337ac1bfc34b35bd8f90c75.png)

可重光高斯编解码化身的方法示意图。图像直接来自于[arxiv论文](https://arxiv.org/abs/2312.03704)。

> 可重光照高斯编码头像。 [斋藤俊介](https://arxiv.org/search/cs?searchtype=author&query=Saito%2C+S)，[加布里埃尔·施瓦茨](https://arxiv.org/search/cs?searchtype=author&query=Schwartz%2C+G)，[托马斯·西蒙](https://arxiv.org/search/cs?searchtype=author&query=Simon%2C+T)，[李俊轩](https://arxiv.org/search/cs?searchtype=author&query=Li%2C+J)，[南吉珠](https://arxiv.org/search/cs?searchtype=author&query=Nam%2C+G)。 Arxiv 预印本，2023年12月6日。 [**链接**](https://arxiv.org/abs/2312.03704)

下一篇论文可能是引起最多关注的那一篇。这篇论文来自 Meta 的现实实验室。除了可以进行动画处理外，还可以更改这些模型的光照，使其更容易融入不同的场景。由于这是 Meta，而 Meta 在“元宇宙”上进行了重大投资，我预计这可能很快会转化为产品。该论文在已经流行的编码头像基础上进行扩展，使用了高斯点云。

## 网格拟合

不幸的是，Meta 使用的网格重建算法稍显复杂，它建立在公司之前几篇论文的基础上。尽管如此，足以说明的是，他们能够在多个帧上重建具有一致拓扑的跟踪网格，并保持时间一致性。他们使用一种非常昂贵且复杂的捕捉装置来完成这一点。

## CVAE 训练 — 在高斯分布之前

![](../Images/021af833be33928fa51526d5365f040e.png)

CVAE 基于论文“可动画面孔的深度可重光照外观模型”中的版本。图片摘自[论文](https://dl.acm.org/doi/pdf/10.1145/3450626.3459829)。

Meta 的先前方法基于 CVAE（条件变分自编码器）。它接收跟踪网格和平均纹理，并将它们编码成一个潜在向量。然后，这个向量在重新参数化后被解码成网格，并使用一组特征来重现纹理。这篇论文的目标是使用类似的模型，但采用高斯分布。

**CVAE 与高斯分布**

要将此模型扩展到高斯点云，需要进行一些更改。然而，编码器却没有改变。这个编码器仍然接收跟踪网格的顶点 V 和一个平均纹理。头像的几何形状和外观是分别解码的。几何形状通过一系列高斯分布来表示。论文中较有趣的部分之一是高斯在 uv 空间中的表示。在这里，为网格模板定义了一个 uv 纹理图，这意味着纹理图中的每个像素（texel）对应于网格表面上的一个点。在这篇论文中，每个 texel 定义了一个高斯分布。每个 texel 高斯分布不是通过绝对位置定义，而是通过它与网格表面的位移来定义，例如，所示的 texel 是一个与眉毛相关联并随其移动的高斯分布。每个 texel 还具有旋转、缩放和透明度的值，以及 RGB 颜色和单色的粗糙度（σ）和 SH 系数。

![](../Images/365960a6362b873392d09c47078d3086.png)

左侧的图像表示右侧的几何体。每个纹素代表一个高斯分布。图像直接来源于[arxiv 论文](https://arxiv.org/abs/2312.03704)。

除了高斯分布外，解码器还预测了表面法线贴图和可见性贴图。这些都通过光照的渲染方程近似结合。以下是一个非常粗略的解释，由于我不是光照方面的专家，这个解释几乎肯定是不准确的或不全面的。

![](../Images/8f153d022e28d60b22abf68c75ec0857.png)

从左到右：法线贴图、镜面反射光照贴图、漫反射光照贴图、反射率和最终光照效果。图像直接来源于[arxiv 论文](https://arxiv.org/abs/2312.03704)。

光的漫反射成分使用球面谐波计算。每个高斯分布都有一个反射率 (ρ) 和 SH 系数 (d)。通常，SH 系数仅表示到第 3 阶，但这不足以表示阴影。为平衡空间节省，作者使用了第 3 阶的 RGB 系数和第 5 阶的单色（灰度）系数。除了漫反射光照外，论文还通过给每个高斯分布分配粗糙度并使用解码后的法线贴图来建模镜面反射（如反射）。如果你对这些工作原理感兴趣，建议阅读论文和补充材料。

最后，一个单独的解码器还预测了模板网格的顶点。所有模型都通过图像层面和网格层面的重建损失一起训练。同时还使用了各种正则化损失。结果是一个具有表达和光照控制的极高质量的虚拟形象。

***TLDR; 将高斯分布表示为 UV 空间图像，分解光照并显式建模，通过这种高斯表示改进编解码器的虚拟形象。***

# MonoGaussianAvatar：单目高斯点基头像

![](../Images/445dce6b913e751e964a92fa3a67de10.png)

MonoGaussianAvatar 的方法图示：图像直接来源于[arxiv 论文](https://arxiv.org/abs/2312.04558)。

> MonoGaussianAvatar：单目高斯点基头像。[Yufan Chen](https://arxiv.org/search/cs?searchtype=author&query=Chen%2C+Y)，[Lizhen Wang](https://arxiv.org/search/cs?searchtype=author&query=Wang%2C+L)，[Qijing Li](https://arxiv.org/search/cs?searchtype=author&query=Li%2C+Q)，[Hongjiang Xiao](https://arxiv.org/search/cs?searchtype=author&query=Xiao%2C+H)，[Shengping Zhang](https://arxiv.org/search/cs?searchtype=author&query=Zhang%2C+S)，[Hongxun Yao](https://arxiv.org/search/cs?searchtype=author&query=Yao%2C+H)，[Yebin Liu](https://arxiv.org/search/cs?searchtype=author&query=Liu%2C+Y)。Arxiv 预印本，2023 年 12 月 7 日。 [**链接**](https://arxiv.org/abs/2312.04558)

这是另一篇针对单目情况（例如，仅使用一个摄像头）进行研究的论文。这个模型依然围绕3DMM构建，但采用了与其他模型略有不同的方法。基于[IMAvatar](https://github.com/zhengyuf/IMavatar?tab=readme-ov-file)和PointAvatars中概述的想法，它将FLAME模型定义的变形扩展为一个连续的变形场。使用该变形场，高斯点可以根据FLAME参数进行变形。这也是一个多阶段的过程。

## FLAME拟合

这里的拟合过程与FlashAvatar非常相似。我已经在这篇文章中介绍过，因此不再重复。如果你感兴趣，请阅读那一部分。

## 扩展FLAME到变形场

理想情况下，我们希望以与FLAME网格的顶点相同的方式变形高斯点。然而，FLAME网格的变形仅定义了它所包含的5023个顶点。虽然大多数其他方法试图将高斯点耦合到网格上的某一点，但本文旨在扩展FLAME的变形，以覆盖规范空间中的所有点。什么是规范空间？我们稍后会介绍。在FLAME中，表情和姿势校正是通过对5023个顶点定义的混合形状进行线性组合来定义的。在本文中，这些校正则由MLP网络表示。假设我们有100种表情，我们将定义一个网络，该网络接受规范空间中的位置，并输出一个（100, 3）大小的矩阵，表示该点的表情基。在每个关节的皮肤加权和姿势校正混合形状也由MLP表示。

![](../Images/3d410995cebb78464f1f1b51f339528e.png)

基于[IMAvatar](https://github.com/zhengyuf/IMavatar?tab=readme-ov-file)的MLP网络表示的表情、姿势和皮肤加权。图片直接来自[arxiv论文](https://arxiv.org/abs/2312.04558)。

这些多层感知机（MLP）与优化过程中的其他部分一起进行训练。通过将每个高斯点与FLAME网格上最近的点对齐，并要求该点的变形场与实际FLAME模型中定义的变形场一致，从而定义一个正则化损失。

## 高斯点拟合 — 3个空间

本文中定义了3个空间。高斯点通过每个空间进行变形，最后进行渲染，然后与真实图像进行比较。

在本文中，高斯分布的所有常见参数并没有像往常一样定义，而是仅通过它们在初始空间中的位置来定义。这是初始化空间。在此基础上，MLPs 预测所有常见属性，以初始化空间位置为输入，生成第二空间中的位置、缩放、旋转等。这称为规范空间。为了提高稳定性，规范空间中的位置作为从初始化空间位置的偏移量给出。最后，每个高斯分布通过变形 MLPs 进行变形，并且一组最终的 MLPs 还根据规范空间中的位置修改所有其他高斯参数。

![](../Images/ccedcdeec1381e5b583f7da1b70ab8fd.png)

三个不同的空间。初始化空间显示在左侧，规范空间显示在顶部，最终姿态空间显示在底部。图像直接来自于 [arxiv 论文](https://arxiv.org/abs/2312.04558)。

本文还使用了密集化技术来提高结果的质量。它更类似于 PointAvatar 中使用的类型。任何透明度 <0.1（例如接近透明）的高斯分布都被删除。每 5 个周期采样一定数量的高斯分布，这通过选择一个父高斯分布，采样一个靠近它的位置，并复制来自父高斯分布的其他参数来完成。随着时间的推移，采样半径会减少。

该模型使用原始高斯损失、上述 FLAME 变形损失以及感知 VGG 损失进行训练。梯度通过三个空间进行反向传播。

***TLDR; 用连续变形场替换离散的 FLAME 模型。在这些场中拟合高斯分布。***

# 伦理问题

高斯溅射技术允许实时生成逼真的人物渲染。这无疑会引发一系列伦理问题。其中最直接显而易见的问题与深伪技术（deepfakes）相关。例如，这些技术可能被用于生成虚假信息或非自愿的露骨材料。鉴于生成具有新水平真实感的虚拟形象的能力，潜在的危害是显著的。

更糟糕的是，现有的针对基于图像的深伪技术（如深伪检测、水印或接种）的方法可能无法应对基于高斯的技术。考虑到这一点，我认为研究人员在其工作中开发这些方法是至关重要的。[一些研究](https://arxiv.org/pdf/2309.11747v1.pdf)表明，特别是在 NeRFs 中，水印是可能的。因此，应该可以将这些技术适应于高斯溅射。

如果我对本文提出的工作有一个批评，那就是缺乏对工作潜在影响的考虑。**在本文列出的论文中，只有两篇提到伦理问题，即使如此，讨论也很简短。**

尽管目前由于实施模型和数据要求的挑战，这项原型技术实际被滥用的难度很大，但我们可能只差几篇论文，就能得到可能造成实际伤害的模型。作为研究社区，我们有责任考虑我们工作的后果。

> 在我看来，是时候围绕数字人类研究建立最佳实践规范了。

# 讨论 — 相似性、差异性和未来方向

这真的有很多论文！我们几乎只是覆盖了其中的一小部分。虽然我认为理解每篇论文的细节很有用，但更有价值的是理解所有论文的总体主题。以下是我从阅读这些文献中得到的一些见解，请随时进行讨论或补充你的观点。

**FLAME：** 每篇论文都试图将高斯附加到现有的网格模型上，在除了Meta的论文外，这就是FLAME。FLAME仍然非常受欢迎，但在我看来，它仍然不完美。明显的问题是缺乏细节，这是两篇论文所解决的，但像“O”这样的某些唇形的建模能力也很普遍。我认为有空间看到新的模型出现并改进这一点。个人而言，我期待像[神经参数化头部模型](https://simongiebenhain.github.io/NPHM/)这样的模型获得更高的关注。通过与2D-uv空间的对应关系，应该可以将一些高斯方法应用到这些模型提供的更优几何结构上。

**附加方法：** 两篇论文将高斯附加到网格上使用uv空间，一篇将其附加到三角形上，另一篇将FLAME扩展到连续变形场。这些方法似乎都很有效。我个人对使用uv空间的方法感到最兴奋。我认为这可能打开了学习生成模型的可能性，以生成高斯头像。例如，如果训练数千个uv空间模型，可以在这些模型上训练扩散模型/GAN，从而允许采样随机、逼真的头像。

**速度：** 所有这些方法都非常快速，运行速度比实时还要快。这将开启许多以前不可能的应用。预计在不久的将来会看到电信、游戏和娱乐的原型。

总结来说，高斯散点图已经成功地进入了头部头像领域，并且看起来有很大的潜力来开启许多令人兴奋的应用。然而，这些应用需要平衡潜在的风险。如果我们能够做好这一点，数字人类研究的未来将会非常光明！
