# 介绍 p 值和带有示例的显著性测试

> 原文：[https://towardsdatascience.com/introduction-to-p-value-and-significance-testing-with-examples-543fe3beb5a1?source=collection_archive---------7-----------------------#2023-01-18](https://towardsdatascience.com/introduction-to-p-value-and-significance-testing-with-examples-543fe3beb5a1?source=collection_archive---------7-----------------------#2023-01-18)

## 通过示例理解假设检验框架背后的思想

[](https://ms-neerajkrishna.medium.com/?source=post_page-----543fe3beb5a1--------------------------------)[![Neeraj Krishna](../Images/7bea17130a09d3382cba3b12ca5e3d7b.png)](https://ms-neerajkrishna.medium.com/?source=post_page-----543fe3beb5a1--------------------------------)[](https://towardsdatascience.com/?source=post_page-----543fe3beb5a1--------------------------------)[![走向数据科学](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----543fe3beb5a1--------------------------------) [Neeraj Krishna](https://ms-neerajkrishna.medium.com/?source=post_page-----543fe3beb5a1--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F8d6b9cde0656&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-p-value-and-significance-testing-with-examples-543fe3beb5a1&user=Neeraj+Krishna&userId=8d6b9cde0656&source=post_page-8d6b9cde0656----543fe3beb5a1---------------------post_header-----------) 发表于 [走向数据科学](https://towardsdatascience.com/?source=post_page-----543fe3beb5a1--------------------------------) ·9 min read·2023年1月18日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F543fe3beb5a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-p-value-and-significance-testing-with-examples-543fe3beb5a1&user=Neeraj+Krishna&userId=8d6b9cde0656&source=-----543fe3beb5a1---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F543fe3beb5a1&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fintroduction-to-p-value-and-significance-testing-with-examples-543fe3beb5a1&source=-----543fe3beb5a1---------------------bookmark_footer-----------)

# 引言

伟大的产品不是一夜之间建成的，而是通过多年的迭代不断改进和完善。最成功的团队在开发产品时遵循*反馈循环*。首先，他们构思一个想法，推向生产环境，并监控这个过程。然后，根据收集的数据，他们分析并确定是否成功。分析获得的见解将指导下一轮的开发。Keras 的创作者弗朗索瓦·肖莱 [称之为进展之环[2]。](https://fchollet.substack.com/p/the-loop-of-progress)

![](../Images/f45d7b482477b4d0f648a39022218fb1.png)

进步的循环。图片由作者提供。

统计学在分析部分的作用至关重要。它帮助我们通过观察监测数据来检验假设并做出决定。现在，有各种针对不同场景的假设检验方法，但在这篇文章中，我们将通过一个简单的例子来了解假设检验框架的思想和过程。

# 现实世界中的选择往往并不明确

![](../Images/2e2b8521b35a39c0e1de70f3dbe710a8.png)

由 [Edge2Edge Media](https://unsplash.com/@edge2edgemedia?utm_source=medium&utm_medium=referral) 拍摄，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

在统计学中，我们将选择称为假设。现实世界中的假设往往不清晰。让我来举例说明。考虑两个场景：

1.  在场景一中，你得到一个骰子，要求你确定这个骰子是公平的还是有作弊的。

1.  在场景二中，你得到两个骰子：一个公平的，一个作弊的。然而，这次你知道作弊骰子的概率分布。如果你随机挑选一个骰子并掷它，你能根据骰子落地的面来确定你挑选的是哪个骰子吗？

这两种场景都是假设检验问题的一种形式。然而，区别在于我们知道后者的*备择假设*是什么。

场景二更容易处理。有一些明确的方法，比如*似然比检验*，可以帮助我们根据单次观察做出决定。

然而，大多数现实世界的情况类似于场景一。

在场景一中，我们不知道备择假设是什么。我们只是假设备择假设是骰子有作弊。我们不知道它作弊的程度。因此，假设检验的最终目标就是证明或否定*原假设*，在这种情况下就是假设骰子是公平的。

这种形式的假设检验也被称为*显著性检验*，是本文的重点。

如果你想了解如何处理第一类问题，可以考虑阅读我的另一篇文章，其中我们从基本原理学习假设检验。

[](/introduction-to-hypothesis-testing-with-examples-60a618fb1799?source=post_page-----543fe3beb5a1--------------------------------) [## 假设检验简介与示例

### 一份关于假设检验的易懂指南，配有示例和可视化

[towardsdatascience.com](/introduction-to-hypothesis-testing-with-examples-60a618fb1799?source=post_page-----543fe3beb5a1--------------------------------)

# 示例——硬币是公平的还是有偏的？

让我们从一个例子开始，并逐步深入。

![](../Images/1beb741dfae9f941f3dc42fbb6df6955.png)

由 [Jizhidexiaohailang](https://unsplash.com/@jizhidexiaohailang?utm_source=medium&utm_medium=referral) 拍摄，刊登在 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

> 你得到了一枚硬币，需要判断它是公平的还是有偏的。

让𝜃表示正面朝上的概率。零假设是𝜃 = 1/2，而替代假设是𝜃 ≠ 1/2。

注意到替代假设不明确。𝜃可以是0到1之间的任何值。

假设检验的关键思想是实验结果依赖于假设。因此，通过观察结果，我们可以得出结论。

在这个例子中，我们投掷硬币*n*次并观察结果。

让结果用`X1, X2, X3, …, Xn`表示，其中`Xi`是一个随机变量，表示第`i`次投掷的结果。如果硬币落在正面，`Xi = 1`，否则`Xi = 0`。

## 拒绝区域和临界比率

让`S = X1 + X2 + X3 + … + Xn`表示正面朝上的总数。*S*被称为**统计量**，它本质上总结了观察结果。现在，如果零假设成立且硬币是公平的，我们更可能观察到*n*/2个正面。如果不是，那么`|S — n/2|`的差异会很大。所以一个合理的决策规则是：

```py
reject the null hypothesis if |S - n/2| > 𝜉
```

这意味着如果观察到的正面数与*n*/2之间的绝对差异大于某个值𝜉，我们将拒绝零假设或假设硬币是公平的。𝜉被称为**临界比率**。

基于这个决策规则，我们可以将观察空间分为两个区域：拒绝区域和接受区域。对于满足决策规则`|S — n/2| > 𝜉`的`S`值属于拒绝区域，而其余值属于接受区域。

![](../Images/9f570632c2714996290c22af97915375.png)

作者提供的图片

区域之间的边界依赖于𝜉。让我们看看如何选择它的值。

## 显著性水平

在零假设下，观察结果落入拒绝区域的概率称为**显著性水平**。它可以计算为：

```py
P(reject H0 ; H0) = 𝛼
```

`H0`是零假设。`P(reject H0 ; H0)`表示在`H0`下观察结果落入拒绝区域的概率。𝛼是显著性水平。

在我们的例子中，它可以表示为：

```py
P(|S - n/2| > 𝜉 ; H0) = 𝛼
```

现在，𝛼依赖于𝜉。随着𝜉的增加，拒绝区域的大小*减少*，𝛼也会随之减少。

如果我们固定了𝛼的值，我们可以找到𝜉的值。如果我们知道在零假设下决策规则的分布，我们就可以为给定的𝛼值找到𝜉的值。

让我们运行一个模拟，看看它的实际效果。

# 通过模拟理解

在我们的例子中，*S*遵循参数为*n*和𝜃的二项分布。然而，随着*n*的增加，*S*根据中心极限定理接近正态分布[4]。

![](../Images/f0c3c7e6d690905533749cd76abb9122.png)

随着n的增加，二项分布趋于正态分布。

所以假设*S*遵循正态分布，我们可以通过减去均值并除以标准差来标准化它。二项分布的均值为`n * 𝜃`，而标准差为`sqrt(n * 𝜃 * (1-𝜃))`。

```py
def standardize(S):
  return (S - n * 𝜃) / sqrt(n * 𝜃 * (1-𝜃))
```

假设我们抛掷硬币`n=1000`次，并且根据原假设，硬币是公平的，所以`𝜃=1/2`。对于显著性水平`𝛼=0.05`，𝜉的值可以如下计算：

```py
 P(|S - n𝜃| > 𝜉 ; H0) = 𝛼

   Divide the inequality by sqrt(n * 𝜃 * (1-𝜃)) which is the std of S

=> P(|S - n𝜃| / sqrt(n * 𝜃 * (1-𝜃)) > 𝜉 / sqrt(n * 𝜃 * (1-𝜃)) ; H0) = 𝛼

   Let Z = |S - n𝜃| / sqrt(n * 𝜃 * (1-𝜃))
   Z is a standard normal variable as discussed above

=> P(|Z| > 𝜉 / sqrt(n * 𝜃 * (1-𝜃)) ; H0) = 𝛼
=> P(|Z| > 𝜉 / sqrt(1000 * 1/2 * (1-1/2)) = 0.05
=> P(|Z| > 𝜉 / sqrt(250)) = 0.05

   According to the standard normal tables, P(Z) = 0.05 if |Z| > 1.96
   This is illustrated in the diagram below.

=> Z > 1.96 or Z < -1.96

   The minimum value of 𝜉 can be calculated as:
   𝜉 / sqrt(250) = 1.96

=> 𝜉 = 31
```

![](../Images/c48fc30578d691c0e20e0fd9a15fb590.png)

标准正态分布

所以对于显著性水平`𝛼 = 0.05`，临界比率是`𝜉 = 31`。这意味着什么？我们的决策规则是：

```py
reject the null hypothesis if |S - n/2| > 𝜉
```

如果我进行实验并抛掷硬币`n=1000`次，观察到正面次数`S=472`，则`|472–500| = 28 < 31`。

所以我们说**H₀在5%的显著性水平下未被拒绝**。

注意我们说*H₀未被拒绝*而不是说*H₀被接受*。这是因为我们没有足够强的依据来接受H₀。根据数据无法证明𝜃确切等于0.5。𝜃也可能等于0.51或0.49。因此，我们说观察值`S=472`不足以在5%的显著性水平下驳斥原假设H₀。

在`𝜉 = 31`的情况下，观察值`S < 469`和`S > 531`落入拒绝区域。此外，如果我增加𝜉的值，拒绝区域会如下面的图所示减小：

![](../Images/6f8c77777b64d9284aabdedd7a78120f.png)

拒绝区域随着`𝜉`的增加而减小。

当我们固定显著性水平𝛼，例如5%时，这意味着在H₀支配的模型下，观察值仅有5%的概率落入拒绝区域。因此，如果观察值最终落入拒绝区域，则提供了H₀可能错误的强有力证据。

# p值

p值的正式定义为：

> ***p*-值**是在假设原假设正确的前提下，获得至少与实际观察结果一样极端的测试结果的概率。 — [维基百科](https://en.wikipedia.org/wiki/P-value) [3]

与在实验前固定的显著性水平不同，p值依赖于实验结果。

假设硬币抛掷实验的结果是`S=430`，那么在假设原假设正确的前提下，“至少同样极端”的结果是`S < 430`和`S > 570`，因为在原假设下的分布是对称的并且围绕均值中心。

我通过改变𝜃模拟了结果*s*，每个结果的p值如下所示：

![](../Images/1dee5b20863eb10ddfe37d5a498aad7c.png)

基于𝜃变化的p值

实质上，p值是*𝛼*的值，使得*s*恰好处于拒绝与不拒绝的临界值之间。

# 正态近似的必要性

在上述示例中，我们知道结果遵循具有参数*n*和𝜃的二项分布。因此，我们可以直接使用任何统计库基于𝛼计算临界比率𝜉，而无需使用正态近似。

然而，在大多数情况下，我们不会知道零假设下的分布。然而，大多数情况下，随着样本量的增加，它们趋向于近似正态分布。因此，在假设检验实验中使用大样本量是很重要的。

# 结论

在科学中，科学家通常提出一个理论，其他科学家通过进行实验来证明或反驳它。他们进行假设检验，其中原始想法是零假设。

假设检验的一般框架可以总结如下[1]：

1.  选择一个代表观察数据的统计量S。它是一个取决于观察结果的标量随机变量。通常，样本均值或样本方差被用作统计量。

1.  制定一个拒绝零假设H₀的决策规则。决策规则是统计量S和临界比率𝜉的函数。根据决策规则和𝜉，可以将观察空间划分为拒绝区域和接受区域。

1.  选择显著性水平𝛼，它是观察结果在零假设下落入拒绝区域的概率。

1.  基于显著性水平𝛼计算临界比率𝜉。为了进行此计算，需要了解零假设下的分布。然而，正如我们讨论的，如果样本量较大，大多数情况下可以将其近似为正态分布。一旦知道了𝜉的值，就可以确定拒绝区域。

一旦我们进行实验并记录了观察结果，就需要做以下事情：

1.  计算统计量S*的值*s*。

1.  如果*s*属于拒绝区域，则拒绝零假设。

假设检验的美妙之处在于没有任何约束。我们可以自由设计实验，并选择零假设和备择假设。

希望你喜欢这篇文章。

```py
**Let's Connect** Hope you've enjoyed the article. Please clap and follow if you did.

You can also reach out to me on [LinkedIn](https://www.linkedin.com/in/neerajkrishnadev/) and [Twitter](https://twitter.com/WingedRasengan).
```

# 图像和图示来源

本文中的所有图像、图形和示意图均由作者创建；除非在说明中明确提及。

# 参考文献

1.  [Dimitri Bertsekas和John Tsitsiklis的《概率论导论》第9章第4节](https://www.amazon.in/Introduction-Probability-Dimitri-P-Bertsekas/dp/188652923X)

1.  [Francois Cholle的进步循环](https://fchollet.substack.com/p/the-loop-of-progress)

1.  [维基百科上的p值](https://en.wikipedia.org/wiki/P-value)

1.  [维基百科上的中心极限定理](https://en.wikipedia.org/wiki/Central_limit_theorem)
