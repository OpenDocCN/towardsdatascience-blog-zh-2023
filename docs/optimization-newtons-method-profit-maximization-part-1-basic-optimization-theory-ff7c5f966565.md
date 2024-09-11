# 优化、牛顿法与利润最大化：第1部分 — 基本优化理论

> 原文：[https://towardsdatascience.com/optimization-newtons-method-profit-maximization-part-1-basic-optimization-theory-ff7c5f966565?source=collection_archive---------10-----------------------#2023-01-10](https://towardsdatascience.com/optimization-newtons-method-profit-maximization-part-1-basic-optimization-theory-ff7c5f966565?source=collection_archive---------10-----------------------#2023-01-10)

![](../Images/e6f766d329c212d0721d49d9bd28830e.png)

所有图片由作者提供

## 学习如何解决和利用牛顿法解决多维优化问题

[](https://medium.com/@jakepenzak?source=post_page-----ff7c5f966565--------------------------------)[![Jacob Pieniazek](../Images/2d9c6295d39fcaaec4e62f11c359cb29.png)](https://medium.com/@jakepenzak?source=post_page-----ff7c5f966565--------------------------------)[](https://towardsdatascience.com/?source=post_page-----ff7c5f966565--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----ff7c5f966565--------------------------------) [Jacob Pieniazek](https://medium.com/@jakepenzak?source=post_page-----ff7c5f966565--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F6f0948d99b1c&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-newtons-method-profit-maximization-part-1-basic-optimization-theory-ff7c5f966565&user=Jacob+Pieniazek&userId=6f0948d99b1c&source=post_page-6f0948d99b1c----ff7c5f966565---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----ff7c5f966565--------------------------------) ·14分钟阅读·2023年1月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fff7c5f966565&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-newtons-method-profit-maximization-part-1-basic-optimization-theory-ff7c5f966565&user=Jacob+Pieniazek&userId=6f0948d99b1c&source=-----ff7c5f966565---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fff7c5f966565&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Foptimization-newtons-method-profit-maximization-part-1-basic-optimization-theory-ff7c5f966565&source=-----ff7c5f966565---------------------bookmark_footer-----------)

> 本文是3部分系列中的**第1部分**。在第1部分中，我们将学习基本的优化理论。然后，在[第2部分](/optimization-newtons-method-profit-maximization-part-2-constrained-optimization-theory-dc18613c5770)，我们将扩展这些理论到约束优化问题。最后，在第3部分中，我们将应用所涵盖的优化理论，以及计量经济学和经济理论，来解决一个利润最大化问题。

数学优化是一个极其强大的数学领域，支撑了我们数据科学家在日常工作中隐性或显性使用的许多工具——实际上，几乎所有的机器学习算法都利用优化理论来实现模型收敛。例如，在分类问题中，我们试图通过选择模型的*最优*参数或权重来*最小化*对数损失。*一般来说，数学优化可以被视为机器学习的主要理论机制*。对数学优化的深刻理解是数据科学家工具箱中非常有用的技能——它使数据科学家能够更深入地理解许多当前使用的算法，并且进一步解决*各种独特的优化问题*。

许多读者可能对梯度下降或相关的优化算法，如随机梯度下降，已经有所了解。然而，本文将更深入地讨论经典的牛顿优化方法，有时称为牛顿-拉夫森方法。我们将从基础数学知识开始，逐步讲解优化理论，到梯度下降，然后深入探讨牛顿方法及其在 Python 中的实现。这将为我们进入[**第2部分**](/optimization-newtons-method-profit-maximization-part-2-constrained-optimization-theory-dc18613c5770)的约束优化和本系列的**第3部分**中的计量经济学利润最大化问题提供必要的前期准备。

## 优化基础——一个简单的二次函数

数学优化可以被定义为“确定数学定义问题的最佳解决方案的科学。”[1] 在一些实际例子中，这可以被概念化为：选择参数以最小化机器学习算法的损失函数，选择价格和广告以最大化利润，选择股票以最大化风险调整后的财务回报等等。形式上，任何数学优化问题都可以抽象地表述如下：

![](../Images/91d0602859b3778af388eb855d77cd2a.png)

(1)

这可以理解为：选择向量**x**的实值，以最小化*目标函数* *f*(**x**)（或最大化-*f*(**x**))，并满足*不等式约束* *g*(**x**)和*等式约束* *h*(**x**)。我们将在本系列的[第二部分](/optimization-newtons-method-profit-maximization-part-2-constrained-optimization-theory-dc18613c5770)中讨论如何求解约束优化问题——因为它们使优化问题变得特别复杂。现在，让我们来看一个无约束的单变量示例——考虑以下优化问题：

![](../Images/776e66032cdf1167135d13f03ac59c6c.png)

（2）

在这种情况下，我们想选择使上面的二次函数最小化的x值。我们可以采用多种方法——首先，一种简单的方法是对x值的大范围进行网格搜索，并选择使*f*(x)具有最低函数值的x。然而，随着搜索空间的增加、函数变得更加复杂或维度增加，这种方法可能很快失去计算上的可行性。

如果存在封闭形式的解，我们可以直接使用微积分来求解。也就是说，我们可以通过微积分解析地求解x的值。通过取导数（或在高维中，如后面所述的梯度）并将其设置为0——*相对最小值的必要一阶条件*——我们可以求解函数的相对极值。然后我们可以取第二导数（或在高维中，如后面所述的Hessian矩阵）来确定这个极值是最大值还是最小值。第二导数大于0（或正定Hessian矩阵）——*相对最小值的必要二阶条件*——意味着是最小值，反之亦然。观察：

![](../Images/b356d5910e63261dd7e34294b0bcc246.png)

（3）

我们可以通过图形验证上述（2）：

![](../Images/6b090480c2e5931b739e8fb4f3c0d907.png)

请注意，当一个函数存在多个极值点（即多个最小值或最大值）时，必须小心确定哪个是全局极值——我们将在本文中简要讨论这个问题。

上述分析方法可以扩展到更高维度，利用梯度和 Hessian 矩阵——然而，我们不会在高维中求解封闭形式的解——但直觉依然相同。我们仍然会利用*迭代方案*来解决更高维的问题。我说的*迭代方案*是什么意思？通常，可能不存在封闭形式（或解析）的解，并且为了存在最大值或最小值，封闭形式的解确实不一定存在。因此，我们需要一种数值方法来解决优化问题。这引导我们到更广泛的*迭代方案*，包括梯度下降法和牛顿方法。

## 迭代优化方案

一般来说，迭代优化方案主要分为三类，即 *零阶*、*一阶* 和 *二阶*，它们分别利用函数的零阶、一阶或二阶导数的局部信息。[1] 要使用每种迭代方案，函数 f(**x**) 必须是相应阶数上连续可微的函数。

**零阶迭代方案**

*零阶迭代方案* 与上述的网格搜索紧密相关——简单来说，你在一定范围内搜索可能的 **x** 值以获得最小的函数值。正如你可能猜到的，这些方法往往比使用高阶方法的计算开销大得多。不用说，它们可以是可靠的并且容易编程。市场上有一些方法可以改进简单的网格搜索，参见[1]了解更多信息；然而，我们将更多关注高阶方案。

**一阶迭代方案**

*一阶迭代方案* 是利用目标函数一阶导数的局部信息的迭代方案。最显著的例子是梯度下降方法。对于如上所述的单变量函数，梯度就是一阶导数。将此推广到 *n* 维度，对于一个函数 *f*(**x**)，梯度是一阶偏导数的向量：

![](../Images/d43e3d17e6b500055468db4db576bd0b.png)

(4) 连续可微函数 *f*(**x**)

梯度下降从选择一个随机的起点开始，并在 *f*(**x**) 的负梯度方向上迭代进行——*函数的最陡方向*。每次迭代步骤可以表示如下：

![](../Images/d0c20a695835779d9d20b310cf778c7d.png)

(5) 梯度下降迭代方案

其中 *γ* 是相应的学习率，控制梯度下降算法在每次迭代中“学习”的快慢。学习率过大会导致我们的迭代不受控制地发散。学习率过小则迭代可能需要很长时间才能收敛。此方案会迭代进行，直到达到一个或多个收敛准则，如：

![](../Images/855991698a9b3c03d179ffcba7c2ef8c.png)

(6) 迭代优化方案的收敛准则

对于某个小的 epsilon 阈值。回到我们的二次例子，将初始猜测设置为 x = 3 和学习率设置为 0.1，步骤如下：

![](../Images/a1ec26982129d237011ea7c103ff6bb3.png)

(7)

视觉化如下：

![](../Images/eb6bbeaa05e96ebae221a06c3e6933e0.png)

梯度下降和一阶迭代方案在性能上显著可靠。实际上，梯度下降算法主要用于神经网络和机器学习模型中的损失函数优化，许多发展已提高了这些算法的效能。然而，它们仍然仅使用关于函数的有限局部信息（仅一阶导数）。因此，在高维情况下，根据目标函数的性质和学习率，这些方案 1) 可能具有较慢的收敛速度，因为它们保持线性收敛率，并且 2) 可能完全无法收敛。由于这个原因，数据科学家扩大优化工具库是有益的！

