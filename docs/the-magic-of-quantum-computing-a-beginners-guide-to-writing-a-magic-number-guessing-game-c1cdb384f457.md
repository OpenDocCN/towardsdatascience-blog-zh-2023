# 量子计算的魔力：编写魔法数字猜测游戏的初学者指南

> 原文：[`towardsdatascience.com/the-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457`](https://towardsdatascience.com/the-magic-of-quantum-computing-a-beginners-guide-to-writing-a-magic-number-guessing-game-c1cdb384f457)

## 教程

## 编程量子计算机很有趣！

[](https://medium.com/@KoryBecker?source=post_page-----c1cdb384f457--------------------------------)![Kory Becker](https://medium.com/@KoryBecker?source=post_page-----c1cdb384f457--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c1cdb384f457--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1cdb384f457--------------------------------) [Kory Becker](https://medium.com/@KoryBecker?source=post_page-----c1cdb384f457--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c1cdb384f457--------------------------------) ·9 分钟阅读·2023 年 2 月 10 日

--

![](img/6931c4891a0299efe800b4c0f900ef11.png)

来源：[稳定扩散](https://stablediffusionweb.com/)。

# 从经典到量子

我不确定量子计算到底是什么让人如此惊叹和着迷，但它确实感觉与编程经典计算机截然不同。量子计算领域仍然年轻且充满潜力，为像*你一样*的人开辟了许多可能性。

我发现，了解一种技术的最佳方式之一就是通过游戏。还有什么比编写自己的游戏更好的方式来了解量子计算呢？

# 简单游戏的设计

我们将使用 Python 和一个名为 Qiskit 的开源量子计算库，来编写一个魔法数字猜测游戏。

我们的游戏非常简单，但将由量子计算的特性驱动。

我们的游戏将具有以下设计：

1.  量子计算机将生成一个从 0 到 15 的随机数字。

1.  玩家将需要猜测这个数字。

1.  在每一轮中，我们会告诉玩家他们的猜测是否过低或过高。

这是一种很棒的方式来开始量子计算编程！

让我们开始吧！

# 量子计算的乐趣

通常，当我们想到量子计算时，首先想到的主题之一是比特同时表示为零和一的想法。这是我们称之为[叠加态](https://en.wikipedia.org/wiki/Quantum_superposition)的微观粒子的行为。

想象一下。在微观层面上，整个世界的行为方式与我们习惯的完全不同。粒子旋转并存在于多个状态。有时，它们甚至可能出现或消失。它们甚至可以穿过物理墙壁，这种行为称为 [量子隧穿](https://rpubs.com/primaryobjects/quantum-tunneling)！

然而，我们可以通过编程量子计算机来利用这些惊人的效果。

由于量子计算最强大的特性之一是同时表示零和一的位的信息，这也是初学者在构建第一个量子电路时最先学习的逻辑门之一。

# 当一个位是量子位

经典计算机上的一个位可以保存零或一的值，但绝对不能同时保存两者。然而，量子计算机上的量子位可以同时保存零和一的值，实现叠加态，直到被测量。

我们将利用叠加态的强大效应作为我们游戏的核心部分。

在量子计算机上，[哈达玛门](https://qiskit.org/textbook/ch-states/single-qubit-gates.html#hgate) 用于将量子位置于叠加态。当这样做时，我们可以测量量子位并得到一个随机的零或一的值。也就是说，我们在测量量子位时，半数时间会得到零，另一半时间会得到一。

由于量子位有 *50/50* 的机会测量到零或一，我们可以利用这种效应来随机生成一个位。

# 生成随机数

现在我们知道可以使用哈达玛门在量子计算机上生成零或一的随机数，让我们将这个想法扩展到四个量子位。这使我们能够生成四个随机位，当它们转换为整数时，结果在 0 到 15 之间。

在特定范围内生成随机数的能力将作为我们游戏的关键部分。

# 在经典计算机上生成随机数

在深入量子计算部分之前，让我们首先看看如何在经典计算机上生成一些随机数。这将使我们能够比较经典计算机和量子计算机上生成的随机数的结果。

使用 Python，我们可以通过以下示例代码轻松生成伪随机数。

```py
import random
for i in range(10):
  num = random.randint(0, 15)
```

上述代码生成了一个从 0 到 15 的随机数，结果为以下值数组：[14, 7, 14, 10, 11, 6, 11, 10, 1, 9]。

如果我们绘制一个从这种方法生成的 300 个随机数的直方图，我们可以看到下面显示的图形。

![](img/2b3830a29cf21ed3841802e715e04e1b.png)

使用 Python 的 random() 模块生成的 300 个随机数的分布。来源：作者。

注意，每个数字出现的频率相对平等，与其他数字相比。

*这似乎非常随机！*

# 从叠加态生成随机数

让我们将其引入量子世界。

就像我们在经典计算机方法中做的那样，现在我们将使用四个量子比特的叠加态来生成四个位的随机数，用于表示从 0 到 15 的数字。

我们可以使用以下代码。

```py
from qiskit import QuantumCircuit, QuantumRegister, ClassicalRegister, execute, Aer
from qiskit.visualization import plot_histogram

# Setup a quantum circuit (4 bit random number 0–15).
qc = QuantumCircuit(4)

# Select the simulator.
simulator = Aer.get_backend(‘aer_simulator’)

# Place all qubits into superposition (50% chance 0 or 1).
qc.h(range(4))

# Measure each qubit.
qc.measure_all()
```

上述示例创建了一个量子电路，使用 Hadamard 门对每个量子比特进行操作，将它们置于叠加态。然后我们测量量子比特，读取每个量子比特的随机值，可能是零或一。

下面是绘制出的量子电路的样子。

![](img/fa48da375bd32aa6db50de128c72be2c.png)

使用叠加态和 Hadamard 门生成 4 位随机数的量子计算电路。来源：作者，生成于 Qiskit。

如你所见，我们只是在每个量子比特上使用了 H 门，并测量了结果值。

你可能想知道这些结果与之前的经典方法相比如何？事实证明，这种方法确实可以生成随机数，如下数组所示：[8, 11, 4, 2, 13, 9, 7, 12, 15, 13]。

我们可以绘制随机数的直方图，以确认随机值的分布。

![](img/078094b4342498d53a79f69270cfc403.png)

使用叠加态生成的 300 个随机数的分布。来源：作者。

就像经典计算方法一样，这种方法也似乎相当随机！

# 从量子噪声中生成随机数

使用叠加态生成随机数非常简单，直接明了。

然而，我们也可以通过利用量子噪声的特性在量子计算机上生成随机数。

当量子门执行时，会有微小的[噪声](https://arxiv.org/pdf/0802.1639.pdf)（或错误）可能影响门操作。正是这微小的噪声我们可以利用来生成随机数。

幸运的是，Qiskit 库使得利用量子噪声变得简单。甚至还有其他[项目](https://github.com/rochisha0/quantum-ugly-duckling)利用这一特性进行[随机数生成](https://github.com/rochisha0/quantum-ugly-duckling/blob/main/quantum-ugly-duckling-main/nqrng.py)。

以下是设置量子电路以利用噪声的示例。

```py
from qiskit.providers.aer.noise import NoiseModel, pauli_error

# Initialize bit-flip error rates.
reset_value = 0.03
measure_value = 0.1
gate_value = 0.05

# Initialize QuantumError on the X and I gates.
reset_error_rate = pauli_error([('X', reset_value), ('I', 1 - reset_value)])
measure_error_rate = pauli_error([('X',measure_value), ('I', 1 - measure_value)])
gate1_error_rate = pauli_error([('X',gate_value), ('I', 1 - gate_value)])
gate2_error_rate = gate1_error_rate.tensor(gate1_error_rate)

# Add errors to a noise model.
noise_model = NoiseModel()
noise_model.add_all_qubit_quantum_error(reset_error_rate, "reset")
noise_model.add_all_qubit_quantum_error(measure_error_rate, "measure")
noise_model.add_all_qubit_quantum_error(gate1_error_rate, ["u1", "u2", "u3"])
noise_model.add_all_qubit_quantum_error(gate2_error_rate, ["cx"])
```

上述代码初始化了我们将在程序中使用的各种门的量子噪声。

一旦噪声准备好，我们可以通过以下代码创建一个量子电路，从噪声中生成随机数。

```py
from qiskit.providers.aer import QasmSimulator

# Setup a quantum circuit (4 bit random number 0–15).
qc = QuantumCircuit(4)
simulator = QasmSimulator()

# Setup quantum circuit for noise generation to build a random value.
for i in range(50):
  qc.u(0,0,0,0)
  qc.u(0,0,0,1)
  qc.u(0,0,0,2)
  qc.u(0,0,0,3)
  qc.cx(0,1)
  qc.cx(1,2)
  qc.cx(0,2)
  qc.cx(0,3)
  qc.cx(1,3)
  qc.cx(2,3)
  qc.barrier()

qc.measure_all()
```

上述代码创建了如下所示的量子电路。

![](img/9f8289bc579f624949ccb60798f56afc.png)

使用量子噪声生成 4 位随机数的量子计算电路。来源：作者，生成于 Qiskit。

注意，我们使用了 U 门与一个受控非门（CX 门）的组合来生成所需的噪声。

你可能注意到了量子电路中的 for 循环。我们需要多次运行电路以放大生成的噪声。否则，我们的随机数将严重倾斜到零或一，而不是随机分布。平衡循环的迭代次数与数字的随机性非常重要，因为电路越长，运行时间也会越长。

最后，我们测量结果的四个量子比特以获得随机值。

再次，结果输出确实显得相当随机，如下数组所示：[4, 0, 6, 9, 2, 9, 15, 5, 2, 12]。

![](img/83bc89638c8142485a7de942a0e06127.png)

使用量子噪声生成的 300 个随机数的分布。来源：作者。

# 创建魔法数字游戏

现在我们准备好实现游戏了，我们需要选择一种生成将要使用的随机数的方法。

两种量子计算方法（叠加和噪声）都同样有效。

叠加方法更常用于生成随机性，因为它使用更简单的电路，而噪声方法则利用量子计算门的独特特性。实际上，每种技术都可以互换使用。

不过，让我们继续使用量子噪声的方法，这是一个更独特的方式，并在我们的游戏中实现它。

# 将量子封装到经典函数中

为了保持整洁，我们将嘈杂的量子电路封装到一个名为*random_number*的 Python 方法中。我们可以调用这个方法来选择玩家需要猜测的魔法数字。

```py
def random_number():
  # Setup a quantum circuit.
  qc = QuantumCircuit(4)
  simulator = QasmSimulator()

  # Setup quantum circuit for noise generation to build a random value with greater iteration.
  for i in range(50):
    qc.u(0,0,0,0)
    qc.u(0,0,0,1)
    qc.u(0,0,0,2)
    qc.u(0,0,0,3)
    qc.cx(0,1)
    qc.cx(1,2)
    qc.cx(0,2)
    qc.cx(0,3)
    qc.cx(1,3)
    qc.cx(2,3)
    qc.barrier()

    qc.measure_all()

    # Execute the circuit.
    job = execute(qc, simulator, basis_gates=noise_model.basis_gates, noise_model=noise_model, shots=1)
    result = job.result()
    counts = result_bit_flip.get_counts(0)

    num=list(counts.keys())[0]
    return int(num, 2)
```

一旦生成了魔法数字，我们就开始游戏循环。在每一轮中，我们会要求用户输入一个从 0 到 15 的猜测，并告诉玩家他们的猜测是太低还是太高。

游戏的代码如下所示。

```py
# Generate a random quantum number.
magic_number = random_number()

guess = -999
count = 0
while guess != magic_number and guess != -1:
  count = count + 1
  guess = int(input("Guess a number from 0 to 15? "))
  if guess < magic_number:
    print("Too low!")
  elif guess > magic_number:
    print("Too high!")
  else:
    print("Great guess! You won in", count, "guesses!")
    break
```

就是这样！

# 灯光、摄像、动作！

正如你所见，我们的游戏本质上非常简单。然而，游戏展示了量子计算机生成随机数的能力，这当然是这个游戏的关键部分。

这个想法有惊人的潜力可以进一步扩展。想象一下将图形、游戏机制、事件 3D 或虚拟现实效果添加到游戏中的可能性——所有这些都由量子计算提供动力。

生成量子随机数的简单动作可以为你游戏设计中的关键部分提供动力。

# 给我速度

现在，重要的是要注意，在量子计算机上执行速度的考虑。毕竟，在模拟器或物理量子计算机（例如 IBM Quantum）上处理量子电路可能需要显著的处理时间和延迟。

目前，游戏仍然可以作为一个优秀的概念验证和学习机会。此外，量子模拟器和硬件每年都在改善。

所以坚持下去，尽可能多地学习！

# 你兴奋了吗？

我希望你和我一样对量子计算及其在各种计算问题中的潜在应用感到兴奋。

现在你已经学会了如何使用 Python Qiskit 量子计算库，以及叠加态或量子噪声，你可以轻松地为各种用途生成随机数。

当然，量子计算的乐趣远不仅仅是随机数。不断增长的量子算法充分利用了量子过程。

这仅仅是冰山一角！

# 未来一片广阔

既然你已经知道了如何用量子计算生成随机数，那么是时候制作你自己的应用程序和游戏了！

你可以在[这里](https://gist.github.com/primaryobjects/e1a5ba52da6d482e0f00d3e2e8a590fe)下载完整的魔法数字猜测游戏代码示例。

# 关于作者

如果你喜欢这篇文章，请考虑在[Medium](https://medium.com/@KoryBecker)、[Twitter](https://twitter.com/PrimaryObjects)和我的[网站](https://primaryobjects.com)上关注我，以便获取我未来的文章和研究工作通知。
