# 探索解决股票切割问题的贪婪算法

> 原文：[`towardsdatascience.com/how-bad-is-being-greedy-cdec75ce52a6`](https://towardsdatascience.com/how-bad-is-being-greedy-cdec75ce52a6)

## 为股票切割问题创建快速估算

[](https://medium.com/@jarom.hulet?source=post_page-----cdec75ce52a6--------------------------------)![Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----cdec75ce52a6--------------------------------)[](https://towardsdatascience.com/?source=post_page-----cdec75ce52a6--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----cdec75ce52a6--------------------------------) [Jarom Hulet](https://medium.com/@jarom.hulet?source=post_page-----cdec75ce52a6--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----cdec75ce52a6--------------------------------) ·7 分钟阅读·2023 年 8 月 3 日

--

目录

1.  股票切割问题的动机

1.  NP-Hard 问题的快速概述

1.  将股票切割问题编码到 Python 中

1.  贪婪算法

1.  与低维空间中的穷举搜索的比较

1.  与随机搜索在更高维空间中的比较

1.  结论

**股票切割问题的动机**

我是一名数据科学家。虽然我的数据科学技能在工作中非常重要（这很自然），但我发现数据科学概念也帮助解决了许多工作之外的问题！

我的数据科学技能在我的 DIY 爱好中派上了用场。一个常见的挑战是知道如何规划材料的切割。我有一个需要从多个同尺寸材料中切割的清单。如何规划这些切割以尽可能少浪费呢？这个挑战被称为“股票切割问题”。事实证明，这个问题很难解决，实际上是 NP-Hard 的！

在本文中，我将探索一种“捷径”（双关语）来解决这个问题，并分析这种方法与长途解决方案的比较。

![](img/caa0f14bb735ef86d7b0cb579cd80b61.png)

我去年制作的白橡木桌面，钢制底座 — 作者图片

**NP-Hard 问题的快速概述**

我不会在这里深入探讨 NP-Hard 问题，但我确实想给出一些直观的理解。使优化问题变得困难的通常是其解决方案空间的大小。也就是说，你需要探索多少可能的解决方案才能找到最佳方案？一个问题的难度通常通过问题规模增长时解决方案空间增长的速度来评估。

对于“股票切割问题”，当你增加更多切割时，问题变得*更大*。看看解决方案空间增长的速度吧！

![](img/c73fc6e412c472b7510e0fa48856af55.png)

作者图片

解空间呈阶乘增长，这意味着必须搜索的解决方案总数是 n!，正如你所看到的，这个数字增长非常快。

NP 代表‘非确定性多项式’，这意味着问题的增长速度快于任何多项式函数。有很多资源深入探讨 NP/NP-hard 问题。这里我只会讨论到这些。

**将库存切割问题编码为 Python**

库存切割问题本质上是一个排序问题。你按照特定顺序进行切割，当某块库存的长度用完时，你开始在下一块库存上进行切割（仍然按照顺序）。

可视化最能解释这一点。假设我们有这个切割顺序：[4', 2', 1', 2', 2', 1']，并且我们有 5' 的库存。浪费计算如下：

![](img/4342c4196943f2f7eb7bda65abdfbcfd.png)

图片作者

这里我们总共浪费了 4'。

但是，如果我们改变顺序（保持所有相同的切割），可能会得到不同的浪费水平。让我们尝试 [4', 1', 2', 2', 1', 2']：

![](img/2628df3b9d055812d911a170c04f4c32.png)

图片作者

在这里，我们只有 3' 的浪费——简单地改变顺序就减少了我们的浪费！这就是这个优化问题的核心思想。我们想找出哪种切割顺序最好。

现在，为了将其编码为 Python 脚本，我们需要 (1) 用于计算每个订单浪费的函数，和 (2) 将列表排序为最优顺序的算法。

计算浪费的函数只是复制了上述逻辑。以下是 Python 代码：

```py
def total_waste(stock_size, solution):

    '''
        Calculates cutoff waste give a specific solution

        inputs
        stock_size (float) : the dimension of the stock 
                             available for purchase
        solution (list)    : list of floats depicting the 
                             order to make the cuts

        output
        cut_off_waste (float) : the total cutoff waste of 
                                the given solution

    '''

    # set up variable to keep track of the total lengths of 
    # cuts on current piece of stock
    temp_cut_length = 0

    # start with no waste
    cut_off_waste = 0

    for cut in solution:

        # if next cut doesn't fit on current stock,
        # calculate new waste and reset for another piece of stock
        if temp_cut_length + cut > stock_size:

            # calculate cutoff waste
            cut_off_waste += stock_size - temp_cut_length

            # reset cut length
            temp_cut_length = cut

        else:
            # add to cumulative length of cuts on stock
            temp_cut_length += cut

    # add in last cut waste -- it is not captured in the loop
    cut_off_waste += stock_size - temp_cut_length

    return cut_off_waste
```

我们将在下一节中介绍所需的算法。

**贪婪算法**

贪婪算法非常简单，只需找到适合当前库存剩余部分的最大块。

使用之前的示例，我们将假设我们想进行这些切割 [4', 2', 1', 2', 2', 1']，并且我们需要一个贪婪算法来优化顺序。

我们首先从适合当前库存的最长切割开始。由于我们还没有进行任何切割，我们当前的库存长度是 5'。4' 是我们拥有的最长切割，它适合在 5' 的剩余库存中。下一个最长的切割是 2'，由于我们只剩下 1' 的库存，这个切割太长了。我们转到下一个最长的切割，即 1'。它适合在剩余库存中，因此我们下一个切割是 1'。该算法按照这种模式进行，直到没有剩余的切割。

以下是该算法在 Python 中的实现：

```py
def greedy_search(cuts, stock_size):

    '''
        Calculates a greedy optimal solution

        inputs:
        cuts (list)        : cuts that need to be made
        stock_size (float) : size of stock available for purchase

        outputs:
        cut_plan (list) : sequence of cuts to obtain greedy 
                          optimal results
        waste (float)   : amount of material wasted by solution

    '''

    # empty cut off plan, to be populated
    cut_plan = []
    # start with cutoff size equal to stock size
    cut_off = stock_size
    # copy cuts list to avoid modifying original list
    cuts_copy = copy(cuts)

    # sort cuts in desc order
    cuts = list(np.sort(cuts)[::-1])

    # continue ordering cuts until
    # all cuts have been ordered
    while len(cuts_copy) > 0:

        for cut_size in cuts_copy:

            # if cut size is smaller than remaining stock,
            # assign the cut now
            if cut_size < cut_off:

                # add cut to plan
                cut_plan.append(cut_size)

                # update the leftover amount
                cut_off -= cut_size

                # remove cut from list of cuts still needing 
                # to be ordered
                cuts_copy.remove(cut_size)

        # reset cut_off to be the full stock size
        cut_off = stock_size

    # calculate waste using total_waste function
    waste = total_waste(stock_size, cut_plan)

    return cut_plan, waste
```

**与低维空间穷举搜索的比较**

通过使用贪婪算法的近似解，我们节省了大量时间，但这种近似解有多好？对解空间进行穷举搜索可以得到全局最优解——这是我们的金标准。让我们将贪婪算法的解与仅有少量切割的全局最优解进行比较（记住，找到大量切割的全局最优解确实很困难）。

我随机创建了 250 个切割列表，切割尺寸范围从 2 到 10。每个切割的范围是 0 到 1，库存尺寸设置为 1.1。以下是性能表现：

![](img/4a00607ae60097207fccb13665eafacd.png)

图片由作者提供

正如你所见，随着'n'的增加，贪婪算法的性能相对于全局最优解变得更差，但仍然相对接近且高度相关。

**与高维空间中的随机搜索比较**

不幸的是，我们有一个重大的盲点……也就是说，我们不知道在高维空间中的全局最优解。现在，我们进入比较不同类型启发式算法（旨在近似全局最优解的算法）的领域。贪婪算法与随机搜索解空间的效果如何？

![](img/afa9178a5b22b28e3fe9bbc59de50f81.png)

图片由作者提供

我们可以看到，随着切割数量的增加，随机搜索的效果变得越来越差。这是有道理的，因为随机搜索是在随机选择 500 个解并挑选最佳的一个——随着解空间的爆炸性增长，我们随机搜索到的解空间的概率百分比越来越小。我们现在可以看到，贪婪解法明显比随机查看潜在解决方案要好。

**结论**

看起来，对于股票切割问题，贪婪解法是一种合理且非常快速的找到相当好解的方法。对我来说，这就足够了。我每年只做几个项目，通常规模较小。然而，对于制造厂等应用，更复杂且密集的方法可能对公司有显著的经济影响。你可以研究一下“混合整数线性规划”（MILP），这是一种寻找更优解的方法。

如果我要深入研究这个问题，我会将贪婪方法与更好的元启发式算法（随机搜索可能是最差的）进行比较，例如各种版本的爬山算法、禁忌搜索或模拟退火。不过现在我就先放到这里，我还有另一个表格要做！

有关本文中使用的所有代码，请参见这个 [repo](https://github.com/jaromhulet/stock_cutting.git)。
