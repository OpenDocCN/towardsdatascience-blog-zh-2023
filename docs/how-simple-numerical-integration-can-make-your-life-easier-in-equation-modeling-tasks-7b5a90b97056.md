# 如何让简单的数值积分在方程建模任务中让你的生活更轻松

> 原文：[`towardsdatascience.com/how-simple-numerical-integration-can-make-your-life-easier-in-equation-modeling-tasks-7b5a90b97056`](https://towardsdatascience.com/how-simple-numerical-integration-can-make-your-life-easier-in-equation-modeling-tasks-7b5a90b97056)

## 仿真和数值建模

## 以迈克利斯-门腾方程作为酶催化的例子，并包括一个可运行的网络应用程序及其代码

[](https://lucianosphere.medium.com/?source=post_page-----7b5a90b97056--------------------------------)![LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----7b5a90b97056--------------------------------)[](https://towardsdatascience.com/?source=post_page-----7b5a90b97056--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b5a90b97056--------------------------------) [LucianoSphere (Luciano Abriata, PhD)](https://lucianosphere.medium.com/?source=post_page-----7b5a90b97056--------------------------------)

·发表于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----7b5a90b97056--------------------------------) ·阅读时间 10 分钟·2023 年 10 月 5 日

--

![](img/70820e156dd12fd58954052b2ccb3331.png)

图像由作者从自己的和免费软件的截图中组成。

在自然科学、工程和经济学的数学中，数值积分的艺术作为描述动态系统随时间变化的行为的强大工具。该技术在这些领域中的许多问题上都有大量具体应用，尤其是在处理微分方程时。

从技术上讲，主要挑战出现在当微分方程将一个或多个变量及其导数混合在一起，使得解析积分最终纠缠了变量的不同出现方式，从而无法将其组合在一起并作为其他变量的函数进行求解。在这种情况下，数值积分，而不是常规积分，将会派上用场。

尽管有整套研究和方法可以高效地实现微分方程的数值积分，例如龙格-库塔方法，但我们可以回归基础，以一种非常简单的“类似欧拉”的方式直接解决问题，即通过使用导数的瞬时值并对小变化进行时间积分。这正是我将在这里展示的示例应用程序的情况：对描述酶催化的迈克利斯-门腾方程进行数值积分。全部包括完整的数学、一个可工作的网络应用程序和所有简单的 JavaScript 代码。

> 本文/教程的风格类似于我最近分享的另一篇文章，该文章介绍了看似复杂的过程实际上是相当简单且极具帮助的：

[](/the-power-and-simplicity-of-propagating-errors-with-monte-carlo-simulations-9c8dcca9d90d?source=post_page-----7b5a90b97056--------------------------------) ## 蒙特卡罗模拟中传播误差的力量与简洁性

### 掌握数据分析和模型拟合中的不确定性，配合实际代码和示例

[towardsdatascience.com

# 进入微分方程及其简单、直接的数值积分

微分方程常常在建模动态系统时出现，这些系统中的变量彼此变化且随时间变化。这些方程通常涉及时间的导数，代表变化率。然而，它们固有的复杂性可能使得提取直观见解或直接计算特定结果变得困难，例如预测系统何时会达到特定状态。

目前，微分方程的主要问题在于它们未能提供时间和系统变量之间的直接关系。它们描述了变量如何持续变化，但不容易提供在特定时间点达到特定条件或浓度的见解。这一限制可能会妨碍我们进行精确预测和理解所考虑系统的动态。

解决这个问题的显而易见的方法是积分包含导数的项。但在科学领域，当你积分一个已经出现在方程中的变量时，它通常会分离成不同的项，无法合并在一起求解变量。你会在我这里讨论的米氏-门腾酶促反应方程的例子中看到这一点。

另一种方法是使用一些数值方法，其中所谓的 Runge-Kutta 方法特别设计用来解决这些问题。但还有一种更简单的数值方法，它直接且明确地与导数的定义相结合：一个变量因另一个变量的小变化而发生的小变化，例如位置随时间的变化，浓度随时间的变化等。

通过离散化时间和近似变量的连续变化，非常简单的数值积分方法允许我们在小的、可管理的步骤中计算系统如何随时间演变。这些方法使我们能够在离散时间点近似动态系统的行为，从而能够预测何时会满足特定条件或分析系统在特定时刻的行为。

在这里，我将向你展示如何通过数值积分轻松处理像迈克利斯-门腾方程这样的复杂方程，使其在酶学中作为基础模型变得可行和有用。这个方程描述了酶催化反应的动力学，为展示数值积分的优雅和多功能性提供了一个理想的案例研究，它本质上将复杂的动态过程分解为可管理的步骤。此外，该方程的解析积分形式面临的挑战在于其最相关的变量不能互相求解，因此使得数值积分方法更具相关性。

# 设定示例问题

迈克利斯-门腾方程是一个基础的酶学模型，描述了涉及底物（S）和酶（E）的酶促反应的速率，以生成产物。这个方程在理解酶催化反应的动力学中起着关键作用。然而，由于没有办法将其项移动得到底物浓度作为时间的解析函数，因此不可能得到底物浓度作为时间的显式解。

但正如我将向你展示的，我们可以通过数值积分方程来模拟在给定初始底物和酶浓度以及一组动力学参数下的底物浓度随时间的变化。我还将把这些应用到一个基于网络的工具中，你可以非常轻松地在浏览器中运行你自己的底物浓度模拟，并且你可以查看和复制代码以便进行临时修改。

## **迈克利斯-门腾方程**

迈克利斯-门腾方程表示为：

![](img/2db17097380c97dcc499b8ed544da89e.png)

其中：

*V*是给定酶和底物浓度下的反应速度，单位为 M/s（摩尔每秒），[*E*]是酶浓度，通常以纳摩尔到微摩尔表示，但为了统一性在此以摩尔单位表示，[*S*]是底物浓度，也以 M 表示，*Km*​是描述催化周期中底物结合部分的迈克利斯常数（此处也以 M 表示），而*k*cat​是周转数或催化常数（单位为 1/s）。

该方程描述了反应速度*V*（即底物浓度[*S*]对时间*t*的导数）如何依赖于酶和底物的浓度，给定一组催化参数*k*cat​和*Km*​。

# 隐式变量，此处为底物浓度

虽然迈克利斯-门腾方程提供了对酶动力学的宝贵见解，但它无法将底物浓度解为时间的显式函数（这仅在对*V*作为[*S*]关于*t*的导数进行积分后才出现）。其核心限制来自方程的非线性和微分性质。

然而，我们可以通过数值方法克服这个限制，特别是通过数值积分反应速度，这可以被看作是当 deltaTime 非常小时的 deltaConcentration/deltaTime，即 d[*S*]/d*t*。

数值积分对于许多问题是一个非常强大的数学技术，它通过将动态系统分解为小的时间步长，从而近似系统随时间的行为，在这些小时间步长中会发生一些事情，变量也会变化。

让我们看看它是如何工作的：

1.  首先，我们使用已知值初始化系统：酶浓度（[*E*]）、初始底物浓度（[*S*]0​）、催化常数（*k*cat​）和米氏常数（*Km*​）。此外，我们定义一个时间步长（*dt*）用于底物浓度变化率的积分，d[S]/dt = V。经过一个时间步长后，我们将得到时间 t = t*previous* + dt 的浓度[S]，而追踪底物浓度的总时间长度由迭代次数给出，我们也必须设定这个值。

1.  在每次迭代中，我们计算由于酶促反应引起的[*S*]的瞬时变化率，这由上述的米氏-门腾方程计算出的反应速度 *V* 表示，代入剩余底物浓度[S]。

1.  这个瞬时速度描述了反应在特定时刻的进行速度。我们可以认为在反应的那个点，在一个无穷小的时间内，会消耗掉相应量的底物。由于计算机中无法使用无穷小的数值，我们将其近似为一个非常小的数值（我们可能需要调整这个数值，稍后会有相关说明）。然后我们将步骤 2 中计算的速度乘以我们的 deltat 或时间步长，以确定在这个短时间间隔内底物浓度的变化。这表示在这个微小时间步长内消耗（或生成）的底物量。

1.  然后我们通过从当前浓度[*S*]中减去上一步计算的变化量（*V x dt*）来更新底物浓度。这一步反映了反应在短时间间隔内的实际进展，并给出了当前时间的新[*S*]。

1.  我们重复这个过程，进行指定次数的迭代，每次根据更新的底物浓度重新计算速度。

1.  一旦在增加的时间*t*下计算了一系列[*S*]，我们就得到了我们想要的轮廓。

## 实现准确的积分

时间步长（*dt*）的选择在数值积分中至关重要。较小的时间步长允许我们捕捉反应动态的更细微的细节，但需要更多的计算资源。相反，较大的时间步长加快了计算速度，但可能导致结果不准确，甚至可能破坏整个过程——在这个例子中，可能会导致底物浓度出现不现实的大变化，从而引入显著的偏差，甚至可能产生比当前底物浓度还大的变化，导致底物浓度变为负值。

因此，在实际操作中，用户应微调时间步长，以在计算效率和准确性之间取得平衡。根据我的经验，最好尝试找到一个小的数值，使曲线形状合理地接近预期（见下文示例），然后慢慢减少或增加*dt*，同时检查曲线的形状是否受到影响。如果没有，那么你的数值积分就进行得很好。

# 使用在线网页应用的示例模拟

这种计算瞬时速度、更新底物浓度并重复多个小时间步长的迭代过程，使我们能够模拟反应的动态行为，即使在无法获得明确的数学解时也是如此。而且，制作一个这样的程序非常简单，就像你可以在这里尝试的网页应用一样：

[## 底物与时间的关系，来自米氏-门腾方程

### 编辑描述

lucianoabriata.altervista.org](https://lucianoabriata.altervista.org/jsinscience/michaelis-menten/test1.html?source=post_page-----7b5a90b97056--------------------------------)

这是一个模拟示例：

![](img/c6f66112e6dd5278c2ec7125e8b2746a.png)

作者截图，来自他自己的网页应用。

这样的工具使用户能够非常轻松地探索酶催化反应的动态行为。特别是，了解在不同参数下底物轮廓如何变化有助于设计实验；例如，向反应中添加多少酶、预期等待多久才能消耗掉给定的底物分数等等。这些都是实验室研究酶性质时的相关问题，从基础化学生物学到制药公司。所有这些都由数学和计算机代码辅助！

# 一些代码——虽然实际上，它非常简单

我觉得很美妙的是，这样一个复杂的数学问题可以通过像这样的策略来解决，虽然使用了蛮力，但可以如此简单地编码。

虽然我邀请你通过在大多数网页浏览器中按 Ctrl-U 来查看我网页应用的完整代码，但这里是它的核心部分：

```py
 var lastS = S0    //Set to the starting concentration of substrate
 var time = 0

 for (var i = 0; i <= iterations; i++) {
   var Vinst = kcat * E * lastS / (Km + lastS)   //Instantaneous velocity at the current substrate concentration
   var deltaS = Vinst * dt                       //Integration by multiplying the velocity by a sufficiently small timestep
   lastS = lastS — deltaS                     //Update substrate concentration
   time = time + dt                           //Update total time
   data.addRow([time, lastS, S0 — lastS]);    //Data is then plotted using Google's chart library in my web app
 }
```

# 与更复杂的数值解微分方程的方法比较

还有一些专门设计来解决这些及其他相关问题的更复杂的方法，其中主要的是 Runge-Kutta 系列方法。虽然我可以在未来讨论 Runge-Kutta 方法，但目前让我与您分享这篇专门的维基百科文章以及与我在这里提出的方法的比较：

[](https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods?source=post_page-----7b5a90b97056--------------------------------) [## Runge-Kutta 方法 - 维基百科

### 在数值分析中，Runge-Kutta 方法（RUUNG-ə-KUUT-tah）是一系列隐式和显式的迭代…

[维基百科](https://en.wikipedia.org/wiki/Runge%E2%80%93Kutta_methods?source=post_page-----7b5a90b97056--------------------------------)

我上面介绍的方法并没有明确实现特定的 Runge-Kutta 方法，但它确实类似于解决常微分方程的数值积分方案。我们看到的数值方法简单得多，并且具有这样的优势：它在概念上与导数的定义直接相关，这里速度作为“在短时间内的微小变化”，并使用基本的算术运算。

我们使用的简单方法的一个重要缺点是它对时间步长非常敏感，不仅是当该参数过长时，而且当涉及的速度过高时也是如此。在 Web 应用中，这种限制通过可以更改时间步长的选项得到了一定程度的弥补，你可以通过重复模拟来轻松检查模拟是否稳健。

相比于设计为随着时间步长趋近于零而收敛于真实解的 Runge-Kutta 方法，这里使用的方法可能需要如此小的时间步长以达到可接受的精度，以至于计算可能变得计算上过于昂贵（因为更小的时间步长需要更多的迭代来实现长时间的模拟）。

要了解 Runge-Kutta 及其他不同方法在迈克利斯-门腾方程问题上的应用，请查看这些论文：

[](https://link.springer.com/article/10.1007/s10910-012-0071-1?source=post_page-----7b5a90b97056--------------------------------) [## 亨利-迈克利斯-门腾模型的底物和酶浓度依赖性探讨…

### 使用经典的亨利-迈克利斯-门腾（HMM）模型（或简称迈克利斯-门腾模型）来研究底物…

[Springer](https://link.springer.com/article/10.1007/s10910-012-0071-1?source=post_page-----7b5a90b97056--------------------------------) [## 迈克利斯-门腾方程的分解法解 - PubMed

### 我们提出了一种低阶递归解法来解决迈克利斯-门腾方程，使用了分解法。这…

[PubMed](https://pubmed.ncbi.nlm.nih.gov/19292514/?source=post_page-----7b5a90b97056--------------------------------)

# 收获

我希望我已经说服你，数值积分是一种简单而强大的数学技术，它能够解决像这样无法获得积分变量显式解的问题。正如你所看到的，这个过程将复杂的动态分解为可管理的步骤，采用类似欧拉的方法，这种方法根植于导数的基本概念，因此从底层数学和编程的角度来看非常简单。

[***www.lucianoabriata.com***](https://www.lucianoabriata.com/) *我写作和拍摄关于我广泛兴趣领域的所有内容：自然、科学、技术、编程等。* [***订阅以获取我的新故事***](https://lucianosphere.medium.com/subscribe) ***通过电子邮件***。* 要* ***咨询小型工作*** *请查看我的* [***服务页面***](https://lucianoabriata.altervista.org/services/index.html)*。你可以* [***在这里联系我***](https://lucianoabriata.altervista.org/office/contact.html)***。***
