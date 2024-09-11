# 微型神经网络用于吉他效果与微控制器

> 原文：[https://towardsdatascience.com/mini-neural-nets-for-guitar-effects-with-microcontrollers-ea9cdad2a29c?source=collection_archive---------1-----------------------#2023-04-07](https://towardsdatascience.com/mini-neural-nets-for-guitar-effects-with-microcontrollers-ea9cdad2a29c?source=collection_archive---------1-----------------------#2023-04-07)

![](../Images/277de6904d28ad22861606c6e0892e3a.png)

我的自制数字吉他效果器使用神经网络（图片由作者提供）

## *少即是多*

[](https://keyth72.medium.com/?source=post_page-----ea9cdad2a29c--------------------------------)[![Keith Bloemer](../Images/3e8d884c0f32430286599b8ffb7b249c.png)](https://keyth72.medium.com/?source=post_page-----ea9cdad2a29c--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ea9cdad2a29c--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ea9cdad2a29c--------------------------------) [Keith Bloemer](https://keyth72.medium.com/?source=post_page-----ea9cdad2a29c--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F744166a323a0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmini-neural-nets-for-guitar-effects-with-microcontrollers-ea9cdad2a29c&user=Keith+Bloemer&userId=744166a323a0&source=post_page-744166a323a0----ea9cdad2a29c---------------------post_header-----------) 发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ea9cdad2a29c--------------------------------) ·7 min read·Apr 7, 2023[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fea9cdad2a29c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmini-neural-nets-for-guitar-effects-with-microcontrollers-ea9cdad2a29c&user=Keith+Bloemer&userId=744166a323a0&source=-----ea9cdad2a29c---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fea9cdad2a29c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fmini-neural-nets-for-guitar-effects-with-microcontrollers-ea9cdad2a29c&source=-----ea9cdad2a29c---------------------bookmark_footer-----------)

我逐渐意识到，限制可以驱动创造力和创新。艺术家拥有的选择越多，想出一个可靠的想法可能就越困难。如果没有音乐的规则，可能性会如此庞大，以至于几乎不可能写出可听的作品。正是这些规则和限制为一个想法提供了结构。当然，知道什么时候打破这些规则与知道它们是什么一样重要。

在过去的三年里，我一直对使用深度神经网络模拟模拟吉他放大器和效果器着迷。我编写了一些 [音频插件](https://github.com/GuitarML)，并测试了无数种神经网络变体用于模拟吉他设备。神经网络对 CPU 的需求很高，但使用 PC 时，你通常可以增加计算机的处理能力以实现更准确的声音。

*有关更多信息，请参见我的文章* [*实时音频的神经网络*](https://medium.com/nerd-for-tech/neural-networks-for-real-time-audio-introduction-ed5d575dc341)*.*

但如果你想在一个便宜的微控制器上运行神经网络，使用一个小型（吉他效果踏板大小）的设备，专门用于运行音频效果而不做其他事情呢？像 [Raspberry Pi](https://www.raspberrypi.org/) 这样的微型计算机足够强大，可以运行操作系统，支持应用程序和吉他效果，还有多余的空间，这意味着更高成本下的资源浪费。这些计算机使用微处理器，而不是微控制器。微-*处理器* 具有外部内存和 I/O，而微-*控制器* 是一个自包含的计算系统，可以独立运行程序。微控制器通常不需要像 PC 或智能手机那样强大，因为它专门用于做一件事。微控制器非常适合数字吉他踏板，但为它们编写软件与智能手机或 PC 是不同的，这是我以前从未做过的。

![](../Images/719b95227fbe9fe0b9ec78ee62895c3f.png)

部分构建的 Terrarium PCB（左）和 Daisy Seed（右）（图片作者提供）

于是，[Daisy Seed](https://www.electro-smith.com/daisy/daisy) 登场了！Daisy Seed 是一个 $30 的音乐嵌入式平台，利用 Cortex-M7 ARM 微控制器。它有自己的软件库 ([libDaisy](https://github.com/electro-smith/libDaisy)) 和音频 DSP 库 ([DaisySP](https://github.com/electro-smith/DaisySP))，都是开源的。对于吉他踏板，你需要一个输入音频缓冲区，用于将模拟吉他信号连接到数字硬件，还有一个输出缓冲区，用于与放大器匹配阻抗。这确保了你的音色在经过效果器时得以保留。PedalPCB 销售一种名为 [Terrarium](https://www.pedalpcb.com/product/pcb351/) ($12) 的接口板，专门为 Daisy Seed 设计，甚至提供了最多 6 个旋钮、4 个开关、2 个脚踏开关、输入/输出单声道音频插孔和 9 伏电源（吉他效果器常用）的连接。

以 42 美元的音频硬件成本，我的成本已经远低于我之前使用的 [NeuralPi 吉他踏板](https://medium.com/towards-data-science/neural-networks-for-real-time-audio-raspberry-pi-guitar-pedal-bded4b6b7f31) 的 Raspberry Pi4。我购买了电路板，得益于我之前对模拟效果的痴迷，手头有足够的硬件来构建 Terrarium 踏板。总成本约为 100 美元（包括外壳、电气组件、旋钮/开关），需要基本的焊接技能来组装。[其他现有项目](https://github.com/tnatoli/Sonic_Daisy/tree/main/rhythm_delay) 在完成的 Terrarium 踏板上运行良好，声音也很棒。

![](../Images/fb0cbf0832b865b93fa315e6a6d0c7ee.png)

我完成的 Terrarium 踏板。（图片作者提供）

不过，我的 [NeuralPi](https://github.com/GuitarML/NeuralPi) 插件无法在微控制器上运行，所以我需要发挥创造力。我决定使用 [RTNeural](https://github.com/jatinchowdhury18/RTNeural) 引擎来运行神经模型，并使用 libDaisy 和 DaisySP 开发最小的 c++ 代码。虽然有几个代码库可以用于 M 系列微控制器的开发，但我没有这些的经验，所以如果可能的话，我希望坚持使用我已经熟悉的。

现在谈谈我提到的那些限制：处理能力和内存。M7 芯片的最大运行频率为 480MHz，在这个速度下只能使用 Flash 内存，一个只有 128KB 的小内存！（但公平地说，[登月只用了 74KB](https://www.independent.co.uk/news/science/apollo-11-moon-landing-mobile-phones-smartphone-iphone-a8988351.html)）。Daisy Seed 上还有其他可以访问的内存区域，但我希望最大化我的处理速度，以运行尽可能大的神经网络，产生最佳的声音。

当我最初编译我的最小 c++ 程序与 RTNeural 时，我超出了 128KB 的限制。因此，我向 RTNeural 的创建者寻求帮助（正如我以前多次做的那样！）。他成功地将编译后的占用量减少了大量，为所需的代码和模型文件腾出了足够的空间。

![](../Images/1e283ece1894dd7e9cc6518249b2e90f.png)

Daisy Seed 通过 USB 连接以上传 c++ 程序（图片作者提供）

一旦我的程序可以加载到 Daisy Seed 上，我非常好奇它能处理多大尺寸的模型。我从 NeuralPi 模型（LSTM 尺寸 20）开始，但这太过庞大了，导致踏板没有响应。然后我决定从小开始，一步步达到极限，因此我使用了 LSTM 尺寸 4，成功了！它可以运行效果（捕获了我的 TS-9 过载踏板），但我对准确性不满意。我确定它能处理的最大尺寸是 LSTM 尺寸 7，这对大多数踏板来说足够好，可能在捕获低增益放大器时能获得一些不错的声音。请记住，Daisy Seed 运行在 48kHz 的音频下，这意味着每秒通过神经网络 48,000 次！

这种神经网络采样率在任何系统上都令人印象深刻，但我希望它能听起来更*好*。近年来的吉他手已经习惯于与真实效果难以区分的数字建模。像 NeuralDSP 的 Quad Cortex、IK Multimedia 的 ToneX 和最近的 Headrush Prime 等产品给吉他手提供了以前只有使用 Kemper 才能实现的选项。尽管我的工作专门针对 DIY 社区，但这并不意味着它的能力要逊色！ （正如最近开源的 [Neural Amp Modeler](https://github.com/sdatkinson/NeuralAmpModelerPlugin) 所证明的那样。）

除了 LSTM 外，还有一种我尚未测试的递归神经网络（RNN），即 GRU（门控递归单元）。该网络在相同内部大小的情况下使用的处理能力约少 33%。我并没有进行过多测试，因为在 PC 上，额外的处理能力并不是一个大问题。

初步测试 GRU 是令人鼓舞的，我在 GRU 上得到的损失值甚至比相同大小的 LSTM 还要低。除了运行速度更快外，GRU 模型还消耗了更少的内存。我通过反复试验确定，大小为 10 的 GRU 大约是我在 Daisy seed 上可以通过 RTNeural 运行的最大值（相比之下，LSTM 为 8），这种差异在某些情况下将损失值降低了一半。我在失真效果器上的损失值低于 0.01，在低增益放大器上的损失值低于 0.02。

在当前版本中，我命名的 NeuralSeed 能够准确捕捉大多数失真/过驱动效果器（最多支持 3 个参数化旋钮）和低增益放大器。使用无参数化旋钮仍然是最准确的，但在我看来，增益和音调的控制灵活性是一个很酷的选项。我之前学习的技巧有助于提高整体音质。这些技巧包括对录音设备的改进，例如使用负载箱录音和使用重新放大箱对效果器进行录音。从一个预训练模型（[迁移学习](https://medium.com/towards-data-science/transfer-learning-for-guitar-effects-4af50609dce1)）开始，并使用专门的输入信号，有助于提高模型训练的准确性。通过使用一种叫做“[蒸馏](https://en.wikipedia.org/wiki/Knowledge_distillation)”（从大模型训练小模型）的方法，我能够捕捉到低到中等增益下管式放大器的复杂失真。

这里是 GitHub 上的开源代码。当加载到 Terrarium 效果器上时，它具有输入/输出电平旋钮、湿/干混音器、4 个 EQ 增益开关，以及最多 3 个旋钮用于控制一个参数化神经模型（例如增益、低音和高音）。左脚踏开关可以旁路或激活效果。右脚踏开关在可用的神经放大器/效果器模型之间循环切换。

[](https://github.com/GuitarML/NeuralSeed?source=post_page-----ea9cdad2a29c--------------------------------) [## GitHub - GuitarML/NeuralSeed: 神经网络用于 Daisy Seed 上的吉他放大器/效果器模拟

### Neural Seed 利用神经网络在 Electro-Smith 的 Daisy Seed 硬件上模拟放大器/踏板，以及 Terrarium……

[github.com](https://github.com/GuitarML/NeuralSeed?source=post_page-----ea9cdad2a29c--------------------------------)

为 Daisy Seed 开发让我更加欣赏现代计算机的强大。M7 处理器（一个非常强大的微控制器！）的限制让我突破思维定式，用更少的资源实现更多的功能。NeuralSeed 代码是开源的，任何人都可以在 Daisy Seed / Terrarium 踏板上尝试、修改和进行自己的调整和改进。此外，还有一个 [Colab 脚本](https://colab.research.google.com/github/GuitarML/Automated-GuitarAmpModelling/blob/ns-capture/ProteusCapture.ipynb) 用于训练自己的模型以与 NeuralSeed 软件配合运行。这里有一个视频演示，展示了完成的 Neural Seed 踏板运行多个实际吉他放大器和踏板的神经网络模型。

Neural Seed 演示（作者视频）

*特别感谢* [*ChowDSP*](https://chowdsp.com/) *的 Jatin，他帮助我减少了 RTNeural 的足迹，并解决了 GRU 实现问题！也感谢 Daisy Discord 社区的朋友们，他们帮助我了解 Daisy Seed 和相关软件，以及 Neural Amp Modeler Facebook 群组提供的训练数据。*
