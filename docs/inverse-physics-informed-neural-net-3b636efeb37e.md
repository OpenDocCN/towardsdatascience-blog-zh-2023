# 逆物理信息化神经网络

> 原文：[https://towardsdatascience.com/inverse-physics-informed-neural-net-3b636efeb37e?source=collection_archive---------2-----------------------#2023-03-27](https://towardsdatascience.com/inverse-physics-informed-neural-net-3b636efeb37e?source=collection_archive---------2-----------------------#2023-03-27)

## iPINN ~with code

## 解决逆微分方程问题

[![John Morrow](../Images/4a8ce62a0b4e1eb1cf77ecaba6b7ddcc.png)](https://medium.com/@john_morrow?source=post_page-----3b636efeb37e--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----3b636efeb37e--------------------------------) [John Morrow](https://medium.com/@john_morrow?source=post_page-----3b636efeb37e--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fb4bcd051bb38&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finverse-physics-informed-neural-net-3b636efeb37e&user=John+Morrow&userId=b4bcd051bb38&source=post_page-b4bcd051bb38----3b636efeb37e---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----3b636efeb37e--------------------------------) ·11 min read·2023年3月27日

--

![](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F3b636efeb37e&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Finverse-physics-informed-neural-net-3b636efeb37e&source=-----3b636efeb37e---------------------bookmark_footer-----------)![](../Images/2e92775d892c0d6963e6ba80092035bf.png)

照片由 [Daniele Levis Pelusi](https://unsplash.com/@yogidan2012?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

**(1) 引言：什么是物理信息化？**

物理学、生物学、化学、经济学、工程学等许多领域的关系由微分方程定义。（请参见[这里](https://en.wikipedia.org/wiki/List_of_named_differential_equations)获取详细列表。）一般来说，微分方程（DE）描述了变量如何受到其他变量变化率的影响。例如，微分方程解释了当质量在弹簧上振动时，其位置如何随时间变化，这与质量的速度和加速度相关。物理信息神经网络（PINN）生成遵循微分方程描述的关系的响应（无论研究对象是物理学、工程学、经济学等）。相比之下，逆物理信息神经网络（iPINN）作用于响应并确定产生该响应的微分方程的参数。PINN和iPINN通过在训练过程中包含一个约束来进行训练，该约束迫使神经网络的输入和输出之间的关系符合所建模的微分方程。

本文从PINN的实现开始，然后在PINN模型的基础上实现iPINN。对建模微分方程的解析解包括在内，以便与PINN和iPINN产生的响应进行比较。

**(2) 二阶微分方程**

本文重点讨论描述阻尼谐波运动的PINN和iPINN，例如带阻尼的弹簧-质量系统（图1）和由电阻、电感和电容（RLC）串联连接的组件组成的电子电路（图2）。

![](../Images/bc61d01bfaeaaae24a842f60ce35308f.png)

图1：**振动质量与弹簧**

![](../Images/b5b5b1212154e54a8958dc8d85c3f2f5.png)

图2：**RLC电路**

这些应用由二阶微分方程（DE）定义，其中包括相对于时间的二阶导数。方程1是弹簧-质量系统的二阶微分方程，其中参数m、c和k分别是质量、阻尼系数和弹簧常数。质量的位移由x表示，时间由t表示。x对t的二阶导数是质量的加速度，而一阶导数是质量的速度。

![](../Images/e60ec55bec5f64b3764e35c64ca0cb8d.png)

**方程1**

方程2是RLC电路的二阶微分方程，其中R、L和C分别是电阻、电感和电容。电路中的电流由i表示，时间由t表示。

![](../Images/4ce837c7a5433b71b222b50c8fc85b74.png)

**方程2**

这两个微分方程产生类似的响应，即质量从静止位置被位移后释放时的运动，以及在预充电电容器并施加初始电压后关闭开关时电流随时间的变化。下一节介绍RLC电路响应的详细信息。

**(3) RLC电路响应**

以下是图 2 中 RLC 电路可能响应的概述，包括每个响应的方程 2 的解析解方程。（作者提供的解析解推导可以在 [这里](https://github.com/jmorrow1000/PINN-iPINN/blob/main/RLC_response_Laplace_solutions.pdf?raw=true) 下载。）解析响应将在后续与 PINN 生成的和 iPINN 生成的响应进行比较。

根据组件的值，这个 RLC 电路可以产生三种不同类型的响应：欠阻尼、临界阻尼和过阻尼。所有这三种响应都基于在开关关闭之前电容器充电到电压 V₀ 和以下初始条件：

![](../Images/a498a6aba3ec6d8c74b4f78ff82522bb.png)

**方程 3**

![](../Images/aeef250d79309763bf6250afc6cccf11.png)

**方程 4**

**(3.1) 欠阻尼响应**

当 R、L 和 C 的值产生以下条件时，会发生欠阻尼响应：

![](../Images/d4a259e5229339f75ab8a576b4911997.png)

**方程 5**

例如，设 R = 1.2（欧姆），L = 1.5（亨利），C = 0.3（法拉），V₀ = 12（伏特）。这些值的方程 2 的解析响应为：

![](../Images/0613eb2d2e99041dc5e7d015ac470e21.png)

**方程 6**

以下是方程 6 的响应图。

![](../Images/c5ddc5d19e742bafe613a1ae968d0aa9.png)

图 3: **欠阻尼响应**

**(3.2) 临界阻尼响应**

当 R、L 和 C 的值产生以下条件时，会发生临界阻尼响应：

![](../Images/14b1fda78c81563e855f75a1d55069dc.png)

**方程 7**

例如，设 R = 4.47（欧姆），L = 1.5（亨利），C = 0.3（法拉），V₀ = 12（伏特）。这些值的方程 2 的解析响应为：

![](../Images/50773846c1c8db0713fc07ee3d1387df.png)

**方程 8**

以下是方程 8 的响应图。

![](../Images/6cd77ecdb7016968b9cf77dd577d97dc.png)

图 4: **临界阻尼响应**

**(3.3) 过阻尼响应**

当 R、L 和 C 的值产生以下条件时，会发生过阻尼响应：

![](../Images/ddb0c4454e0d9649afa987bf9cbccef2.png)

**方程 9**

例如，设 R = 6.0（欧姆），L = 1.5（亨利），C = 0.3（法拉），V₀ = 12（伏特）。这些值的方程 2 的解析响应为：

![](../Images/25770dc13c36594089b9d52b189c76a8.png)

**方程 10**

以下是方程 10 的响应图。

![](../Images/2f33e5d568110768bdc8bb590d19a554.png)

图 5: **过阻尼响应**

**(4) PINN 结构**

通常，神经网络使用已知的输入和输出数据对进行训练。训练输入数据被输入神经网络，产生的输出与训练输出数据通过损失函数进行比较。此函数返回的损失值通过反向传播调整网络权重，以减少损失。PINNs和iPINNs使用自定义损失函数，包括额外的损失组件，以约束神经网络生成符合所建DE的输出。

一个DE的PINN模型在方程2中将时间 *t* 作为神经网络的输入，输出对应的电流 *i*。训练PINN以符合DE需要计算输出相对于输入的第一和第二导数，即 *di/dt* 和 *d²i/dt²*。这些导数在TensorFlow和PyTorch中通过各个平台的自动微分功能获得。本文中的PINN和iPINN是通过TensorFlow的[GradientTape](https://www.tensorflow.org/guide/autodiff)开发的。

对于每个PINN训练输入，GradientTape计算的第一和第二导数与 *R*、*L* 和 *C* 按照方程2中的DE组合，产生的结果应为零。实际结果与零之间的差异称为残差。残差成为用于训练PINN的损失函数的一个组成部分。

二阶DE，如方程2，还要求解符合两个初始条件。在这种情况下，第一个条件是 *t = 0* 时 *i* 的值（方程3），第二个条件是 *t = 0* 时 *di/dt* 的值（方程4）。每个初始条件都作为损失函数的一个组成部分。

图6展示了总损失的组成。损失2来自残差。损失1和损失3来自初始条件。在训练过程中，反向传播用于减少总损失。PINN的 *d²i/dt²* 和 *di/dt* 输出由GradientTape提供。

![](../Images/0069146d0559d5725933f0ae3764758f.png)

图6: **PINN损失函数**

**(5) PINN实现**

以下是PINN实现的python代码。完整的PINN实现代码可以在[这里](https://github.com/jmorrow1000/PINN-iPINN)找到。

**(5.1) 神经网络模型定义**

PINN的神经网络具有两个全连接的隐藏层，每层有128个神经元。时间点有一个输入，响应点有一个输出。

列表1: **PINN TensorFlow模型**

**(5.2) PINN初始化**

在PINN模型中，*R*、*L*和*C*组件值以及初始电容电压（第2-5行）是决定DE响应的常数。共同位置点，指定在时间域中（第8行），是计算残差的点。初始条件（第11和15行）来自方程3和方程4。

列表2: **PINN初始化**

**(5.3) PINN训练步骤**

以下是训练步骤函数的 Python 代码。对于每个训练批次，步骤函数计算三部分损失，然后使用总损失更新神经网络中的权重。

**损失 1：** 方程 3 的初始条件与网络输出 *pred_y*（第 9 行）进行比较。差异的平方是 *model_loss1*（第 10 行）。

**损失 2：** 共定位点（第 30 行）计算残差。它使用来自 GradientTape 的一阶梯度 *dfdx*（第 17 行）和二阶梯度 *dfdx2*（第 26 行），以及网络输出 *pred_y*（第 29 行），来计算方程 2 的左侧。这一值的平方为 *model_loss2*（第 31 行）。

**损失 3：** 方程 4 的初始条件将 L 与一阶梯度 *dfdx*（第 17 行）的乘积与 v_init2 比较。差异的平方为 *model_loss3*（第 19 行）。

三个损失组件的总和，*model_loss*（第 35 行），用于计算损失相对于神经网络权重的梯度（第 38 行）。优化器随后更新权重（第 41 行）。

清单 3：**PINN 训练步骤**

**(6) PINN 结果**

下面是对三个测试案例训练 PINN 的结果。这些测试条件如第 3 节所述：欠阻尼、临界阻尼和过阻尼。每个图展示了三个轨迹：

+   解析方程的响应（蓝色）

+   共定位点（绿色）

+   训练好的 PINN 的输出响应（红色）

**欠阻尼测试案例：**

![](../Images/8259d6aad4b1dfb4c5c07e6b54ada22f.png)

图 7：**欠阻尼响应**

**临界阻尼测试案例：**

![](../Images/f918185248f2bcfa0784383bb5ffa32d.png)

图 8：**临界阻尼响应**

**过阻尼测试案例：**

![](../Images/66d01c13d36c5842087c8587941efdae.png)

**(7) iPINN 结构**

图 10 说明了总损失的组成。与 PINN 模型类似，损失 2 来自残差，只是 *R*、*L* 和 *C* 是在训练过程中确定的变量。相比之下，在 PINN 模型中，*R*、*L* 和 *C* 是常数。与 PINN 一样，损失 1 和损失 3 强制遵守方程 3 和方程 4 的初始条件。

iPINN 模型包括一个额外的损失函数，损失 4，强制输出响应与待调查的微分方程响应匹配。

![](../Images/009c851fccaea1e364c7be30e965998e.png)

图 10：**iPINN 损失函数**

**(8) iPINN 实现**

以下是 PINN 实现的 Python 代码。iPINN 实现的完整代码可以在[这里](https://github.com/jmorrow1000/PINN-iPINN)找到。

iPINN 的神经网络模型定义与 PINN 网络（第 5.1 节）相同，即两个完全连接的隐藏层，每层有 128 个神经元。输入为时间点，输出为响应点。

**(8.1) iPINN 初始化**

被研究的微分方程的响应被加载在第4行。两个初始条件（第9行和第13行）与PINN模型中的相同。如上所述，*R*、*L*和*C*是iPINN模型中的可训练变量（第18–20行）。

**(8.2) iPINN 训练步骤**

**损失 1：** 方程3的初始条件与网络输出*pred_y*（第10行）进行比较。差异的平方为*model_loss1*（第11行）。

**损失 2：** 与PINN训练步骤函数一样，残差（第34行）在由*t_coloc*定义的共定位点处计算，产生*model_loss2*（第35行）。*R*、*L*和*C*是可训练变量。

**损失 3：** 方程4的初始条件将可训练变量*L*与一阶梯度*dfdx*（第18行）的乘积进行比较，以* v_init2*为标准。差异的平方为*model_loss3*（第20行）。

**损失 4：** 该损失组件将网络输出（第39行）与被研究的微分方程的响应*i_coloc*进行比较，产生*model_loss4*（第40行）。

四个损失组件的总和，*model_loss*（第43行），用于计算损失相对于神经网络权重和三个可训练变量：*R*、*L*和*C*（第49行）的梯度。然后优化器更新网络的权重（第52行），并在第53–55行更新*R*、*L*和*C*的值。

**(9) iPINN 结果**

以下是使用iPINN识别三个未知测试响应的结果。呈现给iPINN的测试响应是根据第3节的条件生成的：欠阻尼、临界阻尼和过阻尼。下表比较了用于生成测试响应的*R*、*L*和*C*组件值与iPINN确定的值。每个图表呈现三个轨迹：

+   解析方程的响应曲线（蓝色）

+   需要由iPINN识别的响应数据（60点）（绿色）

+   训练后的iPINN输出响应（红色）

**欠阻尼测试案例：**

![](../Images/eac1f6e7db6392560e5935420c62ba21.png)

表1：**欠阻尼测试案例**

![](../Images/eede3a0349f29cf3456f7b807fabef8b.png)

图11：**欠阻尼响应**

**临界阻尼测试案例：**

![](../Images/4f34b9f789f9aacee47b65aa53263268.png)

表2：**临界阻尼测试案例**

![](../Images/10b1e4c7fdc01752f4d13b1193c5975c.png)

图12：**临界阻尼响应**

**过阻尼测试案例：**

![](../Images/9206967325eb2179483ddcf1ee603702.png)

表3：**过阻尼测试案例**

![](../Images/475b3a56431ef0faf674af074c57c6c6.png)

图13：**过阻尼响应**

**(10) 结论**

这项研究展示了神经网络可以成功求解微分方程，这些方程描述了科学、工程和经济学众多领域中的许多关系。训练了一个物理信息神经网络来解决电子电路的二阶微分方程，结果是一个能够对输入信号产生与实际电路相同响应的神经网络。

这项研究还展示了神经网络能够确定未知微分方程的参数。具体而言，训练了一个逆物理信息神经网络，通过仅使用来自电路的样本响应来确定电子电路的未知组件值。此外，在确定未知组件值后，得到的神经网络能够对输入信号产生与实际电路相同的响应。

**参考文献**

[1] M. Raissi, P. Perdikaris, 和 G. E. Karniadakis, “物理信息深度学习（第一部分）：非线性偏微分方程的数据驱动解法”，2017\. [在线]. 网址: https://arxiv.org/abs/1711.10561

[2] M. Raissi, P. Perdikaris, 和 G. E. Karniadakis, “物理信息深度学习（第二部分）：非线性偏微分方程的数据驱动发现”，2017\. [在线]. 网址: [https://arxiv.org/abs/1711.10566](https://arxiv.org/abs/1711.10566)

**这篇文章的pdf版本可以** [**在这里**](https://github.com/jmorrow1000/PINN-iPINN/blob/main/inverse_physics_informed_neural_net.pdf?raw=true)**下载**。

*除非另有说明，所有图片均由作者提供。*
