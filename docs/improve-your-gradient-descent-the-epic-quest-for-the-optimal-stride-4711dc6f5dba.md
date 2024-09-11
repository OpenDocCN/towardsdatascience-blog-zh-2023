# 改善你的梯度下降：寻找最优步幅的史诗之旅

> 原文：[https://towardsdatascience.com/improve-your-gradient-descent-the-epic-quest-for-the-optimal-stride-4711dc6f5dba?source=collection_archive---------8-----------------------#2023-05-23](https://towardsdatascience.com/improve-your-gradient-descent-the-epic-quest-for-the-optimal-stride-4711dc6f5dba?source=collection_archive---------8-----------------------#2023-05-23)

## 优化梯度下降步长/学习率的技术

[](https://namanagr03.medium.com/?source=post_page-----4711dc6f5dba--------------------------------)[![Naman Agrawal](../Images/6bb885397aec17f5029cfac7f01edad9.png)](https://namanagr03.medium.com/?source=post_page-----4711dc6f5dba--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4711dc6f5dba--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4711dc6f5dba--------------------------------) [Naman Agrawal](https://namanagr03.medium.com/?source=post_page-----4711dc6f5dba--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F5bbb90aa727&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-your-gradient-descent-the-epic-quest-for-the-optimal-stride-4711dc6f5dba&user=Naman+Agrawal&userId=5bbb90aa727&source=post_page-5bbb90aa727----4711dc6f5dba---------------------post_header-----------) 发表在[Towards Data Science](https://towardsdatascience.com/?source=post_page-----4711dc6f5dba--------------------------------) ·14分钟阅读·2023年5月23日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4711dc6f5dba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-your-gradient-descent-the-epic-quest-for-the-optimal-stride-4711dc6f5dba&user=Naman+Agrawal&userId=5bbb90aa727&source=-----4711dc6f5dba---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4711dc6f5dba&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fimprove-your-gradient-descent-the-epic-quest-for-the-optimal-stride-4711dc6f5dba&source=-----4711dc6f5dba---------------------bookmark_footer-----------)![](../Images/bd19435e63da6ad9aff7a330dfde7870.png)

图片由[stokpic](https://pixabay.com/users/stokpic-692575/?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=600468)提供，来自[Pixabay](https://pixabay.com//?utm_source=link-attribution&amp%3Butm_medium=referral&amp%3Butm_campaign=image&amp%3Butm_content=600468)

# 目录

1.  介绍

1.  方法 1：固定步长

1.  方法 2：精确线搜索

1.  方法 3：回溯线搜索

1.  结论

# 介绍

在训练任何机器学习模型时，梯度下降是最常用的优化参数的技术之一。梯度下降提供了一种高效的方式来最小化传入数据的损失函数，特别是在没有问题的封闭解的情况下。一般来说，考虑一个由凸函数和可微分函数f: Rᵈ → R定义的机器学习问题（大多数损失函数都具有这些属性）。目标是找到x* ∈ Rᵈ，使损失函数最小化：

![](../Images/b833bde18991e44329d6ac0003f683be.png)

梯度下降提供了一种迭代方法来解决这个问题。更新规则如下：

![](../Images/5ec9b5a054d2f5965ac5262b08c9c3de.png)

其中x⁽ᵏ⁾指算法第k次迭代中x的值，tₖ指第k次迭代中模型的步长或学习率。算法的一般工作流程如下：

1.  确定损失函数f并计算其梯度∇f。

1.  从随机选择一个x ∈ Rᵈ开始，称之为x⁽⁰⁾（起始迭代）。

1.  直到达到停止标准（例如，误差低于某个阈值），执行以下操作：

    A) 确定x必须减少或增加的方向。在梯度下降中，这是由当前迭代点的损失函数梯度的相反方向给出的。vₖ = ∇ₓ f(x⁽ᵏ ⁻ ¹⁾)

    B) 确定步长或变化的幅度：tₖ。

    C) 更新迭代：x⁽ᵏ⁾= x⁽ᵏ ⁻ ¹⁾ − tₖ∇ₓ f(x⁽ᵏ ⁻ ¹⁾)

这就是整个工作流程的核心：获取当前迭代，寻找需要更新的方向（vₖ），确定更新的幅度（tₖ），并进行更新。

![](../Images/07b2355658dcb4ce63f35fd42aec31b7.png)

梯度下降的示意图 [作者提供的图片]

