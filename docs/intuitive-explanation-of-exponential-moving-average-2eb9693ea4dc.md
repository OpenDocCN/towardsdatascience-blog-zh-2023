# 指数移动平均的直观解释

> 原文：[https://towardsdatascience.com/intuitive-explanation-of-exponential-moving-average-2eb9693ea4dc?source=collection_archive---------2-----------------------#2023-12-16](https://towardsdatascience.com/intuitive-explanation-of-exponential-moving-average-2eb9693ea4dc?source=collection_archive---------2-----------------------#2023-12-16)

## 理解在梯度下降算法中使用的基本算法背后的逻辑

[](https://medium.com/@slavahead?source=post_page-----2eb9693ea4dc--------------------------------)[![Vyacheslav Efimov](../Images/db4b02e75d257063e8e9d3f1f75d9d6d.png)](https://medium.com/@slavahead?source=post_page-----2eb9693ea4dc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----2eb9693ea4dc--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----2eb9693ea4dc--------------------------------) [Vyacheslav Efimov](https://medium.com/@slavahead?source=post_page-----2eb9693ea4dc--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fc8a0ca9d85d8&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintuitive-explanation-of-exponential-moving-average-2eb9693ea4dc&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=post_page-c8a0ca9d85d8----2eb9693ea4dc---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----2eb9693ea4dc--------------------------------) ·6分钟阅读·2023年12月16日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F2eb9693ea4dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintuitive-explanation-of-exponential-moving-average-2eb9693ea4dc&user=Vyacheslav+Efimov&userId=c8a0ca9d85d8&source=-----2eb9693ea4dc---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F2eb9693ea4dc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintuitive-explanation-of-exponential-moving-average-2eb9693ea4dc&source=-----2eb9693ea4dc---------------------bookmark_footer-----------)![](../Images/f65b757753e90cbe70d7ae2c04608e84.png)

# 介绍

在时间序列分析中，通常需要通过考虑之前的值来理解序列的趋势方向。序列中下一个值的近似可以通过多种方法进行，包括使用简单的基线或构建先进的机器学习模型。

**指数（加权）移动平均** 是这两种方法之间的一个强大折衷方案。其内部具有简单的递归方法，使得算法的高效实现成为可能。同时，它非常灵活，可以成功适应大多数类型的序列。

本文涵盖了该方法背后的动机、工作流程描述和偏差修正——一种有效的技术，用于克服近似中的偏差障碍。

# 动机

想象一个问题是近似一个随时间变化的给定参数。在每次迭代中，我们知道所有先前的值。目标是预测下一个值，它依赖于之前的值。

一种简单的策略是直接取最后几个值的平均值。这在某些情况下可能有效，但对于一个参数更依赖于最新值的情况并不十分合适。

克服这个问题的一种可能方法是对更近期的值分配更高的权重，对之前的值分配更少的权重。指数移动平均正是遵循这一原则的策略。**它基于假设变量的近期值对形成下一个值的贡献大于先前的值**。

# 公式

为了理解指数加权移动平均的工作原理，让我们看一下它的递归方程：

![](../Images/1ee395f76a578feac3fbfb7250c75d0f.png)

指数加权移动平均公式

+   vₜ 是一个时间序列，用于近似给定变量。它的索引 t 对应于时间戳 t。由于这个公式是递归的，因此初始时间戳 t = 0 的值 v₀ 是必需的。在实际应用中，v₀ 通常取为 0。

+   θ 是当前迭代中的观测值。

+   β 是一个介于 0 和 1 之间的超参数，用于定义如何在先前的平均值 vₜ-₁ 和当前观测值 θ 之间分配权重的重要性。

让我们写出这个公式来计算前几个参数值：

![](../Images/1ded5cd1e0b8e9dfc5d8637a452907e7.png)

计算 t 时刻的公式

结果是，最终公式如下：

![](../Images/2520ba4bbbbd0aa1c25fe694d2ce7d6e.png)

t 时刻的指数移动平均

我们可以看到，最近的观测值 θ 的权重为 1，倒数第二个观测值的权重为 β，倒数第三个观测值的权重为 β²，依此类推。由于 0 < β < 1，乘法项 βᵏ 随着 k 的增加而指数下降，*因此，观测值越老，其重要性越低*。最后，每个求和项都乘以 (1 —β)。

> 在实践中，β 的值通常选择接近 0.9。

![](../Images/946afdcedcea1cae3b14897b927bb2e1.png)

不同时间戳的权重分布（β = 0.9）

# 数学解释

利用数学分析中的著名第二个奇妙极限，可以证明以下极限：

![](../Images/088ede23a815fe975357a0f9c57e7506.png)

第二个奇妙极限

通过将 β = 1 - *x* 代入，我们可以将其改写成如下形式：

![](../Images/e648d093856d49a1a829473a3bc8008f.png)

另一种形式的奇妙极限

我们还知道，在指数加权平均的公式中，每个观察值都乘以一个 βᵗ 的项，其中 t 表示观察值被计算的时间戳数。由于基数 β 在两种情况中是相等的，我们可以使两个公式的指数相等：

![](../Images/7bedd715d3f938568c452996e6e00451.png)

通过使用这个公式，对于选择的 β 值，我们可以计算出权重项达到 1 / e ≈ 0.368 的大致时间戳数量 t。这意味着在最近的 t 次迭代中计算的观察值的权重项大于 1 / e，而在最后 t 个时间戳范围之外计算的观察值的权重低于 1 / e，其重要性则大大减少。

实际上，低于 1 / e 的权重对指数加权平均的影响微乎其微。这就是为什么人们说**对于给定的 β 值，指数加权平均会考虑到最后的 t = 1 / (1 - β) 个观察值**。

为了更好地理解公式，让我们插入不同的 β 值**：**

![](../Images/de06a335b31a2fbeb4c261e9c312f732.png)

> 例如，选择 β = 0.9 表示大约在 t = 10 次迭代后，权重衰减到 1 / e，相较于当前观察值的权重。换句话说，指数加权平均主要依赖于最后的 t = 10 个观察值。

# 偏差修正

使用指数加权平均的一个常见问题是，在大多数问题中，它不能很好地近似前几项值。这是由于在前几次迭代中缺少足够的数据。例如，假设我们给定了以下时间序列：

![](../Images/9d079e18b99a43f4c6e4aada6d25daa4.png)

目标是用指数加权平均来近似它。然而，如果我们使用常规公式，那么前几个值将对 v₀ 赋予较大权重，而 v₀ 为 0，尽管散点图上的大多数点都在 20 以上。因此，初始的加权平均序列将会过低，无法准确近似原始序列。

一种简单的解决方案是将 v₀ 的值设置得接近第一次观察值 θ₁。虽然这种方法在某些情况下效果很好，但它仍然不完美，特别是当给定序列波动时。例如，如果 θ₂ 与 θ₁ 差异过大，那么在计算第二个值 v₂ 时，加权平均通常会更重视先前的趋势 v₁ 而不是当前的观察值 θ₂。结果，近似值将会非常差。

更加灵活的解决方案是使用一种称为“**偏差修正**”的技术。它不是简单地使用计算得到的值 vₖ，而是将其除以 (1 —βᵏ)。假设 β 选择接近 0.9–1，这个表达式在初始迭代中，当 k 较小的时候，趋近于 0。因此，与其慢慢累积前几个值，其中 v₀ = 0，不如将它们除以一个相对较小的数，使它们变成更大的值。

![](../Images/ed809abff8a65e5f605c4394dfdbec49.png)

带有和不带有偏差修正的指数移动平均计算示例

一般来说，这种缩放方法效果很好，并能精确地调整前几个项。当 k 增大时，分母逐渐接近 1，从而逐渐忽略这种缩放的效果，因为从某一迭代开始，算法可以高可信度地依赖于其最近的值，而无需任何额外的缩放。

![](../Images/a505a4b5dbc0326cb86660f06e5f0fb9.png)

# 结论

在本文中，我们介绍了一种极其有用的时间序列近似技术。指数加权平均算法的鲁棒性主要通过其超参数 β 来实现，该参数可以针对特定类型的序列进行调整。除此之外，引入的偏差修正机制使得即使在信息较少的早期时间戳上，也能有效地近似数据。

指数加权平均在时间序列分析中具有广泛的应用范围。此外，它还被用于梯度下降算法的变体中，以加速收敛。其中最受欢迎的之一是深度学习中的动量优化器，它消除了优化函数的不必要振荡，使其更精确地对准局部最小值。

*除非另有说明，所有图像均由作者提供*
