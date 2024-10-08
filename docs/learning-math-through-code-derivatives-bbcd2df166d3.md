# 通过代码学习数学：导数

> 原文：[`towardsdatascience.com/learning-math-through-code-derivatives-bbcd2df166d3`](https://towardsdatascience.com/learning-math-through-code-derivatives-bbcd2df166d3)

## 通过 Python 深入了解导数

[](https://harrisonfhoffman.medium.com/?source=post_page-----bbcd2df166d3--------------------------------)![哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----bbcd2df166d3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----bbcd2df166d3--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----bbcd2df166d3--------------------------------) [哈里森·霍夫曼](https://harrisonfhoffman.medium.com/?source=post_page-----bbcd2df166d3--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----bbcd2df166d3--------------------------------) ·阅读时间 5 分钟·2023 年 3 月 20 日

--

![](img/c8e5f9d8443fa9c5a1bd237257397a82.png)

莱布尼茨团队。图片由作者提供。

数学对许多人来说是一个 notoriously difficult 的学科。由于其累积性和抽象性，学生可能会发现很难建立联系并理解数学的应用。在我不断的学习过程中，我发现通过实现代码来增强对数学概念的理解是非常有帮助的。

编程要求精确，因为计算机只能执行特定的一组指令。对精确性的需求要求采用逻辑和系统化的问题解决方法，这对理解基础概念非常有帮助。通过编程，我们可以对所实现的理念形成更深刻的直觉。此外，编程允许我们以互动和动手的方式实验、可视化和自动化数学概念，这可以使理论概念生动起来，提升我们的学习体验。

在本文中，我们将通过在 Python 中实现“前向差分商”近似，来更好地理解导数。虽然这是一个简单的实现，所需代码很少，但它揭示了导数所代表的核心。

# 什么是导数？

首先，我们定义导数。由于有许多免费的导数资源，这个解释不会是全面的。函数 f(x) 对 x 的导数定义为：

![](img/ef8ebfb5eb61371d186b6278ca09e135.png)

一维情况下的导数定义。图片由作者提供。

导数告诉我们在某一点，函数变化的*方向*和*速率*。通过选择两个点 x 和 x + h，计算这两个点之间函数的斜率（即(f(x+h) — f(x)) / h），并让 h 无限接近 0，我们恢复了函数在 x 处的瞬时变化率（即导数）。

# 用 Python 近似计算导数

导数最抽象且可能最难理解的部分是 h 接近 0 但实际上不会达到 0\。我们可以用 Python 编写一个函数来近似这个概念：

```py
import numpy as np
from typing import Callable, Union

def derivative(f: Callable[[float], float], x: Union[float, np.ndarray], h: float = 0.001) -> Union[float, np.ndarray]:

    """
    Approximate the derivative of a function f at point x using 
    the forward difference quotient defintion.

    Parameters
    ----------
    f : callable
        A function that takes a float as input and returns a float.
    x : float or ndarray
        The point(s) at which to compute the derivative of f.
    h : float, optional
        The step size used in the finite difference approximation. Default is 0.001.

    Returns
    -------
    float or ndarray
        The derivative of f at point x, approximated using the forward
        difference quotient.
    """

    # If h gets too small, the computation becomes unstable
    if h < 1e-5:

        raise ValueError('h values less than 1e-5 are unstable. Consider increasing h')

    return (f(x + h) - f(x)) / h
```

这个函数接收一个[纯函数](https://en.wikipedia.org/wiki/Pure_function#:~:text=In%20computer%20programming%2C%20a%20pure,arguments%20or%20input%20streams)%2C%20and)作为输入，该函数仅有一个变量，并在 x 参数指定的点处近似计算导数。这个函数的实际逻辑只存在于一行代码中，但它能在可接受的误差范围内近似计算多个导数。

要观察这个过程，我们可以近似计算二次函数的导数。根据幂法则（或通过计算差商的极限），我们知道：

![](img/741adf72fb3150c04e2949019c432f34.png)

二次函数的导数。图片来源：作者。

例如，函数在 x = 3 处的导数是 2*3 = 6\。以下代码近似计算了 x = 3 处的二次函数的导数：

```py
# Define the quadratic function
def f(x):

    """
    The quadratic function f(x) = x²
    """
    return x**2

# Define the input(s) to compute derivatives for
x = 3

# Define the value of h used to approximate the derivative
h = 0.001

# Approximate the derivative
print(derivative(f, x, h))

# Output: 6.000999999999479
```

通过将 h 设置为接近 0 的小正数，我们提取出一个接近真实值的导数近似值。随着 h 变得更小（直到达到[某个容忍度](https://acme.byu.edu/0000017a-17ef-d8b9-adfe-77ef210e0000/vol1b-numericalderivatiation-2017-pdf)），近似值会变得更加准确：

```py
# Define a smaller h value to get a more accurate approximation
h = 1e-5

# Take the derivative
print(derivative(f, x, h))

# Output: 6.000009999951316
```

我们可以将这种行为可视化，以观察 h 值逐渐减小时的效果：

![](img/093fc26d3d0ef32cf7ba07fe23c25682.png)

随着 h 的值减小，导数的近似值变得更准确（趋近于 6）。图片来源：作者。

另一个有趣的例子涉及三角函数。根据导数的定义，我们[知道以下内容](https://www.youtube.com/watch?v=HVvCbnrUxek)：

![](img/860edfc457250e1606013244a887a234.png)

sin(x)的导数是 cos(x)。图片来源：作者。

使用导数函数，我们可以进行如下近似：

```py
import numpy as np
import matplotlib.pyplot as plt

# Define the h value
h = 1e-5

# Define the domain
x = np.linspace(-10, 10, 1000)

# Approximate the derivative of sin(x) (should be close to cos(x))
f_prime = derivative(np.sin, x, h)

# Plot sin(x) vs the approximated derivative
fig, ax = plt.subplots(figsize=(10, 6))
ax.plot(x, np.sin(x), label='f(x) = sin(x)')
ax.plot(x, f_prime, label="f'(x) ~= cos(x)")
ax.set_xlabel('x')
ax.set_ylabel('y')
ax.set_title('Approximated derivative of sin(x) (close to cos(x))')
ax.legend()
plt.show()
```

![](img/128d33bfbf1785f902f978309f5a35f0.png)

sin(x)的近似导数类似于 cos(x)。图片来源：作者。

# 最后的思考

本文实现的导数近似称为“前向差分商”，它是进行数值微分的众多方法之一。需要注意的是，这种近似是不完美的，因为它在`h`的值很小时往往会[崩溃](https://acme.byu.edu/0000017a-17ef-d8b9-adfe-77ef210e0000/vol1b-numericalderivatiation-2017-pdf)。此外，在实际应用中，我们可以计算封闭形式函数的精确导数，避免近似的需求。本文的目的是帮助读者通过代码了解数学如何运作，并希望增强他们对导数的直觉。我鼓励读者在不同的函数上测试代码，探索其他导数近似方法，并理解各自的优缺点。感谢阅读！

# 参考文献

1.  ***数值导数***— [`acme.byu.edu/0000017a-17ef-d8b9-adfe-77ef210e0000/vol1b-numericalderivatiation-2017-pdf`](https://acme.byu.edu/0000017a-17ef-d8b9-adfe-77ef210e0000/vol1b-numericalderivatiation-2017-pdf)
