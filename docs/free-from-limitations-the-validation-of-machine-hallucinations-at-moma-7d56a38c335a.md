# *无限制：在 MoMA 机器幻觉的验证*

> 原文：[https://towardsdatascience.com/free-from-limitations-the-validation-of-machine-hallucinations-at-moma-7d56a38c335a?source=collection_archive---------3-----------------------#2023-08-31](https://towardsdatascience.com/free-from-limitations-the-validation-of-machine-hallucinations-at-moma-7d56a38c335a?source=collection_archive---------3-----------------------#2023-08-31)

## 揭示了人类与 AI 艺术合作的幕后创作过程

[](https://medium.com/@christian_burke?source=post_page-----7d56a38c335a--------------------------------)[![Christian Burke](../Images/50da1e824ed75082a70c3f381d685b5d.png)](https://medium.com/@christian_burke?source=post_page-----7d56a38c335a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7d56a38c335a--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----7d56a38c335a--------------------------------) [Christian Burke](https://medium.com/@christian_burke?source=post_page-----7d56a38c335a--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F764fa444fa3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffree-from-limitations-the-validation-of-machine-hallucinations-at-moma-7d56a38c335a&user=Christian+Burke&userId=764fa444fa3&source=post_page-764fa444fa3----7d56a38c335a---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----7d56a38c335a--------------------------------) · 7 分钟阅读 · 2023年8月31日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F7d56a38c335a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffree-from-limitations-the-validation-of-machine-hallucinations-at-moma-7d56a38c335a&user=Christian+Burke&userId=764fa444fa3&source=-----7d56a38c335a---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F7d56a38c335a&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffree-from-limitations-the-validation-of-machine-hallucinations-at-moma-7d56a38c335a&source=-----7d56a38c335a---------------------bookmark_footer-----------)![](../Images/c28b33d12eb137bc0091b84e6b551bc2.png)

照片由 [Jamison McAndie](https://unsplash.com/@jamomca?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 在 [Unsplash](https://unsplash.com/photos/QpymanRHYTs?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText) 上拍摄

自 1929 年以来，位于纽约市的现代艺术博物馆 (MoMA) 一直是艺术爱好者的圣地。它是一座灯塔，照亮前卫的绘画和雕塑，由于“现代艺术”的定义不断变化，它的收藏也在不断演变。如今，这个杰出的机构正在验证数字艺术。

> **作为 Refik Anadol Studio (RAS) 的首席数据科学家，与 Refik Anadol 合作，我非常激动地看到我们的作品《无监督》被 MoMA 接纳。**

在 RAS，我们将数据美学带给更广泛的公众，展示了 AI 的潜力超越了文本生成。我们生活在见证我们艺术的人类影响力 — 它如何在情感层面上影响各个年龄段和背景的人。这是一种共同的人类体验，并且非常易于接触。

“Unsupervised” 由 gottalovenewyork 拍摄于 YouTube

AI 生成的艺术当然也并非没有争议。其中一个最广泛的误解是，数字艺术通常以及 AI 生成的艺术特别不被视为真正的艺术作品。然而，即便是 AI 生成的艺术也不是完全由机器创造的。它需要人类的触感。**作为《无监督》的创意背后的人，Anadol 从原始数据中创作艺术。** 这在数字艺术中是新的。之前的艺术家们利用数据跟随模板来制作已经存在的东西的复制品。Refik 的作品则完全不同。

## **想象机器的幻觉**

在 RAS，我领导着一个由七名数据科学家组成的团队。我的工作日充满了监督、审查和编写代码，同时还直接与客户对接和进行项目规划。虽然这看起来不太艺术化，但迄今为止，我已经收集了超过三十亿张图像，用于推动 AI 生成艺术的创作。鉴于我每天都被编码和数据集的细节所包围，退后一步看到 RAS 创作的整体作品是一种令人惊叹的体验。

**让我带你体验一下《无监督》的感受。** 想象一下：你走进了 MoMA 的大厅。起初，你会觉得自己走进了任何其他艺术博物馆。但如果你环顾四周，你会突然被这个巨大的屏幕（24 英尺乘 24 英尺）的景象所震撼，周围坐着和站着的人们 — 都在凝视这个展览。

展览本身不断移动。它持续变化，展示出迷人的颜色和形状。你所看到的取决于你进入 MoMA 时偶遇展览的哪个章节，以及来自大厅的实时音频、运动跟踪和天气数据。

Christian Burke 站在 MoMA 展览前

“无监督”试图回答这个问题：“如果一台机器亲自体验 MoMA 的收藏，它会梦到什么或产生什么幻觉？”通过将 MoMA 所有收藏的数据结合起来并推断形成这些机器梦境，“无监督”带领观众穿越艺术的历史，并将聚光灯投射到艺术的潜在未来。

> 艺术有时努力触及更广泛的社会问题。如果你想从“无监督”中获得一个普遍的启示，那就是展览表明 AI 生成的数字艺术在合法化方面的一个转折点。MoMA 对艺术界的意义就像核聚变对物理学家一样——一种圣杯。MoMA 选择展示这项关于计算机如何处理数据——如何“思考”、创造和产生幻觉——的探索，验证了 Anadol 和其他数字艺术家的工作。

但并不是每个参观“无监督”的人都在思考机器和它们的梦境。当你走进 MoMA 的大厅时，你会看到人类的多样性——从跑来跑去的小孩子到年长的人和各行各业的人们——都在享受这种强烈的共同体验。看人们观看展览对我来说和看“无监督”本身一样激动。我见过有人哭泣，也见过喜悦和爱的表情。我自己不是艺术家，但我相信它具有治愈的特质。我也相信，只要你足够关注，任何人在任何地方做的事情中都可以找到艺术。编写代码中甚至也可以存在艺术。

# **人类与 AI 的合作关系**

人类艺术家需要技术技能来创作艺术。他们需要理解诸如色调值呈现、透视、对称，甚至人体解剖等概念。“无监督”将艺术的技术方面向前推进了一大步，通过创造人类与 AI 之间的合作伙伴关系。

RAS 利用来自 MoMA 超过 180,000 件艺术作品的数据创建了“无监督”项目。我们将 Warhol、Picasso、Boccioni 的作品以及 Pac-Man 的图像都输入了软件中。然后我们创建了各种 AI 模型并进行了广泛的测试。选择出最佳模型后，我们训练它不仅仅是将所有输入的艺术作品进行合成，而是创造出一种全新的东西。

> **“无监督”不仅仅是其组成部分的总和；它是完全新的东西。** **由于我们的艺术处理，展览中创造的一切都是原创的。**

人类与机器之间的合作关系需要在硬件和软件方面进行新的创新。我们的团队在创建所需的神经网络并使展览能够实时持续变换其图像，响应独特的环境因素时面临了许多挑战。

![](../Images/cd028e92d424c177d8ced55a9f9c7710.png)

Unsupervised 的静态图像，MoMA

一个挑战是分辨率。如果你在 Stable Diffusion 中输入一个提示，你通常会得到 512 x 512 像素的分辨率。我们使用的 AI 基础——Nvidia 的 StyleGAN——通常提供 1024 x 1024 的分辨率。“Unsupervised”的分辨率是 3840 x 3960，这可能是神经网络合成图像的最高分辨率。当你走进 MoMA 的大厅看到“Unsupervised”时，你会明白高分辨率为何如此重要。它让艺术栩栩如生，让它看起来几乎像一个可以从屏幕上跳出来的生物。

实时性是另一个需要克服的重要挑战。“Unsupervised”以液态流畅性产生其机器幻觉和梦想。这些机器幻觉源自合成超过 180,000 件艺术作品，并考虑实时因素。

> **距离 MoMA 不远的一栋建筑有一个气象站，收集天气相关数据。** 我们将这些数据输入“Unsupervised”，这意味着无论何时是阴天、晴天、雨天还是雾天，机器都会将外部世界的氛围融入其室内展示中。

其次，展览融入了来自观众自身的实时数据。大厅天花板上的摄像头将有关访客数量及其动作的数据传输到机器中。机器在展示其艺术梦想时会考虑这些数据。

> **有一个古老的问题：生活是否更像艺术，还是艺术更像生活？对于“Unsupervised”，答案显然是两者兼而有之。**

即使展览的观众被展品所感动，他们自己也会影响“Unsupervised”的呈现方式。

Irma Zandl 在 YouTube 上捕捉的 MoMA 展览的 Unsupervised 画面

类似地，AI 与人类之间的合作关系也存在双向街道的描述。有一种说法认为数字艺术涉及在传统艺术过程中加入一些额外的技术技能。然而，我更愿意把它看作是一个双向互动的过程。

数字艺术确实涉及将技术工具融入艺术过程，例如扩散模型和提示工程。另一方面，AI本身消除了进入艺术世界的一些障碍。比如说，我喜欢绘画，但我画人的能力很差。AI允许我弥补这一技术局限。

# **AI的未来**

**由于受欢迎的需求，“Unsupervised”在 MoMA 的展出已多次延长，机器的幻觉可能会无限期地继续下去。** 展望未来，我希望看到 AI 生成数字艺术的更大合法化。模型将继续改进，希望技术能够变得更加普及。

AI 可能是一种通过提高可访问性来使艺术世界民主化的手段，但目前仍存在技术障碍。我希望看到 AI 工具以更简单、更直观的界面出现，这样可以减少技术知识障碍。我们目前在 RAS 正在进行的一个新项目是网络集成工具，这将使人们更容易使用和互动 AI。这是我们在 RAS 的主要目标：创造与 AI 更大互动的手段。

由于“无监督”需要大量的人为干预，我有时被问及我是否认为 AI 总会需要这种人类干预。至少目前，答案肯定是肯定的。AI 在许多方面表现出色，比如合成，但在大规模工程和创新方面缺乏能力。

AI 生成的艺术可能看起来很有创意，但 AI 本身并不具备创意。实际上，它与创意正好相反。如果我们想在 AI 和科技领域不断前进和取得进展，我们需要依靠自己——而不是机器。

—

*作者说明：MoMA 授权 Refik Anadol Studio (RAS) 使用他们的训练数据。*

[*Christian Burke*](https://www.christianburke.com/) *领导 Refik Anadol Studio 的数据科学团队，该团队包括 AI、机器学习、网络和 Web3 开发。*

*你可以在* [*Twitter*](https://twitter.com/christianburke0) *和* [*LinkedIn*](http://www.linkedin.com/in/christianburke0) *上关注 Christian。*
