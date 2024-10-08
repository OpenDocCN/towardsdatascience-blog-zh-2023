# 线性代数鸟瞰图：映射的度量——行列式

> 原文：[`towardsdatascience.com/a-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be`](https://towardsdatascience.com/a-birds-eye-view-of-linear-algebra-the-measure-of-a-map-determinant-1e5fd752a3be)

[](https://medium.com/@rohitpandey576?source=post_page-----1e5fd752a3be--------------------------------)![Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----1e5fd752a3be--------------------------------)[](https://towardsdatascience.com/?source=post_page-----1e5fd752a3be--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e5fd752a3be--------------------------------) [Rohit Pandey](https://medium.com/@rohitpandey576?source=post_page-----1e5fd752a3be--------------------------------)

·发布于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----1e5fd752a3be--------------------------------) ·阅读时间 16 分钟·2023 年 11 月 5 日

--

![](img/b0a624404feecd67f0eeefedb248be8c.png)

图像由 midjourney 创建

这是正在进行的线性代数书籍《线性代数鸟瞰图》的第二章。到目前为止的目录：

1.  第一章: [基础](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f)

1.  **第二章**: （当前）映射的度量——行列式

1.  第三章: [为什么矩阵乘法是这样的？](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-why-is-matrix-multiplication-like-that-a4d94067651e)

1.  第四章: [方程组、线性回归和神经网络](https://medium.com/p/fe5b88a57f66)

1.  第五章: 秩与零度及行秩==列秩

线性代数是多维工具。不管你在做什么，只要你扩展到 *n* 维，线性代数就会出现。

在 [前一章](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f) 中，我们描述了抽象的线性映射。在本章中，我们挽起袖子开始处理矩阵。实际考虑因素如数值稳定性、效率算法等将开始被探讨。

# I) 如何量化线性映射？

在 [前一章](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f) 中，我们讨论了向量空间的概念（基本上是 n 维数值集合——更广泛地说，是 [域](https://en.wikipedia.org/wiki/Field_(mathematics)) 的集合）和对这两个向量空间进行操作的线性映射。

作为这些映射的一种例子，一个向量空间可以是你坐的行星的表面，另一个可以是你可能坐的桌子的表面。从这个意义上说，世界地图也是地图，因为它们将地球表面的每一点“映射”到纸上或桌面上的一点，尽管它们不是线性映射，因为它们不保持相对面积（例如在某些投影中格林兰的面积看起来比实际大得多）。

![](img/b3b6c94f34c69adff0f339b5d2ec2d9b.png)

地球表面的实际地图在线性代数的意义上也是一种地图，但它不是线性映射。图像由 midjourney 创建。

一旦我们为向量空间选择了一个 [基](https://en.wikipedia.org/wiki/Basis_(linear_algebra))（空间中 *n* 个“独立”向量的集合；通常可以有无限的选择），所有在该向量空间上的线性映射都有唯一的矩阵与之对应。

目前，让我们将注意力限制在将向量从 n 维空间映射回 n 维空间的映射上（我们稍后会进行推广）。这些线性映射对应的矩阵是 *n* x *n*（见 [第一章](https://medium.com/towards-data-science/a-birds-eye-view-of-linear-algebra-the-basics-29ad2122d98f) 的第 III 节）。将这种线性映射“量化”，以单一数字表示其对向量空间 R^n 的影响可能是有用的。我们处理的这种映射，实际上是将向量从 R^n 中“扭曲”到同一空间中的其他向量。原始向量 *v* 和映射转换后的向量 *u* 都有一些长度（例如 |*v*| 和 *|u|*）。我们可以考虑映射改变了向量长度的多少，*|u|/|v|*。也许这可以量化映射的影响？它对向量的“拉伸”程度？

这种方法有一个致命的缺陷。比率不仅依赖于线性映射，还依赖于它作用的向量 *v*。因此，它并不是线性映射本身的一个严格性质。

假如我们现在考虑两个向量，*v_1* 和 *v_2*，它们通过线性映射转化为向量 *u_1* 和 *u_2*。就像单个向量 *v* 的度量是它的长度一样，两个向量的度量是它们之间的平行四边形的面积。

![](img/bedfea891b309d55e79ba75b893049d7.png)

由两个向量形成的平行四边形的面积。图像由 midjourney 创建。

就像我们考虑 *v* 长度变化的量一样，我们现在可以讨论 *v_1* 和 *v_2* 之间的面积在通过线性映射后变成 *u_1, u_2* 时的变化量。遗憾的是，这同样不仅依赖于线性映射，还依赖于选择的向量。

接下来，我们可以扩展到三个向量，考虑它们之间的 [平行六面体](https://en.wikipedia.org/wiki/Parallelepiped) 体积的变化，并遇到相同的问题，即初始向量的影响。

![](img/31c46dc1d5d7dbbc6f15f6ad9e645ea9.png)

三维空间中的一个三维区域。如果线性映射作用于这三个向量，无论初始选择的向量是什么，体积的变化量都相同。图像由 midjourney 创建。

但现在考虑原始向量空间中的一个 n 维区域。这个区域将具有某种“n 维度量”。要理解这一点，一个二维量度是面积（以平方公里计算）。三维量度是用于测量水的体积（以升计算）。四维量度在我们习惯的物理世界中没有对应物，但在数学上同样合理，是测量由四个 4-d 向量形成的平行六面体中四维空间的量，依此类推。

![](img/d70bf6a71b72e4addf0955abcf3d767f.png)

在 2 维空间中的度量是面积，在 3 维空间中的度量是体积。这些概念可以扩展到 4 维空间及以上。

*n* 个原始向量（*v_1, v_2, …, v_n*）形成一个平行六面体，该平行六面体被线性映射变换为*n* 个新向量，*u_1, u_2 … u_n*，它们形成自己的平行六面体。然后我们可以问新区域的 n 维度量与原始区域的关系。结果发现，这个比例确实只是线性映射的函数。无论原始区域的样子、位置等如何，线性映射作用于它后的度量与作用之前的度量之比将相同，是线性映射的纯粹函数。这个 n 维度量的比例（后/前）就是我们一直在寻找的。一个量化线性映射效果的*唯一*属性。

通过线性映射改变的***任何***n 维空间片段的比例是量化它对所作用空间影响的一个好方法。这个比例称为线性映射的行列式（这个名字的原因将在第 V 节中显现）。

目前，我们只是简单陈述了线性映射从 R^n 到 R^n“拉伸”任何 n 维空间片段的程度仅依赖于映射本身，而没有提供证明，因为这里的目的是激励。我们将在稍后（第 VI 节）提供证明，一旦我们准备好一些工具。

# II) 计算行列式

那么，如何在给定一个从向量空间 *R^n* 到 *R^n* 的线性映射时找到这个行列式呢？我们可以取任何*n*个向量，找到它们之间的平行六面体的度量，然后找到线性映射作用于所有向量后的新平行六面体的度量。最后，将后者除以前者。

我们需要使这些步骤更具体。首先，让我们开始在这个 *R^n* 向量空间中玩耍。

*R^n* 向量空间只是 *n* 个实数的集合。最简单的向量是 *n* 个零——*[0, 0,…, 0]*。这是零向量。如果我们用一个标量乘它，我们得到的仍然是零向量。这并不有趣。对于下一个最简单的向量，我们可以将第一个 *0* 替换为 *1*。这就得到了向量：*e1=[1, 0, 0,.., 0]*。现在，乘以一个标量 *c* 会得到一个不同的向量。

*c.[1, 0, 0,.., 0] = [c, 0, 0, …, 0]*

我们可以用 *e1* “生成”无限多个向量，具体取决于我们选择的标量 *c*。

如果 *e1* 是第一个元素为 *1* 其余为 *0* 的向量，那么 *e2* 是什么？第二个元素为 *1* 其余为 *0* 看起来是一个合乎逻辑的选择。

*e2 = [0,1,0,0,…0]*

将此推向逻辑的结论，我们得到一个由 *n* 个向量组成的集合：

*e1 = [1, 0, 0,.., 0]*

*e2 = [0,1,0,0,…0]*

*e2 = [0,0,1,0,…0]*

…

*en = [0,0,0,0,…1]*

这些向量构成了 *R^n* 向量空间的一个基底。这意味着什么？*R^n* 中的任何向量 *v* 都可以表示为这些 *n* 个向量的线性组合。这意味着对于某些标量 *c1, c2,…, cn：*

*v = c1.e1+c2.e2+…+cn.en*

所有向量，*v* 都是由向量集 *e1, e2,…, en* “生成”的。

这个特定的向量集合不是唯一的基底。任何 *n* 个向量的集合都可以。唯一的注意事项是，这些 *n* 个向量中没有一个应由其余向量“生成”。换句话说，所有的 *n* 个向量应当是线性无关的。如果我们从大多数连续分布中随机选择 *n* 个数，并重复这个过程 *n* 次来创建这 *n* 个向量，你将以 100%的概率获得一组线性无关的向量（在概率术语中为“几乎肯定”）。只是非常非常不可能随机向量恰好由其他 *k<n* 个随机向量“生成”。

回到本节开始时我们用来寻找线性映射行列式的公式，我们现在有一个基底来表示我们的向量。确定基底也意味着我们的线性映射可以表示为一个矩阵（参见第一章第 III 节）。由于该线性映射将向量从 *R^n* 映射回 *R^n*，因此相应的矩阵是 *n* x *n*。

接下来，我们需要 *n* 个向量来形成我们的平行六面体。为什么不使用我们之前定义的 *e1*, *e2, …* *en* 标准基底。由这些向量包含的空间补丁的度量恰好是 *1*，这就是定义。下面的 *R³* 图像希望能让这一点变得清晰。

![](img/fa5e1f9d8a6f18827a82f4027589640b.png)

向量 *e1, e2, e3, …, en* 之间包含的标准空间补丁。在这种情况下，我们有三个向量，因为空间是三维的。图像由 midjourney 创建。

如果我们将这些标准基底的向量收集成一个矩阵（行或列），我们会得到单位矩阵（主对角线上的 1，其他地方为 0）：

![](img/33231727fbd66a497f87aa568da3cd0c.png)

单位矩阵。图像由作者提供。

当我们说可以将线性变换应用于任何 n 维空间的补丁时，我们也可以将其应用于这个“标准”补丁。

但，很容易证明将任何矩阵与单位矩阵相乘会得到相同的矩阵。因此，应用线性映射后的结果向量是表示线性映射本身的矩阵的列。因此，线性映射改变“标准补丁”体积的量与表示映射本身的矩阵的列向量之间的平行六面体的 n 维度量是相同的。

总结一下，我们从激发行列式作为线性映射改变 n 维空间补丁度量的比率开始。现在，我们展示了这个比率本身是 n 维度量。特别是，包含在任何表示线性映射的矩阵的列向量之间的度量。

# III) 激发基本属性

我们在前一节中描述了线性映射的行列式应该简单地是任何其矩阵表示之间的度量。在这一节中，我们使用二维空间（其中度量是面积）来激发行列式必须具有的一些基本属性。

第一个属性是多重线性。行列式是一个将一组向量（收集在矩阵中）映射到一个标量的函数。由于我们限制在二维空间中，我们将考虑两个向量，都是二维的。我们的行列式（由于我们将其解释为向量之间平行四边形的面积）可以表示为：

*det = A(v1, v2)*

如果我们将一个向量添加到两个向量中的一个，函数应该如何表现？多重线性属性要求：

*A(v1+v3, v2) = A(v1,v2)+A(v3,v2) __ (1)*

从下面的动态图中可以看出这一点（注意新增的面积）。

![](img/4cd5f3fd20fce6e1f0c619432a102d76.png)

行列式的加法属性。图片作者提供。

这个可视化也可以用于观察（通过缩放其中一个向量而不是添加另一个向量）：

*A(c.v1, v2) = c.A(v1, v2) __ (2)*

第二个属性具有一个重要的含义。如果我们将负的 *c* 代入方程中，面积 *A(v1, v2)* 应该与 *A(c.v1,v2)* 的符号相反。

这意味着我们需要引入负面积和负行列式的概念。

如果我们接受负长度的概念，这就很有意义。如果一维空间中的长度度量可以是正的或负的，那么二维空间中的面积度量也应该允许为负。因此，任何维度的空间度量也应如此。

结合方程（1）和（2）就是多重线性属性。

另一个与行列式符号有关的重要属性是“交替属性”。它要求：

*A(v1, v2) = -A(v2, v1) __(3)*

交换两个向量的顺序会使行列式（或它们之间的度量）符号取反。如果你学过 [叉积](https://en.wikipedia.org/wiki/Cross_product#:~:text=The%20cross%20product%20a%20×,parallelogram%20that%20the%20vectors%20span.) 3D 向量，这个性质会非常自然。为了说明这个性质，先考虑两个位置向量之间的一维距离 *d(v1, v2)*。显然 *d(v1, v2) = -d(v2, v1)*，因为当我们从 *v2* 移动到 *v1* 时，我们的方向与从 *v1* 移动到 *v2* 时的方向相反。类似地，如果向量 *v1* 和 *v2* 之间的面积是正的，那么 *v2* 和 *v1* 之间的面积必须是负的。这个性质在 *n* 维空间中也成立。如果在 *A(v1, v2, …, vn*) 中我们交换两个向量，它会导致符号的改变。

交替性质还意味着如果其中一个向量只是另一个向量的标量倍数，则行列式必须是 *0*。这是因为交换两个向量应该会使行列式取反：

*A(v1, v1) = -A(v1, v1)*

=> *2 A(v1, v1) = 0*

*=> A(v1, v1) = 0*

我们还可以通过多线性（公式 2）：

*A(v1, c.v1) = c A(v1, v1) = 0*

从几何角度来看这是合理的，因为如果两个向量彼此平行，那么它们之间的面积是 *0*。

[视频 [6]](https://www.youtube.com/watch?v=9IswLDsEWFk) 通过非常好的可视化展示了这些性质的几何动机，[视频 [4]](https://www.youtube.com/watch?v=Ip3X9LOh2dk&t=295s) 也很好地可视化了交替性质。

# IV) 代数推导：推导莱布尼茨公式

在这一部分，我们远离几何直观，从另一个角度，即冷冰冰的代数计算，来探讨行列式的话题。

如前一节通过几何学所激发的多线性和交替性质，（显著地）足以为我们提供一个非常具体的行列式代数公式，称为莱布尼茨公式。

这个公式帮助我们看到行列式的性质，这些性质从几何方法或其他代数公式中会非常困难。

莱布尼茨公式可以简化为拉普拉斯展开，涉及沿着某一行或列进行计算并计算余子式，这些在高中时许多人见过。

让我们推导莱布尼茨公式。我们需要一个函数，它接受矩阵的 *n* 列向量，𝛼*1,* 𝛼2, …, 𝛼n 作为输入，并将它们转换为一个标量 *c*。

![](img/6deafeac9e62bd5fdff44b3b4278e1f4.png)

我们可以用空间的标准基表示每个列向量。

![](img/c5cadb1dde3b10898ae93347fb00d1f6.png)

现在，我们可以应用多线性性质。暂时考虑第一个列，𝛼*1*。

![](img/d8b0489c4d178c771f108d14543a589d.png)

对第二列也可以这样做。我们只取上面求和中的第一个项，看看结果。

![](img/eaab85176481b60806e0dc37a58b9831.png)

注意在第一个项中，我们得到向量*e1*出现两次。根据交替属性，该项的函数*f*变为*0*。

为了让两个*e1*出现，积中两个*a*的第二个指标必须都变成*1*。

所以，一旦我们对所有列进行这个操作，按照交替属性不会变为零的项将是那些第二个指标没有任何重复的项——即从*1*到*n*的所有不同数字。换句话说，我们在寻找*a*的第二个指标中出现*1*到*n*的排列。

那么，*a*的第一个指标是什么呢？这些指标只是从*1*到*n*的顺序，因为我们首先提取*a_1x*，然后是*a_2x*，以此类推。用更简洁的代数符号表示，

![](img/4393761627ae977ce9371178e9dd0482.png)

在右边的表达式中，区域*f(e_j1, e_j2, …, e_jn)* 可以是 *+1*、*-1* 或 *0*，因为*e_j*都是彼此正交的单位向量。我们已经确定，任何具有重复*e_j*的项将变为*0*，留下的只是排列（没有重复）。在这些排列中，我们有时会得到 *+1*，有时会得到 *-1*。

排列的概念带有[符号](https://en.wikipedia.org/wiki/Parity_of_a_permutation#:~:text=The%20sign%2C%20signature%2C%20or%20signum,1%20if%20σ%20is%20odd.)。这些区域的符号与排列的符号相等。如果我们用*S_n*表示*{1,2,…,n}*的所有排列集合，那么我们得到的就是行列式的莱布尼茨公式：

![](img/c9d8fd2543edb17b179f5a961751aeba.png)

如果你对这个公式仍然感到困惑，可以查看[mathexchange 帖子，[3]](https://math.stackexchange.com/questions/319321/understanding-the-leibniz-formula-for-determinants#:~:text=The%20formula%20says%20that%20det,permutation%20get%20a%20minus%20sign.&text=where%20the%20minus%20signs%20correspond%20to%20the%20odd%20permutations%20from%20above)。如果你是那种需要看到实际代码才能理解的人，这里有一些 Python 代码：

```py
import numpy as np
import itertools

def permut_sign(a):
    """
    The sign of a permutation is simply the number of inversions.
    This is a simple, O(n²) algorithm for finding the inversions. It can
    be done more efficiently with merge sort in O(n.log(n)) time.
    """
    cnt = 0
    for i in range(len(a)):
        for j in range(i+1, len(a)):
            if a[i] > a[j]:
                cnt += 1
    # Convert the 0-1 into -1 and +1
    return (cnt % 2 - .5) * -2

def leibniz_determinant(a):
    """
    Here, a is the matrix whose determinant we want to calculate.
    """
    n = len(a)
    arr = np.arange(n)
    determinant = 0
    for perm in itertools.permutations(arr):
        sign1 = permut_sign(perm)
        term = 1
        for i in range(len(perm)):
            j = int(perm[i])
            term = term * a[i, j]
        determinant += sign1 * term
    return determinant

a = np.array([[2, 1, 5],
              [4, 3, 1],
              [2, 2, 5]])

det1 = leibniz_determinant(a)
det2 = np.linalg.det(a)
print(abs(det1 - det2) <= 0.00001)
```

实际上，不应该使用这个公式来计算矩阵的行列式（除非只是为了娱乐或展示）。它是有效的，但由于涉及到所有排列的求和（即*n!*，超指数级），因此显得非常低效。

然而，行列式的许多理论性质在使用莱布尼茨公式时变得容易看出，而如果从它的其他形式开始，可能会很难解读或证明。例如：

1.  使用这个公式可以明显看出，一个矩阵及其转置具有相同的行列式：*|A| = |A^T|*。这是公式对称性的简单结果。

1.  可以使用与上述非常相似的推导来证明，对于两个矩阵 *A* 和 *B*，*|AB| = |A|.|B|*。参见[这个答案](https://math.stackexchange.com/a/60293/155881)和[mathexchange 帖子，[8]](https://math.stackexchange.com/questions/60284/how-to-show-that-detab-deta-detb)。

1.  使用莱布尼茨公式，我们可以很容易地看到，如果矩阵是上三角矩阵或下三角矩阵（下三角矩阵意味着矩阵对角线以上的每个元素都是零），行列式仅仅是对角线元素的乘积。这是因为除了一个排列：*(a_11.a_22…a_nn)*（主对角线），其他排列都得到一些零项或其他，从而使求和中的项为*0*。

![](img/57b484c50ad54119997e39dd1577e0e9.png)

上三角矩阵。主对角线下方的所有元素都是 0。作者提供的图像。

第三个事实实际上导致了最有效的行列式计算算法，这是大多数线性代数库使用的。矩阵可以高效地分解为下三角矩阵和上三角矩阵（称为 LU 分解，我们将在下一章中介绍）。在完成这种分解后，第三个事实用于将这些下三角矩阵和上三角矩阵的对角线元素相乘以得到它们的行列式。最后，第二个事实用于将这两个行列式相乘以获得原始矩阵的行列式。

许多高中或大学的学生在首次接触行列式时，学习关于拉普拉斯展开的方法，包括按行或列展开，为每个元素找余因子并求和。这可以通过收集类似项从上面的莱布尼茨展开推导出来。参见[这个答案](https://math.stackexchange.com/a/4225580/155881)中的[mathexchange 帖子，[2]](https://math.stackexchange.com/a/4225580/155881)。

# V) 历史激发

![](img/e0ae6dadb79d423fc427fc015f6e979e.png)

行列式的历史激发。图像由 midjourney 创建。

行列式最初是在解决线性方程组的背景下发现的。假设我们有*n*个方程和*n*个变量*(x_0, x_1, …, x_n)*。

![](img/08d8acc42c6ecd21dc0163c0fe9cc12f.png)

线性方程组。作者提供的图像。

这个系统可以用矩阵形式表示：

![](img/c23217c45d321b5125f4eac5d17ee2b4.png)

矩阵形式的线性方程组。作者提供的图像。

更简洁地：

*A.x = b*

一个重要的问题是上述系统是否有唯一解*x*。行列式是一个“决定”这一点的函数。当且仅当*A*的行列式非零时，才存在唯一解。

这种历史性激发的方法使得行列式作为一个多项式，这个多项式在我们尝试解决与线性映射相关的线性方程组时出现。

更多内容请参见[这个优质回答](https://math.stackexchange.com/a/4782557/155881)中的[mathexchange 帖子，[8]](https://math.stackexchange.com/a/4782557/155881)。

# VI) 我们所激发的性质的证明

我们在本章开始时通过将行列式作为*R^n 到 R^n* 线性映射改变 n 维空间小块的度量来进行动机说明。我们还提到，这对 1、2、.. n-1 维度的度量不适用。下面是一个证明，我们使用了在其他部分遇到的一些性质。

![](img/187785e848c6429aad42afbb8fa79c71.png)

如果你喜欢这本书，考虑请我喝杯咖啡以激励我 :)

[`www.buymeacoffee.com/w045tn0iqw`](https://www.buymeacoffee.com/w045tn0iqw)

# 参考文献

[1] Mathexchange 帖子：线性映射的行列式与基的选择无关： [`math.stackexchange.com/questions/962382/determinant-of-linear-transformation`](https://math.stackexchange.com/questions/962382/determinant-of-linear-transformation)

[2] Mathexchange 帖子：矩阵的行列式拉普拉斯展开（高中公式） [`math.stackexchange.com/a/4225580/155881`](https://math.stackexchange.com/a/4225580/155881)

[3] Mathexchange 帖子：理解莱布尼茨公式的行列式 [`math.stackexchange.com/questions/319321/understanding-the-leibniz-formula-for-determinants#:~:text=The%20formula%20says%20that%20det,permutation%20get%20a%20minus%20sign.&text=where%20the%20minus%20signs%20correspond%20to%20the%20odd%20permutations%20from%20above`](https://math.stackexchange.com/questions/319321/understanding-the-leibniz-formula-for-determinants#:~:text=The%20formula%20says%20that%20det,permutation%20get%20a%20minus%20sign.&text=where%20the%20minus%20signs%20correspond%20to%20the%20odd%20permutations%20from%20above).

[4] Youtube 视频：3B1B 关于行列式 [`www.youtube.com/watch?v=Ip3X9LOh2dk&t=295s`](https://www.youtube.com/watch?v=Ip3X9LOh2dk&t=295s)

[5] 连接莱布尼茨公式与几何 [`math.stackexchange.com/questions/593222/leibniz-formula-and-determinants`](https://math.stackexchange.com/questions/593222/leibniz-formula-and-determinants)

[6] Youtube 视频：莱布尼茨公式即面积： [`www.youtube.com/watch?v=9IswLDsEWFk`](https://www.youtube.com/watch?v=9IswLDsEWFk)

[7] Mathexchange 帖子：行列式的乘积等于乘积的行列式 [`math.stackexchange.com/questions/60284/how-to-show-that-detab-deta-detb`](https://math.stackexchange.com/questions/60284/how-to-show-that-detab-deta-detb)

[8] 行列式的历史背景： [`math.stackexchange.com/a/4782557/155881`](https://math.stackexchange.com/a/4782557/155881)
