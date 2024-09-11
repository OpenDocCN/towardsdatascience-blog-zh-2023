# 深度学习笔记：梯度下降

> 原文：[https://towardsdatascience.com/gradient-descent-f09f19eb35fb?source=collection_archive---------13-----------------------#2023-11-04](https://towardsdatascience.com/gradient-descent-f09f19eb35fb?source=collection_archive---------13-----------------------#2023-11-04)

## 神经网络如何“学习”

[](https://medium.com/@luisdamed?source=post_page-----f09f19eb35fb--------------------------------)[![Luis Medina](../Images/d83d326290ae3272f0618d0bd28bd875.png)](https://medium.com/@luisdamed?source=post_page-----f09f19eb35fb--------------------------------)[](https://towardsdatascience.com/?source=post_page-----f09f19eb35fb--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----f09f19eb35fb--------------------------------) [Luis Medina](https://medium.com/@luisdamed?source=post_page-----f09f19eb35fb--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F562a027a34f0&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-f09f19eb35fb&user=Luis+Medina&userId=562a027a34f0&source=post_page-562a027a34f0----f09f19eb35fb---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----f09f19eb35fb--------------------------------) ·11分钟阅读·2023年11月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ff09f19eb35fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-f09f19eb35fb&user=Luis+Medina&userId=562a027a34f0&source=-----f09f19eb35fb---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ff09f19eb35fb&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fgradient-descent-f09f19eb35fb&source=-----f09f19eb35fb---------------------bookmark_footer-----------)![](../Images/c5a5af3b75e58cac86deb3abd95692a0.png)

图片由 [Rohit Tandon](https://unsplash.com/@sepoys?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit) 提供 / [Unsplash](https://unsplash.com/?utm_source=ghost&utm_medium=referral&utm_campaign=api-credit)

人工神经网络（ANNs）是 [通用函数逼近器](https://www.youtube.com/watch?v=lkha188L4Gs%3Fref%3Dmakerluis.com)。只要提供足够的数据、具有适当的架构，并且经过 *足够长时间的训练*，它们可以逼近任何复杂的函数。

那么“训练”网络到底是什么意思呢？

在 [之前关于前馈过程的文章](https://medium.com/@luisdamed/feedforward-artificial-neural-networks-52bcf96d6ac3) 中，我提到训练一个网络意味着调整其权重的值，以获得对我们试图逼近的函数更好的拟合。

在这篇文章中，我将描述梯度下降算法，它用于调整人工神经网络（ANN）的权重。

让我们从基本概念开始。

# 从山上下降

想象一下，我们在山顶上，需要到达旁边山谷的最低点。

我们没有地图，天气阴霾，天色渐暗，我们丢失了路线，需要尽快到达底部。这不是一个好场景，但它展示了问题的“边界”。

为了安全起见，假设山上没有陡峭的山脊，因此它类似于一个[可微函数](https://en.wikipedia.org/wiki/Differentiable_function?ref=makerluis.com)。

![](../Images/95b5dc5e213b18cdb7bc509d24da0138.png)

从蒙维索峰（Monviso peak）下山。[靠近翁奇诺，库内奥的小山谷](https://www.google.com/maps/@?api=1&map_action=map&basemap=satellite&center=44.659382%2C7.120437&zoom=16&ref=makerluis.com)。图像由作者提供。

当天黑时，我们看不到我们移动的方向。我们唯一能做的就是迈小步，并检查我们是否处于较低的高度。

如果我们注意到自己向上移动了，我们就朝相反的方向前进。如果我们向下移动，我们就继续这样做。我们重复这个过程，直到最终到达底部。

如你所见，这不一定是最佳的方法。我们可能会到达一个小山谷，而不是山底，或者可能会在一个平原上花费大量时间。

这说明了梯度下降的基本工作原理及其主要挑战。我们会回到这个例子，但首先让我们看看更正式的解释。

# 什么是梯度？

梯度是函数变化率的表示。它指示了最大增减方向。直观地说，这意味着在局部最大值或局部最小值处梯度为零。

对于依赖多个变量（或坐标轴）的函数，梯度是一个向量，其分量是函数在给定点的偏导数。这用符号∇（nabla）表示，代表向量微分算子。

让我们用数学符号来看一下。假设我们有一个n维函数f：

该函数在点p（由n个坐标确定）处的梯度为：

回到山的例子，山上有些地方地势陡峭，比如山坡，还有些地方地势几乎平坦，比如山谷或平原。山谷和平原代表局部最小值，这通常是关键点。

# 梯度下降方法

对于许多优化问题，我们的目标是最小化损失函数，以实现最准确的结果。

在深度学习和人工神经网络（ANN）中，我们使用的损失函数是可微的：它们在整个定义域内平滑且无不连续性。

这使我们能够使用相对于自变量的损失函数的导数作为是否正在朝着解决方案（全局最小值）移动的指示。

我们在与导数成比例时的步长有多大？这由步长参数*η*决定（当我们谈论深度学习时，我们可以称之为**学习率**）。它将乘以梯度，调整步长的大小。

这样一来，更陡的梯度将产生更大的步长。当我们接近局部最小值时，斜率（梯度）将趋近于零。

让我们看一下下面的图，以说明在优化一维函数时这如何工作：

![](../Images/cdef9ea00750a1e9e7aebad15276301b.png)

一维问题中梯度下降的简化示例。图片由作者提供。

正如你所看到的，我们从一个任意点（我画了两个例子，A和B）初始化我们的“搜索”以寻找最小值。我们逐渐向最近的最小值迈进，*θ*的变化与斜率成比例。

插图表示以下算法的功能（伪代码）[1]：

```py
θ(0) = θ_init       # Initialization of optimization variable
η = 0.02            # Arbitrary step-size parameter
Є = 0.01            # Optimization accuracy
k = 0               # Iteration counter

while |f(θ(k+1) - f(θ(k))| > Є:
 θ(k+1) = θ(k) - η ∇f(θ(k))
 k = k + 1
```

这里的损失函数就像我们在黑暗中没有地图的山脉：我们不知道它的样子。我们想知道哪个*θ*值能使***J***最小化，但优化算法并不知道***J***在所有可能的*θ*输入下的值。

这就是为什么我们用任意的*θ*值初始化我们的优化算法。例如，图中的点A和B表示两个不同的初始化值。

# 梯度下降的潜在问题

梯度下降算法是有效的，因为它可以帮助我们为任何凸函数获得一个近似解。

如果我们尝试优化的函数是凸的，对于任何值*ϵ*，都有某个步长*η*使得梯度下降将在*ϵ*内收敛到*θ*的真实最优值*θ*。 [1]

然而，正如你可能猜到的，这并不完美。算法可能会收敛，但这并不保证我们会找到全局最小值。

梯度下降的主要挑战如下：

## 任意初始化对结果有影响

使用不同的初始化值，我们可能会遇到局部最小值而不是全局最小值。

例如，从上图中的点B开始，而不是点A。

或者更不明显的情况，例如，像下图中的蓝线所示，收敛到一个平台（梯度消失问题）。

![](../Images/e06647b8def86857c479409b11888d71.png)

香草梯度下降在鞍形表面上的效果。动画显示了不同初始化点可能产生不同结果的情况。图片由作者提供。

> *💡* 如果你对如何创建这种动画感兴趣，可以查看我的文章 [在 Python 中创建梯度下降动画](/creating-a-gradient-descent-animation-in-python-3c4dcd20ca51)。

## 选择适当的步长需要在收敛速度和稳定性之间做出权衡。

步长或学习率与我们应使用的轮次数之间存在相互作用，以实现准确的结果。

以下面的参数实验结果为例。这些图像来自 Mike X Cohen 的在线课程 [深度学习的深刻理解](https://www.udemy.com/course/deeplearning_x/?ref=makerluis.com)，我强烈推荐给任何对深度学习感兴趣并使用 [PyTorch](https://pytorch.org/?ref=makerluis.com) 的人，遵循科学的方法。

在这种情况下，Mike 展示了如何在独立改变学习率和训练轮次（一个参数随时间变化，网格上不同的值）时测试梯度下降优化的结果。我们可以看到这两个参数如何影响这个特定案例的结果。

![](../Images/4bd9c40334c3a73736990c791efe9f16.png)

图像来自 Mike X Cohen 的在线课程 [深度学习的深刻理解](https://www.udemy.com/course/deeplearning_x/?ref=makerluis.com)。

函数的真正全局最小值在-1.4左右。对于较小的学习率，收敛到该结果需要更多的训练轮次。因此，单纯使用更大的学习率似乎可以帮助我们减少计算时间。

但实际上，这并不是一个简单的收敛速度问题。

大步长可能导致非常缓慢的收敛，阻止算法完全收敛（在极小值周围不断振荡），或引发发散行为。

下一张图显示了不同学习率如何影响优化结果，即使我们在同一位置 x = 2 初始化算法。

![](../Images/17adbdaca929c0d64adab695e1b7f420.png)

对 *f*(*x*)=*x^2* 应用不同步长的随机梯度下降示例。在所有情况下，算法都在 x = 2 处初始化。图像由作者提供，基础代码改编自 [Saraj Rival 的笔记本](https://github.com/llSourcell/The_evolution_of_gradient_descent/blob/master/GD_vs_SGD.ipynb?ref=makerluis.com)。

在这里，很明显大步长能提高收敛速度，但仅到达某一点为止。

将学习率提高一个数量级导致算法陷入困境。*η* = 1 的结果在 x = 2 和 x = -2 之间振荡，这仅由左侧图中的蓝色水平线表示。

在某些情况下，大步长可能会将结果“射”向无限，导致程序的数值溢出。

另一方面，过小的步长可能导致非常缓慢的收敛或完全不收敛。

# 神经网络训练的梯度下降

在深度学习的背景下，我们试图优化的函数是我们的损失函数 ***J***。我们将训练损失定义为所有训练数据集损失的平均值：

其中 Dtrain 是我们训练数据集中的样本数量。

因此，我们可以基于以下算法实现梯度下降，其中我们计算训练损失的梯度以执行模型权重的更新，并重复若干次迭代[2]

```py
w = [0, ... ,0]    # Initialize the weights
for k = 1,..., n_iters:          # Repeat for n iterations
  grad = ∇w TrainLoss(w)         # Gradient of the Training losses 

  w[k] <-- w[k-1] - η * grad     # Update the model weights
```

由于我们计算的是损失函数的梯度平均值，因此我们对它有更好的估计。因此，权重更新更有可能朝着改善模型性能的方向进行。

这种梯度下降实现的问题是其速度较慢。

对于一个有几个点和一个简单函数的玩具示例，它可能效果很好，但想象一下我们在开发一个ANN，并且我们有一百万个数据点来训练它。

要用这个算法训练ANN，我们需要计算模型对每个训练数据样本的输出，然后将它们的损失平均作为一个大批量。仅仅为了做*一次*权重更新。然后不断重复，直到达到收敛。

这被称为*批量*梯度下降。它每次迭代做一次（准确的）权重更新，但每次迭代可能需要*很长时间*，因为我们重复模型计算 n 次。

为了克服这个问题，我们可以使用所谓的随机梯度下降算法。

## 随机梯度下降

为了克服批量梯度下降的慢收敛问题，我们可以基于训练集中的每个样本来更新模型权重。其优点是我们不需要等到遍历整个数据集后才进行一次权重更新。

我们可以通过使用每个个体样本的损失函数来做到这一点，而不是使用考虑整个数据集作为批量的训练损失。

这就是随机梯度下降算法（SGD）的样子

```py
w = [0, ... ,0]                          # Initialize the weights
for k = 1,..., n_epoch:      
    for (x, y) ∈ Dtrain:                 # For each sample 
        grad = ∇w J(x,y,w)              
        w[k] <--  w[k-1] - η(k) * grad   # Update the model weights
```

注意步长是训练迭代的一个函数。这是因为为了使算法收敛，η必须随着迭代次数的增加而减小。

在[MIT 6.036的讲义](https://openlearninglibrary.mit.edu/courses/course-v1:MITx+6.036+1T2019/courseware/Week4/gradient_descent/?activate_block_id=block-v1%3AMITx%2B6.036%2B1T2019%2Btype%40sequential%2Bblock%40gradient_descent%3Fref%3Dmakerluis.com&ref=makerluis.com) [1]中提到以下定理：

定理 4.1。如果 J 是凸的，并且 η(t) 是一个满足

然后 SGD 以概率1收敛到最优*θ*。

人们采取不同的方法来在训练过程中降低η的值，这通常被称为“退火”：

+   根据训练轮次调整学习率（例如，η(t) = 1/t），或者在达到某个学习轮次后将其设为更小的值。这个方法效果很好，但与模型性能无关。这被称为“学习率衰减”，是处理此问题的行业标准。

+   将学习率乘以损失函数的梯度：这种方法很好，因为它对问题是自适应的，但需要仔细调整。这已经被纳入了RMSprop和Adam梯度下降的变体中。

+   将学习率乘以损失：优点是这种方法同样对问题自适应，但也需要缩放。

SGD在仅访问部分数据后可能表现良好。这种行为对相对较大的数据集很有用，因为我们可以减少所需的内存量，并且与“原生”梯度下降实现相比，总运行时间较短。

我们可以说，批量实现较慢，因为它需要遍历所有样本才能进行一次权重更新。

SGD在每个样本上执行更新，但更新的质量较低。我们可能有噪声数据或一个非常复杂的函数需要用我们的ANN拟合。

使用大小为Dtrain的批量很慢，但准确，使用大小为1的批量很快，但不够准确。

在两者之间有一个术语，称为“mini-batch”梯度下降。

# Mini-Batch梯度下降

![](../Images/e49352109af2b9cbce81357c4eb26785.png)

图片由[Sebastian Herrmann](https://unsplash.com/@herrherrmann?utm_source=medium&utm_medium=referral)提供，来源于[Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

如果我们将数据分成大小相等的更小批量，我们可以做批量梯度下降所做的事情，但针对每个*mini-batch*。

假设我们将数据分成100个更小的部分。

我们将数据分成100步。在每一步中，我们仅查看当前mini-batch中数据的训练损失，并改进我们的模型参数。我们重复这一过程，直到查看所有样本，然后重新开始循环。

每个循环称为**周期**。我之前更宽泛地使用了这个术语来指代优化过程中的迭代次数，但通常的定义指的是每次遍历训练数据集。对于批量梯度下降，*迭代*就是一个周期。

当我们设置训练ANN的周期数时，我们是在定义通过训练数据集的遍历次数。

使用mini-batch的优点是我们在每个mini-batch上更新模型参数，而不是在查看整个数据集后再更新。

对于批量梯度下降，批量大小是训练数据集中样本的总数。对于mini-batch梯度下降，mini-batch通常是2的幂次：32个样本、64、128、256等。

当mini-batch大小减少到训练数据集中的单个示例时，SGD将是一个极端情况。

使用mini-batch梯度下降的缺点是我们引入了一定程度的变异性——虽然比随机梯度下降（SDG）轻微。这并不保证每一步都会使我们更接近理想参数值，但总体方向仍然是向最小值靠近。

这种方法是行业标准之一，因为通过找到最佳批量大小，我们可以在处理非常大的数据集时在速度和准确性之间进行折衷。

感谢阅读！我希望这篇文章对你有趣，并且帮助你澄清了一些概念。我还分享了撰写这篇文章时使用的资料，供你参考，以便深入了解更正式的内容。

在未来的帖子中，我将写关于 [更高级的梯度下降方法](https://medium.com/towards-data-science/dl-notes-advanced-gradient-descent-4407d84c2515)（那些人们在实际应用中使用的方法）以及我们如何在训练过程中实际更新模型权重，使用反向传播，因为梯度下降只是整个过程的一部分。

与此同时，你可能会对阅读我之前的文章感兴趣，关于前馈人工神经网络：

[## 前馈人工神经网络](https://medium.com/@luisdamed/feedforward-artificial-neural-networks-52bcf96d6ac3?source=post_page-----f09f19eb35fb--------------------------------)

### 基本概念解释

[medium.com](https://medium.com/@luisdamed/feedforward-artificial-neural-networks-52bcf96d6ac3?source=post_page-----f09f19eb35fb--------------------------------)

# 参考文献

[1] [MIT 开放学习图书馆：6.036 机器学习导论。第 6 章：梯度下降](https://openlearninglibrary.mit.edu/assets/courseware/v1/d81d9ec0bd142738b069ce601382fdb7/asset-v1:MITx+6.036+1T2019+type@asset+block/notes_chapter_Gradient_Descent.pdf?ref=makerluis.com)

[2] [斯坦福在线：人工智能与机器学习 4 — 随机梯度下降 | 斯坦福 CS221 (2021)](https://www.youtube.com/watch?v=bl2WgBLH0tI%3Fref%3Dmakerluis.com&ref=makerluis.com)

[3] 在线课程 [深入理解深度学习](https://www.udemy.com/course/deeplearning_x/?ref=makerluis.com)，由 Mike X Cohen 提供（ [sincxpress.com](https://sincxpress.com/?ref=makerluis.com)）

[4] [斯坦福在线：CS231 视觉识别中的卷积神经网络](https://cs231n.github.io/neural-networks-3?ref=makerluis.com)

*最初发表于* [*https://www.makerluis.com*](https://www.makerluis.com/gradient-descent/) *于 2023 年 11 月 4 日。*
