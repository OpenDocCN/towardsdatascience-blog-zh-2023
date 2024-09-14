# 从头实现最速下降算法

> 原文：[`towardsdatascience.com/implementing-the-steepest-descent-algorithm-in-python-from-scratch-d32da2906fe2`](https://towardsdatascience.com/implementing-the-steepest-descent-algorithm-in-python-from-scratch-d32da2906fe2)

[](https://nicolo-albanese.medium.com/?source=post_page-----d32da2906fe2--------------------------------)![Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----d32da2906fe2--------------------------------)[](https://towardsdatascience.com/?source=post_page-----d32da2906fe2--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----d32da2906fe2--------------------------------) [Nicolo Cosimo Albanese](https://nicolo-albanese.medium.com/?source=post_page-----d32da2906fe2--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----d32da2906fe2--------------------------------) ·11 分钟阅读·2023 年 2 月 20 日

--

![](img/0fbf614bb4efdb8d5bcf885b4aa8bb4b.png)

图片由作者提供。

# 目录

1.  介绍

1.  最速下降算法

    2.1 搜索方向

    2.2 步长

    2.3 算法

1.  实现

    3.1 常数步长

    3.2 带有 Armijo 条件的线搜索

1.  结论

# 1\. 介绍

优化是寻找一组变量`x`，使得目标函数`f(x)`最小化或最大化的过程。由于最大化一个函数等同于最小化它的负数，我们可以专注于最小化问题：

![](img/b67697b8b88e26d3229b118e3a5329a3.png)

对于我们的例子，我们将定义一个二次的多变量目标函数`f(x)`如下：

![](img/f5c859c0e02132db35f45dc85c9173f2.png)

它的梯度 `∇f(x)` 为

![](img/d119ef11abcf12441f1941a8d8bc58fe.png)

```py
import numpy as np

def f(x):
    '''Objective function'''
    return 0.5*(x[0] - 4.5)**2 + 2.5*(x[1] - 2.3)**2

def df(x):
    '''Gradient of the objective function'''
    return np.array([x[0] - 4.5, 5*(x[1] - 2.3)])
```

可以利用流行的`[scipy.optimize.minimize](https://docs.scipy.org/doc/scipy/reference/generated/scipy.optimize.minimize.html)`函数来快速找到最优值，该函数来自于流行的`[SciPy](https://scipy.org/)`库：

```py
from scipy.optimize import minimize

result = minimize(
    f, np.zeros(2), method='trust-constr', jac=df)

result.x
```

```py
array([4.5, 2.3])
```

我们可以绘制目标函数及其最小值：

```py
import matplotlib.pyplot as plt

# Prepare the objective function between -10 and 10
X, Y = np.meshgrid(np.linspace(-10, 10, 20), np.linspace(-10, 10, 20))
Z = f(np.array([X, Y]))

# Minimizer
min_x0, min_x1 = np.meshgrid(result.x[0], result.x[1])   
min_z = f(np.stack([min_x0, min_x1]))

# Plot
fig = plt.figure(figsize=(15, 20))

# First subplot
ax = fig.add_subplot(1, 2, 1, projection='3d')
ax.contour3D(X, Y, Z, 60, cmap='viridis')
ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
ax.set_xlabel('$x_{0}$')
ax.set_ylabel('$x_{1}$')
ax.set_zlabel('$f(x)$')
ax.view_init(40, 20)

# Second subplot
ax = fig.add_subplot(1, 2, 2, projection='3d')
ax.contour3D(X, Y, Z, 60, cmap='viridis')
ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
ax.set_xlabel('$x_{0}$')
ax.set_ylabel('$x_{1}$')
ax.set_zlabel('$f(x)$')
ax.axes.zaxis.set_ticklabels([])
ax.view_init(90, -90);
```

![](img/fc108ae1c10bd83187c0e3ddd302a0e6.png)

图片由作者提供。

现在我们介绍**最速下降算法**并从头实现它。我们的目标是解决优化问题并找到最小值`[4.5, 2.3]`。

# 2\. 最速下降算法

要解决优化问题`minₓ f(x)`，我们首先在坐标空间中的某一点开始。然后，我们通过搜索方向`p`迭代地移动，朝着`f(x)`的最小值更好的近似值前进：

![](img/41fa6cdc5495051c2cdea3f913f2160f.png)

在这个表达式中：

+   `x` 是输入变量；

+   `p` 是*搜索方向*；

+   `α > 0` 是*步长*或*步幅*；它描述了我们在每次迭代 `k` 中应该沿着方向 `p` 移动多少。

这种方法需要适当选择步长 `α` 和搜索方向 `p`。

## 2.1\. 搜索方向

作为搜索方向，最陡下降算法使用在当前迭代 `xₖ` 中评估的负梯度 `-∇f(xₖ)`。这是一个合理的选择，因为函数的负梯度总是指向函数下降最快的方向。

因此，我们可以将表达式重写为：

![](img/5dc508c0983cdcac4fd017f61700104c.png)

由于最小值是一个驻点，当梯度的范数小于给定的容忍度时，停止算法是合理的：如果梯度达到零，我们可能找到了最小值。

## 2.2\. 步长

由于我们试图最小化 `f(x)`，理想的步长 `α` 是以下目标函数 `φ(α)` 的最小值：

![](img/3cf141dc2c095c9d7480a14e2cf13019.png)

不幸的是，这需要在当前优化任务中解决额外的优化任务：`minₓ f(x)`。此外，虽然 `φ(α)` 是一元的，但找到它的最小值可能需要对 `f(x)` 及其梯度进行过多的评估。

简而言之，我们在寻找`α`的选择与做出选择所需时间之间的权衡。为此，我们可以简单地选择一个步长值，确保目标函数至少减少一定量。实际上，一个流行的不精确线搜索条件指出，`α`应该通过以下不等式导致`f(x)`的*充分减少*：

![](img/26615237f66d404aa0cc12e898942907.png)

在文献中，这被称为**充分减少**或 [**Armijo 条件**](https://en.wikipedia.org/wiki/Wolfe_conditions#Armijo_rule_and_curvature)，并且它属于 [**Wolfe 条件**](https://en.wikipedia.org/wiki/Wolfe_conditions) 的集合。

常数 `c` 被选择为较小的值；一个常见的值是 10^-4。

由于最陡下降法使用负梯度 `-∇f(xₖ)` 作为搜索方向 `pₖ`，表达式 `+ ∇f(xₖ)^T * pₖ` 等于梯度的负平方范数。在 Python 中：`-np.linalg.norm(∇f(xₖ))**2`。因此，我们的充分减少条件变为：

![](img/4abf53a86e890f8567da4396e412673a.png)

## 2.3\. 算法

我们现在可以写出所需的最陡下降法步骤：

1.  选择一个起始点 `x = x₀`

1.  选择一个最大迭代次数 `M`

1.  选择一个接近零的容忍度 `tol` 来评估梯度

1.  设置步数计数器 `n`

1.  在循环中重复：

    5.1 通过 Armijo 条件（线搜索）更新 `α`

    5.2 构造下一个点 `x = x - α ⋅ ∇f(x)`

    5.3 评估新的梯度 `∇f(x)` 5.4 更新步数计数器 `n = n + 1`

    5.5 如果当前梯度的范数足够小 `||∇f(x)|| < tol` 或达到最大迭代次数 `n = M`，则退出循环

1.  返回 `x`

让我们用 Python 来实现它。

# 3\. 实现

在这一部分，我们分享了最陡下降算法的实现。特别是，我们按步骤进行：

1.  我们从常数步长开始，然后

1.  我们添加了带有 Armijo 条件的线搜索。

## 3.1 常数步长

让我们从实现我们迭代方法的简化版本开始

![](img/d1796f79057c2d4dbb0ea8841eef2a92.png)

在所有迭代中应用 **常数** 步长值 `α`。我们的目的是实际验证对于任何常数 `α` 收敛性并不保证，因此需要实现线搜索：

```py
def steepest_descent(gradient, x0 = np.zeros(2), alpha = 0.01, max_iter = 10000, tolerance = 1e-10): 
    '''
    Steepest descent with constant step size alpha.

    Args:
      - gradient: gradient of the objective function
      - alpha: line search parameter (default: 0.01)
      - x0: initial guess for x_0 and x_1 (default values: zero) <numpy.ndarray>
      - max_iter: maximum number of iterations (default: 10000)
      - tolerance: minimum gradient magnitude at which the algorithm stops (default: 1e-10)

    Out:
      - results: <numpy.ndarray> of size (n_iter, 2) with x_0 and x_1 values at each iteration
      - number of steps: <int>
    '''

    # Prepare list to store results at each iteration 
    results = np.array([])

    # Evaluate the gradient at the starting point 
    gradient_x = gradient(x0)

    # Initialize the steps counter 
    steps_count = 0

    # Set the initial point 
    x = x0 
    results = np.append(results, x, axis=0)

    # Iterate until the gradient is below the tolerance or maximum number of iterations is reached
    # Stopping criterion: inf norm of the gradient (max abs)
    while any(abs(gradient_x) > tolerance) and steps_count < max_iter:

        # Update the step size through the Armijo condition
        # Note: the first value of alpha is commonly set to 1
        #alpha = line_search(1, x, gradient_x)

        # Update the current point by moving in the direction of the negative gradient 
        x = x - alpha * gradient_x

        # Store the result
        results = np.append(results, x, axis=0)

        # Evaluate the gradient at the new point 
        gradient_x = gradient(x) 

        # Increment the iteration counter 
        steps_count += 1 

    # Return the steps taken and the number of steps
    return results.reshape(-1, 2), steps_count
```

让我们使用这个函数来解决我们的优化任务：

```py
# Steepest descent
points, iters = steepest_descent(
  df, x0 = np.array([-9, -9]), alpha=0.30)

# Found minimizer
minimizer = points[-1].round(1)

# Print results
print('- Final results: {}'.format(minimizer))
print('- N° steps: {}'.format(iters))
```

```py
Final results: [4.5 2.3]
N° steps: 72
```

使用 `α = 0.3` 时，最小值在 72 步中达到了。我们可以绘制每次迭代中的点 `x`：

```py
# Steepest descent steps
X_estimate, Y_estimate = points[:, 0], points[:, 1] 
Z_estimate = f(np.array([X_estimate, Y_estimate]))

# Plot
fig = plt.figure(figsize=(20, 20))

# First subplot
ax = fig.add_subplot(1, 2, 1, projection='3d')
ax.contour3D(X, Y, Z, 60, cmap='viridis')
ax.plot(X_estimate, Y_estimate, Z_estimate, color='red', linewidth=3)
ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
ax.set_xlabel('$x_{0}$')
ax.set_ylabel('$x_{1}$')
ax.set_zlabel('$f(x)$')
ax.view_init(20, 20)

# Second subplot
ax = fig.add_subplot(1, 2, 2, projection='3d')
ax.contour3D(X, Y, Z, 60, cmap='viridis')
ax.plot(X_estimate, Y_estimate, Z_estimate, color='red', linewidth=3)
ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
ax.set_xlabel('$x_{0}$')
ax.set_ylabel('$x_{1}$')
ax.set_zlabel('$f(x)$')
ax.axes.zaxis.set_ticklabels([])
ax.view_init(90, -90);
```

![](img/dc549e08add1e4a549507241ec007c65.png)

图片来源于作者。

我们观察到最陡下降法的特征是通过一个显著的“之”字形路径向最小值前进。这是由于选择了负梯度 `-∇f(x)` 作为搜索方向 `p`。

正如我们讨论的，**对于任何步长值收敛性并不保证**。在之前的示例中，我们使用了常数步长 `α = 0.3`，但如果选择不同的步长会发生什么呢？

```py
# Step sizes to be tested
alphas = [0.01, 0.25, 0.3, 0.35, 0.4]

# Store the iterations for each step size
X_estimates, Y_estimates, Z_estimates = [], [], []

# Plot f(x) at each iteration for different step sizes
fig, ax = plt.subplots(len(alphas), figsize=(8, 9))
fig.suptitle('$f(x)$ at each iteration for different $α$')

# For each step size
for i, alpha in enumerate(alphas):

    # Steepest descent
    estimate, iters = steepest_descent(
      df, x0 = np.array([-5, -5]), alpha=alpha, max_iter=3000)

    # Print results
    print('Input alpha: {}'.format(alpha))
    print('\t- Final results: {}'.format(estimate[-1].round(1)))
    print('\t- N° steps: {}'.format(iters))

    # Store for 3D plots
    X_estimates.append(estimate[:, 0])
    Y_estimates.append(estimate[:, 1])  
    Z_estimates.append(f(np.array([estimate[:, 0], estimate[:, 1]])))

    # Subplot of f(x) at each iteration for current alpha
    ax[i].plot([f(var) for var in estimate], label='alpha: '+str(alpha))
    ax[i].axhline(y=0, color='r', alpha=0.7, linestyle='dashed')
    ax[i].set_xlabel('Number of iterations')
    ax[i].set_ylabel('$f(x)$')
    ax[i].set_ylim([-10, 200])
    ax[i].legend(loc='upper right')
```

```py
Input alpha: 0.01
 - Final results: [4.5 2.3]
 - N° steps: 2516
Input alpha: 0.25
 - Final results: [4.5 2.3]
 - N° steps: 88
Input alpha: 0.3
 - Final results: [4.5 2.3]
 - N° steps: 71
Input alpha: 0.35
 - Final results: [4.5 2.3]
 - N° steps: 93
Input alpha: 0.4
 - Final results: [ 4.5 -5\. ]
 - N° steps: 3000
```

![](img/fa7f953a2f65336e40f051e91e5934c6.png)

图片来源于作者。

当步长过大（`α = 0.4`）时，算法不收敛：`xₖ` 一直振荡而没有达到最小值，直到达到最大迭代次数。

我们可以通过观察每次迭代中的点 `x` 来更好地理解这种行为：

```py
fig = plt.figure(figsize=(25, 60))

# For each step size
for i in range(0, len(alphas)):

    # First subplot
    ax = fig.add_subplot(5, 2, (i*2)+1, projection='3d')
    ax.contour3D(X, Y, Z, 60, cmap='viridis')
    ax.plot(X_estimates[i], Y_estimates[i], Z_estimates[i], color='red', label='alpha: '+str(alphas[i]) , linewidth=3)
    ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
    ax.set_xlabel('$x_{0}$')
    ax.set_ylabel('$x_{1}$')
    ax.set_zlabel('$f(x)$')
    ax.view_init(20, 20)
    plt.legend(prop={'size': 15})

    # Second third
    ax = fig.add_subplot(5, 2, (i*2)+2, projection='3d')
    ax.contour3D(X, Y, Z, 60, cmap='viridis')
    ax.plot(X_estimates[i], Y_estimates[i], Z_estimates[i], color='red', label='alpha: '+str(alphas[i]) , linewidth=3)
    ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
    ax.set_xlabel('$x_{0}$')
    ax.set_ylabel('$x_{1}$')
    ax.set_zlabel('$f(x)$')
    ax.axes.zaxis.set_ticklabels([])
    ax.view_init(90, -90)
    plt.legend(prop={'size': 15})
```

![](img/dd26779d73130835fe22a708cd9ccc1c.png)

图片来源于作者。

为了保证最陡下降法的收敛性，我们需要通过线搜索迭代更新 `α`。

## 3.3\. 使用 Armijo 条件的线搜索

让我们通过添加 Armijo 规则来修改我们之前的方法。在最陡下降循环中，**在**计算下一个点 `x - α ⋅ ∇f(x)` 之前，我们需要选择一个合适的步长：我们从初始猜测 `α = 1` 开始，逐步将其值减半，直到满足 Armijo 条件。这个过程称为 *回溯线搜索*。

```py
def line_search(step, x, gradient_x, c = 1e-4, tol = 1e-8):
    '''
    Inexact line search where the step length is updated through the Armijo condition:
    $ f (x_k + α * p_k ) ≤ f ( x_k ) + c * α * ∇ f_k^T * p_k $

    Args:
      - step: starting alpha value
      - x: current point
      - gradient_x: gradient of the current point
      - c: constant value (default: 1e-4)
      - tol: tolerance value (default: 1e-6)
    Out:
      - New value of step: the first value found respecting the Armijo condition
    '''
    f_x = f(x)
    gradient_square_norm = np.linalg.norm(gradient_x)**2

    # Until the sufficient decrease condition is met 
    while f(x - step * gradient_x) >= (f_x - c * step * gradient_square_norm):

        # Update the stepsize (backtracking)
        step /= 2

        # If the step size falls below a certain tolerance, exit the loop
        if step < tol:
            break

    return step

def steepest_descent(gradient, x0 = np.zeros(2), max_iter = 10000, tolerance = 1e-10): 
    '''
    Steepest descent with alpha updated through line search (Armijo condition).

    Args:
      - gradient: gradient of the objective function
      - x0: initial guess for x_0 and x_1 (default values: zero) <numpy.ndarray>
      - max_iter: maximum number of iterations (default: 10000)
      - tolerance: minimum gradient magnitude at which the algorithm stops (default: 1e-10)

    Out:
      - results: <numpy.ndarray> with x_0 and x_1 values at each iteration
      - number of steps: <int>
    '''

    # Prepare list to store results at each iteration 
    results = np.array([])

    # Evaluate the gradient at the starting point 
    gradient_x = gradient(x0)

    # Initialize the steps counter 
    steps_count = 0

    # Set the initial point 
    x = x0 
    results = np.append(results, x, axis=0)

    # Iterate until the gradient is below the tolerance or maximum number of iterations is reached
    # Stopping criterion: inf norm of the gradient (max abs)
    while any(abs(gradient_x) > tolerance) and steps_count < max_iter:

        # Update the step size through the Armijo condition
        # Note: the first value of alpha is commonly set to 1
        alpha = line_search(1, x, gradient_x)

        # Update the current point by moving in the direction of the negative gradient 
        x = x - alpha * gradient_x

        # Store the result
        results = np.append(results, x, axis=0)

        # Evaluate the gradient at the new point 
        gradient_x = gradient(x) 

        # Increment the iteration counter 
        steps_count += 1 

    # Return the steps taken and the number of steps
    return results.reshape(-1, 2), steps_count
```

现在让我们使用线搜索优化目标函数：

```py
# Steepest descent
points, iters = steepest_descent(
  df, x0 = np.array([-9, -9]))

# Found minimizer
minimizer = points[-1].round(1)

# Print results
print('- Final results: {}'.format(minimizer))
print('- N° steps: {}'.format(iters))

# Steepest descent steps
X_estimate, Y_estimate = points[:, 0], points[:, 1] 
Z_estimate = f(np.array([X_estimate, Y_estimate]))

# Plot
fig = plt.figure(figsize=(20, 20))

# First subplot
ax = fig.add_subplot(1, 2, 1, projection='3d')
ax.contour3D(X, Y, Z, 60, cmap='viridis')
ax.plot(X_estimate, Y_estimate, Z_estimate, color='red', linewidth=3)
ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
ax.set_xlabel('$x_{0}$')
ax.set_ylabel('$x_{1}$')
ax.set_zlabel('$f(x)$')
ax.view_init(20, 20)

# Second subplot
ax = fig.add_subplot(1, 2, 2, projection='3d')
ax.contour3D(X, Y, Z, 60, cmap='viridis')
ax.plot(X_estimate, Y_estimate, Z_estimate, color='red', linewidth=3)
ax.scatter(min_x0, min_x1, min_z, marker='o', color='red', linewidth=10)
ax.set_xlabel('$x_{0}$')
ax.set_ylabel('$x_{1}$')
ax.set_zlabel('$f(x)$')
ax.axes.zaxis.set_ticklabels([])
ax.view_init(90, -90);
```

```py
- Final results: [4.5 2.3]
- N° steps: 55
```

![](img/3eae8394c4fa88fd16e953cb10f9e764.png)

图片来源于作者。

我们注意到在 55 步中达到了最小值，而使用常数步长 `α` 则导致了更多的迭代，或者没有收敛。

# 4\. 结论

在这篇文章中，我们介绍并实现了最陡下降法，并在两个变量的二次函数上进行了测试。特别是，我们展示了如何使用足够减少条件（Armijo）迭代更新步长。

带有 Armijo 线搜索的最陡下降法保证了收敛，但通常较慢，因为它需要大量的迭代。

在未来的帖子中，我们将探讨通过改变线性搜索策略并提供不同的步长初始化方法来修改这个基本算法。
