# PyTorch 的 Nesterov 动量实现是否有误？

> 原文：[https://towardsdatascience.com/is-pytorchs-nesterov-momentum-implementation-wrong-5dc5f5032008?source=collection_archive---------10-----------------------#2023-09-02](https://towardsdatascience.com/is-pytorchs-nesterov-momentum-implementation-wrong-5dc5f5032008?source=collection_archive---------10-----------------------#2023-09-02)

[](https://medium.com/@jasonvega14?source=post_page-----5dc5f5032008--------------------------------)[![Jason Vega](../Images/712b2e34c1af7a8572193296bdd8b206.png)](https://medium.com/@jasonvega14?source=post_page-----5dc5f5032008--------------------------------)[](https://towardsdatascience.com/?source=post_page-----5dc5f5032008--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----5dc5f5032008--------------------------------) [Jason Vega](https://medium.com/@jasonvega14?source=post_page-----5dc5f5032008--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fa9932c231079&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-pytorchs-nesterov-momentum-implementation-wrong-5dc5f5032008&user=Jason+Vega&userId=a9932c231079&source=post_page-a9932c231079----5dc5f5032008---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----5dc5f5032008--------------------------------) ·6 分钟阅读·2023年9月2日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F5dc5f5032008&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-pytorchs-nesterov-momentum-implementation-wrong-5dc5f5032008&user=Jason+Vega&userId=a9932c231079&source=-----5dc5f5032008---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F5dc5f5032008&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fis-pytorchs-nesterov-momentum-implementation-wrong-5dc5f5032008&source=-----5dc5f5032008---------------------bookmark_footer-----------)![](../Images/0e58bb7374f65a47beb941d924f19ba1.png)

动量帮助 SGD 更有效地遍历复杂的损失景观。图片由 [Maxim Berg](https://unsplash.com/@maxberg?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)。

# 引言

如果仔细查看 PyTorch 的 [文档](https://pytorch.org/docs/stable/generated/torch.optim.SGD.html)，你会发现他们对 Nesterov 动量的实现与 [原始论文](http://proceedings.mlr.press/v28/sutskever13.pdf) 中的公式有一些不同。最显著的是，PyTorch 的实现是在当前参数上计算梯度，而 Nesterov 动量的核心是评估偏移参数的梯度。不幸的是，关于这些差异的讨论在互联网上很少。在这篇文章中，我们将检查并解释 PyTorch 实现与 Nesterov 动量原始公式之间的差异。最终，我们将看到 PyTorch 的实现并非错误，而是一种近似，并推测其实现的好处。

# 公式

[原始论文](http://proceedings.mlr.press/v28/sutskever13.pdf) 使用以下更新规则描述了 Nesterov 动量：

其中 *`v_{t+1}`* 和 *`θ_{t+1}`* 分别是时间 *t* 的速度向量和模型参数，*μ* 是动量因子，*ε* 是学习率。PyTorch 的 SGD [文档](https://pytorch.org/docs/stable/generated/torch.optim.SGD.html) 中的说明称他们使用以下更新规则：

其中 *`g_{t+1}`* 代表用于计算 *`v_{t+1}`* 的梯度。我们可以展开 *`θ_{t+1}`* 的更新规则得到：

从中我们可以推断：

和更新规则变成：

这些是 PyTorch 理论上使用的更新规则。我之前提到过，PyTorch 实际上是在当前参数上评估梯度，而不是偏移参数。通过查看 PyTorch SGD 文档中的算法描述可以看到这一点。我们将在后面进一步调查这一点。

请注意，对于原始公式 (1, 2) 和 PyTorch 公式 (3, 4) 来说，如果 *`v_0 = 0`*，则 *`θ`* 的第一次更新变为：

尽管 PyTorch SGD 文档中说明算法在第一步将动量缓冲区初始化为梯度，但我们稍后会显示这意味着 *`v_0 = 0`*。

# 初步差异

从原始公式 (1, 2) 到 PyTorch 公式 (3, 4) 存在两个直接的区别：

1.  学习率被移出了 *`v_{t+1}`*。

1.  在*`v_{t+1}`*的更新规则中，涉及梯度的项被添加而不是减去，而在*`θ_{t+1}`*的更新规则中，涉及速度向量的项被减去而不是添加。这种梯度项符号的差异仅仅是前一节中展示的结果。

要理解这些差异，让我们首先展开更新规则。如 [这里](https://github.com/pytorch/pytorch/issues/1099#issuecomment-289190614) 所提示的，如果我们考虑学习率调度，第一种差异的影响会更为明显。因此，我们考虑一个更新规则的广义化，其中 *ε* 不再是固定的，而是可以随时间变化，并将 *ε_t* 记作时间步 *t* 时的学习率。为简洁起见，设：

假设 *v_0 = 0*，原始公式变为：

那么 PyTorch 公式变为：

在原始公式（6）中，如果学习率在时间 *t* 发生变化，那么仅会影响求和中 *i = t* 的项的大小，而所有其他项的大小保持不变。因此，学习率变化的直接影响是相当有限的，我们必须等待学习率变化在随后的时间步中“逐渐”发挥更强的作用。相比之下，在 PyTorch 公式（7）中，如果学习率在时间 *t* 发生变化，那么整个步骤的大小会立即受到影响。

对于 *v_0 = 0*，从展开的规则可以看出，第二种差异最终没有影响；在任一公式中，这一步的结果都是从当前参数中减去折扣后的梯度和。

# 主要差异

忽略权重衰减和阻尼，通过分析 PyTorch 的 [文档](https://pytorch.org/docs/stable/generated/torch.optim.SGD.html)，我们可以看到实现的更新规则是：

其中 *θ’_{t+1}* 是时间 *t* 时的模型参数

我们将方程 3 和 4 称为 PyTorch 的“笔记”公式，将方程 8 和 9 称为 PyTorch 的“实现”公式。我们在*θ*和*θ’*之间做了区分，原因会很快显现出来。与笔记公式最明显的区别是梯度在当前参数处进行评估，而不是在偏移后的参数处。从这一点来看，算法实现的更新规则似乎并不是 Nesterov 动量的正确实现。

我们现在将考察 PyTorch 算法如何最终近似 Nesterov 动量。旧版本 PyTorch 的推导可以在 Ivo Danihelka 提供的 [这里](https://github.com/fidlej/optim/raw/master/dok/nesterov_simple.pdf) 找到，并在 [这个 GitHub 问题](https://github.com/torch/optim/issues/27) 中引用。当前版本 PyTorch 的推导可以在 [这里](https://github.com/pytorch/pytorch/pull/5920#issuecomment-375181908) 找到，这相对是对之前推导的简单调整。为了方便读者，我们在此提供这些（重新推导的）推导的 LaTeX 渲染。实现的公式是通过简单的变量变换得到的。具体而言，我们设：

很明显，*v_{t+1}* 的注释更新规则 (3) 在变量变换后与 *v_{t+1}* 的实现更新规则 (8) 等效。我们现在想要推导一个关于 *θ’_{t+1}* 的更新规则，以 *θ’_t* 为变量：

这正是我们在 PyTorch (9) 中看到的更新规则。总体上，PyTorch 实现假设当前参数 *θ’_t* 已经是“实际”参数 *θ_t* 的偏移版本。因此，在每一个时间步， “实际”参数 *θ_t* 与当前参数 *θ’_t* 之间的关系为：

然而，从源代码来看，PyTorch SGD 实现似乎在算法结束时没有对“实际”参数进行任何修正，因此最终输出在技术上是“实际”参数的近似值。

最后，我们现在展示 *v_0* 必须为 0：

此外，我们可以确认对“实际”参数的第一次更新与原始表述中 *v_0 = 0* 时的第一次更新相同：

我们可以看到这等同于方程 5。

# 实现表述的好处

当然，剩下的大问题是：为什么 PyTorch 要从方程 3 和 4 重新表述 Nesterov 动量到方程 8 和 9？一个可能的解释是，重新表述可能在所需的算术操作数量上有所节省。为了评估这个可能的解释，我们来计算一下算术操作的数量。对于注释的表述 (3, 4)，我们有：

在这里，总共有七个操作。对于实现的表述 (8, 9)，我们有：

在这里，总共有六个操作。PyTorch 实现中的第二个梯度只是使用了第一个梯度计算的保存结果，因此每个时间步只执行一个梯度计算。因此，一个明显的好处是 PyTorch 实现减少了每一步的一个额外乘法操作。

# 结论

总结

1.  PyTorch 的 SGD 文档注释 (3, 4) 中的更新规则与原始 Nesterov 动量更新规则 (1, 2) 中的学习率位置不同。这允许学习率调度对整体步长有直接影响，而原始表述则会使学习率变化在随后的时间步中“逐步”体现。

1.  PyTorch SGD 算法 (8, 9) 中实现的更新规则是在简单的变量变换后对文档注释 (3, 4) 中更新规则的近似。尽管“实际”参数可以在每个时间步从当前参数中轻松恢复，但 PyTorch 实现并没有在算法结束时进行任何修正，因此最终参数在技术上仍然是“实际”最终参数的近似值。

1.  PyTorch 实现的一个明显好处是它避免了每个时间步的额外乘法操作。

# 参考文献

1.  “SGD。” SGD — PyTorch 2.0 文档，pytorch.org/docs/stable/generated/torch.optim.SGD.html。访问日期：2023年9月2日。

1.  Sutskever, Ilya等人。“[深度学习中初始化和动量的重要性](http://proceedings.mlr.press/v28/sutskever13.pdf)”。国际机器学习会议。PMLR，2013年。

1.  Danihelka, Ivo. “[Nesterov的动量简明介绍](https://github.com/fidlej/optim/raw/master/dok/nesterov_simple.pdf)”。2012年8月25日。

1.  Chintala, Soumith. “SGD中的Nesterov动量是错误的 · Issue #27 · torch/optim。” GitHub，2014年10月13日，[github.com/torch/optim/issues/27](https://github.com/torch/optim/issues/27)。

1.  Gross, Sam. “在文档中添加关于优化中使用的动量公式的说明 · Issue #1099 · pytorch/pytorch。” GitHub，2017年3月25日，[github.com/pytorch/pytorch/issues/1099#issuecomment-289190614](https://github.com/pytorch/pytorch/issues/1099#issuecomment-289190614)。

1.  Zhao, Yilong. “修复Nesterov动量错误 · Issue #5920 · pytorch/pytorch。” GitHub，2018年3月21日，[https://github.com/pytorch/pytorch/pull/5920#issuecomment-375181908](https://github.com/pytorch/pytorch/pull/5920#issuecomment-375181908)。
