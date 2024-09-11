# 通过将GAN与扩散模型交叉提升图像生成

> 原文：[https://towardsdatascience.com/boosting-image-generation-by-intersecting-gans-with-diffusion-models-6a22f935b0f3?source=collection_archive---------9-----------------------#2023-05-09](https://towardsdatascience.com/boosting-image-generation-by-intersecting-gans-with-diffusion-models-6a22f935b0f3?source=collection_archive---------9-----------------------#2023-05-09)

## 一种稳定高效的图像到图像转换方法

[](https://medium.com/@essolanoc?source=post_page-----6a22f935b0f3--------------------------------)[![Edgardo Solano Carrillo](../Images/e160dddf2cdcfc793c0e2cd81bfb5b61.png)](https://medium.com/@essolanoc?source=post_page-----6a22f935b0f3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----6a22f935b0f3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----6a22f935b0f3--------------------------------) [Edgardo Solano Carrillo](https://medium.com/@essolanoc?source=post_page-----6a22f935b0f3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fdb88c072b0d3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-image-generation-by-intersecting-gans-with-diffusion-models-6a22f935b0f3&user=Edgardo+Solano+Carrillo&userId=db88c072b0d3&source=post_page-db88c072b0d3----6a22f935b0f3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----6a22f935b0f3--------------------------------) ·8分钟阅读·2023年5月9日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F6a22f935b0f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-image-generation-by-intersecting-gans-with-diffusion-models-6a22f935b0f3&user=Edgardo+Solano+Carrillo&userId=db88c072b0d3&source=-----6a22f935b0f3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F6a22f935b0f3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fboosting-image-generation-by-intersecting-gans-with-diffusion-models-6a22f935b0f3&source=-----6a22f935b0f3---------------------bookmark_footer-----------)![](../Images/3424e2a8c94a60eb2f2cee483a0cfb08.png)

ATME是GAN ∩ 扩散模型类中的一个模型。图像由DALL·E 2生成。

视觉基础模型（VFM）是如[Visual ChatGPT](https://arxiv.org/abs/2303.04671)¹等前沿技术的核心。在本文中，我们将简要讨论最近的进展，融合VFM“汤”的两个重要成分：GANs和扩散模型，最终达到它们的交集ATME。ATME是我在论文[Look ATME: The Discriminator Mean Entropy Needs Attention](https://arxiv.org/abs/2304.09024)²中介绍的一种新模型，GitHub仓库可在[此处](https://github.com/dlr-mi/atme)找到。

我们将讨论每种生成模型的相关弱点和优势。然后我们讨论合并它们的两类解决方案：朴素的GAN ∪ Diffusion 和更深入的高效的GAN ∩ Diffusion模型类别。最后，你将了解一些VFM研究目前如何发展。

# 生成模型

首先，提供一些背景。条件生成模型的目标是学习如何从目标领域生成数据 *y*，利用源领域的信息 *x*。这两个领域可以是图像、文本、语义图、音频等。两种建模方法已取得很大成功：生成对抗网络（GANs）和扩散概率模型。具体来说，

+   GANs 通过训练生成器模型来学习如何从数据分布 *p*(*y*∣*x*) 中采样，生成的数据按照 *g*​(*y*∣*x*) 分布。它使用一个鉴别器模型，指导生成器从盲目生成数据到准确生成数据，通过最小化 *g*​ 和 *p* 之间的散度（或距离）来实现。

+   扩散模型通过减少从 *p*(*y*∣*x*) 中采样的潜在变量 *y*₁​、*y*₂​、⋯、*y*ₙ​ 来学习。这些变量是 *y*（或 *y* 的编码）的一系列逐渐噪声化的版本，减少的过程是通过学习去噪模型来完成的。

如果你需要关于这些建模类型的更多细节，网上有大量资源可供查阅。关于GANs，你可以从[这篇文章](/must-read-papers-on-gans-b665bbae3317)开始，对于扩散模型，可以从[这篇文章](https://lilianweng.github.io/posts/2021-07-11-diffusion-models/)开始。

![](../Images/a1e76447e5b6b44d18afbf198d1576bc.png)

*图1：Visual ChatGPT* [*演示*](https://github.com/microsoft/TaskMatrix) *来自微软，已获许可。*

现在我们已经设置了基础，让我们讨论一些应用。图1展示了官方的Visual ChatGPT演示。它使用了几个用于视觉-语言交互的模型，部分模型列在下面的表格中。

表1：一些支持Visual ChatGPT的VFM。信息来源于[论文](https://arxiv.org/abs/2303.04671)¹的附录。

其中大多数是生成模型，其中大部分基于[稳定扩散](https://github.com/CompVis/stable-diffusion)。这表明了一个近期的兴趣转变，从 GAN 转向扩散模型，这一转变由[证据](https://papers.nips.cc/paper/2021/hash/49ad23d1ec9fa4bd8d77d02681df5cfa-Abstract.html)³ 触发，表明后者在图像合成方面优于前者。本文的一个要点是，这并不意味着扩散模型在所有图像生成任务中都优于 GAN，因为这些模型的聚合往往比独立部分表现更好。

在讨论这一点并到达 ATME 之前，让我们通过重新审视 GAN 和扩散模型的主要弱点和优势来铺平道路。

# GANs

原始 GAN [论文](https://papers.nips.cc/paper_files/paper/2014/hash/5ca3e9b122f61f8f06494c97b1afccf3-Abstract.html)⁴ 中引入的主要前提，并在[教程](https://arxiv.org/abs/1701.00160)中强调的是，在一个足够大的模型和无限数据的极限情况下，生成器和判别器之间的极小极大游戏会收敛到纳什均衡，其中（原始）GAN 目标达到值 −log4。然而，在实践中，这几乎没有观察到。这一理论结果的偏离产生了广泛被称为 GAN 训练不稳定的问题。这与模式崩溃一起，是其主要缺点。它们通过轻量级模型一次生成仍能达到较高的图像生成质量来弥补这一点。

# 扩散模型

相比之下，扩散模型虽然稳定，但由于学习去噪分布所需的步骤过多而效率较低。这是因为这种分布通常假设为高斯分布，这只有在去噪步骤非常小时才是合理的。

最近开发的替代方法通过使用多模态分布来减少去噪步骤的数量（甚至减少到 2 步），这需要将扩散模型与 GAN 结合，如我们在接下来的讨论中所述。

# GAN ∪ 扩散

目前的 GAN 与扩散训练方法非常有前景。它们可以被归类为使用生成对抗训练与**多步骤**扩散过程相结合的 GAN ∪ 扩散模型类。

为了改善 GAN 的训练稳定性和模式覆盖，这些模型通过遵循扩散过程注入实例噪声，这些过程可能有多达几千个步骤（如在 [*Diffusion-GAN*](https://github.com/Zhendong-Wang/Diffusion-GAN)⁵ 中），也可能只有两个步骤（如在 [*去噪扩散 GAN*](https://nvlabs.github.io/denoising-diffusion-gan/)⁶ 中）。这些模型在各种数据集上的表现优于强 GAN 基线，但仍需多个去噪步骤。因此，

> *是否有可能在一次生成中使用 GAN 创建图像，并同时利用去噪扩散过程？*

答案是肯定的，这定义了 GAN ∩ 扩散类模型。

# GAN ∩ 扩散

结果是，单个技巧可以使 [pix2pix](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix)⁷ 视觉基础GAN模型在设计上稳定：*关注判别器均值熵*。

![](../Images/f7ae64932bbe981be9858f7315f79c53.png)

*图2：ATME 使用来自扩散模型的 UNet 生成图像，这些图像由来自 pix2pix 的 patchGAN 判别器进行判断。图片由作者提供。*

结果模型，[ATME](https://github.com/DLR-MI/atme)，如图2所示。给定源图像和目标图像的联合分布 *p*(*x*,*y*)，输入图像 *x* 在时代 *t* 被 *Wₜ* = *W*(*Dₜ-*）所损坏，如下所示。

*xₜ* = *x* (1+*Wₜ*)

其中 *W* 是一个小型确定性网络，它将前一时代 𝑡- = *t-1* 的判别器决策图 *Dₜ-*​ 转换为 *Wₜ*。转换后的图 *Wₜ* 包含与输入空间中的补丁相关的模式，这些补丁是生成器之前未能欺骗判别器的区域，以及来自判别器的噪声，判别器尚未完全优化，因此在决策上出现错误。

生成器看到损坏的源图像，并根据 𝑦̂​ = *y*ᵩ(*xₜ*​, 𝑡̃) 生成目标图像，使用用于 [去噪扩散概率模型](https://hojonathanho.github.io/diffusion/) 的网 *y*ᵩ​，这些模型具有适当的注意机制。生成器的任务是使 *x* ⊕ 𝑦̂​​ 看起来与 *x* ⊕ *y* 对判别器不可区分（其中 ⊕ 表示连接）。通过这样做，它学会了去除输入图像中的注入噪声。值得注意的是，去噪发生在一个沿着“时间轴”的展开过程中，而不是像扩散模型那样在每个时代内需要一个独立的时间轴。

*Wₜ* 信号的展平，作为将输入图像去噪到生成器的结果，转化为 *Dₜ* 所有条目的平坦分布。这正是纳什均衡，判别器处于最大熵状态。

# … 扩散在哪里？

被损坏的输入图像在训练时期的演变可以写作

*dxₜ* = *x dWₜ* 

这可以视为更一般的 [SDE](https://en.wikipedia.org/wiki/Stochastic_differential_equation) 扩散过程的有限差分实例，即 *dxₜ* = *μ*(*xₜ*​,*t*) *dt* + *σ*(*xₜ*​,*t*) *dWₜ*，前提是 *Wₜ* 是一个维纳过程（也称标准布朗运动）。

在 ATME 中，没有设计选择使 *W* 产生维纳过程。这在大量情况下自然发生，从图3中可以看出，通过分析在 [Maps](http://efrosgans.eecs.berkeley.edu/pix2pix/datasets/) 数据集上训练过程中 5 个随机选择的图像的 5 个随机选择像素的时间序列 *dWₜ*​​ 的属性。

首先，维纳过程是平稳的。这是使用扩展的迪基-富勒检验进行测试的，其结果统计量的 *p* 值都远低于 0.01，因此所有非平稳时间序列的原假设都被拒绝。其次，维纳过程是马尔可夫过程，因此所有 *dWₜ* 的自相关函数在所有滞后处应当消失。从图 3 中以 99% 的置信水平可以明显看出这一点。

![](../Images/1658f1f8ea0c94d17192b2e69e17e379.png)![](../Images/3aa843f74e35fd69f6d5b56e81e6fe27.png)

*图 3: 选定图像和像素的 dW 时间序列 (上) 和相应的自相关函数 (下)。图像作者提供。*

最后，维纳过程具有高斯 *dWₜ*。这是使用 Shapiro-Wilk 检验进行的测试（在 64% 的情况下），测试统计量的 *p* 值大于 0.01，因此不能在相当多的情况下拒绝系列来自正态分布的原假设。

![](../Images/72206bd7896a545500abfddeabfec815.png)

*图 4: (重缩放) 判别器决策图 (左) 及其相关表示 (中) 和在源图像空间中的变化 (右)。图像作者提供。*

随着模型遍历的轮次增加，*Dₜ* 的条目趋向于平坦分布，从图 4 中观察到这一点，对应于图 3 中处理的一张图像。可以在 ATME 的 GitHub [代码库](https://github.com/DLR-MI/atme) 中找到包含这些测试的 Jupyter notebook。

# 如何检查稳定性？

稳定性意味着，无论数据集和模型权重的初始化如何，只要满足足够的数据和模型容量条件，GAN 的目标就会收敛到相同的值（对于普通 GAN 为 -log4）。在 ATME 中通常是这种情况，正如图 5 所观察到的，更多的示例可以在 [论文](https://arxiv.org/abs/2304.09024)² 中找到。其他流行的 GAN 无法实现这一点，这也是你可能听说过 GAN 模型“难以训练”的原因。

![](../Images/c16b9051205cec6c7625ef7649c037b6.png)

*图 5: ATME 中的 GAN 目标趋向于纳什均衡的理论值。图像作者提供。*

通过关注判别器的均值熵，ATME 的去噪程序设计为稳定地将 GAN 带到最大熵平衡状态。这是不是很棒？

# 结语

ATME 在有监督的图像到图像翻译中取得了最先进的结果，成本低于流行的 GAN 和潜在扩散。如果你喜欢物理学，你可能会对 ATME 的思想如何与麦克斯韦妖对第二热力学定律的违反相关感兴趣。你可以在 [论文](https://arxiv.org/abs/2304.09024)² 中找到这些内容及更多信息。

![](../Images/e59b922b5673c856a8bfee0f3c4bf41a.png)

*麦克斯韦妖 (来源:* [*astrogewgaw*](https://astrogewgaw.com/post/demons/)*). 已获得许可。*

技术的进步方式令人印象深刻且鼓舞人心。期待看到 GAN 和扩散模型结合后还会带来什么新进展。

目前就是这些！我希望你和我一样喜欢阅读这篇文章😉

[1] Chenfei Wu *等*，《视觉 ChatGPT：与视觉基础模型对话、绘图和编辑》，arXiv 2303.04671（2023）。

[2] Edgardo Solano-Carrillo *等*，《Look ATME：判别器均值熵需要关注》，arXiv 2304.09024（2023）。

[3] Prafulla Dhariwal, Alexander Nichol，《扩散模型在图像合成中超越 GAN》，神经信息处理系统进展 34（NeurIPS 2021）。

[4] Ian Goodfellow *等*，《生成对抗网络》，神经信息处理系统进展 27（NIPS 2014）。

[5] Zhendong Wang *等*，《扩散-GAN：用扩散训练 GAN》，arXiv 2303.04671（2022）。

[6] Zhisheng Xiao, Karsten Kreis, Arash Vahdat，《使用去噪扩散 GAN 解决生成学习三难问题》，国际学习表示大会（ICLR 2022）。

[7] Phillip Isola *等*，《基于条件对抗网络的图像到图像翻译》，计算机视觉与模式识别会议（CVPR 2017）。
