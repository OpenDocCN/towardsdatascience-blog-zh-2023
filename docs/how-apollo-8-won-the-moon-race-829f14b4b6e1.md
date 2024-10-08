# 《阿波罗 8 号如何赢得月球竞赛》

> 原文：[`towardsdatascience.com/how-apollo-8-won-the-moon-race-829f14b4b6e1`](https://towardsdatascience.com/how-apollo-8-won-the-moon-race-829f14b4b6e1)

## 快速成功的数据科学

## 使用 Python 模拟自由返回轨迹

[](https://medium.com/@lee_vaughan?source=post_page-----829f14b4b6e1--------------------------------)![李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----829f14b4b6e1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----829f14b4b6e1--------------------------------)![走向数据科学](https://towardsdatascience.com/?source=post_page-----829f14b4b6e1--------------------------------) [李·沃恩](https://medium.com/@lee_vaughan?source=post_page-----829f14b4b6e1--------------------------------)

·发表于 [走向数据科学](https://towardsdatascience.com/?source=post_page-----829f14b4b6e1--------------------------------) ·阅读时长 10 分钟·2023 年 11 月 21 日

--

![](img/5af2feeda610a3cfed8107e9bd3afaa8.png)

“地球升起”由阿波罗 8 号宇航员比尔·安德斯（NASA）拍摄

五十五年前的今年 12 月，阿波罗 8 号的机组人员成为首批离开地球引力范围、前往其他天体的人。本月，那次任务的指挥官弗兰克·博曼在 95 岁时去世。为了纪念这一历史事件及其杰出的机组人员，我将回顾阿波罗 8 号飞行路径的计算机模拟。这样的模拟对于任务规划、教学辅助手段以及媒体和公众的传播都是非常有用的。

# 为什么阿波罗 8 号如此重要？

在 1968 年夏季，美国在太空竞赛中处于劣势。苏联的 Zond 飞船看起来已准备好前往月球，CIA 拍到了停在发射台上的巨大苏联 N-1 火箭，而美国陷入困境的阿波罗计划仍需进行三次测试飞行。

但在那年的 8 月，NASA 的经理乔治·洛提出了一个大胆的想法。*现在就去月球*。与其在地球轨道上进行更多测试，不如在 12 月绕月飞行，让*这*成为测试！

在那个时刻，太空竞赛实际上已经结束。不到一年后，苏联就已经投降，尼尔·阿姆斯特朗完成了他对全人类的伟大飞跃。

决定将阿波罗 8 号飞船送上月球并非易事。1967 年，阿波罗 1 号的舱内三名宇航员遇难，多次无人驾驶任务也发生了爆炸或其他失败。

在这种背景下，且事关重大，一切都取决于*自由返回轨迹*的概念。任务设计如此，以便如果服务模块引擎未能点火，飞船将简单地绕月球一圈，像回旋镖一样返回地球。这个概念的优雅之美甚至被融入了官方任务徽章中。

![](img/68968930ebc9d71d6ac6d60cb636cd72.png)

阿波罗 8 号任务徽章（NASA，CC BY-SA 4.0，来自维基媒体共享资源）

在本文中，我们将使用用 Python 编程语言构建的计算机模拟来探讨自由返回轨道。虽然不是必需的，如果你想在阅读过程中参与模拟，可以从这个[GitHub 仓库](https://github.com/rlvaugh/Real_World_Python/tree/master/Chapter_6)下载源代码和支持文件。你将需要文件*apollo_8_free_return.py*、*earth_100x100.gif*和*moon_27x27.gif*。

# 了解阿波罗 8 号任务

阿波罗 8 号任务的目标仅仅是*绕月*，因此不需要带上登月舱组件。宇航员们乘坐的是指令舱和服务舱，这两个部分合称为 CSM。

![](img/b7648ecb5d3fbccabe4ba5c0be772a99.png)

阿波罗指令舱和服务舱（来源：Real-world Python）

在 1968 年秋季，CSM 引擎仅在地球轨道中经过测试，对其可靠性存在合理的担忧。要绕月球运行，引擎必须点火两次，一次是减速使航天器进入月球轨道，再一次是离开轨道。

通过自由返回轨道，如果第一次机动失败，宇航员仍然可以顺利返回家园。事实证明，引擎在两次点火时都表现完美，阿波罗 8 号绕月球运行了 10 圈。（不过，命运多舛的阿波罗 13 号却充分利用了其自由返回轨道！）

## 自由返回轨道

尽管计算自由返回轨道需要大量复杂的数学运算（毕竟*这是*火箭科学），但你仍然可以使用一些简化的参数在二维图中绘制，如下所示。

![](img/3152073545a3d188139dffbb0d2eb12f.png)

自由返回轨道（不按比例缩放）（来源：Real-world Python）

为了在二维中模拟自由返回，我们需要几个关键值：CSM 的起始位置（R₀）、CSM 的速度和方向（V₀），以及 CSM 与月球之间的相位角（gamma₀）。相位角，也称为*引导*角，是从起始位置到最终位置所需的 CSM 轨道时间位置的变化。

跨月注入速度（V₀）是一种推进机动，用于将 CSM 设置为前往月球的轨道。这是在地球的停泊轨道上完成的，在此过程中，航天器进行内部检查，并等待与月球的相位角达到最佳时机。此时，土星五号火箭的第三级点火并脱落，留下 CSM 前往月球。

由于月球在移动，你必须预测它的未来位置或*引导*它，就像用猎枪打飞碟一样。这需要知道在跨月注入时的相位角（gamma₀）。

引导月球与射击霰弹枪有所不同，因为太空是*弯曲的*，你需要考虑地球和月球的引力。这两个天体对航天器的牵引力会产生难以计算的扰动。实际上如此困难，以至于计算在物理学领域获得了一个特殊的名称：*三体问题*。

## 三体问题

三体问题是预测三个相互作用的天体行为的挑战。艾萨克·牛顿的引力方程对于预测*两个*天体的行为（如地球和月球）效果很好，但如果再加入一个天体，无论是航天器、彗星、卫星等，情况就会变得复杂。牛顿从未能将三个或更多天体的行为简化为一个简单的方程。275 年来——即使国王提供奖金以寻找解决方案——世界上最伟大的数学家们也徒劳无功。

![](img/bb4182799484b4f68ae64d1211ad569b.png)

三体问题（Mgolden96, CC BY-SA 4.0, via Wikimedia Commons）

问题在于三体问题不能使用简单的代数表达式或积分来解决。计算多个引力场的影响需要在不使用高速计算机的情况下进行数值迭代，这在没有高速计算机的情况下是不可行的，例如你的笔记本电脑。

1961 年，迈克尔·米诺维奇（Michael Minovitch），一位在喷气推进实验室的暑期实习生，利用当时世界上最快的计算机 IBM 7090 找到了第一个数值解。他发现，数学家们可以通过使用*修正圆锥方法*来减少求解限制性三体问题（如我们的地月-CSM 问题）所需的计算次数。

## 修正圆锥方法

修正圆锥方法是一种分析性近似方法，它假设你在地球引力影响范围内处理一个简单的两体问题，而在月球引力影响范围内处理另一个问题。这是一种粗略的“简便计算”，提供合理的出发和到达条件估计，减少了初始速度和位置向量的选择。剩下的就是通过反复的计算机模拟来优化飞行路径。

因为研究人员已经找到了并记录了阿波罗 8 号任务的修正圆锥解，我们不需要再计算它。我已经将其调整到我们将要查看的 2D 模拟中。

> 如果你下载源代码，你可以通过调整参数如 R₀ 和 V₀ 来实验不同的解决方案。

# 模拟

自由返回轨迹模拟将捕捉飞行路径的基本要素，同时为了“观赏效果”做出若干近似。这些近似包括在 2D 环境中运行模拟，忽略阿波罗 8 号的十次月球轨道，并扭曲时间和距离的尺度。作为简化处理，地球和月球都不旋转。

## 时间尺度失真

在 1968 年，往返月球大约需要 *六天*。没有人愿意实时观看这个过程，所以模拟每次循环将时间单位增加 0.001。这意味着，当运行 4,100 次循环的模拟时，每个时间步代表现实世界中约 *两分钟* 的时间。时间步长越长，模拟速度越快，但结果的准确性越低，因为小错误会随着时间积累。

> 你可以通过首先运行一个小时间步以获得最大准确性，然后使用结果找到产生类似结果的最大时间步，来优化飞行计划模拟中的时间步。

## 距离尺度失真

模拟不是按比例的。虽然地球和月球的图像将具有正确的 *相对尺寸*，但它们之间的距离比实际距离 *更小*，因此需要相应调整相位角。我在模型中减少了距离，因为外太空很大。*真的很大*。如果你想按比例显示模拟并将其全部放在计算机显示器上，那么你必须接受一个荒谬的小地球和月球。

![](img/a0ae401098bb78923c426eb0d61b8f7d.png)

地球和月球系统在最近距离，或近地点，按比例显示（作者图像）

为了保持两个天体的可识别性，模拟使用了更大的、适当缩放的图像，并减少了它们之间的距离。这种配置将更容易让观众理解，同时仍然可以复现自由返回轨迹。

![](img/eade0181769047f74b7104b0a18d2860.png)

模拟中的地球和月球系统，仅按比例缩放了天体大小（真实世界 Python）

由于在模拟中地球和月球距离较近，月球的轨道速度将比实际生活中更快，这符合开普勒第二定律。为了补偿这一点，月球的起始位置被设计为适当减少相位角。

为了最终提高可视性，CSM 的大小被大幅增加。按比例显示时，它会比一个像素还小。

## 运行模拟

模拟追踪了阿波罗 8 从土星五号火箭的第三阶段停止点火进行月球注入直到指令模块在太平洋溅落的整个过程。初始加速代表了飞船在自由返回机动期间需要点燃推进器的唯一时间。

> 注意：下面的动画是 GIF 格式，根据 Medium 的要求。直接从 Python 脚本运行程序时，图像质量更好。

![](img/54b17f88e4c47401ba6f9ff09594c8b6.png)

自由返回轨迹模拟（作者）

如果你在 Python 脚本中取消注释 `self.pendown()` 选项，你可以通过在 CSM 和月球后面绘制一条线来追踪旅程。自由返回轨迹的“8”字形模式非常清晰可见，以及飞船接近月球时的情况，距离仅为 69 英里（110 公里）。

![](img/6a92dca9c7a0e9d0d68d06aaceab1a95.png)

模拟慢动作并记录路径（作者）

如果你调整模拟输入，你会发现对参数的最小调整可能会导致任务出现偏差。负面结果包括撞击月球、以过陡的角度进入地球大气层，或是永远飞向太空。

要模拟月球撞击，将代表 V₀月球注入速度的`Vo_X`变量从其初始值 485 更改为 475。

![](img/c394af77ffc8c824821d6251293d4767.png)

将 Vo_X 速度变量从 485 更改为 475 会导致 CSM 坠毁（作者）

尽管这种敏感性部分是由于小型模拟的自由度有限，但现实世界中也存在相同的问题。在关键时刻，太空旅行变成了秒和厘米的游戏，这也是为什么在轨道插入点燃等重要机动中需要计算机来控制推进器的原因。

## 模拟重力推进

迈克尔·米诺维奇解决三体问题的方案引领他发现了另一个重要的发现：*重力推进*。也称为*重力助推*和*弹弓机动*，该技术涉及利用大型行星的重力场来加速或减速航天器，使其进入不同的轨道。最著名的应用是在 1970 年代，将旅行者号航天器送往太阳系的壮丽旅行。

你可以通过将`Vo_X`变量设置为 520 到 540 之间的值来模拟重力推进。这会导致 CSM*在*月球后方通过，并偷取一些月球的动量，从而增加飞船的速度并改变其飞行轨迹。

![](img/cbb3755bc88dec10791931178cceabc8.png)

模拟重力推进概念。再见，阿波罗 8 号！（作者）

有趣的是，以这种方式加速 CSM 将*减缓*月球在其轨道上的速度，因为物理法则要求能量必须守恒。然而，由于两者质量的巨大差异，对月球的影响微不足道。

# 总结

本文提到的 Python 模拟允许你尝试著名的自由返回轨道，这一轨道使 NASA 决定冒险提前登月。模拟的好处在于，如果失败了，你还能活着再试。NASA 为所有拟议任务运行无数次模拟，结果帮助 NASA 在竞争的飞行计划之间做出选择，找到最有效的路线，决定在出现问题时该如何处理，等等。

模拟对于外太阳系探索尤其重要，因为巨大的距离使实时通信变得不可能。关键事件的时机，如点燃推进器、拍摄照片或释放探测器，都是基于详细的模拟预编程的。

# 参考文献及进一步阅读

[*现实世界的 Python: 黑客解决问题的代码指南*](https://a.co/d/9R11swV)（无淀粉出版社，2020）由李·沃恩编著，专门用一章详细解释自由返回模拟代码。

[*阿波罗 8 号: 第一次登月任务的惊险故事*](https://a.co/d/4P52WFk)（亨利·霍尔特公司，2017），由杰弗里·克卢格编著，涵盖了阿波罗 8 号历史性任务的从不太可能的开始到“不可思议的胜利”。

[*PBS Nova 阿波罗 8 号如何离开地球轨道*](https://www.pbs.org/wgbh/nova/video/how-nasas-apollo-8-left-earths-orbit/) 是一个关于阿波罗 8 号转月注入机动的短视频剪辑，标志着人类首次离开地球轨道，前往另一个天体。

[*NASA Voyager 1 & 2 维修手册*](https://a.co/d/bMEY9um)（海恩斯，2015），由克里斯托弗·赖利、理查德·科菲尔德和菲利普·道林撰写，提供了关于三体问题以及迈克尔·米诺维奇对太空旅行众多贡献的有趣背景。

维基百科的[*引力助推*](https://en.wikipedia.org/wiki/Gravity_assist)页面包含了许多有趣的引力助推机动和历史行星掠过的动画，这些动画可以通过本文提到的阿波罗 8 号模拟器来重现。

[*追逐新视野: 探索首个冥王星任务的史诗*](https://a.co/d/7PcNznK)（皮卡多出版社，2018），由艾伦·斯特恩和大卫·格林斯普恩编著，记录了模拟在 NASA 任务中的重要性和普遍性。

# 谢谢

感谢阅读，请关注我以获取更多未来的*快速成功数据科学*文章。