那么，这篇文章的主题是什么？在这篇文章中，我们将重点关注第3B步：寻找最优步长或tₖ的幅度。对于梯度下降来说，这是优化模型时最容易被忽视的方面之一。步长的大小可以大大影响算法收敛到解决方案的速度以及收敛到的解决方案的准确性。大多数情况下，数据科学家仅在整个学习过程中设置一个固定值的步长，或者偶尔使用验证技术来训练它。但解决这个问题还有许多更高效的方法。在这篇文章中，我们将讨论确定步长tₖ的三种不同方法：

1.  固定步长

1.  精确线搜索

1.  回溯线搜索（阿米乔规则）

对于这些方法中的每一种，我们将讨论理论并实现它以计算示例的前几个迭代。特别是，我们将考虑以下损失函数来评估模型：

![](../Images/a4d6f7cefd8b208d5a564e89f466958f.png)

下面是该函数的3D图：

![](../Images/9c8a8138be8a65264602f8bdbfde2eda.png)

损失函数（3D 图） [由作者使用 [LibreTexts](https://c3d.libretexts.org/CalcPlot3D/index.html) 生成的图片]

从图中可以明显看出，全球最小值为 x* = [0; 0]。在本文中，我们将手动计算前几次迭代，并计算每种方法的收敛步骤数。我们还将跟踪下降模式（即迭代轨迹）以了解这些技术如何影响[收敛过程。通常，参考函数的等高线图（而不是其 3D 图）可以更好地评估不同的轨迹。函数的等高线图可以通过以下代码轻松生成：

```py
# Load Packages
import numpy as np
import matplotlib.pyplot as plt
%matplotlib inline
import seaborn as sns
sns.set()
sns.set(style="darkgrid")
from matplotlib import cm
from matplotlib.ticker import LinearLocator, FormatStrFormatter
from mpl_toolkits.mplot3d import Axes3D
```

```py
# Define Function
f = lambda x,y:  2*x**2 + 3*y**2 - 2*x*y - 1

# Plot contour
X = np.arange(-1, 1, 0.005)
Y = np.arange(-1, 1, 0.005)
X, Y = np.meshgrid(X, Y)
Z = f(X,Y)
plt.figure(figsize=(12, 7))
cmap = plt.cm.get_cmap('viridis')
plt.contour(X,Y,Z,250, cmap=cmap)
```

![](../Images/e50b0e25faa9beb8212094ed043b5aec.png)

f 的等高线图 [由作者使用 Python 生成的图片]

让我们开始吧！

# 方法 1：固定步长

这种方法最简单易用，也是训练 ML 模型时最常用的方法。这涉及到设置：

![](../Images/a2c27db5d282e8e68d0ec89e4ea4eb66.png)

在使用这种方法时，选择合适的 t 值需要非常小心。虽然较小的 t 值可以导致非常准确的解决方案，但收敛速度可能会变得相当慢。另一方面，较大的 t 值使算法更快，但会牺牲准确性。使用这种方法要求实施者仔细平衡收敛速度和所获得解决方案的准确性之间的权衡。

在实践中，大多数数据科学家使用验证技术，如保留验证或 k 折交叉验证，来优化 t。该技术涉及创建训练数据的一个分区（称为验证数据），该数据用于通过在 t 可以取的离散值集上运行算法来优化性能。让我们来看一下我们的例子：

![](../Images/7c9f2143f112403654b8507fec02f9a1.png)

第一步是计算它的梯度：

![](../Images/a746fe2830d6c4404efacde46c07ab8d.png)

对于所有后续计算，我们将初始化设为 x⁽⁰⁾= [1; 1]。在这种策略下，我们设置：

![](../Images/3f4c3a94f893c7064c735c06c109e18f.png)

前两次迭代的计算如下：

![](../Images/c8ec49eaf61f5cf74dfe1ed286c9cbfe.png)

我们通过以下 Python 代码程序化计算其余的迭代：

```py
# Define the function f(x, y)
f = lambda x, y: 2*x**2 + 3*y**2 - 2*x*y - 1

# Define the derivative of f(x, y)
def df(x, y):
    return np.array([4*x - 2*y, 6*y - 2*x])

# Perform gradient descent optimization
def grad_desc(f, df, x0, y0, t=0.1, tol=0.001):
    x, y = [x0], [y0]  # Initialize lists to store x and y coordinates
    num_steps = 0  # Initialize the number of steps taken
    # Continue until the norm of the gradient is below the tolerance
    while np.linalg.norm(df(x0, y0)) > tol:  
        v = -df(x0, y0)  # Compute the direction of descent
        x0 = x0 + t*v[0]  # Update x coordinate
        y0 = y0 + t*v[1]  # Update y coordinate
        x.append(x0)  # Append updated x coordinate to the list
        y.append(y0)  # Append updated y coordinate to the list
        num_steps += 1  # Increment the number of steps taken
    return x, y, num_steps

# Run the gradient descent algorithm with initial point (1, 1)
a, b, n = grad_desc(f, df, 1, 1)

# Print the number of steps taken for convergence
print(f"Number of Steps to Convergence: {n}")
```

在上述代码中，我们定义了以下收敛标准（将始终用于所有方法）：

![](../Images/fd8d411ce856dbac80a73d18e31bea1e.png)

运行上述代码后，我们发现收敛大约需要26步。下面的图展示了在梯度下降过程中迭代轨迹：

```py
# Plot the contours
X = np.arange(-1.1, 1.1, 0.005)
Y = np.arange(-1.1, 1.1, 0.005)
X, Y = np.meshgrid(X, Y)
Z = f(X,Y)
plt.figure(figsize=(12, 7))
plt.contour(X,Y,Z,250, cmap=cmap, alpha = 0.6)
n = len(a)
for i in range(n - 1):
    plt.plot([a[i]],[b[i]],marker='o',markersize=7, color ='r')
    plt.plot([a[i + 1]],[b[i + 1]],marker='o',markersize=7, color ='r')
    plt.arrow(a[i],b[i],a[i + 1] - a[i],b[i + 1] - b[i], 
      head_width=0, head_length=0, fc='r', ec='r', linewidth=2.0)
```

![](../Images/4310c3950c0829f09b148a44a2862a1b.png)

等高线图：固定步长 = 0.1 [由作者使用 Python 生成的图片]

为了更好地理解在这种方法中选择正确的t有多关键，我们来衡量一下增加或减少t的效果。如果我们将t的值从0.1减少到0.01，收敛的步数会从26骤增至295。该情况的迭代轨迹如下所示：

![](../Images/9504a487fc6e5ac71a5e7bdac1473d07.png)

轮廓图：固定步长 = 0.01 [作者使用Python生成的图像]

然而，将t从0.1增加到0.2时，收敛的步数从26减少到仅仅11，如下所示的轨迹所示：

![](../Images/314498c4f2e42830524e51fbdd7a0a07.png)

轮廓图：固定步长 = 0.2 [作者使用Python生成的图像]

然而，需要注意的是，这并不总是如此。如果步长的值过大，可能导致迭代过程偏离最优解而无法收敛。实际上，将t从0.2增加到约0.3会导致迭代值急剧上升，使得无法收敛。这可以从以下的轮廓图（t = 0.3）看出，仅针对前8步：

![](../Images/ddcfcec2dd06b6afb851b027dc0e092d.png)

轮廓图：固定步长 = 0.3 [作者使用Python生成的图像]

因此，显而易见的是，在此方法中找到正确的t值极其重要，甚至小幅的增加或减少都可能极大地影响算法的收敛能力。现在，让我们谈谈确定t的下一种方法。

# 方法2：精确线搜索

在这种方法中，我们不会在每次迭代时分配一个简单的预设t值。相反，我们将找到最优t的问题本身视为一个一维优化问题。换句话说，我们关注于找到最优的更新t，以最小化函数值：

![](../Images/c6b7515d4864a734d9cbf7340e03e547.png)

注意这有多酷！我们有一个多维优化问题（最小化f），我们尝试使用梯度下降来解决。我们知道更新迭代值的最佳方向（vₖ = − ∇ₓ f(x⁽ᵏ ⁻ ¹⁾）），但我们需要找到最优步长tₖ。换句话说，下一次迭代的函数值仅依赖于我们选择使用的tₖ值。因此，我们将其视为另一个（但更简单的！）优化问题：

![](../Images/4bef8d9aaa5ea116affcf9ab732eb870.png)

因此，我们更新x⁽ᵏ⁾，使其成为最小化损失函数f的最佳迭代。这确实有助于提高收敛速度。然而，这也增加了额外的时间要求：计算g(t)的最小化器。通常，这不是问题，因为它是一个一维函数，但有时可能会花费比预期更长的时间。因此，在使用这种方法时，平衡减少收敛步数与计算argmin的额外时间要求之间的权衡是很重要的。让我们看看我们的例子：

![](../Images/88e51c2844dbb22ff81ac17c789e8711.png)

前两个迭代值计算如下：

![](../Images/d6f59641966f55e3207383065f4c07de.png)

我们通过以下Python代码以编程方式计算其余迭代

```py
# Import package for 1D Optimization
from scipy.optimize import minimize_scalar

def grad_desc(f, df, x0, y0, tol=0.001):
    x, y = [x0], [y0]  # Initialize lists to store x and y coordinates
    num_steps = 0  # Initialize the number of steps taken
    # Continue until the norm of the gradient is below the tolerance
    while np.linalg.norm(df(x0, y0)) > tol:  
        v = -df(x0, y0)  # Compute the direction of descent
        # Define optimizer function for searching t
        g = lambda t: f(x0 + t*v[0], y0 + t*v[1]) 
        t = minimize_scalar(g).x # Minimize t
        x0 = x0 + t*v[0]  # Update x coordinate
        y0 = y0 + t*v[1]  # Update y coordinate
        x.append(x0)  # Append updated x coordinate to the list
        y.append(y0)  # Append updated y coordinate to the list
        num_steps += 1  # Increment the number of steps taken
    return x, y, num_steps

# Run the gradient descent algorithm with initial point (1, 1)
a, b, n = grad_desc(f, df, 1, 1)

# Print the number of steps taken for convergence
print(f"Number of Steps to Convergence: {n}")
```

和以前一样，在上述代码中，我们定义了以下收敛标准（所有方法将始终使用）：

![](../Images/fd8d411ce856dbac80a73d18e31bea1e.png)

运行上述`code`时，我们发现仅需10步即可收敛（这是相对于固定步长的重大改进）。以下图表显示了梯度下降过程中的迭代轨迹：

```py
# Plot the contours
X = np.arange(-1.1, 1.1, 0.005)
Y = np.arange(-1.1, 1.1, 0.005)
X, Y = np.meshgrid(X, Y)
Z = f(X,Y)
plt.figure(figsize=(12, 7))
plt.contour(X,Y,Z,250, cmap=cmap, alpha = 0.6)
n = len(a)
for i in range(n - 1):
    plt.plot([a[i]],[b[i]],marker='o',markersize=7, color ='r')
    plt.plot([a[i + 1]],[b[i + 1]],marker='o',markersize=7, color ='r')
    plt.arrow(a[i],b[i],a[i + 1] - a[i],b[i + 1] - b[i], head_width=0, 
      head_length=0, fc='r', ec='r', linewidth=2.0)
```

![](../Images/01a5b6d8820366f31f5fc0cab5273946.png)

等高线图：精确线搜索 [作者使用Python生成的图像]

现在，让我们讨论确定t的下一种方法。

# 方法3：回溯线搜索

回溯法是一种选择最优步长的自适应方法。根据我的经验，这被认为是优化步长的最有用策略之一。其收敛速度通常比固定步长要快，而无需在精确线搜索中最大化一维函数g(t)的复杂性。该方法包括从一个较大的步长（t¯ = 1）开始，并继续减小步长，直到观察到f(x)的所需减少。我们首先来看看算法，随后将讨论具体细节：

![](../Images/e4ee0a61ec5b9b0c4d8d7987dc8cd43c.png)

算法1：回溯（Armijo–Goldstein条件）[作者提供的图像]

换句话说，我们从一个较大的步长开始（这在算法的初始阶段通常很重要），并检查它是否有助于我们按给定阈值改善当前迭代。如果步长过大，我们通过将其与标量常数β ∈ (0, 1)相乘来减少它。我们重复这个过程，直到获得所需的f减少。具体来说，我们选择最大的t，使得：

![](../Images/203a2bc30892cf22af6bd0f28f788373.png)

即，减少至少为σt || ∇ₓ f(x⁽ᵏ ⁻ ¹⁾) ||²。但是，为什么选择这个值？可以通过数学方法（通过泰勒一阶展开）证明t || ∇ₓ f(x⁽ᵏ ⁻ ¹⁾) ||²是我们可以期望在当前迭代中通过改进得到的f的最小减少。条件中有一个额外的σ因子。这是为了考虑即使我们不能实现理论上保证的减少t || ∇ₓ f(x⁽ᵏ ⁻ ¹⁾) ||²，我们至少希望达到由σ缩放的该减少的一部分。也就是说，我们要求减少的f至少是f在x⁽ᵏ⁾的泰勒一阶近似所承诺的减少的固定比例σ。如果条件不满足，我们通过β将t缩小到较小值。我们来看一下我们的例子（设置t¯= 1, σ = β = 0.5）：

![](../Images/88e51c2844dbb22ff81ac17c789e8711.png)

前两个迭代计算如下：

![](../Images/2364678ddcf744731712fc9c5c0f9482.png)

同样，

![](../Images/d4a875e91d02e832711496c5cd647c0c.png)

我们通过以下Python代码编程计算剩余迭代：

```py
# Perform gradient descent optimization
def grad_desc(f, df, x0, y0, tol=0.001):
    x, y = [x0], [y0]  # Initialize lists to store x and y coordinates
    num_steps = 0  # Initialize the number of steps taken
    # Continue until the norm of the gradient is below the tolerance
    while np.linalg.norm(df(x0, y0)) > tol:  
        v = -df(x0, y0)  # Compute the direction of descent
        # Compute the step size using Armijo line search
        t = armijo(f, df, x0, y0, v[0], v[1]) 
        x0 = x0 + t*v[0]  # Update x coordinate
        y0 = y0 + t*v[1]  # Update y coordinate
        x.append(x0)  # Append updated x coordinate to the list
        y.append(y0)  # Append updated y coordinate to the list
        num_steps += 1  # Increment the number of steps taken
    return x, y, num_steps

def armijo(f, df, x1, x2, v1, v2, s = 0.5, b = 0.5):
    t = 1
    # Perform Armijo line search until the Armijo condition is satisfied
    while (f(x1 + t*v1, x2 + t*v2) > f(x1, x2) + 
            t*s*np.matmul(df(x1, x2).T, np.array([v1, v2]))):
        t = t*b # Reduce the step size by a factor of b
    return t

# Run the gradient descent algorithm with initial point (1, 1)
a, b, n = grad_desc(f, df, 1, 1)

# Print the number of steps taken for convergence
print(f"Number of Steps to Convergence: {n}")
```

与之前一样，在上述代码中，我们定义了以下收敛标准（将为所有方法一致使用）：

![](../Images/fd8d411ce856dbac80a73d18e31bea1e.png)

运行上述代码时，我们发现仅需10步即可收敛。以下图示显示了梯度下降过程中的迭代轨迹：

```py
# Plot the contours
X = np.arange(-1.1, 1.1, 0.005)
Y = np.arange(-1.1, 1.1, 0.005)
X, Y = np.meshgrid(X, Y)
Z = f(X,Y)
plt.figure(figsize=(12, 7))
plt.contour(X,Y,Z,250, cmap=cmap, alpha = 0.6)
n = len(a)
for i in range(n - 1):
    plt.plot([a[i]],[b[i]],marker='o',markersize=7, color ='r')
    plt.plot([a[i + 1]],[b[i + 1]],marker='o',markersize=7, color ='r')
    plt.arrow(a[i],b[i],a[i + 1] - a[i],b[i + 1] - b[i], head_width=0, 
      head_length=0, fc='r', ec='r', linewidth=2.0)
```

![](../Images/1e5cba8e49668b16b60db196d0898156.png)

等高线图：回溯 [图像由作者使用Python生成]

# 结论

在这篇文章中，我们了解了一些优化梯度下降算法步长的有用技巧。特别是，我们介绍了三种主要技巧：固定步长，即在训练过程中保持相同的步长或学习率，精确线搜索，即将损失函数最小化为t的函数，以及Armijo回溯，即逐渐减少步长直到满足阈值。虽然这些是调优优化的最基本技巧，但还有大量其他方法（例如，将t设置为迭代次数的函数）。这些工具通常用于更复杂的环境，如随机梯度下降。本文的目的不仅是介绍这些技巧，还使你意识到可能影响优化算法的复杂性。虽然大多数技巧用于梯度下降，但它们也可以应用于其他优化算法（例如牛顿-拉夫森方法）。每种技巧都有其优点，并可能在特定应用和算法中优于其他技巧。

希望你喜欢阅读这篇文章！如有任何疑问或建议，请在评论框中回复。欢迎通过[邮件](mailto:naman.agr03@gmail.com)与我联系。

如果你喜欢我的文章并想阅读更多，请关注我。

注意：所有图像均由作者制作。
