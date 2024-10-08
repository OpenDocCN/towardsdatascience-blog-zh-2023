# 穆布里奇德比：利用人工智能使动物运动照片栩栩如生

> 原文：[`towardsdatascience.com/muybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec`](https://towardsdatascience.com/muybridge-derby-bringing-animal-locomotion-photographs-to-life-with-ai-b1918e6622ec)

## 我如何使用 Midjourney 和 RunwayML 将埃德瓦德·穆布里奇的照片序列转化为高分辨率视频

[](https://robgon.medium.com/?source=post_page-----b1918e6622ec--------------------------------)![Robert A. Gonsalves](https://robgon.medium.com/?source=post_page-----b1918e6622ec--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b1918e6622ec--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1918e6622ec--------------------------------) [罗伯特·A·贡萨尔维斯](https://robgon.medium.com/?source=post_page-----b1918e6622ec--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b1918e6622ec--------------------------------) ·阅读时间 16 分钟·2023 年 7 月 25 日

--

![](img/13a194ce4c3d4ddc03b88feb11f5e9fd.png)

**第 2 帧** [**第 626 号板，奔跑**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider) 由埃德瓦德·穆布里奇（左），**基于 Midjourney 参考图像的 RunwayML Gen-1 视频生成器的转换**（中间和右侧），*这些图像由 AI 图像创建程序生成*

# 背景

我相信你一定见过 19 世纪英国摄影师埃德瓦德·穆布里奇拍摄的奔跑的马的系列图片。为了帮助回忆，这里有一个 GIF 动画展示了他更著名的照片系列之一。

![](img/75eacf68bff18bc34a17bcac5652d108.png)

[**第 626 号板，奔跑**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider) 由埃德瓦德·穆布里奇创作，动画 GIF 由作者制作

这是穆布里奇的肖像，附有他为拍摄该系列照片而制作的设备插图。

![](img/c9ec33691e714d02c8070506ca18cdf6.png)![](img/4a6856cb5f8cf5662885e37a3cd321b2.png)

**埃德瓦德·穆布里奇的肖像**（左） 图片来自 [维基媒体](https://commons.wikimedia.org/wiki/File:Optic_Projection_fig_411.jpg)，**穆布里奇的设备**（右），图片来自 [维基媒体](https://commons.wikimedia.org/wiki/File:Portret_van_Eadweard_Muybridge_Eadweard_Muybridge_(1830,_%2B1904.)_(titel_op_object),_RP-F-2001-7-509-65.jpg)

## 埃德瓦德·穆布里奇

穆布里奇是自然摄影师，由加州州长利兰·斯坦福委托拍摄他的豪宅和财物。斯坦福给穆布里奇提出了一个令人兴奋的挑战：他能否拍摄到一匹奔跑的马的清晰照片？

> 1872 年是马依布里奇开始热衷于运动摄影的年份。他受加州州长利兰·斯坦福委托，拍摄了他的赛马“西方”的运动步态。在此之前，马的运动步态仍是个谜。马蹄何时触地？四只脚是否曾同时离开地面？绘制奔跑中的马蹄一直是艺术家的难题。... [他使用] 12 台相机，每台相机连接到一个电器装置，当马奔跑经过时，装置会触发快门。 … 马依布里奇于 1879 年发明了 zoopraxiscope，这是一台可以将多达两百张单独图像投影到屏幕上的机器。1880 年，他首次向加州美术学院的一群人展示了投影移动图像，从而成为电影之父。- 维·惠特迈尔 [1]

马依布里奇不仅拍摄了移动的马。他还拍摄了移动的猫、狗、水牛、鸵鸟、人物等类似序列。

# 马依布里奇德比

对于这个项目，我想看看是否可以使用 AI 系统将马依布里奇的动物运动照片转换为高清晰度全彩视频。在尝试了各种技术后，我使用[Midjourney](https://www.midjourney.com/home/)通过文本提示生成参考帧，并结合 RunwayML 的 Gen-1 视频生成器，使原始序列变得更为逼真。为了好玩，我制作了一个短动画，“马依布里奇德比”，展示了这一工作。这里就是。

**“马依布里奇德比，”基于爱德华·马依布里奇的动物运动照片，** 作者制作的视频

在接下来的部分，我将描述如何转换运动序列、生成背景滚动以及将这些元素结合起来创建动画。

# 使用 Midjourney 生成参考帧

作为将马依布里奇照片系列转换为高清晰度视频的前提，我使用了原始系列的一张照片和 Midjourney 中的文本提示生成了高分辨率参考帧。

例如，这里是我用于生成马和骑手参考帧的提示，“**一个戴着蓝色帽子、穿着蓝色夹克、白色裤子和黑色靴子的男人骑着一匹棕色马，背景为白色 -**- **ar 4:3**。”请注意，--ar 4:3 参数表示 4:3 的宽高比。

![](img/33a83553394d80d2ab60b90f232c191f.png)![](img/e5f012484b2b5eacccbbe3ab47ec1534.png)![](img/a671a89c6fc9d7d946f9b4e8d42c7580.png)

**[**第 626 号板画的**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider)** **第二帧**（左），**Midjourney 缩略图**（中），**精选图像，修饰后**，作者提供的图像（右）

我将穆布里奇第 2 帧的图像链接和提示粘贴到 Midjourney 中，它生成了四个缩略图。所有四张生成的图像都很不错。我喜欢这些图像的细节和质感，包括骑师的衣服和马的毛发光泽。虽然它们都没有完全匹配原始马的姿势，但我发现当对视频进行风格化时，这并不重要。RunwayML 中的视频风格化器仅捕捉图像的一般外观。我选择了右下角（以绿色轮廓标出）的缩略图，并在 Photoshop 中进行了一些编辑；我将图像水平翻转，将马的颜色更改为棕色，并改变了骑师帽子的风格。

我对动画中的其他四种动物重复了这个过程，包括一只猫、一只水牛、一只大象和一只鸵鸟。以下是结果。你可以在下面的左列看到穆布里奇照片系列中的一张图片。中间列显示了使用穆布里奇图像和文本（如“跑步中的猫的全彩照片，侧面视图，-- ar 4:3”）从 Midjourney 得到的结果。所选缩略图以绿色轮廓标出。右列展示了经过稍微清理和在 Photoshop 中水平翻转（如有需要）的所选图像。

![](img/3b5a750615a94dd5cbe0198fe3b63491.png)![](img/be8fad3b7a7be282e4757abb0b9989cb.png)![](img/4a181c22443ee60521ce968f821085a2.png)![](img/0219b349907f7463307102218bcd0ae1.png)![](img/a96f2dc22ce8a0a13201fc7bf0c30005.png)![](img/f9ecc180c482ff7a4e51e487b09ec75f.png)![](img/3e1da164cfdc63d1624e724362c34262.png)![](img/142b5ee90f63e4a8cac47c91a0d11b3a.png)![](img/072b2fd8e72885a3990b97d4f3023e02.png)![](img/82f423f40f9d14b668ece23047c610a2.png)![](img/97e409525596b24f65ebdd9733792e9d.png)![](img/398b372ee6514ac8f491c8a7bc99c3b8.png)

**运动照片的** [**猫**](https://www.royalacademy.org.uk/art-artists/work-of-art/cat-trotting-change-to-galloping)**、** [**水牛**](https://www.royalacademy.org.uk/art-artists/work-of-art/buffalo-galloping)**、** [**大象**](https://www.royalacademy.org.uk/art-artists/work-of-art/elephant-walking)** 和** [**鸵鸟**](https://www.royalacademy.org.uk/art-artists/work-of-art/ostrich-running)**，**由伊德华德·穆布里奇（左），**Midjourney 缩略图**（中），**所选 Midjourney 图像，**（作者）

Midjourney 系统在生成参考图像方面表现出色。动物的细节令人惊叹。你可以点击任何一张图片来放大查看。虽然它没有完全匹配参考图像中的姿势，但整体渲染质量非常优秀。有关 Midjourney 的更多信息，你可以查看我之前的文章这里。

接下来，我将展示如何利用参考图像使用 RunwayML 转换照片系列。

# RunwayML

Runway 是一家位于纽约市的初创公司，研究并提供使用机器学习（ML）的媒体创建和编辑服务。他们因其网站的 URL[runwayml.com](https://runwayml.com/)而被称为 RunwayML。他们提供不同价格点的[订阅层级](https://runwayml.com/pricing/): 免费、每月$12、每月$28 等。

这是他们提供的一些服务的列表：

1.  超慢动作 - 将视频转换为超平滑的运动

1.  视频对视频编辑 - 使用文本或图像更改视频风格

1.  移除背景 - 移除、模糊或替换视频背景

1.  文本对视频生成 - 使用文本提示生成视频

1.  图像对图像编辑 - 使用文本提示转换图像

我使用了前三个来为我的视频中的 Muybridge 序列进行风格化。

![](img/a8a8f3f6824bfb049a0d3fdf4ac7a3ec.png)

[**626 号牌，奔马**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider)**，**由 Eadweard Muybridge 创作，**由 RunwayML 风格化**，作者动画

# RunwayML 的视频对视频编辑模型

RunwayML 的视频对视频编辑服务允许用户上传输入视频并提供文本或参考图像作为提示。然后，机器学习模型将通过施加提示中指定的风格来“编辑”镜头，同时保持输入视频的主要元素完整。该过程在他们的论文中有详细描述，*结构和内容引导的视频合成与扩散模型* [2]。

> 在这项工作中，我们展示了一种结构和内容引导的视频扩散模型，该模型根据对期望输出的视觉或文本描述编辑视频。由于两个方面之间的解耦不足，用户提供的内容编辑与结构表示之间会发生冲突。作为解决方案，我们展示了在不同细节水平的单眼深度估计上进行训练提供了对结构和内容保真的控制。……我们发现从输入视频帧提取的深度估计提供了所需的属性，因为它们编码的内容信息显著少于更简单的结构表示。—— P. Esser 等人，RunwayML

注意“单眼深度估计”指的是深度图，其中像素的值表示从相机到场景中物体表面的距离。为了获得深度估计，他们使用了一组欧洲研究人员的另一个机器学习模型[3]。这个模型叫做 MiDaS（我猜，这是**单眼深度估计器**的倒编词？）MiDaS 系统在 3D 电影场景的数据集上进行了训练，如下图所示。

![](img/bd71874f5aaabadebd55c2f6f582ba6c.png)

**来自 3D 电影数据集的样本图像**，来自[MiDaS 论文](https://arxiv.org/pdf/1907.01341.pdf)的图像

你可以看到深度图中浅黄色显示了场景中较近的点，而深蓝色则显示了背景中较远的点。训练过的 MiDaS 模型可以从任何输入模型估计深度图。以下是论文中的一些结果。

![](img/78e6690fdbbc805168fe1cfeb8afb342.png)

**使用 MiDaS 的预测深度图**，图片来自[MiDaS 论文](https://arxiv.org/pdf/1907.01341.pdf)

你可以看到 MiDaS 模型在深度估计方面表现出色。例如，你可以看到狗的尾巴非常清晰地突显出它在后面的水流中。

RunwayML 的视频到视频模型使用输入视频的预测深度图来条件化一个由文本提示或参考图像指导的扩散视频生成模型。

> 我们的潜在视频扩散模型根据结构和内容信息合成新的视频。我们通过根据深度估计进行条件处理来确保结构一致性，同时内容由图像或自然语言控制。通过模型中的额外时间连接和图像与视频的联合训练，实现了时间稳定的结果。— P. Esser 等人，RunwayML

你可以看到一些来自论文的文本提示的视频编辑结果。

![](img/312e621313470a1d5ba79849fcf506a7.png)

**RunwayML 的图像到视频编辑与文本的结果**，图片来自[RunwayML 的论文](https://arxiv.org/pdf/2302.03011.pdf)

各种风格，包括铅笔素描、动漫和低多边形渲染，转化了输入视频以创建输出。你可以看到应用风格在每一帧中的一致性。以下是一些论文中的例子，使用图像提示来美化视频。

![](img/19e1263184b7e0c3050c3a06a193280b.png)

**RunwayML 的图像到视频编辑与参考图像的结果**，图片来自[RunwayML 的论文](https://arxiv.org/pdf/2302.03011.pdf)

再次，你可以看到颜色调色板和提示图像的外观如何将视频转变为指定的风格。生成帧的细节也在最终视频中保持一致。

# 使用 RunwayML 的视频到视频编辑服务

为了使用该服务，我[创建了一个账户](https://app.runwayml.com/login)并登录了。如上所述，你可以使用有限制的免费版本，比如生成的视频最长只能为四秒。我选择每月支付 12 美元，这允许我创建最长 15 秒的视频并享受[其他好处](https://runwayml.com/pricing/)。

我拍摄了一段关于兔子的简短影像以测试系统，用编辑系统进行了清理，然后上传到 RunwayML。我选择了 Gen-1: Video to Video 工具。我加载了剪辑，输入了提示语“在田野中的逼真兔子，耳朵下垂”，然后点击了**预览风格**按钮。系统思考了一会儿，渲染了四个缩略图。你可以在下面的截图底部看到它们。

![](img/8b11962e741f5e333a01e196d3204089.png)

**RunwayML Gen-1: 视频到视频屏幕**，图像由作者提供

四个缩略图看起来都不错。它们都遵循了影子木偶的形式，但画面中出现了一只逼真的兔子。我选择了第三个，然后点击 **生成视频**。视频渲染大约花了 20 分钟。我还用提示“在田野里有垂耳的 2D 动画兔子”制作了一个视频。你可以在下面看到结果，原始影子木偶视频，以及我的清理版本供参考。

![](img/4edc4a191c9e8324a707991350088ab7.png)![](img/25f4375ebc7c6c32c049df2d560ad9ae.png)![](img/629fece34a73dbe140667f8eaf03f796.png)![](img/5dc768435388f93b2f82f72e9d40d4d6.png)

**原始影子木偶视频**（左上），**清理后的影子木偶视频**（右上），**使用提示“逼真 …”的 RunwayML 风格化视频**（左下），和 **使用提示“2D 动画 …”的 RunwayML 风格化视频**（右下），视频由作者提供

生成的视频效果很好！左下角的逼真视频效果最好，兔子的眼睛、耳朵和鼻子细节非常漂亮。2D 动画渲染效果有些偏差。系统似乎对耳朵的识别有些困惑，背景也不太有趣。接下来，我用 Midjourney 生成的两个参考图像尝试了同样的实验。

![](img/b091e18161db3b91bd6d305f81fbed81.png)![](img/4df4310a1769d85687ac041f4d0ad6e7.png)![](img/6b85cc56b398faee5770bba60c4163d1.png)![](img/809f1515cb4259eb711fbcf52d32ea5d.png)

**使用提示“逼真 …”的 Midjourney 图像**（左上），**使用提示“2D 动画 …”的 Midjourney 图像**（右上），**使用逼真参考的 RunwayML 风格化视频**（左下），和 **使用动画参考的 RunwayML 风格化视频**（右下），视频由作者提供

这些也做得很好。它们都从参考图像中拾取了风格，同时遵循了原始影子木偶视频中的形状和动作。然而，右边的那个视频有一个奇怪的效果从右侧出现，几乎看起来像是太阳光晕。注意到两个生成的动画都显示了参考帧中的背景细节，比如右侧漂亮的云朵。但参考帧中的前景形状却缺失了，比如左侧的麦粒和右侧的树。这可能是由于 RunwayML 使用的训练数据中包含了深度图像。它展示了我的手部动作被转换成兔子作为前景图像，但保留了参考图像中的背景元素，比如田野和天空。

# 使穆伊布里奇的照片栩栩如生

我使用了上述描述的 RunwayML 技术，并进行了小幅变更，将穆伊布里奇的原始图像序列转换为高分辨率版本。

## 超慢动作

由于穆伊布里奇的实验中动物动作很快，帧间存在大量的运动。例如，这里是马匹序列的三个帧。

![](img/60779274859ba1ecc20b6ce18561dca1.png)![](img/d865c1f5d3407587f3c09e4b2d67563b.png)![](img/36c66e53bb7d36ef3aff636f527a8dd1.png)

**帧 2、3 和 4 的** [**编号 626，疾驰**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider) 由 Eadweard Muybridge 制作

注意马腿间帧的运动量。我在尝试快速移动动画的视频到视频风格化时，结果并不理想。我的解决方案是先使用 RunwayML 的**超慢动作**功能将运动减慢两倍，然后应用转换，最后将结果视频的速度提高两倍。

这是减速视频的效果。

![](img/60779274859ba1ecc20b6ce18561dca1.png)![](img/813e00ad13b7f2ae2c694bb213b5608f.png)![](img/d865c1f5d3407587f3c09e4b2d67563b.png)

**帧 2**（左）**和 3**（右）**的** [**编号 626，疾驰**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider) 由 Eadweard Muybridge 制作，**RunwayML 帧插值**（中）由作者

你可以看到帧间的运动减少，尤其是马的腿部。这是原始马匹序列与 50% 慢动作版本的对比。

![](img/75eacf68bff18bc34a17bcac5652d108.png)![](img/fad48e4ace87d9e47314f5aa6f2233a5.png)

[**编号 626，疾驰**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider)由 Eadweard Muybridge 制作，作者动画（左），**RunwayML 的 50% 超慢动作**，作者动画

系统在运动插值方面表现出色。总体而言，使用 RunwayML 的超慢动作时运动更为流畅。当序列重置时，动作有一点小的卡顿，但当我将转换后的视频速度提高两倍时，这种情况会被掩盖。

## **视频到视频转换**

我首先将减速的马匹动画上传到 RunwayML 来创建转换后的视频，然后选择了**Gen-1: 视频到视频**工具。我选择了**图像风格** **参考**，并上传了我使用 Midjourney 创建的马匹参考帧。转换有多种设置，包括以下内容。

+   **风格：结构一致性** - 更高的值使输出与输入视频在结构上更有差异。

+   **风格：权重** - 更高的值强调匹配风格而非输入视频。

+   **帧一致性** - 值低于 1 会减少时间上的一致性；值高于 1 会增加帧与前一帧的相关性。

你可以在 RunwayML 的 [帮助页面](https://help.runwayml.com/hc/en-us/articles/15161225169171-Gen-1-Advanced-Settings) 上看到这些设置的变体示例。我试验了这些设置，但使用了默认值，即结构一致性 2、权重 8.5 和帧一致性 1。

然后我点击了**预览样式**，它在底部显示了四个选项。

![](img/f527b973d7f1a88fc86c4f606ab8d9e1.png)

**RunwayML 的 Gen-1：视频到视频功能的屏幕截图**，图片由作者提供

我选择了第三个预览，并点击了**生成视频**按钮。这里是参考图像、原始马匹序列和经过风格化的动画，速度加快了两倍以匹配初始速度。

![](img/57c42c2174be01d41b7a2db17e7b55bc.png)![](img/b761027e0eaab13a844c639c6d76106f.png)![](img/30b9351e38f1889a940737c6ce288314.png)

**Midjoruney 参考图像**（左），[**626 号版图，飞跑**](https://www.royalacademy.org.uk/art-artists/work-of-art/horses-gallop-thoroughbred-bay-mare-annie-g-with-male-rider)（中），**使用 RunwayML 风格化的动画**（右）

这做得很好！你可以看到参考图像的风格如何被施加到原始的梅布里奇动画上，同时保持了马匹和骑手的动作完整。系统还进行了基于机器学习的视频缩放，将最终视频调整到 640x480，这带来了不少细节。请注意，系统有一个“放大”设置，它会将分辨率水平和垂直方向都翻倍。

我对另外四个图像序列执行了相同的操作。你可以在下面看到结果，包括 Midjourney 的参考帧、梅布里奇的原始动物照片序列和 RunwayML 的风格化视频。

![](img/4a181c22443ee60521ce968f821085a2.png)![](img/fb301fc8d7f5af37276b3e47d25d2736.png)![](img/01a8769a7800547ce3be7cd932b0799b.png)![](img/0ae712c5397cfc01b8a5e4e92d2355bb.png)![](img/3ee35d9bc47c82ea89b038263a020f57.png)![](img/e1aa014505828df4f357beeb80240dcc.png)![](img/fcfc2b4e4ce180c64836e5ffd521d10e.png)![](img/bab698fe2d0daf00bf15ce4985d9e52a.png)![](img/a50fac88f71945520144b9995ed8b9ff.png)![](img/96214d9a39a53cc289044e804c95dd2a.png)![](img/65334658f4100f2a64ed9b7b8afd5fe4.png)![](img/48aae389c3f9ad66e2551e456b9acd01.png)

**Midjoruney 参考图像**（左），**伊德华·梅布里奇的动物运动研究**（中），**使用 RunwayML 风格化的动画**（右）

这些看起来也很棒！就像马匹动画一样，RunwayML 模型从参考图像中提取了纹理和颜色，并将其应用于原始动画，同时保持运动的完整性。然而，新动画中的背景并没有从右到左滚动。但这不是问题。你可以在下一部分看到我如何创建一个“alpha mask”来保留前景图像，并将动物合成到新的背景上。

## 移除背景图像

我使用了 RunwayML 的 **Remove Background** 功能来替换奔跑动物片段的背景。我加载了来自 Muybridge 照片的原始视频片段，并用光标选择了两个点，即马匹和骑师的腿。系统思考了一会儿，然后显示了所选区域为绿色，如下面的截图所示。

![](img/ad4a2d6bb6ea38908c0eddb41192c348.png)

**RunwayML 背景移除功能截图**，图像由作者提供

系统展示了它如何选择视频中所有帧的前景，我可以将其作为预览播放。它做得非常出色，没有花费我太多的工作。然后，我将 alpha matte 保存为视频以供合成应用程序使用。

我在 Midjourney 中创建了一个赛马场的静态图像，并将其用作动画的滚动背景。凡是 matte 为黑色的地方，会显示背景（赛马场）；凡是 matte 为白色的地方，会显示前景（马匹和骑师）。这里是马匹的风格化剪辑、alpha matte 和最终结果。

![](img/959918e566271716b28bd1f1cc94b504.png)![](img/7a7b0ef1ce36b97ca34f351145930d9b.png)![](img/8bb6fee237df623e45aac9a1d569f5fb.png)

**使用 RunwayML 风格化的动画**（左），**Alpha Matte**（中），和 **最终结果**（右），动画由作者提供

在我的合成程序中，我需要稍微清理一下 alpha matte。例如，我模糊了尾部，使其看起来更像头发而不是固体物体。你可以看到滚动背景如何帮助展示马匹向前奔跑的效果，而在原始风格化动画中并没有这个效果。

这里是最终的动画，这次稍微大一点，以便你查看细节。

**“Muybridge Derby”，基于 Eadweard Muybridge 的动物运动照片**，视频由作者提供

如果你想在大屏幕上观看动画，它将于 2023 年 8 月 5 日至 9 月 30 日在加州卡马里奥的 Studio Channel Islands Art Center 展出，地址是 [*The Next Big Thing*](https://studiochannelislands.org/nbt23/)。

## 最终想法

我很享受使用 Muybridge 图片以及利用 Midjourney 和 RunwayML 的工具生成和修改媒体。如果你熟悉我在 Medium 上的写作，你知道我喜欢尝试新的制作方法，但我并不总是能创作出完整的作品。因此，将多个元素结合在一起让我感到很满意。作为一个“深度切入”，我使用了我为之前的文章生成的 AI 歌曲作为片尾音乐。这首歌叫做“我会在到达时到达”，这对一场德比比赛来说有点合适。😄

[](/ai-tunes-creating-new-songs-with-artificial-intelligence-4fb383218146?source=post_page-----b1918e6622ec--------------------------------) ## AI-Tunes: 使用人工智能创作新歌曲

### 如何微调 OpenAI 的 GPT-3 来生成具有全球结构的音乐

towardsdatascience.com

# 输入和生成媒体的所有权

Midjourney 和 RunwayML 对于用于提示的图像和文本及生成的图像有不同的政策。Midjourney 区分付费用户和免费用户，而 RunwayML 对两种用户使用相同的政策。

## Midjourney 条款

根据 Midjourney 的 [服务条款](https://docs.midjourney.com/docs/terms-of-service)，免费服务层的用户不拥有他们生成的图片。Midjourney 拥有这些图片。这些图片根据知识共享署名非商业性 4.0 国际许可证授权给非付费用户用于非商业用途。付费服务的用户拥有他们生成的图片，这些图片可以用于商业用途。

如果你在一家大公司工作，还有额外的限制。

> 如果你是年收入超过 1,000,000 美元的公司的员工或所有者，并且你是代表你的雇主使用这些服务，你必须为每一个代表你访问服务的个人购买“Pro”或“Mega”会员，以便拥有你创建的资产。如果你不确定你的使用是否代表你的雇主，请假设是。— Midjourney 服务条款

仅供参考，Pro 计划每人每月 60 美元，Mega 计划每月 120 美元。

此外，根据条款，所有用户授予 Midjourney 许可，以任何目的使用任何文本提示、上传的作为提示的图片以及生成的图片，包括用于训练未来版本的模型。

## RunwayML 条款

根据 RunwayML 的 [使用条款](https://runwayml.com/terms-of-use/)，所有用户拥有并可以商业化使用其生成的内容。然而，所有用户都授予 RunwayML 许可，以任何目的使用他们的输入和输出，包括训练未来的模型版本。

# 图片和动画的许可条款

我将为此项目发布的图片和动画采用知识共享署名相同方式共享许可证。

![](img/be9434bf14bc4eab9a454950cbe4a9c5.png)

**知识共享署名-相同方式共享**

# 致谢

我想感谢 Jennifer Lim 对文章的审阅和反馈。

# 参考文献

[1] V. Whitmire，《国际摄影名人堂：伊德华·迈布里奇》*(2017)*

[2] P. Esser 等人，[结构与内容引导的视频合成与扩散模型](https://arxiv.org/pdf/2302.03011.pdf) (2023)

[3] R. Ranftl 等人，[迈向鲁棒的单目深度估计：混合数据集进行零样本跨数据集迁移](https://arxiv.org/pdf/1907.01341.pdf) (2020)