**二阶迭代方案**

如你现在可能已经明白，*二阶迭代方案* 是利用目标函数的一阶导数和二阶导数的局部信息的迭代方案。最显著的是，我们有牛顿法 (NM)，它使用目标函数的海森矩阵。对于单变量函数，海森矩阵仅仅是二阶导数。类似于梯度，将其推广到 *n* 维度，海森矩阵是一个 *n* x *n 对称* 矩阵，包含一个两次连续可微函数 *f*(**x**）的二阶偏导数。

![](../Images/02d6b85d3d647d76e0d3a4440e2afba7.png)

(8) 二次连续可微函数 *f*(**x**）的海森矩阵

现在转到导出 NM，首先回忆最小值的一阶必要条件：

![](../Images/9716e1afa505ae541eed48d13afd769a.png)

(9) x* 处相对最小值的一级必要条件

我们可以使用泰勒级数展开来近似 ***x****：

![](../Images/f671020420ba3b2e88ede14734e5cb88.png)

(10)

每次迭代的增量 **Δ** 是对 **x*** 的一个更好的预期近似。因此，使用 NM 的每次迭代步骤可以表示如下：

![](../Images/aeea601b1cde8c66b5fd09c5d76f1710.png)

(11) 牛顿法迭代方案

回到我们的二次函数示例，将初始猜测设置为 x = 3，步骤如下：

![](../Images/a0c7a8dfd976ea5a79692fc8ba4b574c.png)

(12)

我们优雅地在第一次迭代时就收敛到最优解。注意，无论方案如何，收敛标准都是相同的。

> 请注意，所有优化方案都可能陷入局部极值，而不是全局极值（即，考虑具有多个极值（最小值和/或最大值）的高阶多项式——我们可能会陷入一个局部极值，而实际上，另一个极值可能在全球范围内对我们的实际问题更为优化）。已有的方法，并且仍在不断开发，用于处理全局优化，我们将不会深入探讨。你可以利用函数形式的先验知识来设置对结果的预期（即，如果一个严格凸函数有一个临界点，则它必须是全局最小值）。**然而，作为一般经验法则，通常明智的做法是对不同的初始值x迭代优化方案，然后研究结果的稳定性，通常选择具有最优函数值的结果。**

## 多维示例——罗森布罗克的抛物线谷

现在让我们考虑以下两个变量的优化问题：

![](../Images/932d986307b137d667c91cffb7f002e0.png)

(13) 罗森布罗克的抛物线谷

![](../Images/74f4e8929ed07a5c8fb08b9506e49b66.png)

我们将首先通过手动求解上述优化问题，然后在 Python 中进行求解，均使用牛顿法。

**通过手动求解**

要手动求解，我们需要求解梯度，求解 Hessian，选择我们的初始猜测 **Γ** = [x,y]，然后迭代将这些信息输入到 NM 算法中，直到收敛为止。首先，求解梯度，我们得到：

![](../Images/cedfd948a0bb2401ef6ccb4831579ccf.png)

(14)

求解 Hessian，我们得到：

![](../Images/420358b7e714700813e65ad39ff22643.png)

(15)

将我们的初始猜测设置为 **Γ** = [-1.2,1]，我们得到：

![](../Images/c34850f961e3220718d01cfeb43d05b9.png)

(16)

因此，我们成功地求解了目标函数的最优最小值为 **Γ*** = [1,1]。

**通过 Python 求解**

我们现在将转向用 Python 求解这个问题，并将其推广到任何函数，使用 S[ymPy](https://www.sympy.org/en/index.html) —— 一个用于符号数学的 Python 库。首先，让我们定义罗森布罗克的抛物线谷，并计算该函数的梯度和 Hessian：

```py
import sympy as sm
import numpy as np

# Define symbols & objective function
x, y = sm.symbols('x y')
Gamma = [x,y]
objective = 100*(y-x**2)**2 + (1-x)**2

def get_gradient(
    function: sm.core.expr.Expr,
    symbols: list[sm.core.symbol.Symbol],
) -> np.ndarray:
    """
    Calculate the gradient of a function.

    Args:
        function (sm.core.expr.Expr): The function to calculate the gradient of.
        symbols (list[sm.core.symbol.Symbol]): The symbols representing the variables in the function.

    Returns:
        numpy.ndarray: The gradient of the function.
    """
    d1 = {}
    gradient = np.array([])

    for i in symbols:
        d1[i] = sm.diff(function, i, 1)
        gradient = np.append(gradient, d1[i])

    return gradient

def get_hessian(
    function: sm.core.expr.Expr,
    symbols: list[sm.core.symbol.Symbol],
    x0: dict[sm.core.symbol.Symbol, float],
) -> np.ndarray:
    """
    Calculate the Hessian matrix of a function.

    Args:
    function (sm.core.expr.Expr): The function for which the Hessian matrix is calculated.
    symbols (list[sm.core.symbol.Symbol]): The list of symbols used in the function.

    Returns:
    numpy.ndarray: The Hessian matrix of the function.
    """
    d2 = {}
    hessian = np.array([])

    for i in symbols:
        for j in symbols:
            d2[f"{i}{j}"] = sm.diff(function, i, j)
            hessian = np.append(hessian, d2[f"{i}{j}"])

    hessian = np.array(np.array_split(hessian, len(symbols)))

    return hessian
```

SymPy 允许我们调查方程的符号表示。例如，如果我们调用 `objective` ，我们将看到相应的输出：

![](../Images/9df35cc55695fea8ebbfb88b4c9ee3c8.png)

SymPy 的函数符号表示

此外，SymPy 允许我们利用 `sm.diff()` 命令对相关函数进行求导。如果我们运行定义的函数以获得梯度 `get_gradient(objective,Gamma)` ，我们得到一个表示梯度的 numpy 数组：

![](../Images/21ab38bf204a0bc7b70ba2ed3655f69c.png)

SymPy 求解的梯度

访问特定元素时，我们可以看到符号表示 `get_gradient(objective, Gamma)[0]`：

![](../Images/6f149886b0c1fe9de8b3c3b61e564d05.png)

SymPy 解出的 df(**Γ**)/dx

类似地，对于 Hessian 矩阵，我们可以调用 `get_hessian(objective, Gamma)`：

![](../Images/b8ce56d342c7cbe4e30c8e69748f863d.png)

SymPy 解出的 Hessian 矩阵

访问特定元素 `get_hessian(objective,Gamma)[0][1]`

![](../Images/a4359f5c2a62c74d4e04d877482c8e8e.png)

SymPy 解出的 df(**Γ**)/dxdy

可以轻松验证梯度和 Hessian 矩阵与我们手动计算得到的结果是相同的。SymPy 允许对给定符号的指定值评估任何函数。例如，我们可以通过如下调整函数来评估初始猜测处的梯度：

```py
import sympy as sm
import numpy as np

def get_gradient(
    function: sm.core.expr.Expr,
    symbols: list[sm.core.symbol.Symbol],
    x0: dict[sm.core.symbol.Symbol, float], # Add x0 as argument
) -> np.ndarray:
    """
    Calculate the gradient of a function at a given point.

    Args:
        function (sm.core.expr.Expr): The function to calculate the gradient of.
        symbols (list[sm.core.symbol.Symbol]): The symbols representing the variables in the function.
        x0 (dict[sm.core.symbol.Symbol, float]): The point at which to calculate the gradient.

    Returns:
        numpy.ndarray: The gradient of the function at the given point.
    """
    d1 = {}
    gradient = np.array([])

    for i in symbols:
        d1[i] = sm.diff(function, i, 1).evalf(subs=x0) # add evalf method
        gradient = np.append(gradient, d1[i])

    return gradient.astype(np.float64) # Change data type to float

def get_hessian(
    function: sm.core.expr.Expr,
    symbols: list[sm.core.symbol.Symbol],
    x0: dict[sm.core.symbol.Symbol, float],
) -> np.ndarray:
    """
    Calculate the Hessian matrix of a function at a given point.

    Args:
    function (sm.core.expr.Expr): The function for which the Hessian matrix is calculated.
    symbols (list[sm.core.symbol.Symbol]): The list of symbols used in the function.
    x0 (dict[sm.core.symbol.Symbol, float]): The point at which the Hessian matrix is evaluated.

    Returns:
    numpy.ndarray: The Hessian matrix of the function at the given point.
    """
    d2 = {}
    hessian = np.array([])

    for i in symbols:
        for j in symbols:
            d2[f"{i}{j}"] = sm.diff(function, i, j).evalf(subs=x0)
            hessian = np.append(hessian, d2[f"{i}{j}"])

    hessian = np.array(np.array_split(hessian, len(symbols)))

    return hessian.astype(np.float64)
```

我们现在可以通过调用 `get_gradient(objective, Gamma, {x:-1.2,y:1.0})` 来计算给定起始点的梯度：

![](../Images/b7bf3a05cba745cf7b866255c08fe451.png)

初始点处的梯度

类似地，对于 Hessian 矩阵 `get_hessian(objective, Gamma, {x:-1.2,y:1.0})`：

![](../Images/f285e3836dc3ecf69ad2b37843093ca7.png)

初始点处的 Hessian 矩阵

同样，我们可以通过以上手动计算的工作来验证这些值是否正确。*现在我们拥有了编写牛顿法所需的所有要素*（梯度下降的代码在本文末尾也提供）*：*

```py
import sympy as sm
import numpy as np

def newton_method(
    function: sm.core.expr.Expr,
    symbols: list[sm.core.symbol.Symbol],
    x0: dict[sm.core.symbol.Symbol, float],
    iterations: int = 100,
) -> dict[sm.core.symbol.Symbol, float] or None:
    """
    Perform Newton's method to find the solution to the optimization problem.

    Args:
        function (sm.core.expr.Expr): The objective function to be optimized.
        symbols (list[sm.core.symbol.Symbol]): The symbols used in the objective function.
        x0 (dict[sm.core.symbol.Symbol, float]): The initial values for the symbols.
        iterations (int, optional): The maximum number of iterations. Defaults to 100.

    Returns:
        dict[sm.core.symbol.Symbol, float] or None: The solution to the optimization problem, or None if no solution is found.
    """

    x_star = {}
    x_star[0] = np.array(list(x0.values()))

    print(f"Starting Values: {x_star[0]}")

    for i in range(iterations):

        gradient = get_gradient(function, symbols, dict(zip(x0.keys(), x_star[i])))
        hessian = get_hessian(function, symbols, dict(zip(x0.keys(), x_star[i])))

        x_star[i + 1] = x_star[i].T - np.linalg.inv(hessian) @ gradient.T

        if np.linalg.norm(x_star[i + 1] - x_star[i]) < 10e-5:
            solution = dict(zip(x0.keys(), x_star[i + 1]))
            print(f"\nConvergence Achieved ({i+1} iterations): Solution = {solution}")
            break
        else:
            solution = None

        print(f"Step {i+1}: {x_star[i+1]}")

    return solution
```

我们现在可以通过 `newton_method(objective,Gamma,{x:-1.2,y:1})` 来运行代码：

![](../Images/e02ab685850059438bb587571f23aa35.png)

## 结论

就这样！如果你已经阅读到这一步，你现在对如何思考和抽象地制定无约束数学优化问题有了扎实的理解，包括基本的分析方法和更复杂的迭代方法。显然，我们在迭代方案中可以融入的信息越多（即更高阶的导数），收敛速度就越高效。***请注意，我们只是触及了数学优化复杂世界的表面。*** 尽管如此，我们今天讨论的工具在实践中绝对可以使用，并可以扩展到更高维的优化问题。

请关注 [**第 2 部分**](/optimization-newtons-method-profit-maximization-part-2-constrained-optimization-theory-dc18613c5770) **的系列文章**，我们将在其中扩展我们在这里学到的内容，解决有约束的优化问题——这是无约束优化的一个极其实用的扩展。实际上，大多数实际优化问题都会对选择变量有某种形式的约束。然后我们将转向**本系列的第 3 部分**，在其中我们将应用学到的优化理论和额外的计量经济学与经济理论来解决一个简单的利润最大化问题。希望你和我一样喜欢阅读这篇文章！

## 奖励——牛顿法的陷阱

尽管牛顿法具有吸引力，但它也有自身的陷阱。尤其是，存在两个主要陷阱——1）即使在选择接近解的起始点时，NM 并不总是收敛；2）NM 在每一步都需要计算 Hessian 矩阵，这在高维情况下计算开销非常大。对于陷阱 #1），一种相应的解决方案是改进牛顿法（MNM），可以粗略地认为它是梯度下降法，其中搜索方向由牛顿步长**Δ**给出。对于陷阱 #2），已经提出了准牛顿方法，如 DFP 或 BFGS，这些方法通过近似每一步使用的逆 Hessian 矩阵来减轻计算负担。有关更多信息，请参见[1]。

## 梯度下降函数

```py
import sympy as sm
import numpy as np

def gradient_descent(
    function: sm.core.expr.Expr,
    symbols: list[sm.core.symbol.Symbol],
    x0: dict[sm.core.symbol.Symbol, float],
    learning_rate: float = 0.1,
    iterations: int = 100,
) -> dict[sm.core.symbol.Symbol, float] or None:
    """
    Performs gradient descent optimization to find the minimum of a given function.

    Args:
        function (sm.core.expr.Expr): The function to be optimized.
        symbols (list[sm.core.symbol.Symbol]): The symbols used in the function.
        x0 (dict[sm.core.symbol.Symbol, float]): The initial values for the symbols.
        learning_rate (float, optional): The learning rate for the optimization. Defaults to 0.1.
        iterations (int, optional): The maximum number of iterations. Defaults to 100.

    Returns:
        dict[sm.core.symbol.Symbol, float] or None: The solution found by the optimization, or None if no solution is found.
    """
    x_star = {}
    x_star[0] = np.array(list(x0.values()))

    print(f"Starting Values: {x_star[0]}")

    for i in range(iterations):

        gradient = get_gradient(function, symbols, dict(zip(x0.keys(), x_star[i])))

        x_star[i + 1] = x_star[i].T - learning_rate * gradient.T

        if np.linalg.norm(x_star[i + 1] - x_star[i]) < 10e-5:
            solution = dict(zip(x0.keys(), x_star[i + 1]))
            print(f"\nConvergence Achieved ({i+1} iterations): Solution = {solution}")
            break
        else:
            solution = None

        print(f"Step {i+1}: {x_star[i+1]}")

    return solution
```

## 资源

[1] Snyman, J. A., & Wilke, D. N. (2019). *实用数学优化：基本优化理论与基于梯度的算法*（第2版）。Springer。

[2] [https://en.wikipedia.org/wiki/Gradient_descent](https://en.wikipedia.org/wiki/Gradient_descent)

[3] [https://en.wikipedia.org/wiki/Newton%27s_method#:~:text=In%20numerical%20analysis%2C%20Newton%27s%20method%2C%20also%20known%20as,roots%20%28or%20zeroes%29%20of%20a%20real%20-valued%20function](https://en.wikipedia.org/wiki/Newton%27s_method#:~:text=In%20numerical%20analysis%2C%20Newton%27s%20method%2C%20also%20known%20as,roots%20%28or%20zeroes%29%20of%20a%20real%20-valued%20function).

*通过此 GitHub Repo 访问所有代码:* [https://github.com/jakepenzak/Blog-Posts](https://github.com/jakepenzak/Blog-Posts)

*感谢你阅读我的帖子！我在 Medium 上的帖子旨在探讨利用* ***计量经济学*** *和* ***统计/机器学习*** *技术的现实世界和理论应用。此外，我还希望通过理论和模拟提供某些方法论的理论基础。最重要的是，我写作是为了学习！我希望让复杂的话题对所有人稍微更易于理解。如果你喜欢这篇文章，请考虑* [***在 Medium 上关注我***](https://medium.com/@jakepenzak)*！*
