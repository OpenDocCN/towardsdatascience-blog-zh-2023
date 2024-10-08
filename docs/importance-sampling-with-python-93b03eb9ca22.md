# 使用 Python 的重要性采样

> 原文：[`towardsdatascience.com/importance-sampling-with-python-93b03eb9ca22`](https://towardsdatascience.com/importance-sampling-with-python-93b03eb9ca22)

![](img/f7f54b21a31c0992de0b707432134091.png)

图片由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 学习如何在只能访问另一个分布时从一个分布中采样

[](https://medium.com/@marcellopoliti?source=post_page-----93b03eb9ca22--------------------------------)![Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----93b03eb9ca22--------------------------------)[](https://towardsdatascience.com/?source=post_page-----93b03eb9ca22--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----93b03eb9ca22--------------------------------) [Marcello Politi](https://medium.com/@marcellopoliti?source=post_page-----93b03eb9ca22--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----93b03eb9ca22--------------------------------) ·阅读时间 5 分钟·2023 年 5 月 10 日

--

## 介绍

在数据科学家必须知道的各种采样方法中，最重要的方法之一就是所谓的“重要性采样”。

这种方法使我们能够**即使我们实际上只能从另一个分布中采样，也能从一个分布中采样！** 让我们看看它是如何工作的。

## 重要性采样

假设，例如，从分布 g(x) 中采样是不可行的，因为，例如，这样做成本太高。但与此同时，我们有一个可以采样的分布 f(x)，我们称之为重要性分布。

我们可以通过对分布 f(x) 进行采样来计算我们真正感兴趣的分布 g(x) 的统计信息。让我们看看如何做。

想象一下，我们有一个表示每个骰子面的概率的分布 f(x)。如果骰子是“公平的”，每一面都有 1/6 的概率，因此我们可以将分布表示如下。

![](img/a3ed8d55d5ee16647742bd12209afb03.png)

公平分配（图片由作者提供）

我们还有另一个分布 g(x)，我们希望从中采样，但由于某种原因被阻止。在这种情况下，骰子是不公平的，因此分布是有偏的。所以有些面会比其他面有更高的概率。

![](img/35d84494aa391b60e19e22476a288436.png)

偏倚分布（图片由作者提供）

使用我们的数学概念，我们能够计算每个分布的期望值。因此，例如，第一种分布 f(x) 的 E[x] 将是：

![](img/b94758ec8321bd4c3367ae18f1cf26ba.png)

期望值 f(x)（图片由作者提供）

同样的原理自然适用于 g(x)。

![](img/442a89d0704b4fecb20512b2a76d1c4e.png)

预期值 g(x)（作者提供的图片）

现在想象我们想通过抽样从我们的总体中计算统计量。例如，我们想计算掷 n 次骰子的结果的平均值。

我们掷一个公平骰子 n 次，我们知道根据中心极限定理，随着 n 的增加，这个平均值将趋向于预期值。

![](img/93cf9c3feccfee9d19bfd2e8319baaa9.png)

作者提供的图片

现在我们想通过掷 n 次另一个骰子，即不公平的骰子，来计算相同的统计量，那个骰子具有不同的概率分布。问题是这个骰子不存在，因此我们无法实验！

那我们该如何做到这一点呢？我们可以通过始终使用公平骰子来计算这个统计量，但使用一个“*技巧*”。

我们知道如何计算这个骰子的预期值，使用分布 g(x)。接下来我们要做的是乘以并除以相同的数量 f(x)。

![](img/6dc6af3b7fd31e770164f1dd167954a3.png)

作者提供的图片

现在如果我们将 x 叫做求和中的第一部分，这个值实际上是 f(x) 的预期值，其中 x 是加权的。

![](img/4d5ec83a525f073282736e154548ee72.png)

作者提供的图片

所以我们可以使用之前的相同思路，进行一个抽样，其中 **我们按 g(x)/f(x) 权重每次抽取**，结果应该接近 g 的预期值。

一切都很好，但这真的有效吗？让我们做一些实验！

## 让我们编码吧！

首先，我们导入一些需要的库。

```py
import numpy as np
import matplotlib.pyplot as plt
```

现在我们用两个 numpy 数组表示两个骰子的分布。第一个是公平骰子，每一面的概率相同。第二个则是每一面概率不同的骰子。

```py
f = np.array([1/6, 1/6, 1/6, 1/6, 1/6, 1/6])
g = np.array([0.4, 0.3 ,0.1 ,0.1, 0.06, 0.04])
```

如果你愿意，可以使用 Matplotlib 绘制分布图。

```py
plt.bar(np.arange(1,7), f)
plt.show()
```

![](img/d19fddf8f9ae16a557f601bf3e5c7c09.png)

作者提供的图片

```py
plt.bar(np.arange(1,7), g)
plt.show()
```

![](img/045e74847b6b1e333bde6b6bb209bbd5.png)

作者提供的图片

现在让我们定义一个函数来计算实验的经验均值。也就是说，我们掷一个骰子 n 次，然后将获得的结果总和除以 n。选择的数字 n 越大，经验均值就会越接近预期值。

```py
def compute_avg(distr, n = 10_000):
  mean = 0
  for i in range(n):
    mean += np.random.choice(a = np.arange(1,7),  p=distr)
  print(f"average from sampling: {mean/n}")
```

如果你现在在两个分布 f 和 g 上计算这个经验均值，你应该会找到一个类似于预期值的均值。

```py
compute_avg(f)
compute_avg(g)
```

问题在于我们必须假设一个你无法直接计算 g 的经验均值的情况，因为例如，你没有这样的骰子，因此无法实验。你唯一拥有的骰子是公平骰子，尽管如此，你知道两个骰子的分布。

然后，正如我们之前看到的，我们可以创建一个从分布 f 中抽样的函数，然后通过之前展示的方式加权提取的数字，我们可以计算出好像它是从分布 g 中提取的均值。现在我们要做的就是编写这个函数，看看它是否真的有效。

```py
def importance_sampling(f, g, n = 10_000):
  mean = 0
  for i in range(n):
     x = np.random.choice(a = np.arange(1,7),  p=f)
     weight = g[x-1]/f[x-1]
     x = x*weight
     mean += x
  print(f"average from sampling: {mean/n}")
```

因此，我们在之前提到的 f 和 g 分布上进行了 n = 10,000 次的重要性采样。

```py
importance_sampling(f,g)
```

这个函数的结果是 2.24920，非常接近预期值！所以我们通过编写 Python 代码具体展示了这个方法的有效性！

# 最后的想法

在这篇文章中，我们探讨了数据科学家最重要的采样技术之一。重要性采样允许我们从一个分布中进行采样，即使我们只能访问另一个分布。如果例如从目标分布中采样成本过高，或者由于任何原因不可能进行采样，也可能发生这种情况。我希望你在这篇文章中学到了有用的东西，如果你对这类文章感兴趣，可以在 Medium 上关注我！[😉](https://emojipedia.org/it/apple/ios-15.4/faccina-che-fa-l-occhiolino/)

# 结束

*马尔切洛·波利提*

[Linkedin](https://www.linkedin.com/in/marcello-politi/), [Twitter](https://twitter.com/_March08_), [Website](https://marcello-politi.super.site/)
