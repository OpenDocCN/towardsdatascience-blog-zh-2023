# 强化学习能否超越训练泛化？

> 原文：[https://towardsdatascience.com/can-reinforcement-learning-generalize-beyond-its-training-3b9012d8e4cf?source=collection_archive---------20-----------------------#2023-01-24](https://towardsdatascience.com/can-reinforcement-learning-generalize-beyond-its-training-3b9012d8e4cf?source=collection_archive---------20-----------------------#2023-01-24)

## 模型泛化案例研究

[](https://medium.com/@john_morrow?source=post_page-----3b9012d8e4cf--------------------------------)[![John Morrow](../Images/4a8ce62a0b4e1eb1cf77ecaba6b7ddcc.png)](https://medium.com/@john_morrow?source=post_page-----3b9012d8e4cf--------------------------------)[](https://towardsdatascience.com/?source=post_page-----3b9012d8e4cf--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3b9012d8e4cf--------------------------------) [John Morrow](https://medium.com/@john_morrow?source=post_page-----3b9012d8e4cf--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb4bcd051bb38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-reinforcement-learning-generalize-beyond-its-training-3b9012d8e4cf&user=John+Morrow&userId=b4bcd051bb38&source=post_page-b4bcd051bb38----3b9012d8e4cf---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b9012d8e4cf--------------------------------) ·6 min read·2023年1月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F3b9012d8e4cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-reinforcement-learning-generalize-beyond-its-training-3b9012d8e4cf&user=John+Morrow&userId=b4bcd051bb38&source=-----3b9012d8e4cf---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b9012d8e4cf&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcan-reinforcement-learning-generalize-beyond-its-training-3b9012d8e4cf&source=-----3b9012d8e4cf---------------------bookmark_footer-----------)![](../Images/7c6eb049b8373330c9019cd75174f39e.png)

[János Szüdi](https://unsplash.com/@szudi?utm_source=medium&utm_medium=referral)拍摄于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

论文中详细描述的项目，[强化学习：模型泛化案例研究](https://github.com/jmorrow1000/RL-generalize/blob/main/Reinforcement%20Learning%20-%20A%20Case%20Study%20in%20Model%20Generalization.pdf?raw=true)，探讨了一个使用强化学习（RL）训练的模型在泛化能力上的表现，即当遇到训练中未接触的数据时，能够产生可接受的结果。本研究中的应用是一个具有多个控制的工业过程，这些控制决定了产品在过程中的过渡效果。在这种环境中确定最佳控制设置可能具有挑战性。例如，当控制之间存在相互作用时，调整一个设置可能需要重新调整其他设置。此外，控制与其效果之间的复杂关系使得找到最佳解决方案变得更加困难。这里展示的结果表明，经过RL过程训练的模型在这种环境下表现良好，并能够泛化到不同于训练条件的情况下。

[论文](https://morrowconsultants.com/rl-model-generalization-paper)描述了一种RL模型，该模型被训练以寻找用于将电子组件焊接到电路板上的回流焊炉的最佳控制设置（图1）。烤箱的移动传送带将产品（即电路板）运输通过多个加热区域。此过程根据所需的温度-时间目标曲线加热产品，以生产可靠的焊接连接。

![](../Images/449434f952710cedc7ac09afa5d05594.png)

图1：**烤箱传送带上的电路板**（*图像来自Adobe，授权给John Morrow*）

人工操作员通常采取以下步骤来确定成功焊接电路板所需的加热器设置：

• 运行一次产品通过烤箱的过程

• 观察传感器读数中的温度-时间曲线

• 调整加热器设置以改善曲线，接近目标曲线

• 等待烤箱温度稳定到新的设置

• 重复此过程，直到传感器读数中的曲线足够接近目标曲线

**学习策略**

一个RL系统用两阶段过程取代了操作员步骤。在第一阶段，代理学习烤箱的动态，并在各种烤箱条件下创建更新加热器设置的策略。

由于在改变加热器设置并将产品通过烤箱后，需要相当长的时间来稳定烤箱的温度，因此使用了烤箱模拟器以加速学习过程。模拟器在几秒钟内模拟了产品通过加热曲线的单次过程，而物理烤箱则需要许多分钟。（[第二部分](https://medium.com/@john_morrow/can-reinforcement-learning-generalize-beyond-its-training-part-2-79d7b864dc55)提供了模拟器的详细信息。）

在每一轮学习阶段中，代理从其当前状态采取行动，向模拟器发送八个加热器的新设置。模拟运行后，模拟器报告产品温度读数（每秒采集三百个读数）。

代理的奖励基于返回读数与目标温度-时间曲线之间的差异。如果当前运行的差异小于之前的差异，则奖励为正；否则，奖励为负。一部分读数决定系统的新状态。代理通过从新状态中采取行动来开始学习阶段的下一轮。

**使用策略进行规划**

在第二阶段，代理按照学习到的策略寻找最佳加热器设置。这些设置将使实际产品曲线与目标温度-时间曲线之间的匹配最接近。图2展示了代理遵循策略找到最佳设置的结果。蓝色轨迹是目标温度-时间曲线，红色轨迹是由最佳设置产生的实际曲线。

![](../Images/ae5249872da7b2f455c158de0603140e.png)

图2：**示例规划结果 蓝色轨迹：目标曲线。红色轨迹：实际产品曲线。**

**强化学习系统**

如上所述，RL系统包括一个代理在环境中采取行动，以学习一个策略来实现目标。环境对每个行动作出回应，提供奖励以指示行动是否有助于实现目标。环境还会返回代理在环境中的状态。代理由两个神经网络组成：模型网络和目标网络。代理的目标是找到加热器设置，使得生产的产品时间-温度曲线与目标曲线非常接近。环境是回流炉模拟器。图3显示了RL系统的组成部分，文中对每个部分进行了详细描述。

![](../Images/67227dda344339d0894f8f6dfddb2cb8.png)

图3：**强化学习系统**

**泛化：状态和奖励定义**

状态和奖励定义对于RL模型在目标曲线和产品参数与训练期间使用的参数不同的新环境中的泛化能力至关重要。具体来说，状态和奖励都是根据产品和目标曲线温度之间的相对差异定义的，并通过允许的加热器值的最大范围进行规范化。

状态参数在八个加热器区域的中心定义。每个状态参数被定义为产品中心温度与每个加热器区域中心温度的规范化差异。

当代理执行一个动作时，环境会返回一个奖励，表明该动作在实现代理目标方面的有效性。奖励是基于该动作是否减少了实际温度与目标曲线之间的总温差。状态和奖励函数在论文中有更详细的描述。

**结果**

以下是论文中在不同产品材料配置和温度-时间曲线下运行规划过程的两个测试结果。所有测试都使用了一个模型神经网络，该网络经过以下产品和曲线参数的训练：

![](../Images/65ee37070838ea349f597be3f2bedfad.png)

**测试 1：基准测试**

测试 1 作为测试模型性能的基准，使用与训练模型时相同的参数。以下是测试 1 的误差、最佳热区设置以及目标曲线与实际温度-时间图：

![](../Images/3e4804da2b61492040372400ab03a69e.png)

**测试 1：蓝色曲线：目标曲线。红色曲线：实际产品曲线。**

**测试 6**

测试 6 将产品从 FR4 更改为氧化铝（铝土矿 99%），更改了产品的尺寸，并且更改了曲线。烤箱参数值与测试 1 的基准相同，只是顶部和底部加热元件都处于活动状态。以下表格反映了该测试所用的曲线和产品参数（相对于基准训练参数的更改为粗体）：

![](../Images/ed10da52ad97359a26fb5aee47904a36.png)

以下是测试 6 的误差、最佳热区设置以及目标曲线与实际温度/时间图：

![](../Images/22a233e42efc39a0548e61a13f3038d3.png)

**结论**

本项目展示了强化学习系统如何为控制复杂的工业过程提供解决方案。具体来说，强化学习系统成功学习了用于将电子组件焊接到电路板上的回流焊炉的最佳控制设置。此外，一旦训练完成，系统可以泛化到在与训练时不同要求的环境中产生可接受的结果。

论文链接：[强化学习：模型泛化的案例研究](https://github.com/jmorrow1000/RL-generalize/blob/main/Reinforcement%20Learning%20-%20A%20Case%20Study%20in%20Model%20Generalization.pdf?raw=true)

除非另有说明，所有图片均为作者所用。
