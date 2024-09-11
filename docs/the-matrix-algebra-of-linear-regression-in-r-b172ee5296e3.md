# R中的线性回归矩阵代数

> 原文：[https://towardsdatascience.com/the-matrix-algebra-of-linear-regression-in-r-b172ee5296e3?source=collection_archive---------12-----------------------#2023-05-10](https://towardsdatascience.com/the-matrix-algebra-of-linear-regression-in-r-b172ee5296e3?source=collection_archive---------12-----------------------#2023-05-10)

## 探索如何使用R的矩阵运算符估计回归参数

[](https://medium.com/@dataforyou?source=post_page-----b172ee5296e3--------------------------------)[![Rob Taylor, PhD](../Images/5e4e86da7b77404ed42d00a60ea5eacf.png)](https://medium.com/@dataforyou?source=post_page-----b172ee5296e3--------------------------------)[](https://towardsdatascience.com/?source=post_page-----b172ee5296e3--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----b172ee5296e3--------------------------------) [Rob Taylor, PhD](https://medium.com/@dataforyou?source=post_page-----b172ee5296e3--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F98de080592fc&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-matrix-algebra-of-linear-regression-in-r-b172ee5296e3&user=Rob+Taylor%2C+PhD&userId=98de080592fc&source=post_page-98de080592fc----b172ee5296e3---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----b172ee5296e3--------------------------------) ·12分钟阅读·2023年5月10日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fb172ee5296e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-matrix-algebra-of-linear-regression-in-r-b172ee5296e3&user=Rob+Taylor%2C+PhD&userId=98de080592fc&source=-----b172ee5296e3---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fb172ee5296e3&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fthe-matrix-algebra-of-linear-regression-in-r-b172ee5296e3&source=-----b172ee5296e3---------------------bookmark_footer-----------)![](../Images/db58cd733b595a3b246084687d7db11e.png)

图片由 [Breno Machado](https://unsplash.com/@brenomachado?utm_source=medium&utm_medium=referral) 提供，来源于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

## 介绍

我最近写了一篇[文章](/the-matrix-algebra-of-linear-regression-6fb433f522d5)，探讨了线性回归背后的矩阵代数和数学运算。虽然掌握理论原则确实很重要，但没有什么能比得上*实际*进行这些计算。所以，在这篇后续文章中，我们将探讨如何使用R实现这些矩阵运算。

本文应作为我之前的[文章](/the-matrix-algebra-of-linear-regression-6fb433f522d5)的补充，如果你还没读过这篇文章，我鼓励你去看看；不过即使没读过，你也能继续跟进。

## 数据

作为我们的工作示例，我选择了 R 中 `datasets` 包里的 `cars` 数据集。这是一个简单的数据集，包含了在不同速度下的汽车停止距离。因此，这个数据集包含两个变量：`speed` 和 `dist`。不过，这些观测数据是1920年代的，因此绝不是最新的数据！尽管如此，它非常适合用来建立一个简单的线性回归模型。

首先，我们快速查看一下数据：

```py
> str( cars )
'data.frame': 50 obs. of  2 variables:
 $ speed: num  4 4 7 7 8 9 10 10 10 11 ...
 $ dist : num  2 10 4 22 16 10 18 26 34 17 ...
```

这里没有什么复杂的。我们共有50个观测值和两个数值变量。需要注意的一点是 `speed` 和 `dist` 都是整数值。这确实引入了一些离散化，但目前这不太重要。

为了了解 `dist` 和 `speed` 之间的关系，下面我绘制了停止距离与速度的关系图。这些变量之间存在较强的正相关，表明停止距离随着速度的*增加*而增加。我还使用 ggplot 的 `geom_smooth` 函数叠加了最佳拟合回归线。

因此，我们的目标是*不*使用内置的 `lm` 函数来估计这条直线的参数。相反，我们将应用矩阵运算来获得回归系数，然后生成应当落在这条直线上的拟合值。

![](../Images/ecde3aaf52c5f24e897d7e5b79fed328.png)

这是一个展示停止距离与速度关系的图。数据来源于 R 的汽车数据集（图片由作者提供）。

## 简要回顾

值得提醒自己我们实际的目标是什么。为此，我们试图将包含 *n* 个观测值的响应向量 *Y* 建模为 *m* 个预测变量的加权线性组合，加上一个截距项。对于这里的示例数据，我们只有一个预测变量 `speed`，因此我们可以设定 *m =* 1。

预测变量和截距项共同形成一个 *n* × *p* 设计矩阵，其中 *p = m + 1* 反映了模型中需要从数据中估计的未知回归系数的数量。估计需要找到以下*正规方程*的解：

![](../Images/b1d44c78834574b4e7515cae3829549b.png)

正规方程（图片由作者提供）。

重新排列方程以求解未知系数，我们得到以下解：

![](../Images/83c7b142968ce142f76bbd673a288b5b.png)

估计方程（图片由作者提供）。

其中 **b** 是一个 *p* 维向量，包含最佳拟合的回归系数。

在解决这个问题时，我将把估计方程分解成处理不同操作的部分，然后再将它们合并以估计模型参数。实际上，我们需要做三个操作，每个操作返回一个我们在某个时候需要使用的矩阵。

## R 中的矩阵运算

在深入研究之前，我将快速回顾一下我们将要使用的矩阵运算。这个例子中只有 *三* 个重要的操作。第一个是 *矩阵乘法*。如果你还没有接触过这个，R 中的矩阵乘法是使用 `%*%` 运算符来执行的。第二个操作涉及求 *矩阵的逆*，在 R 中使用 `solve` 函数来完成（不，我们不会手动完成这部分！）。最后一个操作是 `t`，这是 *转置* 操作符。

## 设置

首先，我们需要定义一个 *响应向量* 并创建 *设计矩阵*。对于我们的示例，我们将距离建模为速度的函数，因此 `dist` 是我们的 *响应变量*。让我们为这些值分配一个名为 `y_vec` 的向量：

```py
# Assign dependent variable to the response vector
y_vec <- cars$dist
```

请注意，在这里使用 `$` 访问 `cars` 数据框中的 `dist` 列。我发现这种方法并不总是方便或整洁，因此你会看到我很快转而使用 `with` 进行一些计算。

接下来，我们需要使用 `model.matrix` 函数将 `speed` 分配给设计矩阵。在这里，我创建了一个名为 `x_mat` 的变量：

```py
# Create a design matrix with the single independent variable
x_mat <- model.matrix( dist ~ speed, data = cars )
```

请注意，对 `model.matrix` 的调用与 `lm` 函数调用非常相似。显然，这样做有一个很好的理由，但在这里公式只是让 `model.matrix` 知道 `speed` 是一个预测变量。让我们快速看一下它产生了什么：

```py
> head(x_mat)
  (Intercept) speed
1           1     4
2           1     4
3           1     7
4           1     7
5           1     8
6           1     9
```

我们可以看到我们有 *两* 列：一个是我们的 `speed` 变量，另一个只包含 `1`。回顾一下，设计矩阵包括一个额外的列以容纳截距项。

现在，我们已经将 `y_vec` 和 `x_mat` 加载到我们的工作空间中，我们可以继续进行矩阵运算。

## 步骤 1

如果我们观察估计方程的右手边，第一项是设计矩阵的逆与其自身的乘积。在步骤2中，我们将处理逆，因此我们首先需要计算 *X*ᵀ*X*。

对于我们的简单线性回归模型，*X*ᵀ*X* 将是一个 2 × 2 的方阵。出于实际目的，我将简单地将其表示为矩阵 *A*。使用矩阵乘法 `%*%` 和转置 `t` 运算符来计算这个矩阵，我将将输出赋值给一个叫做 `A` 的对象：

```py
# Compute cross-product of the design matrix
A  <- t( x_mat ) %*% x_mat
```

现在，在我之前的帖子中，我们通过矩阵运算了解了这个矩阵中包含的元素的一些信息。我不会在这里重新讨论具体细节，只是将结果如下记录：

![](../Images/b1682048cb8a51d79ef59ef220e99985.png)

设计矩阵的交叉乘积（作者提供的图片）。

我们可以看到 *A* 中的每个元素都很容易解释，确实也容易计算。注意 *a₁₁* 只是观察总数 *n*，对于 `cars` 数据集来说是 50。次对角线上的元素 *a₁₂* 和 *a₂₁* 只是 `speed` 的总和，而 *a₂₂* 是 `speed` 的平方和。

我们可以做一些快速检查，以验证我们的对象 `A` 确实包含这些值。让我们首先打印 `A` 来看看我们得到了什么：

```py
> A
            (Intercept) speed
(Intercept)          50   770
speed               770 13228
```

`A[1,1]` 的值与 `cars` 中的总观察数匹配，因此我们有了一个良好的开端。接下来，我们可以通过简单地对 `speed:` 进行求和来检查 `A[1,2]` 和 `A[2,1]` 的值。

```py
> sum( cars$speed )
[1] 770
```

另一个匹配——一切看起来都很好！最后，我们可以通过计算 `speed` 的平方并对这些值进行求和来检查 `A[2,2]` 的值。

```py
> sum( cars$speed ^ 2 )
[1] 13228
```

这也是正确的！

## 第2步

下一步是找到矩阵 *XᵀX* 的*逆*。在 R 中，我们使用 `solve` 函数对 `A` 执行此操作。让我们先执行这个操作并将输出分配给一个名为 `A_inv` 的对象：

```py
# Inverting the matrix A
A_inv <- solve( A )
```

现在，在查看 `A_inv` 之前，让我们先检查 *XᵀX* 的逆矩阵的解：

![](../Images/936f0f5ae04f7aeb3071c1cb3b4ece9c.png)

设计矩阵交叉乘积的逆矩阵（图片由作者提供）。

如果现在忽略第一个项，那么我们看到的是原矩阵 *A* 中的一些元素已经被交换，而其他元素稍微发生了变化。具体来说，元素 *a₁₂* 和 *a₂₁* 的符号被反转，现在反映了 `speed` 的*负*总和。此外，原矩阵中 *a₁₁* 和 *a₂₂* 的值交换了位置，因此现在 *a₂₂* 是观察总数，而 *a₁₁* 是 `speed` 的平方和。

让我们编译一个包含这些调整后元素的矩阵，并将其分配给一个名为 `A_rev` 的对象，然后检查一切是否正常：

```py
# Shifting the elements in A
n_obs <- nrow( cars )
A_rev <- with(cars, matrix( c(sum( speed ^ 2), -sum( speed ), -sum( speed ), n_obs), nrow = 2))
> A_rev
      [,1] [,2]
[1,] 13228 -770
[2,]  -770   50
```

一切看起来都很好！我们现在可以将 `A_rev` 乘以第一个项，即一个*标量常数*，它等于 *n* 的倒数乘以 `speed` 的平方偏差和。让我们通过创建一个名为 `c` 的变量来快速计算常数项：

```py
# Computing the scalar constant for matrix inversion
c <- (n_obs * with( cars, sum( (speed - mean(speed)) ^ 2) )) ^ -1
```

我不会打印出来，因为这是一个极小的值。现在剩下的就是将 `c` 和 `A_rev` 相乘，看看我们得到什么：

```py
# Computation of the matrix inverse
> c * A_rev
            [,1]         [,2]
[1,]  0.19310949 -0.011240876
[2,] -0.01124088  0.000729927
```

现在让我们将这些值与使用 `solve` 得到的解进行比较，通过将 `A_inv` 打印到控制台：

```py
# Print output from solve() function
> A_inv
            (Intercept)        speed
(Intercept)  0.19310949 -0.011240876
speed       -0.01124088  0.000729927
```

太棒了——`A_inv` 中的每个元素都与 `A_rev` 中的对应元素匹配。

在进入下一步之前，我们可以做一个额外的检查。如果出于某种原因我们怀疑 `solve` 的输出有误，我们可以依赖于以下事实：任何 *n* × *n* 的方阵 *A* 被认为是*可逆的*，当且仅当存在另一个方阵 *n* × *n* 的矩阵 *B*，使其结果为以下等式：

![](../Images/6a845c04b13e0f9540be32cbc3ce0519.png)

矩阵可逆性的条件（图片由作者提供）。

这意味着，如果`A_inv`确实是`A`的乘法逆矩阵，那么如果我们将`A`和`A_inv`相乘，我们应该得到*单位矩阵*：

```py
> round( A %*% A_inv )
            (Intercept) speed
(Intercept)           1     0
speed                 0     1
```

看起来`solve`函数在做合理的事情。

在继续之前，有一点需要说明：这些对矩阵输出的检查对于任何实际目的并不必要。相反，我希望通过包含这些小测试进一步增强你对这些概念的理解，实际上将数学应用到实践中。

## 第三步

现在我们已经处理了*XᵀX*的逆矩阵，我们可以将注意力转向估计方程中的最后一项：*XᵀY*。这个操作涉及将*设计矩阵*与*响应向量*相乘。鉴于我们处理的数据，这个计算将产生一个*p × 1*向量，我们将其简单地表示为*B*。同样，我们将应用矩阵乘法运算符，并将输出赋值给一个变量，这次我们称这个变量为`B:`。

```py
# Matrix multiplying the design matrix and response vector
B <- t( x_mat ) %*% y_vec
```

如上所述，让我们考虑一下这个操作的预期输出是什么：

![](../Images/694d2c7c07ee8921b843d9a6cd10f55d.png)

设计矩阵与响应向量的叉乘（图片作者提供）。

再次，我们有一些相当容易直观理解的元素。其中一个是响应变量`dist`的总和，另一个是`speed`和`dist`的乘积总和。让我们快速计算这些值，看看它们是否与已存储在`B:`中的值匹配：

```py
# Compute vector and compare with B
> with( cars, matrix( c( sum( dist ), sum( speed * dist) ) , ncol = 1 ))
      [,1]
[1,]  2149
[2,] 38482
>
> B
             [,1]
(Intercept)  2149
speed       38482
```

我们匹配了！

## 参数估计

好的。我们现在拥有了估计模型系数所需的一切。实际上，我们只需要`A_inv`和`B`，然后只需将它们相乘即可：

```py
# Estimate the parameter values using solution to normal equation
b_vec <- A_inv %*% B
```

让我们将其打印到控制台，看看我们得到什么：

```py
> b_vec
                  [,1]
(Intercept) -17.579095
speed         3.932409
```

快速查看斜率参数，我们得到了一个正值。这是一个好兆头，因为之前观察到的正相关，且希望这意味着我们在正确的轨道上。现在让我们看看这些估计值与使用`lm`函数返回的系数相比如何：

```py
> coef( lm( dist ~ speed, data = cars ) )
(Intercept)       speed 
 -17.579095    3.932409 
```

一致的！

我想指出最后一件事。我在之前的帖子中展示了，如果你对估计方程做一些代数运算，你会得到一个非常方便的解。也就是说，斜率参数等于：

![](../Images/34ab197a6dc7deb317ffcd1787e9a765.png)

斜率参数（图片作者提供）。

并且截距参数等于：

![](../Images/80d70d35f595c980c8a328469c442ee2.png)

截距参数（图片作者提供）。

这两者计算起来非常直接，可以在R中如下进行：

```py
# The slope parameter
b_1 <- with(cars, cov(speed, dist) / var(speed) )

# The intercept parameter
b_0 <- with(cars, mean(dist) - mean(speed) * b_1 )
```

将这些对象打印到控制台显示，我们得到相同的参数值：

```py
> c( "intercept" = b_0, "slope" = b_1 )
 intercept      slope 
-17.579095   3.932409
```

## 解释系数

看我们的估计值，我们可以看到有一个负的截距项，这并不太合理。这里的截距是`dist`在`speed`设为零时的值，这意味着当车辆的速度为零时——也就是说，*静止不动*——它的*负*制动距离。我不会在这里解决这个问题，所以现在我们就这样接受它（尽管这并不合理）。

另一方面，斜率参数肯定更有用。这表明每增加一个单位的*速度*（以每小时英里为单位），制动距离*增加*接近4英尺。因此，如果车辆的速度从10英里每小时增加到11英里每小时，则制动距离从21.7英尺跳跃到25.7英尺。然而，如果汽车的速度从60英里每小时增加到61英里每小时，制动距离的增加也应该是相同的，这可能不太合理。

## 计算拟合值

我们需要做的最后一件事是计算拟合值。为此，我们只需取出`b_vec`中的参数估计值，并将其与设计矩阵相乘：

```py
# Compute the fitted values
y_hat <- x_mat %*% b_vec

# Compute the residuals
e_vec <- y_hat - y_vec
```

我在上面的代码中也计算了误差向量，但这仅仅是为了向你展示如何计算。我们不会在这里处理这些值。

我们能做的最后一件事是将我们之前生成的图上的拟合值进行叠加。如果一切顺利，我们应该能看到拟合值很好地沿回归线分布：

![](../Images/c01051643702151c2b223a07e79f0e53.png)

在之前生成的图上叠加拟合值（图片作者提供）。

确实如此。

## 总结

这里的重点主要放在了参数估计上，因此有很多内容我没有涉及。例如，模型诊断、计算参数估计的标准误差以及像决定系数*R²*这样的模型拟合指标。由于这篇文章旨在作为我早期文章的补充，我希望避免在这里引入新概念，但我会在另一时间讨论这些概念。尽管如此，我希望这篇文章在你进行线性代数学习的过程中能有所帮助。如果你想查看本文使用的代码，可以在我的[Git](https://github.com/dataforyounz/matrix-regression-in-r)页面找到。

## 相关文章

+   [线性回归的矩阵代数](/the-matrix-algebra-of-linear-regression-6fb433f522d5)

+   [线性代数入门](https://medium.com/towards-data-science/a-primer-on-linear-algebra-414111d195ca)

+   [线性代数入门：第二部分](https://medium.com/towards-data-science/a-primer-on-linear-algebra-part-2-eba53c564a90)

+   [多重共线性：问题，还是不是问题？](https://medium.com/towards-data-science/multicollinearity-problem-or-not-d4bd7a9cfb91)

感谢阅读！

如果你喜欢这篇文章并希望保持更新，请考虑[关注我在 Medium 上的账号](https://medium.com/@dataforyou)。这将确保你不会错过任何新内容。

要获取对所有内容的无限访问权限，请考虑订阅 [Medium](https://medium.com/membership)。

你还可以在 [Twitter](https://twitter.com/dataforyounz)、[LinkedIn](https://www.linkedin.com/in/dataforyou/) 上关注我，或者查看我的 [GitHub](https://github.com/dataforyounz)，如果你更喜欢这样的话。
