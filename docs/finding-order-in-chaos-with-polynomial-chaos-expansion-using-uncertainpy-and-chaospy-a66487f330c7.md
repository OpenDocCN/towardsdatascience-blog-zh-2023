# 利用多项式混沌扩展、使用 uncertainpy 和 chaospy 寻找混乱中的秩序

> 原文：[https://towardsdatascience.com/finding-order-in-chaos-with-polynomial-chaos-expansion-using-uncertainpy-and-chaospy-a66487f330c7?source=collection_archive---------6-----------------------#2023-10-12](https://towardsdatascience.com/finding-order-in-chaos-with-polynomial-chaos-expansion-using-uncertainpy-and-chaospy-a66487f330c7?source=collection_archive---------6-----------------------#2023-10-12)

## 下面是如何利用数学、物理学、Python 和数据科学来解决混乱问题的方法

[](https://piero-paialunga.medium.com/?source=post_page-----a66487f330c7--------------------------------)[![Piero Paialunga](../Images/de2185596a49484698733e85114dd1ff.png)](https://piero-paialunga.medium.com/?source=post_page-----a66487f330c7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a66487f330c7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a66487f330c7--------------------------------) [Piero Paialunga](https://piero-paialunga.medium.com/?source=post_page-----a66487f330c7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F254e653181d2&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-order-in-chaos-with-polynomial-chaos-expansion-using-uncertainpy-and-chaospy-a66487f330c7&user=Piero+Paialunga&userId=254e653181d2&source=post_page-254e653181d2----a66487f330c7---------------------post_header-----------) 发布在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a66487f330c7--------------------------------) ·9 min read·2023年10月12日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa66487f330c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-order-in-chaos-with-polynomial-chaos-expansion-using-uncertainpy-and-chaospy-a66487f330c7&user=Piero+Paialunga&userId=254e653181d2&source=-----a66487f330c7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa66487f330c7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Ffinding-order-in-chaos-with-polynomial-chaos-expansion-using-uncertainpy-and-chaospy-a66487f330c7&source=-----a66487f330c7---------------------bookmark_footer-----------)![](../Images/d4bb664d6374834cbeda94edc5ee2d95.png)

图片由作者使用 Midjourney 生成

三年前，我从意大利罗马搬到了美国俄亥俄州辛辛那提，接受了辛辛那提大学的博士offer。我非常怀念我的城市，有很多东西：美食、天气、永恒之城的美丽。我绝对不怀念我城市的一件事是**疯狂的交通**。

我的一个好朋友前几天给我发了短信说

> “皮耶罗，今天交通太糟糕了，城市一片**混乱**。”

现在，显然，我没有纠正他（尤其是知道罗马的交通情况），但术语***混乱***在数学和物理中有着与我们日常生活中使用的“混乱”完全不同的意义。

当我们谈到数学中的混乱时，一个流行的定义是：一个由**确定性方程**控制的问题，*但* **系统的演变极度依赖于初始条件**。这意味着即使初始条件发生极其微小的变化，系统的演变也可能会极其不同。用洛伦茨¹的话来说，这意味着：

> “现在决定未来，但近似的现在不能近似决定未来。”

¹ [http://mpe.dimacs.rutgers.edu/2013/03/17/chaos-in-an-atmosphere-hanging-on-a-wall/](http://mpe.dimacs.rutgers.edu/2013/03/17/chaos-in-an-atmosphere-hanging-on-a-wall/)

这意味着我们能够预测状态的演变的唯一方法是从**概率视角**考虑它。给定过程的起始点，我们无法准确预测系统的到达点，因为它是混乱的，但我们能够**概率性地**预测，比如我们可以得到平均值和初始偏差。

这种混乱可以通过数值方法处理，例如使用**Python**。在这篇博客文章中，我们将从抽象的随机游走开始，描述多项式混沌展开（PCE），并应用于一个实际案例，如我们的咖啡温度☕️。

让我们开始吧！

# 1\. 随机游走

随机游走是所有阅读的数学家和物理学家都很熟悉的东西。这个模型几乎在所有地方都被使用，从金融到物理，它非常简单。它在文献中也被称为**布朗运动**，其工作原理如下：

1.  我们从点x = 0开始

1.  以相同的概率，我们可以从x=0到x=1，或者从x=0到x=-1。我们将这个点定义为x_1

1.  再次，我们可以将x_1的值增加或减少1。我们将定义这一点为x_2。

1.  我们用x_2重复第3点N-2次

有时候，我觉得伪代码比用文字解释更容易理解

```py
RandomWalk(N):
  x = 0
  i = 0 
  while i<N:
    p = random(-1,1)
    x = x+p
    i = i+1
  return x 
```

现在我们来探讨一下这个问题，好吗？

## 1.2 代码

在这一部分中，我们将使用Python语言代码描述随机游走。你需要导入像**numpy**和**matplotlib.pyplot**这样的基本库。

这是随机游走代码：

如果我们运行这个，比如100次，我们会得到以下路径：

很有趣的是，如果你考虑最后一步，你可以发现**高斯分布**。

![](../Images/d1c14e422519e82b378758908effb147.png)

现在我们先留在这里。我保证，我们会用到这些。

# 2\. 微分方程

现在，关于生活的一切，等一下，**字面上**，我们所知道的关于生活的一切都是因为**微分方程**。

微分方程是物理学用来描述系统演变的工具。我的高中老师通过这样解释：

> “描述世界你需要两样东西：区分和积分。区分非常容易，积分非常困难。”

例如，让我们考虑爬树的松鼠的**y**位置：

![](../Images/c6b50398778a8997ac8e748de167d6c2.png)

图片由作者使用 Midjourney 生成

假设**松鼠的速度**是 v(t) = (t/60)**2，其中 t = 秒。所以我们的超级英雄开始时的 v(t=0) = 0，经过两分钟，他的速度达到 v(120) = 2**2 = 4 m/s。

给定这些信息，超级松鼠的位置在哪里？

我们需要做的是积分速度方程，然后得到：

![](../Images/272245bcb161b5f3172af92f70f346b7.png)

我们如何得到这个 c 常数？我们只是设置 t = 0 时发生的情况。我们假设我们的松鼠从高度 = 0 开始，所以 c = 0。

所以我们爬树的超级松鼠的位置如下：

![](../Images/d5eed2ee3be59a387830ec33e4c50c98.png)

通常，一个特定的解决方案 **y** 可以看作是另一量，例如 x 的积分以及一个初始条件。

在这个方案中，我们讨论了：

+   时间（**t**），这是时间变量（从实验开始到结束）

+   **x**，即我们正在积分的对象（在上述例子中，**x**是速度）

+   解决方案（**y**）是我们通过积分**x**得到的解决方案（在上述例子中，**y**是超级松鼠的位置）

所以在这种情况下，我们可以说 x (t) = y(t) 的积分。

**还有更多**。你可以在系统中拥有一些固定的参数，但这些参数可以**改变系统的演变。** 所以：

x(t, 参数列表) = y(t, 参数列表) 的积分

例如。我们来谈谈咖啡。咖啡？是的，咖啡。

## 2.1 牛顿的冷却定律

一位在物理学上相当出色（哈哈）的家伙，名叫艾萨克·牛顿，在他留给我们的众多礼物中，解释了如何描述热体的热传导。换句话说，他告诉我们物体如何冷却。

![](../Images/04ef256a9a8643a6b3f82259f96b230b.png)

图片由作者使用 Midjourney 生成

牛顿提出的冷却定律指出，从身体到外界的热量流失速率与一个常数 **k** 成正比，该常数依赖于表面积及其**热传导系数**，以及时间 t 的温度 T 和环境温度 T_env 之间的差异。

如果我们想获得**温度（T）**，我们需要积分**热量流失速率（dT/dt）**。**这是方程：**

![](../Images/5e0f6d8b7d153483627f834b21a08185.png)

图片由作者提供

要获得温度 T，**给定 T_env 和 k（记住这一点！！！）**，我们需要积分 dT/dt。

# 3\. 威纳的混沌！

我们**非常罕见**地能够定义微分方程的解析解。*这就是我高中教授说它很难积分的原因*。我们更可能需要进行**数值积分**，即用数值方式（也就是用算法）解决微分方程。

有一些非常著名的积分方法（算法），比如[梯形规则](https://en.wikipedia.org/wiki/Trapezoidal_rule)或[黎曼和](https://en.wikipedia.org/wiki/Riemann_sum)。它们有效，有利有弊，并且高效。它们不是问题所在。

**真正**的问题在于**微分方程的参数（例如 T_env 和 kappa）**。让我详细说明一下。

你还记得上面方程中的**T_env**和**k**吗？我们完全不知道它们实际是什么，它们可能会完全改变我们系统的演化。

[**诺伯特·维纳**](https://en.wikipedia.org/wiki/Norbert_Wiener)的美丽心灵为带有**额外随机参数**的微分方程提供了非常优雅的公式。特别是，***现在我们所有的讨论都很有意义了***，带有随机参数的微分方程被定义为**混沌的**，可以用**随机游走（啊哈！）作为多项式来描述**。通过这样做，我们能够以**概率方式**理解解 T(t)！

我理解这可能会让人困惑：让我们一步一步来 :)

## 3.1 设置

我们在 Python 中需要做的第一件事是定义我们的微分方程：

如我们所见，这不仅仅是**T**（我们的变量）的问题，还有**kappa 和 T_env**的问题。

这是我们需要积分的函数。**在此之前**，让我们导入一些朋友🦸‍♂️

你可能会遇到错误，因为你没有**chaospy 和 uncertainpy**。它们是我们的魔法小助手：它们实现了多项式混沌展开方法。安装它们非常简单：

```py
pip install uncertainpy
```

## 3.2 积分函数

让我们使用梯形规则设置需要**积分**的函数：

所以：

+   我们设定咖啡的初始温度，比如 T_0 = 95

+   我们设定我们问题的时间步长，比如 500 个时间步

+   我们使用梯形规则进行积分

+   我们返回时间和温度

## 3.3 关于 uncertainpy

现在，我得说一下：[**uncertainpy**](https://uncertainpy.readthedocs.io/en/latest/index.html) **非常棒**。你可以用它做很多事情，我真的推荐你花时间在[这里](https://uncertainpy.readthedocs.io/en/latest/index.html)了解一下。

我们要做的是：

+   我们设定一个可能的**kappa**分布。例如，kappa 从具有给定 mu 和方差的正态分布中抽取。

+   对**T_env**做同样的处理

+   我们应用 uncertainpy **并提取给定输入分布的温度可能值分布**

游戏很简单：如果我们知道**得益于Wiener的混沌**参数的可能分布，我们就能知道输出的分布。

这可能听起来有点混乱，但我保证展示代码后会更清楚：

模型通过**coffee_cup**定义，这就是我们的微分方程。接着，我们定义参数分布（使用**chaospy**）并定义相应的参数字典。

现在。对于每个 kappa 和 T_env 值，我们有一个具有不同参数和不同温度 T(t) 的微分方程，这些是**积分**的结果。得益于神奇的**chaospy**，解决方案变成了一个具有均值和标准差的**分布**。

让我们看看如何操作：

**就是这样！**（就像 Biggie 所说）**。非常简单。**

## 3.4 整体内容

整个内容可以放在这个代码块中：

![](../Images/96b4cb1096b1d0fa4b11ebe07a963aed.png)

这难道不美好吗？我们能够将**输入参数的分布**转化为**输出结果的分布**。在时间 t=0 时，温度是 T= T_0 = 95。随着时间的推移，**参数的不确定性**变得越来越明显。在时间 = 200 分钟时，我们有一个很大的不确定性（假设从 5 到 30），这取决于 k 和 T_env，可能会很冷或稍微有点热。

# 4\. 结果

在这篇博客文章中，我们描述了美妙的**chaospy 和 uncertainpy 库**。这些库使我们能够处理**Wiener 混沌**问题，它使用**随机游走**来定义一种**多项式混沌**。这种多项式混沌用于处理**带有分布的微分方程**，而不是**参数**。我们按以下顺序进行了操作：

+   我们在第 1 章中描述了**随机游走**。

+   我们在第 2 章中描述了**微分方程**。特别是我们描述了牛顿冷却定律。

+   我们按照**Wiener**的描述了**混沌**并在第 3 章中应用了**多项式混沌**。

# 5\. 结论

如果你喜欢这篇文章并想了解更多关于机器学习的内容，或者你只是想问我一些问题，你可以：

A. 在 [**Linkedin**](https://www.linkedin.com/in/pieropaialunga/) 上关注我，我会发布所有的故事。

B. 订阅我的 [**通讯**](https://piero-paialunga.medium.com/subscribe)。它将让你了解新故事，并给你机会与我联系，获取所有可能的更正或疑问。

C. 成为一个 [**会员**](https://piero-paialunga.medium.com/membership)，这样你就不会有“每月故事的最大数量限制”，可以阅读我（以及成千上万其他机器学习和数据科学顶级作者）写的关于最新技术的内容。
