# 幽灵图像与量子位：可视化量子叠加的新方法

> 原文：[`towardsdatascience.com/ghostly-images-and-qubits-a-new-way-to-visualize-quantum-superposition-94b582889549`](https://towardsdatascience.com/ghostly-images-and-qubits-a-new-way-to-visualize-quantum-superposition-94b582889549)

## 教程

## 探索量子计算的奥秘！

[](https://medium.com/@KoryBecker?source=post_page-----94b582889549--------------------------------)![Kory Becker](https://medium.com/@KoryBecker?source=post_page-----94b582889549--------------------------------)[](https://towardsdatascience.com/?source=post_page-----94b582889549--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----94b582889549--------------------------------) [Kory Becker](https://medium.com/@KoryBecker?source=post_page-----94b582889549--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----94b582889549--------------------------------) ·阅读时间 10 分钟·2023 年 3 月 9 日

--

![](img/6322d4be6fdff1aabc739218f6b977ee.png)

来源：[Stable Diffusion](https://stablediffusionweb.com)。

# 我一直喜欢创建游戏

每当我学习一种新的编程语言或框架时，我经常会尝试编写一个[游戏](https://medium.com/towards-data-science/the-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457)作为初次使用该平台各种功能的尝试。

在量子计算的情况下，为了学习这些概念，创建一个[游戏的开始](https://medium.com/towards-data-science/programming-in-3d-my-first-steps-into-quantum-computing-566b9b93929d)是绝对完美的。

现在，量子计算有许多主题和特性需要学习。这些可能包括量子位的制作方法、量子位的[操控](https://medium.com/towards-data-science/programming-in-3d-my-first-steps-into-quantum-computing-566b9b93929d)、各种门，但本教程的重点将是**叠加**这一全能概念！

# 令人难以置信的量子位

叠加是编程量子计算机的核心特性之一，因为它允许量子位同时保存 0 和 1 的值。

当一个量子位处于叠加状态时，它在量子态中运行。这使它变得强大！

# 指数级处理能力

一个具有一个比特的经典计算机可以处理一个比特的信息（*0* ***或*** *1*）。相比之下，一个具有一个量子位的量子计算机可以处理两个比特的信息（*0* ***和*** *1*）。

类似地，两个经典比特可以处理两个比特的信息，而两个量子位则扩展到处理四个比特的信息（*00, 01, 10, 11*）。

最终，这使得具有 n 个量子位的量子计算机可以在一个 CPU 周期内处理 2^n 位信息。

![](img/204aa50228e7b6a2acfb1f1d65a50084.png)

一个具有 n 个量子位的量子计算机可以在每个周期处理 2^n 个状态。来源：作者。

考虑到一个仅有 55 个量子位的量子计算机可以在一个周期内处理近 5 PB (*2⁵⁵ bits*)的信息，这确实是非常惊人的。

> 这可能是你最喜欢的社交媒体网站的整个数据库，一瞬间处理完成！

# 这真是太多了！

确实，这就是经典计算机和量子计算机之间的惊人差异，它是由于量子位在叠加态中的同时存在性，也称为[量子并行性](https://www.sciencedirect.com/topics/mathematics/quantum-parallelism)。

实际上，叠加态允许通过利用这一独特的量子能力，从算法中获得平方和指数级的性能提升。

当然，对于我们的项目，我们只需要一个量子位。

# 经典计算机上的量子位

为了解释和使用量子位的值，我们必须对其进行测量。

测量量子位的这一简单行为导致量子态的坍缩，这将量子位从同时持有 0 和 1 的值变为仅持有 0 或 1 的值——就像经典比特一样。

由于测量的量子位现在可以被解释为 0 或 1 的值，我们可以像使用经典计算机一样使用量子位。

*记住这一点，因为这就是乐趣的开始！*

# 独角兽和小鬼

量子位叠加态的可视化通常使用各种[图示](https://quantum-computing.ibm.com/composer/docs/iqx/visualizations)，包括：q-球体、相位盘和状态向量。

不过，我想尝试一些更有趣的东西！

由于处于叠加态的量子位将同时保持 0 和 1 的值，我认为将两幅图像叠加在一起会很有趣。

![](img/6e5ce0558948fedfa4972580765158fe.png)

根据量子位的测量来叠加的两幅图像。来源：[Stable Diffusion](https://stablediffusionweb.com/)。

每幅图像将代表量子位可能处于的 0 或 1 状态。根据量子位在[计算基态](https://www.quantum-inspire.com/kbase/qubit-basis-states/)的测量以及量子位测量为 0 或 1 的概率，我们可以调整两幅图像的透明度（或阿尔法混合），以便更突出地显示其中一幅。

将图像叠加在一起的动作有助于创建一个美丽的效果，展示量子位的行为。

# 幽灵图像

置于叠加态的量子位将有 50%的默认测量机会得到 0 和 50%的机会得到 1。

处于叠加态的量子位本质上是随机的。

然而，我们可以部分地将量子位反转到其中一个值。这导致量子位更频繁地测量为 0 或 1，具体取决于施加的反转量。

我们将使用量子计算的这一特性来控制图像的透明度。为此，我们需要使用一种特殊的量子门。

# 当 X-gate 还不够时

通过使用[X-gate](https://qiskit.org/documentation/stubs/qiskit.circuit.library.XGate.html)，量子比特可以从 0 值翻转为 1，反之亦然。

X-gate 就像经典计算机的 NOT 运算符。它只是简单地翻转量子比特的值。

然而，我们不希望只是将量子比特从 0 翻转为 1，因为那样会立即将我们的图像从一种状态翻转到另一种状态，中间没有任何过渡。相反，我们希望显示从一种图像到另一种图像的渐变，以及所有中间状态。

幸运的是，量子工具包中不仅仅有几种[门](https://qiskit.org/textbook/ch-states/single-qubit-gates.html)。

# 三维量子比特

一个量子比特包含三个影响其测量行为的轴。这些包括 X、Y 和 Z 轴。

![](img/f141ddfb0060cf73105257ab02aeb16c.png)

[Smite-Meister](https://commons.wikimedia.org/wiki/File:Bloch_sphere.svg)，[CC BY-SA 3.0](https://creativecommons.org/licenses/by-sa/3.0)，通过维基媒体共享资源。

某些量子门在不同轴上以不同的方式旋转，这最终会影响量子比特的测量结果。

例如，X-gate 在 X 轴周围进行 180 度旋转，将量子比特值从 0 翻转为 1（或从 1 翻转为 0）。

还有 Y 和 Z-gates，它们以类似的方式绕各自的轴旋转。

这些门对量子比特测量的结果都有独特的影响。然而，有一个特定的门能提供我们所寻找的部分反转效果。

# 介绍 U-gate

[U-gate](https://qiskit.org/documentation/stubs/qiskit.circuit.library.UGate.html) 是一个特殊的门，接受三个参数。这些包括*theta*、*phi* 和*lam*。这些参数允许修改量子比特的[欧拉角](https://en.wikipedia.org/wiki/Euler_angles)，以便在[Bloch 球](https://en.wikipedia.org/wiki/Bloch_sphere)上[旋转](https://quantumcomputinguk.org/tutorials/introduction-to-the-u3-gate-with-code)。

U-gate 实际上是单量子比特量子门的一个[广义版本](https://qiskit.org/textbook/ch-states/single-qubit-gates.html#generalU)，根据参数的值，它甚至可以用作替代。

你可以尝试调整 U-gate 的各种参数，查看对量子比特测量的影响。

事实证明，改变 *theta* 参数可以让我们以我们期望的方式倾斜量子比特测量 0 或 1 的机会！

# 玩弄 U-gate

U-gate 的 *theta* 值通常范围从 -1.5 到 1.5。

在范围的较低端，量子比特的测量结果将更接近于 0。在范围的较高端，测量结果将更接近于 1。任何介于两者之间的值都会导致偏向 0 或 1。

例如，我们可以将量子比特的*theta*角设置为 0.8，使得测量为 0 的概率为 20%，测量为 1 的概率为 80%。

# 调整翻转量

继续调整 U-gate 的值，我们可以看到量子比特的测量结果根据 U-gate 的*theta*值有不同的 0 或 1 的概率。

由于我们使用的*theta*范围是-1.5 到 1.5，如果我们将*theta*的率设置为 0.0，我们将看到传统的 50/50 测量 0 或 1 的概率。然而，如果我们将*theta*值设置为-0.6，我们可以看到一个偏斜的行为，导致测量 0 的概率远高于测量 1。

![](img/f5e71334e61adc344701233b189dbe36.png)

一个量子比特在叠加态下，U-gate 部分翻转后的测量分布。量子比特测量为 0 的概率远高于 1。来源：作者。

类似地，如果我们将*theta*设置为-1.5，我们将有接近 100%的概率总是测量为 0。同样，将值设置为 1.5 将导致测量为 1。

解决了机械问题后，让我们将程序组合起来吧！

# 我们的攻击计划

我们将从创建一个将单个量子比特置于叠加态的量子程序开始。

由于量子比特在叠加态的默认性质导致测量 0 或 1 的概率为 50/50，我们将使用 U-gate 稍微推动量子比特向特定方向。

我们将根据量子比特的测量值调整两张图像的透明度，同时上下滑动 U-gate 的*theta*参数。

# 创建量子程序

我们的量子电路非常简单。我们只需创建一个程序，将 Hadamard 门用于将量子比特置于叠加态，再加上一个 U-gate 用于应用所需的旋转。

```py
# Create a quantum circuit with 1 qubit.
qc = QuantumCircuit(1)

# Initialize the simulator.
simulator = Aer.get_backend('aer_simulator')

# Place the qubit into superposition.
qc.h(0)

# Apply partial inversion to the qubit.
# If our range is 0.0 to 1.0, we can set: theta = rate * 3 - 1.5
qc.u(theta, 0, 0, 0)

# Measure the result.
qc.measure_all()
```

![](img/e2f186b70567672b57f3275833c0d4c4.png)

一个部分翻转的量子比特。测量结果将根据 U-gate *theta*值偏向 0 或 1。来源：作者。

# 创建翻转方法以设置透明度

由于我们想在图像显示时修改翻转率，我们可以创建一个辅助方法来翻转量子比特。

此外，我们将使用一个更自然的*rate*参数来控制翻转，其范围从 0.0 到 1.0，而不是更尴尬的*theta*范围-1.5 到 1.5。

最后，我们将返回结果量子比特的测量值。

```py
def invert(rate):
    # Convert the rate of inversion from [0.0 to 1.0]
    # to a theta range of [-1.5 to 1.5] using the formula:
    # ((old_value - old_min) / (old_max - old_min)) * (new_max - new_min) + new_min
    theta = rate * 3 - 1.5

    # Create a circuit.
    qc = QuantumCircuit(1)

    # Initialize the simulator.
    simulator = Aer.get_backend('aer_simulator')

    # Place the qubit into superposition.
    qc.h(0)

    # Apply partial inversion to the qubit.
    qc.u(theta, 0, 0, 0)

    # Measure the result.
    qc.measure_all()

    # Execute the circuit.
    job = execute(qc, simulator)
    result = job.result()

    # Return the counts of 0 and 1.
    return result.get_counts()
```

如果我们调用*invert()*方法，设置率为 0.5，我们将得到传统的 50/50 测量 0 或 1 的概率。

然而，如果我们将率设置为 0.3，我们将有 70-80%的概率测量为 0，而不是 1，如下所示结果。

> {‘0’: 792, ‘1’: 232}

我们可以调整*rate*值的滑动来相应地影响量子比特的测量结果。

# 显示的战斗

现在我们的量子计算程序已经到位，我们可以使用量子比特作为唯一指标，以更高的不透明度绘制独角兽或小妖精。

如果量子比特的零测量次数更多，我们将更清晰地绘制独角兽。

如果量子比特的测量值为 1 的次数更多，我们将更清晰地绘制小妖精。

如果量子比特的测量值介于两者之间，我们将把这两张图像重叠在一起，透明度与量子比特的测量值相等。

# 初始化图形

我们将使用[ipycanvas](https://ipycanvas.readthedocs.io/)库来绘制和动画图像，并结合我们的量子计算程序。

我们开始通过初始化一个图形画布来绘制图像。

```py
def init():
    canvas = Canvas()
    display(canvas)

    canvas.fill_style = 'white'
    canvas.fill_rect(0, 0, 300, 300)

    canvas.fill_style = 'black'
    canvas.fill_rect(5, 295, 290, 28)

    return canvas

# Load sprites.
unicorn = Image.from_file("unicorn.jpg")
gremlin = Image.from_file("gremlin.jpg")
```

# 图像融合——量子风格

我们还需要一些辅助方法来绘制图像并更新其透明度值。

我们可以通过创建一个*draw()*方法来实现，它将两个精灵（独角兽和小妖精）作为参数，以及一个用于混合的 alpha 值。

我们还将创建一个*update()*方法，它调用我们的量子计算程序，对量子比特应用部分反转，并使用结果中的 0 和 1 的计数来设置混合图像的透明度。

```py
def draw(sprite1, sprite2, alpha):
    canvas.global_alpha = alpha
    canvas.draw_image(sprite1, 5, 5, 290, 290)

    canvas.global_alpha = 1 - alpha
    canvas.draw_image(sprite2, 5, 5, 290, 290)

    canvas.global_alpha = 1
    canvas.font = "20px arial"
    canvas.fill_style = '#00ff00'
    canvas.fill_text('Qubit inverted  ' + str(round(alpha, 1) * 100) + '%', 10, 316)

    return canvas

def update(sprite1, sprite2, rate):
    # Run the quantum program and obtain the qubit measurements.
    counts = invert(rate)

    # Get the counts for "unicorn" from the qubit measuring 0, and the gremlin for the qubit measuring 1.
    alpha = counts['0'] / 1024 if '0' in counts else 0

    draw(sprite1, sprite2, alpha)
```

# 将独角兽变形为小妖精

最后，让我们把一切整合在一起！

我们将创建一个*animate()*方法，它会简单地循环多次，在每次迭代时运行量子程序，同时逐渐调整量子比特的反转率(*theta*)。

结果是一个幽灵般的效果，因为独角兽会慢慢变成小妖精，同时量子比特的测量结果与 0 或 1 相符。

```py
def animate(rate = 0):
    with hold_canvas():
        for i in range(10):
            show_unicorn = True

            for j in range(20*2):
                canvas.fill_style = 'black'
                canvas.fill_rect(5, 295, 290, 28)

                update(unicorn, gremlin, rate)
                rate += 0.05 if show_unicorn else -0.05
                if rate > 1:
                    show_unicorn = False
                elif rate < 0:
                    show_unicorn = True

                canvas.sleep(125)

canvas = init()
animate()
```

看啊——我们的创作成果！

![](img/781e1dd73c5899f4f83ceb541c00f02a.png)

根据量子比特的测量叠加的图像。来源：作者，[Stable Diffusion](https://stablediffusionweb.com/)。

这是一种多么迷人的方式来可视化量子比特的测量！试想一下，将量子计算与经典游戏结合所能实现的所有可能性。我希望这能激发你对未来工作的创造力。

你可以[在这里](https://gist.github.com/primaryobjects/c9e21e15958812544d835a2f3e3baf82)下载独角兽小妖精程序的完整代码示例。

# 关于作者

如果你喜欢这篇文章，请考虑在[Medium](https://medium.com/@KoryBecker)、[Twitter](https://twitter.com/PrimaryObjects)和我的[网站](https://primaryobjects.com/)上关注我，以便获得我未来的帖子和研究工作的通知。
