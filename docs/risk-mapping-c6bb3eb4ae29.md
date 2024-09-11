# 一张图表中的博弈论与风险管理

> 原文：[https://towardsdatascience.com/risk-mapping-c6bb3eb4ae29?source=collection_archive---------13-----------------------#2023-03-27](https://towardsdatascience.com/risk-mapping-c6bb3eb4ae29?source=collection_archive---------13-----------------------#2023-03-27)

## 数据可视化

## 风险管理原则介绍

[](https://gdelongeaux.medium.com/?source=post_page-----c6bb3eb4ae29--------------------------------)[![Gabriel de Longeaux](../Images/8b5cc3be3035af2a852d75498212c776.png)](https://gdelongeaux.medium.com/?source=post_page-----c6bb3eb4ae29--------------------------------)[](https://towardsdatascience.com/?source=post_page-----c6bb3eb4ae29--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----c6bb3eb4ae29--------------------------------) [Gabriel de Longeaux](https://gdelongeaux.medium.com/?source=post_page-----c6bb3eb4ae29--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F99542e924b20&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frisk-mapping-c6bb3eb4ae29&user=Gabriel+de+Longeaux&userId=99542e924b20&source=post_page-99542e924b20----c6bb3eb4ae29---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----c6bb3eb4ae29--------------------------------) ·9分钟阅读·2023年3月27日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fc6bb3eb4ae29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frisk-mapping-c6bb3eb4ae29&user=Gabriel+de+Longeaux&userId=99542e924b20&source=-----c6bb3eb4ae29---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fc6bb3eb4ae29&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Frisk-mapping-c6bb3eb4ae29&source=-----c6bb3eb4ae29---------------------bookmark_footer-----------)

*从数据科学家的角度出发，使用期望效用理论介绍基本的风险管理原则。风险管理策略在一张图表中进行总结（图表的详细解读在下文中），其中体现了等风险曲线。*

![](../Images/662d0b13b733acd0551615f8ab5f0750.png)

风险地图与风险对象（G. de Longeaux）

# 一个简单的风险情况

首先假设我们有一个总财富 W₀。如果我们在不久的将来面临丧失一个金额 L（显然小于 W₀）的风险，概率为 p，我们不能认为我们的财富仍然是 W₀（当 p = 1 时显然如此）。相反，我们应该计算在风险环境下，博弈论中所称的“确定等价物”。

## 如何减少财富的风险

确定性等价是指金额 Wₑ，使得 Wₑ 的效用与我们资产面临损失的效用相同。如果我们是风险中性的，使用的效用函数就是身份函数，因此确定性等价只是我们当前财富减去损失期望（即使没有效用函数的概念，这也很直观）：证明是，如果 Wₑ 的效用（即 Wₑ）等于我们财富面临损失的效用（在博弈论中称为“彩票”），则 Wₑ = (1-p) W₀ + p (W₀-L) = W₀ - pL（在博弈论中，彩票的效用是彩票结果的期望效用），因此 Wₑ= W₀ - pL。如果我们是风险厌恶的，我们可以例如使用对数作为效用函数，因此 ln(Wₑ) 等于 (1-p) ln(W₀) + p ln(W₀-L)，这意味着 Wₑ = W₀^(1-p) (W₀-L)^p。

## 风险成本是什么

注意，我们的表示非常简单，因为我们只有一个概率 p 在不久的将来损失正好为 L（没有应用贴现率），但这足以说明概念。现在我们需要问自己风险的成本是什么，这非常简单：即我们在暴露于任何损失之前的财富 W₀ 与因风险暴露而现在拥有的财富 Wₑ 之间的差异。对于风险中性的人，风险成本 r = W₀ - Wₑ = W₀ - (W₀ - pL) = pL，这很直观，因为它只是期望损失。对于风险厌恶的人（使用对数效用函数），风险成本会稍微复杂一些，r 等于 W₀-W₀^(1-p) (W₀-L)^p = W₀ [1 - (1 - L/W₀)ᵖ]。

# 绘制风险图

很明显，不同的损失金额 L 和损失概率 p 的组合可以关联到相同的风险成本 r。我们应在图表上绘制出所有这些表示相同风险成本 r 的点，其中损失金额 L 在 y 轴上表示，概率 p 在 x 轴上表示。为了找到所有点关联相同风险 r 的曲线方程（这就是这些曲线通常被称为“等风险曲线”的原因），我们只需将 r 设为常数，并推导出 L 关于 p 的表达式。对于风险中性实体，我们从 r=W₀-Wₑ = pL 开始，这意味着 L = r/p。对于风险厌恶的实体，我们从 r = W₀-Wₑ = W₀ - W₀^(1 - p) (W₀ - W)^p 开始，最终得到 L=W₀ [1 - (1-r/W₀)^(1/p)]（对于 p = 0 这没有定义，但这个情况不有趣，除了看到与 Allais 悖论相关的不连续性：即使 p 极小，我们应该根据所选择的效用函数考虑风险，但实际上我们永远不会考虑如此小的风险——我们将忽略这一点，因为这与我们主要关注的问题无关）。

![](../Images/46454a27fb4b472971347d445d8f486d.png)

等风险曲线 (G. de Longeaux)

为了绘制图表，我取了W₀ = 5。我们可以看到不同风险成本r值下的风险中立和厌恶风险个体的等风险曲线。

# 探索地图

## 表示风险对象

这就是一切变得有趣的地方。企业拥有暴露于损失的工厂、仓库、商店、办公室……它们可以在我们创建的地图上标出这些地点：例如，一个暴露于40%概率损失2的仓库将在地图上显示为点（x = 0.4, y = 2）。

![](../Images/3613001afce9dc7def4be3bfd7674815.png)

风险对象的等风险曲线（G. de Longeaux）

根据图表上绘制的等风险曲线，我们可以看到风险成本略低于r = 1（假设r = 0.9），如果我们假设公司是厌恶风险的。然而，并不是所有公司都厌恶风险：保险公司接近于风险中立（忽略其费用），因为它们可以通过大数法则来分摊风险。对于保险公司来说，风险成本r会更低（假设r = 0.8），因此保险公司将能够提供一个交易：仓库的所有者将支付0.85的保险费，将风险转移给保险公司，从而节省0.9作为风险成本。最终，保险公司将获得0.85以承担0.8的成本，从而赚取0.05，而另一家公司将支付0.85以摆脱0.9的成本，也赚取0.05。

## 确定战略区域

进一步探索图表，我们可以识别出四个主要区域：

+   位于风险地图左下角的风险对象（工厂、仓库、商店、办公室等）具有非常好的风险概况：保险费用便宜（保险费始终低于1）。

+   位于左上角的风险对象具有“严重损失”特征，这意味着可能会发生大损失。然而，由于损失概率较小（低于40%），风险概况仍然良好，保险非常有用，因为相对于风险成本来说，它非常便宜：一个成本为3.5的风险可以以1.5的费用投保（-57%）。

+   位于右下角的风险对象具有“概率损失”或“频率损失”特征，因为损失发生的概率很高（超过40%）（注意在我们简单的表示中，彩票是一个简单的伯努利试验，损失概率和损失频率是相同的，但一般情况下并非如此）。在这种情况下，风险转移似乎并不十分有趣，因为企业和保险的风险成本基本相同。通常，对于很可能发生的小损失，几乎没有保险的兴趣。如果有的话，公司可以通过自保（通过自保公司）或使用免赔额来减少总体支付的保险费：保险公司不会支付发生在这些地点的低于1.5的损失。

+   地图右上角的风险对象具有较差的风险状况：损失既大又高度可能。即使考虑到风险成本，保险费用也很高。例如，一个风险成本为4.5的对象可以以4的价格投保（-11%）。

![](../Images/624d51220cbd6e9ca6301b496739638a.png)

风险状况和保险策略（G. de Longeaux）

# 应对风险

## 预防和保护

在高风险情况下，应采取措施减少可能损失的严重性（保护措施）或降低损失发生的概率（预防措施）——或者两者兼顾。

![](../Images/e4f8c7f56a9eb2db8679156b0e6ccff8.png)

风险管理和保险策略原则（G. de Longeaux）

帮助公司进行预防和保护措施，以改善其风险状况、保护业务和拯救生命，这是风险工程师的工作——数据科学家可以支持他们的工作。

## 风险转移

采取适当的措施将有助于最大限度地利用风险转移策略，通过承担减少的风险成本或以有利价格投保风险对象（尽管短期内保险费用不直接取决于风险状况，因为市场条件的变化，但长期内是会有影响的）。

# 结论

通过一个非常基本的统计模型结合博弈论，我们能够轻松理解何时以及为何投保是有利的，并定义减少风险的主要策略及其对我们从保险中获得的收益的影响。所有结论都可以在一个图表中展示，其中可以同时识别支付的保险费和投保的财务收益。

# R 代码

如果你想使用R重新制作或改进图表，代码如下：

```py
#### Graphical options ####
background <- TRUE
drawArrows <- TRUE # Arrows are drawn only if background is displayed
drawPoints <- TRUE
nb_cases <- 10
sharpness <- 10000

#### Total wealth ####
x0 <- 5

#### Plot ####
r <- x0/10
p <- seq(from = 0, to = 1, by = 1/sharpness)
n <- length(p)
xNeutral <- r/p # Loss estimates for a risk-neutral entity
xAverse <- x0 * (1 - (1-r/x0)^(1/p)) # Loss estimates for a risk-averse entity

plot(x = p, y = xAverse, type = "l", col = "red",
    xlim = c(0, 1.01), ylim = c(0, 1.1 * x0),
    xlab = "", ylab = "",
    xaxs = "i", yaxs = "i",
    main = "Iso-risk curves for risk-neutral and risk-averse entities")
title(sub = "Risk cost r is defined as the difference between the current wealth \n and the certainty equivalent in a risky environment", cex.sub = 0.8)

for (r in seq(from = x0/nb_cases, to = x0, by = x0/nb_cases)){
  xNeutral <- r/p
  xAverse <- x0 * (1 - (1-r/x0)^(1/p))
  lines(x = p, y = xAverse, col = "red")
  lines(x = p, y = xNeutral, col = "blue")
  text(x = p[n] - 0.03, y = xAverse[n] - x0/125, label = paste("r =", round(r, digits = 2)), cex = 0.8)
}

if (background){
  rect(xleft = 0, ybottom = 0, xright = 0.4, ytop = 0.3*x0,
      col = rgb(red = 30.59/100, green = 89.41/100, blue = 30.59/100, alpha = 0.3), border = "transparent")
  text(x = 0.175, y = 1 * x0/5, label = "Low risk: inexpensive insurance", col = "darkgreen", cex = 0.8)

  rect(xleft = 0.4, ybottom = 0, xright = 1.01, ytop = 0.3*x0,
      col = rgb(red = 25.1/100, green = 72.55/100, blue = 100/100, alpha = 0.3), border = "transparent")
  text(x = 0.707, y = 0.22 * x0/5, label = "Frequency losses:", col = "blue", cex = 0.8)
  text(x = 0.8, y = 0.08 * x0/5, label = "self-insured or inexpensive insurance", col = "blue", cex = 0.8)

  rect(xleft = 0, ybottom = 0.3*x0, xright = 0.4, ytop = x0,
      col = rgb(red = 25.1/100, green = 72.55/100, blue = 100/100, alpha = 0.3), border = "transparent")
  text(x = 0.09, y = 1.79 * x0/5, label = "Severity losses:", col = "blue", cex = 0.8)
  text(x = 0.174, y = 1.65 * x0/5, label = "relatively inexpensive insurance", col = "blue", cex = 0.8)

  rect(xleft = 0.4, ybottom = 0.3*x0, xright = 1.01, ytop = x0,
      col = rgb(red = 100/100, green = 25.1/100, blue = 25.1/100, alpha = 0.3), border = "transparent")
  text(x = 0.76, y = 1.65 * x0/5, label = "High risk: expensive insurance", col = "darkred", cex = 0.8)

  title(xlab = "Loss probability", line = 2, cex.lab = 1)
  title(ylab = "Loss estimate", line = 2, cex.lab = 1)
}else{
  title(xlab = "Loss probability", line = -1, cex.lab = 1)
  title(ylab = "Loss estimate", line = -1, cex.lab = 1)
}

if ((background) && (drawArrows)){
  arrows(x0 = 0.55, y0 = 2.5 * x0/5, x1 = 0.3, y1 = 2.5 * x0/5, length = 0.1, col = "gray22")
  text(x = 0.46, y = 2.6 * x0/5, label = "Prevention", col = "gray22", cex = 0.8)
  arrows(x0 = 0.55, y0 = 2.5 * x0/5, x1 = 0.55, y1 = 1 * x0/5, length = 0.1, col = "gray22")
  text(x = 0.61, y = 2 * x0/5, label = "Protection", col = "gray22", cex = 0.8)
}

if(drawPoints){
  points(x = 0.4, y = 0.4 * x0, pch = 16, col = "gray22")
  arrows(x0 = 0.4, y0 = 0, x1 = 0.4, y1 = 0.4 * x0, length = 0, col = "gray22", lty = 3)
  arrows(x0 = 0, y0 = 0.4 * x0, x1 = 0.4, y1 = 0.4 * x0, length = 0, col = "gray22", lty = 3)
  text(x = 0.4, y = 0.425 * x0, label = "Warehouse", cex = 0.8, col = "gray22")
}

text(x = 0.083, y = 1.02 * x0, label = "Current wealth", cex = 0.8)
legend("bottomleft", legend = c("Risk-neutral", "Risk-averse"), col = c("blue", "red"), pch = c("_", "_")) 
```
