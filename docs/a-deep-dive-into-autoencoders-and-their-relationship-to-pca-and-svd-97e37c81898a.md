# 深入探讨自编码器及其与 PCA 和 SVD 的关系

> 原文：[`towardsdatascience.com/a-deep-dive-into-autoencoders-and-their-relationship-to-pca-and-svd-97e37c81898a`](https://towardsdatascience.com/a-deep-dive-into-autoencoders-and-their-relationship-to-pca-and-svd-97e37c81898a)

## 对自编码器和降维的深入探索

[](https://reza-bagheri79.medium.com/?source=post_page-----97e37c81898a--------------------------------)![Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----97e37c81898a--------------------------------)[](https://towardsdatascience.com/?source=post_page-----97e37c81898a--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----97e37c81898a--------------------------------) [Reza Bagheri](https://reza-bagheri79.medium.com/?source=post_page-----97e37c81898a--------------------------------)

·发表于 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----97e37c81898a--------------------------------) ·阅读时间 46 分钟·2023 年 6 月 13 日

--

![](img/e2dacc0a1ecbcd18de0a94b4ba484e25.png)

作者提供的图片

自编码器是一种学习重构输入的神经网络。它由一个编码器网络和一个解码器网络组成，其中编码器将输入数据压缩到一个低维空间中，而解码器则从该空间重构输入数据。编码器和解码器被联合训练，以最小化输入数据及其重构之间的重构误差。

自编码器可以用于各种任务，例如数据压缩、去噪、特征提取、异常检测和生成建模。它们在计算机视觉、自然语言处理和语音识别等众多领域都有应用。自编码器还可以用于降维。实际上，自编码器的主要目的之一是学习输入数据的压缩表示，这可以作为一种降维形式。

在本文中，我们将讨论自编码器背后的数学原理，并了解它们如何进行降维。我们还将研究自编码器、主成分分析（PCA）和奇异值分解（SVD）之间的关系。我们还将展示如何在 Pytorch 中实现线性和非线性自编码器。

**自编码器架构**

图 1 显示了自编码器的架构。如前所述，自编码器学习重构其输入数据，因此输入层和输出层的大小总是相同的 (*n*)。由于自编码器学习自己的输入，因此不需要标记数据进行训练。因此，它是一种无监督学习算法。

那么，学习相同的输入数据有什么意义呢？如你所见，这种架构中的隐藏层呈双侧漏斗状，层间的神经元数量从第一个隐藏层开始逐渐减少，到达称为*瓶颈层*的层时达到最小值。在瓶颈层之后，神经元数量再次增加，直到到达输出层，与输入层的神经元数量相等。值得注意的是，瓶颈层的神经元数量少于*n*。

![](img/4907f5017e45106718d028137958095e.png)

图 1（图像由[`alexlenail.me/NN-SVG/`](https://alexlenail.me/NN-SVG/)生成）

在神经网络中，每一层都学习输入空间的抽象表示，因此瓶颈层确实是信息在输入层和输出层之间传递的瓶颈。这一层学习输入数据的最紧凑表示，并且还学习提取输入数据的最重要特征。这些新特征（也称为*潜变量*）是将输入数据点转换为连续低维空间的结果。实际上，潜变量可以以更简单的方式描述或解释输入数据。瓶颈层中神经元的输出表示这些潜变量的值。

瓶颈层的存在是这种架构的关键特征。如果网络中的所有层都具有相同数量的神经元，网络可以通过将所有输入数据值传递通过网络轻松地记住这些值。

自编码器可以分为两个网络：

1.  编码网络：它从输入层开始，到达瓶颈层结束。它将高维输入数据转换为由潜变量形成的低维空间。瓶颈层中神经元的输出表示这些潜变量的值。

1.  解码网络：它从瓶颈层开始，到达输出层结束。它接收来自瓶颈层的低维潜变量的值，并从这些值中重建原始的高维输入数据。

在本文中，我们将讨论自编码器与 PCA 之间的相似性。为了理解 PCA，我们需要一些线性代数的概念。因此，我们首先回顾一下线性代数。

**线性代数复习：基，维度和秩**

一组向量 {***v***₁, ***v***₂, …, ***v_****n*} 形成一个 *基*，如果它们是线性无关的并且张成 *V*。如果一组向量是线性无关的，那么该组中的任何向量都不能表示为其他向量的线性组合。一组向量 {***v***₁, ***v***₂, …, ***v_****n*} *张成* 一个向量空间，如果该空间中的每个其他向量都可以表示为这一组向量的线性组合。因此，任何 *V* 中的向量 ***x*** 可以写作：

![](img/db318fb0276021f90b866bc053913712.png)

其中 *a*₁, *a*₂, …, *a_n* 是一些常数。向量空间 *V* 可以有许多不同的向量基，但每个基总是有相同数量的向量。一个向量空间的基中的向量数量被称为该向量空间的 *维度*。当所有向量都是标准化的（标准化向量的长度为 1）并且正交（相互垂直）时，基 {***v***₁, ***v***₂, …, ***v_****n*} 是 *正交标准* 的。在欧几里得空间 R² 中，向量：

![](img/aba4a7dc633c5c17110785ce2f863767.png)

形成一个被称为 *标准基* 的正交标准基。它们是线性无关的，并且张成 R² 中的任意向量。由于基中只有两个向量，所以 R² 的维度是 2。如果我们有另一对线性无关且张成 R² 的向量，这对向量也可以作为 R² 的基。例如

![](img/15b0927abd9a5b6c792696ae2e0b57d9.png)

也是一个基，但不是一个正交标准基，因为这些向量不正交。更一般地，我们可以将 R^*n* 的标准基定义为：

![](img/5011ff5e783449a9fbeba3f9dbc0ea6b.png)

其中在 ***e****ᵢ* 中，第 *i* 个元素是 1，其他所有元素都是 0。

让向量集 *B*={***v***₁, ***v***₂, …, ***v_****n*} 形成一个向量空间的基，那么我们可以用基向量表示该空间中的任何向量 ***x***：

![](img/cf613598f4dcc27482a05a11e50d7c0c.png)

因此，***x*** 相对于这个基 *B* 的坐标可以写作：

![](img/4af591b9909d5d6388181efcfe4b2b5b.png)

实际上，当我们像这样定义 R² 中的一个向量

![](img/c34ab85e5a4a3a9b4bd00d1fef939b37.png)

这个向量的元素就是它相对于标准基的坐标：

![](img/d6feba6ba63ed5ead1d5eae1a14a57c2.png)

我们可以轻松找到一个向量相对于另一个基的坐标。假设我们有一个向量：

![](img/db318fb0276021f90b866bc053913712.png)

其中 *B*={***v***₁, ***v***₂, …, ***v_****n*} 是一个基。现在我们可以写作：

![](img/9a6f82324a4c29dfedfd1fff32421c02.png)

这里的 *P_****B*** 被称为 *坐标变换矩阵*，它的列是基 *B* 中的向量。因此，如果我们知道 ***x*** 相对于基 *B* 的坐标，我们可以使用方程 1 计算它相对于标准基的坐标。图 2 显示了一个示例。这里的 *B*={***v***₁, ***v***₂} 是 R² 的一个基。向量 ***x*** 被定义为：

![](img/532801f6aef630fb00b44fcaa8a66c86.png)

并且 ***x*** 相对于 *B* 的坐标是：

![](img/23faa3c12fc93d93f8eac85e09dfb754.png)

所以，我们有：

![](img/b0e0376e15b3434662492f52dc0bd565.png)![](img/e162172c918a6b69b0bd05fd42c8d37c.png)

图 2（作者提供的图片）

矩阵 ***A*** 的 *列空间*（也写作 *Col* ***A***) 是 ***A*** 列的所有线性组合的集合。假设我们用向量 ***a***₁*,* ***a***₂*, …* ***a_****n* 表示矩阵 ***A*** 的列。现在对于任何像 ***u*** 的向量，***Au*** 可以写作：

![](img/90092a0420df92cf83227923f87e8451.png)

因此，***Au*** 是 ***A*** 的列的线性组合，而 ***A*** 的列空间是可以表示为 ***Au*** 的向量集合。

矩阵 ***A*** 的 *行空间* 是 ***A*** 行的所有线性组合的集合。假设我们用向量 ***a***₁*ᵀ,* ***a***₂*ᵀ, …* ***a_m***ᵀ 表示矩阵 ***A*** 的行：

![](img/511b37c1e3dd944d0b35cda3431a7215.png)

***A*** 的行空间是所有可以表示为

![](img/3dadba71b02602c9ff99a4a89f08a76e.png)

*Col* ***A*** 的基向量数量或 *Col* ***A*** 的维度称为 ***A*** 的 *秩*。***A*** 的秩也是 ***A*** 中线性无关列的最大数量。还可以证明，矩阵 ***A*** 的秩等于其行空间的维度，并且同样等于 ***A*** 中线性无关行的最大数量。因此，矩阵的秩不能超过其行数或列数。例如，对于一个 *m*×*n* 矩阵，秩不能大于 *min*(*m*, *n*)。

**PCA：综述**

主成分分析（PCA）是一种线性技术。它找出数据中捕获最多变化的方向，然后将数据投影到这些方向所张成的低维子空间上。PCA 是一种广泛使用的数据降维方法。

PCA 将数据转换为一个新的正交坐标系统。该坐标系统的选择使得投影到第一个坐标轴（称为 *第一个主成分*）的数据点的方差最大化。投影到第二个坐标轴（称为 *第二个主成分*）的数据点的方差在所有与第一个主成分正交的方向中最大化，通常地，投影到每个坐标轴的数据点的方差在所有与前面的主成分正交的方向中最大化。

假设我们有一个具有 *n* 特征和 *m* 数据点或观测值的数据集。我们可以使用 *m*×*n* 矩阵

![](img/cc1fb90e7351c571c80cb2c965c7bbc0.png)

为了表示这个数据集，我们称之为*设计矩阵*。因此，***X***的每一行代表一个数据点，每一列代表一个特征。我们还可以将***X***写作

![](img/fde5b00c82eb7f34ca0f0dcdf5784cf9.png)

其中每列向量

![](img/a72a2e1746940e875e7ac28760e9db48.png)

表示这个数据集中的一个观察值（或数据点）。因此，我们可以将数据集视为 R^*n* 中的 *m* 个向量集合。图 3 显示了 *n*=2 的示例。在这里，我们可以将每个观察值绘制为 R² 中的一个向量（或简单的点）。

![](img/540fe5a0703eceb257a050e9b05f84e6.png)

图 3（作者提供的图像）

设 ***u*** 为单位向量，则我们有：

![](img/8722f77d7f569a2133cb4f2c2cb4ffbb.png)

每个数据点 ***x****ᵢ* 在向量 ***u*** 上的标量投影是：

![](img/1654984871834e088c00938210f0f952.png)

图 4 显示了 *n*=2 的示例。

![](img/027ece092c781babe4bc9a0817829d18.png)

图 4（作者提供的图像）

我们用来表示 ***X*** 每列的均值

![](img/25eb7d0a8b84a7ff9a9330bc5c8b0063.png)

那么数据集的均值定义为：

![](img/422418c453d73783aa4d2d885870fde6.png)

我们也可以将其写作：

![](img/9411fd8c91233df7d47dba76e21021e6.png)

现在这些投影数据点的方差定义为：

![](img/d93b0dc5f5774ad6402d7df482912cca.png)

方程 1 可以进一步简化。术语

![](img/f85decc86bbf3fc7fff666709bdcd059.png)

是一个标量（因为 a 的结果是一个标量量）。此外，我们知道标量量的转置等于其自身。因此，我们可以得到

![](img/6341d8b4cfedea591b32884141ca8350.png)

因此，将数据点在 ***X*** 上投影到向量 ***u*** 的方差可以写作

![](img/1a88c83520a48d9dc6260ddc81d7363c.png)

其中

![](img/887cc3c1ec0efd9c12837d65b16b6c4a.png)

被称为*协方差矩阵*（图 5）。

![](img/f171668a215c6391e2fb65e940551868.png)

图 5（作者提供的图像）

通过简化方程 5，可以显示协方差矩阵可以写作：

![](img/b76dc95b2513e57ce0a0698969700db8.png)

其中

![](img/3248a08844ca898b822eaba23cb6674a.png)

其中 *xᵢ*,*ₖ* 是设计矩阵 ***X*** 的 (*i*, *k*) 元素（或简单地说是向量 ***x****ᵢ* 的 *k*th 元素）。

对于具有 *n* 个特征的数据集，协方差矩阵是一个 *n*×*n* 矩阵。此外，基于方程 6 中 *Sᵢ*,*ⱼ* 的定义，我们有：

![](img/fd83b0d5fd5802a8ce940981b60dded5.png)

因此，其 (*i*, *j*) 元素等于其 (*j*, *i*) 元素，这意味着协方差矩阵是对称矩阵，并且等于其转置：

![](img/3b87633cc808fb26dfe5e3b711a7b13c.png)

现在我们找到向量 ***u***₁ 使其最大化

![](img/bd08d75370f655c15fcd079acd0d3ff2.png)

由于 ***u***₁ 是一个标准化向量，我们将此约束添加到优化问题中：

![](img/f321e5ecb2216372fcff949fee64f154.png)

我们可以通过添加拉格朗日乘数*λ*₁并最大化来解决这个优化问题

![](img/766d1bb6ffa1658bee11a6f5c045403f.png)

为此，我们将该项对***u***₁的导数设为零：

![](img/f99e5827faa23cec7a2e0f0588de77fe.png)

我们得到：

![](img/d29b71d631f11881e01bfa08d8480ac4.png)

这意味着***u***₁是协方差矩阵***S***的特征向量，*λ*₁是其对应的特征值。我们称特征向量***u***₁为第一个*主成分*。接下来，我们要找到单位向量***u***₂，使其在所有与第一个主成分正交的方向中最大化***u***₂ᵀ***Su***₂。因此，我们需要找到在这些约束条件下使***u***₂*ᵀ****Su***₂最大化的向量***u***₂：

![](img/d3039d4fa2822fd4d3b7b1bb0c8c3e00.png)![](img/a4032df1626261724645ff5f3e98e062.png)

可以证明***u***₂ 是这个方程的解：

![](img/e587a287535f41ea34777ccdfd6430a4.png)

所以我们得出结论，***u***₂也是***S***的特征向量，*λ*₂ 是其对应的特征值（证明见附录）。更一般地，我们希望找到单位向量***u****ᵢ*，使其在所有与先前主成分***u***₁…***u****i*-1 正交的方向中最大化***u****ᵢᵀ****Su****ᵢ*。因此，我们需要找到使得

![](img/278b15c978542f6ee4700b08847045c1.png)

在这些约束条件下：

![](img/c53a1113e808d0bd90ad22f90a4bece1.png)![](img/6560b5efee8d1c3c3d95b35f0eef3164.png)

再次可以证明，***u****ᵢ* 是这个方程的解

![](img/4b494b69d5107c9b88a7a642cd40fe0a.png)

因此，***u****ᵢ* 是***S***的特征向量，*λᵢ* 是其对应的特征值（证明见附录）。向量***u****ᵢ* 称为第*i*个主成分。如果我们将前面的方程乘以***u****ⱼ*ᵀ，得到：

![](img/c0d5bf7cbc1f6b08e842935eadb4fd5c.png)

因此，我们得出结论，数据点在***X***中沿着特征向量***u****ᵢ*的标量投影的方差等于其对应的特征值。

如果我们有一个具有*n*特征的数据集，那么协方差矩阵将是一个*n*×*n*的对称矩阵。这里每个数据点可以表示为 R^*n*中的一个向量（***x****ᵢ*）。如前所述，R^*n*中向量的元素给出了相对于标准基的坐标。

可以证明一个*n*×*n*的对称矩阵有*n*个实特征值，以及*n*个线性无关且正交的对应特征向量（谱定理）。这*n*个正交特征向量是这个数据集的主成分。可以证明一组*n*个正交向量可以形成 R^n*的一个基。因此，这些主成分形成了一个正交基，可以用来为数据点定义一个新的坐标系统（图 6）。

我们可以很容易地计算每个数据点***x****ᵢ*相对于这个新坐标系统的坐标。令*B*={***v***₁, ***v***₂, …, ***v_****n*}为主成分的集合。我们首先将***x****ᵢ*用基向量表示：

![](img/58877e06237f1b6ba7fa58406fb555c0.png)

现在如果我们将这个方程的两边都乘以***v****ᵢᵀ*，我们有：

![](img/6c184280a7f4314af45ee3b0c942e440.png)

由于我们有一个正交基：

![](img/23fd9936ef40e5d49304bff8a44316d6.png)

因此，得出：

![](img/d53da74f67ce7a9f40edd6c3cb25dadc.png)

由于点积是交换律的，我们也可以写作：

![](img/4b34900c0ff768f400ee756513f39c41.png)

因此，***x****ᵢ*相对于*B*的坐标为：

![](img/8c8a81a42068a2cf2ad932bc17687112.png)

设计矩阵可以写作

![](img/292866b1f22920a345aa921f44ea684e.png)

在新的坐标系统中。这里每行表示新坐标系统中的一个数据点（观察）。图 6 展示了*n*=2 的一个示例。

![](img/4dd6cac6f24eb2d68468cbf0690050f3.png)

图 6（作者提供的图片）

数据点在每个特征向量（主成分）上的标量投影的方差等于其对应的特征值。第一个主成分具有最大的特征值（方差）。第二个主成分具有第二大的特征值，以此类推。现在我们可以选择前*d*个主成分，并将原始数据点投影到由它们张成的子空间中。

因此，我们将原始数据点（具有*n*特征）转换为这些投影的数据点，这些数据点属于一个*d*维子空间。通过这种方式，我们将原始数据集的维度从*n*减少到*d*，同时最大化投影数据的方差。现在，方程 9 中的前*d*列给出了投影数据点的坐标：

![](img/378ef9b400c36f6f6987fcbd0d6d0deb.png)

图 7 给出了这种转换的一个示例。原始数据集具有 3 个特征（*n*=3），我们通过将数据点投影到由前两个主成分（***v***₁, ***v***₂）形成的平面上，将其维度减少到*d*=2。子空间中每个数据点***x****ᵢ*的坐标为：

![](img/188092dd8e69f772385f8e4391487248.png)![](img/8f8509c9df30f70be8dde9cd08953bad.png)

图 7（作者提供的图片）

在进行 PCA 分析之前，通常会将数据集以零为中心。为此，我们首先创建表示我们数据集的设计矩阵***X***（方程 2）。然后通过从每列的元素中减去每列的均值来创建一个新矩阵***Y***。

![](img/24e96abb3bbcca8d75d8878acebc6497.png)

矩阵***Y***代表中心化的数据集。在这个新矩阵中，每列的均值为零：

![](img/022b51899c9497d6e7fa1aafb522612e.png)

因此，数据集的均值也为零：

![](img/806a53877eb163ea345cb632f785cd39.png)

现在假设我们从一个中心化的设计矩阵***X***开始，并希望计算其协方差矩阵。因此，***X***的每列的均值为零。从方程 6 我们有：

![](img/f91326b63332fb300aa99087bb2951cd.png)

其中[***X***]_*k*,*j* 表示矩阵***X***的（*k*, *j*）元素。通过使用矩阵乘法的定义，我们得到

![](img/b11bfdeeaafd7ee26e5c9dcaf91f5744.png)

请注意，这个方程仅在设计矩阵（***X***）是中心化时有效。

**PCA 与奇异值分解（SVD）的关系**

假设***A***是一个*m*×*n*的矩阵。那么***A****ᵀ****A***将是一个*n*×*n*的方阵，并且可以很容易地证明它是对称矩阵。由于***A****ᵀ****A***是对称的，它有*n*个实特征值和*n*个线性独立且正交的特征向量（谱定理）。我们称这些特征向量为***v***₁, ***v***₂, …, ***v_****n*，并假设它们是标准化的。可以证明***A****ᵀ****A***的特征值都是正的。现在假设我们按降序标记它们，即：

![](img/b653a829442c2fe625796d745402f2a2.png)

设***v***₁, ***v***₂, …, ***v_****n* 是***A****ᵀ****A***的特征向量，分别对应这些特征值。我们定义矩阵***A***的*奇异值*（记作*σᵢ*）为*λᵢ*的平方根。因此，我们有

![](img/ff02620b0ea15a6a74fb51d8587c2a97.png)

现在假设***A***的秩为*r*。那么可以证明，***A****ᵀ****A***的非零特征值的数量或***A***的非零奇异值的数量是*r*：

![](img/143d9c3615dfd4d0585e04fcdff0c9be.png)

现在***A***的奇异值分解（SVD）可以写成

![](img/fbd608f5637d92e23176f9ddb0660c50.png)

这里***V***是一个*n×n*的矩阵，其列为***v****ᵢ*：

![](img/3ba305e512e8d72055941aac9e8a6a4d.png)

***Σ***是一个*m*×*n*的对角矩阵，且所有元素均为零，除了前*r*个对角元素，这些元素等于***A***的奇异值。我们定义矩阵***U***为

![](img/d3e04330a9b43747a5052a8c263f43a9.png)

我们定义***u***₁ *到* ***u_****r* 为

![](img/725763b1db2caf57b49bc415976051bd.png)

我们可以很容易地证明这些向量是正交的：

![](img/3978641faa43dfb1eb106c4bb938252b.png)

这里我们使用了***v_****j* 是***A****ᵀ****A***的特征向量，并且这些特征向量是正交的。由于这些向量是正交的，它们也是线性无关的。其他的***u****ᵢ* 向量（*r<i≤m*）被定义为***u***₁, ***u***₂, *…****u****_m* 构成一个*m*维向量空间（***R^****m*）的基。

设***X***为一个中心化的设计矩阵，其 SVD 分解如下：

![](img/45893a365216f8687be4a61093e2df13.png)

如前所述，***v***₁, ***v***₂, …, ***v_****n* 是 ***X****ᵀ****X*** 的特征向量，而奇异值是其对应特征值的平方根。因此，我们有：

![](img/48ff2947d2be0fb1975b5b1bfd2cccf0.png)

现在我们可以将前一个方程的两边除以 *m*（其中 *m* 是数据点的数量），并使用方程 10，得到：

![](img/c15bc382793ef750e30828f1b43f4d9b.png)

因此，***v****ᵢ* 是协方差矩阵的特征向量，其对应的特征值是其对应奇异值的平方除以 *m*。所以，SVD 方程中的矩阵 ***V*** 给出了 ***X*** 的主要成分，并且使用 ***Σ*** 中的奇异值，我们可以轻松计算特征值。总之，我们可以使用 SVD 来进行 PCA。

让我们看看从 SVD 方程中还能得到什么。我们可以使用方程 3 和方程 11 来简化方程 12 中的 ***UΣ***：

![](img/d6e5bee059cbe2770e7184e384ef8511.png)

与方程 9 相比，我们可以得出结论，***UΣ*** 的第 *i* 行给出了数据点 ***x****ᵢ* 相对于主要成分定义的基的坐标。

现在假设在方程 12 中，我们只保留 ***U*** 的前 *k* 列、***V*** 的前 *k* 行以及 ***Σ*** 的前 *k* 行和列。如果我们将它们相乘，我们得到：

![](img/4dbed1e1d397ef36fee631b4a2e0afe4.png)

请注意，***X****ₖ* 仍然是一个 *m*×*n* 矩阵。如果我们将 ***X****ₖ* 乘以具有 *n* 个元素的向量 *b*，我们得到：

![](img/7e21e89f86125daf2b55e20688ee43d9.png)

其中 [***Cb***]*ᵢ* 是向量 ***Cb*** 的 *i* 项元素。由于 ***u***₁, ***u***₂, …, ***u****ₖ* 是线性无关的向量（记住它们构成一个基，因此应该是线性无关的），并且它们生成了 ***X****ₖ****b***，我们可以得出结论，它们构成了 ***X****ₖ****b*** 的基。这一基有 *k* 个向量，所以 ***X****ₖ* 的列空间维度是 k。因此，***X****ₖ* 是一个秩为 *k* 的矩阵。

但 ***X****ₖ* 代表什么呢？使用方程 13，我们可以写成：

![](img/e2e3b0efa44486b437999d136176e6c5.png)

所以，***X****ₖ* 的第 *i* 行是以下的转置：

![](img/dd908a370b8d3585568465ace026a906.png)

这是数据点 ***x****ᵢ* 在由主要成分 ***v***₁, ***v***₂, … ***v****ₖ* 张成的子空间上的向量投影。记住，***v***₁, ***v***₂, … ***v_****n* 是我们原始数据集的基。此外，***x****ᵢ* 相对于这一基的坐标为：

![](img/83a7881c292c9664ba48d9a5a68b9498.png)

因此，使用方程 1，我们可以将 ***x****ᵢ* 写成：

![](img/9ec4d7f941048378697084d99b930b04.png)

现在我们可以将 ***x****ᵢ* 分解为两个向量。一个在由 ***v***₁, ***v***₂, … ***v****ₖ* 定义的子空间中，另一个在由剩余向量定义的子空间中。

![](img/1a717a39ffb75adaf876cce9dbdeb5ab.png)

第一个向量是 ***x****ᵢ* 在由 ***v***₁、***v***₂、… ***v****ₖ* 定义的子空间上的投影结果，等于 ***x̃****ᵢ*。

记住，设计矩阵 ***X*** 的每一行代表一个原始数据点。类似地，***X****ₖ* 的每一行代表同一数据点在由主成分 ***v***₁、***v***₂、… ***v****ₖ* 张成的子空间上的投影（图 8）。

![](img/611c8e23b710a72bc5afd26eec12fc31.png)

图 8（作者提供的图片）

现在我们可以计算原始数据点 (***x****ᵢ*) 和投影数据点 (***x̃****ᵢ*) 之间的距离。向量 ***x****ᵢ* 和 *x̃ᵢ* 之间的距离平方为：

![](img/905c89fec77def9e1672fdc68c8b2811.png)

如果我们对所有数据点的距离平方进行累加，我们得到：

![](img/81d5352479dd7bafd9075b0536cbff71.png)

一个 *m*×*n* 矩阵 ***C*** 的弗罗贝尼乌斯范数定义为：

![](img/b6cad328dbf3f941728bd5567e308bf1.png)

由于向量 ***x****ᵢ* 和 ***x̃****ᵢ* 是矩阵 ***X*** 和 ***X****ₖ* 行的转置，我们可以写出：

![](img/09324c163ac9fd6f16a27fab93350e3c.png)

因此，***X***-***X****ₖ* 的弗罗贝尼乌斯范数与原始数据点和投影数据点之间距离的平方和成正比（图 9），而当投影数据点越来越接近原始数据点时，|| ***X***-***X****ₖ* ||_*F* 会减少。

![](img/12d5b906b1d56e135a990293b0bb12bc.png)

图 9（作者提供的图片）

我们希望投影点能很好地近似原始数据点，因此我们希望 ***X****ₖ* 在所有秩为 *k* 的矩阵中，***X***-***X****ₖ* 的值最小。

假设我们有一个秩为 *r* 的 *m*×*n* 矩阵 ***X***，并且 ***X*** 的奇异值已排序，则我们有：

![](img/a3ec7a72e61d91be3bbfd91796432947.png)

可以证明，***X****ₖ* 在所有秩为 *k* 的 *m*×*n* 矩阵 ***A*** 中，最小化了 ***X***-***A*** 的弗罗贝尼乌斯范数。从数学角度来看：

![](img/b8a35344f08fbc667dc89e8e9250e38d.png)

***X****ₖ* 是所有秩为 *k* 的矩阵中与 ***X*** 最近的矩阵，可以被视为设计矩阵 ***X*** 的最佳秩为 *k* 近似。这也意味着，投影数据点由 ***X****ₖ* 表示，是原始数据点由 ***X*** 表示的秩为 *k* 的最佳近似（在总误差方面）。

现在我们可以尝试以不同的格式书写之前的方程。假设 ***Z*** 是一个 *m*×*k* 矩阵，***W*** 是一个 *k*×*n* 矩阵。我们可以证明：

![](img/5e3d088cd09f112d82971634ff31071e.png)

因此，找到一个秩为 *k* 的矩阵 ***A***，使 ||***X***-***A***||_*F* 最小化，相当于找到矩阵 ***Z*** 和 ***W***，使 ||***X***-***ZW***||_*F* 最小化（证明见附录）。因此，我们可以写成：

![](img/68062afc6044b6b2b97b0d59a57cd8be.png)

其中 ***Z****（*an*m*×*k* 矩阵**）和 ***W****（*k*×*n* 矩阵）是最小化问题的解，我们有

![](img/fe87575101ce799d0a2a4670fc20d711.png)

现在，根据方程 13 和 14，我们可以写出：

![](img/7a81721d1659060c1046f7bbe16074ef.png)

因此，如果我们使用 SVD 求解方程 18 中的最小化问题，我们得到以下 ***Z**** 和 ***W**** 的值：

![](img/0d4732d15e6efd52c2d5193abb75cdd9.png)

和

![](img/737d7836d4dd78fb820bc97e83b1ef9a.png)

***W**** 的行给出了主成分的转置，***Z**** 的行给出了相对于这些主成分形成的基的每个投影数据点的坐标的转置。重要的是，主成分形成了一个正交归一基（因此主成分既是归一化的，又是正交的）。实际上，我们可以假设 PCA 仅仅是寻找一个矩阵 ***W***，其中行形成一个正交归一集合。我们知道当两个向量正交时，它们的内积为零，因此我们可以说 PCA（或 SVD）解决了最小化问题

![](img/ed824011dba0506e4e91eb96bc23170a.png)

具有以下约束：

![](img/2010d212356df33524845684a4065c3c.png)

其中 ***Z*** 和 ***W*** 是 *m*×*k* 和 *k*×*n* 矩阵。此外，如果 ***Z**** 和 ***W**** 是最小化问题的解，那么我们有

![](img/8fa2fe8a2c73fb62a2e832d2f4496c97.png)

这个公式非常重要，因为它使我们能够建立 PCA 和自编码器之间的联系。

**PCA 与自编码器之间的关系**

我们从一个只有三层的自编码器开始，如图 10 所示。这个网络有 *n* 个输入特征，表示为 *x*₁…*x_n*，以及 *n* 个输出层神经元。网络的输出表示为 *x^*₁…*x^_n*。隐藏层有 *k* 个神经元（其中 *k*<*n*），隐藏层的输出表示为 *z*₁…*zₖ*。矩阵 ***W^***[1] 和 ***W^***[2] 分别包含隐藏层和输出层的权重。

![](img/eb4da49a74bf4d860aade53b7a261851.png)

图 10（作者提供的图片）

在这里

![](img/8d5705a4a14d59a9895717a0ebf61b53.png)

表示第 *i* 个神经元在第 *l* 层的输入 *j* 的权重（来自第 *j* 个神经元在第 *l*-1 层）（图 11）。在这里我们假设隐藏层的 *l*=1，输出层的 *l*=2。

![](img/65947050c4fea7c4ce5d13f9d325969f.png)

图 11（作者提供的图片）

因此，隐藏层的权重由下式给出：

![](img/83dfe42f7a7be7e122be9c825b95e777.png)

该矩阵的第 *i* 行给出了隐藏层中第 *i* 个神经元的所有权重。同样，输出层的权重由下式给出：

![](img/56d04d8a1865ed2a982c479f9fc47190.png)

每个神经元都有一个激活函数。我们可以使用权重矩阵 ***W^***[1] 和输入特征计算隐藏层中神经元的输出（该神经元的激活）：

![](img/be59678a64fe58bbd5932904528473d4.png)

其中*bᵢ*^[1]是第*i*个神经元的偏置，*g^*[1]是第 1 层神经元的激活函数。我们可以将这个方程写成向量化形式：

![](img/49e5878cdf4fde3b10e9af434a218edd.png)

其中***b***是偏置向量：

![](img/856de7aeb3d175f99f9f0a0cad8df56a.png)

而***x***是输入特征的向量：

![](img/7e9d70a3cde486a3c677d0d63e293a26.png)

同样地，我们可以将输出层的第*i*个神经元的输出写成：

![](img/caba3c1cc8acffa518f1562bdc2ca89a.png)

在向量化形式中，它变为：

![](img/3f3e1ca2f0386c6ae1417cc6b69e236b.png)

现在假设我们使用以下设计矩阵作为训练数据集来训练这个网络：

![](img/102d9c1989fffb688deed2146a811664.png)

因此，训练数据集有*m*个观察值（样本）和*n*个特征。记住，第*i*个观察值由向量表示

![](img/a72a2e1746940e875e7ac28760e9db48.png)

如果我们将这个向量输入到网络中，网络的输出由这个向量表示：

![](img/9f89d6b7d3a2633ca974cef6928096aa.png)

我们还需要做以下假设，以确保自编码器模拟 PCA：

1-训练数据集是中心化的，因此***X***的每列均值为零：

![](img/58a30a9a00cf4eca4e2c7c8cf5e4a89a.png)

2-隐藏层和输出层的激活函数是线性的，所有神经元的偏置为零。这意味着我们在这个网络中使用的是线性编码器和解码器。因此，我们有：

![](img/d7a85cf23b8490b901dca850ba990b23.png)

3-我们使用二次损失函数来训练这个网络。因此，成本函数是均方误差（MSE），定义为：

![](img/01e8dd9cb447ce0c92498f999b8ec1f1.png)

现在我们可以证明：

![](img/63545cbdfcbc8291f5d52a47c4b7a9c3.png)

其中***Z***定义为：

![](img/8050e399e609f7ab6f1ed1c4f24b4c75.png)

证明在附录中给出。这里***Z***的第*i*行表示当第*i*个观察值输入网络时隐藏层的输出。因此，最小化这个网络的成本函数等同于最小化：

![](img/24ec93b6748632ed86cadeb764d4caa4.png)

其中我们定义矩阵***W***为

![](img/b59158741e371db665d931bf3cff17cc.png)

请注意，***W***^[2]的每一行是***W***的一列。

我们知道，如果用一个正的乘数乘以一个函数，其最小值不会改变。因此，我们可以在最小化成本函数时去掉乘数 1/(2*m*)。因此，通过训练这个网络，我们解决了这个最小化问题：

![](img/ccd9f3f42aede7a1eb82f57de3006ff2.png)

其中***Z***和***W***分别是*m*×*k*和*k*×*n*矩阵。如果我们将这个方程与方程 20 进行比较，我们会发现它是 PCA 的相同最小化问题。因此，解决方案应该与方程 20 的结果相同：

![](img/e5ebe99fe7fd2781001281dbe68c25d3.png)

然而，这里有一个重要的区别。方程 21 的约束没有在这里应用。那么问题来了。自动编码器找到的 ***Z*** 和 ***W*** 的最优值是否与 PCA 的相同？***W*** 的行是否总是应该形成正交集？

首先，让我们扩展之前的方程。

![](img/cc3ed899728203115fdd29dfa14b03d9.png)

我们知道，***X***ₖ* 的 *i* 行是以下内容的转置：

![](img/dd908a370b8d3585568465ace026a906.png)

这是数据点 ***x***ᵢ* 在由主成分 ***v***₁、***v***₂、… ***v***ₖ* 张成的子空间上的向量投影。因此，***x̃***ᵢ* 属于 *k* 维子空间。向量 ***w***₁、***w***₂、… ***w***ₖ* 应该是线性无关的。否则，***W*** 的秩将小于 *k*（记住 ***W*** 的秩等于 ***W*** 的最大线性无关行数），根据方程 A.3，***X***ₖ* 的秩也将小于 *k*。可以证明，一组 *k* 个线性无关的向量形成 *k* 维子空间的基底。因此，我们得出结论，向量 ***w***₁、***w***₂、… ***w***ₖ* 也形成与主成分所张成的相同子空间的基底。我们现在可以使用方程 24 将 ***X***ₖ* 的 *i* 行表示为向量 ***w***₁、***w***₂、… ***w***ₖ* 的线性组合。

![](img/40b2efbe3ad938905d36a9e1d0ba3365.png)

这意味着 ***Z*** 的 *i* 行仅给出相对于由向量 ***w***₁、***w***₂、… ***w***ₖ* 形成的基底的 ***x̃***ᵢ* 的坐标。图 12 显示了 *k*=2 的示例。

![](img/53f5c77da04bb7960484954ff71792f5.png)

图 12（作者提供的图像）

总结来说，自动编码器找到的矩阵 ***Z*** 和 ***W*** 可以生成与主成分所张成的相同子空间。由于以下原因，我们也得到了 PCA 的相同投影数据点：

![](img/f4c6afe30370b94f94e96e5dae1a361a.png)

然而，这些矩阵定义了该子空间的新基底。与 PCA 找到的主成分不同，这个新基底的向量不一定是正交的。***W*** 的行给出了新基底向量的转置，而 ***Z*** 的行给出了相对于该基底的每个数据点坐标的转置。

因此，我们得出结论，线性自编码器不能找到主成分，但它可以使用不同的基找到由主成分张成的子空间。这里有一个例外……假设我们只想保留第一个主成分***v***₁。因此，我们希望将原始数据集的维度从*n*减少到 1。在这种情况下，子空间只是由第一个主成分张成的直线。线性自编码器也会找到同一条直线，但使用不同的基向量***w***₁。这个基向量不一定是标准化的，可能具有与***v***₁相反的方向，但它仍然在同一条直线上（子空间）。这在图 13 中得到了证明。现在，如果我们将***w***₁标准化，我们将得到数据集的第一个主成分。因此，在这种情况下，线性自编码器能够间接地获得第一个主成分。

![](img/f85ad79cdd64767fceae0572aef5e0f0.png)

图 13

到目前为止，我们讨论了自编码器和 PCA 的基础理论。现在让我们看一个 Python 示例。在下一部分，我们将使用 Pytorch 创建一个自编码器，并与 PCA 进行比较。

**案例研究：PCA 与自编码器**

我们首先需要创建一个数据集。清单 1 创建了一个具有 3 个特征的简单数据集。前两个特征（*x*₁和*x*₂）具有二维多元正态分布，第 3 个特征（*x*₃）等于*x*₂的一半。这个数据集存储在数组`X`中，充当设计矩阵。我们还对设计矩阵进行了中心化处理。

```py
# Listing 1
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.decomposition import PCA
from scipy.stats import multivariate_normal
import torch
import torch.nn as nn
from numpy import linalg as LA
from sklearn.preprocessing import MinMaxScaler
import random
%matplotlib inline

np.random.seed(1)
mu = [0, 0]
Sigma = [[1, 1],
         [1, 2.5]]

# X is the design matrix and each row of X is an example
X = np.random.multivariate_normal(mu, Sigma, 10000)
X = np.concatenate([X, X[:, 0].reshape(len(X), 1)], axis=1)
X[:, 2] = X[:, 1] / 2
X = (X - X.mean(axis=0))
x, y, z = X.T
```

清单 2 创建了这个数据集的 3d 图，结果如图 14 所示。

```py
# Listing 2
fig = plt.figure(figsize=(10, 10))
ax1 = fig.add_subplot(111, projection='3d')

ax1.scatter(x, y, z, color = 'blue')

ax1.view_init(20, 185)
ax1.set_xlabel("$x_1$", fontsize=20)
ax1.set_ylabel("$x_2$", fontsize=20)
ax1.set_zlabel("$x_3$", fontsize=20)
ax1.set_xlim([-5, 5])
ax1.set_ylim([-7, 7])
ax1.set_zlim([-4, 4])
plt.show()
```

![](img/bb3727796e4dbb6040fdcc8418487e49.png)

图 14

如你所见，这个数据集定义在由*x*₃= *x*₁/2 表示的平面上。现在我们开始 PCA 分析。

```py
pca = PCA(n_components=3)
pca.fit(X)
```

我们可以使用`components_`字段轻松获取主成分（`X`的特征向量）。它返回一个数组，每行表示一个主成分。

```py
# Each row gives one of the principal components (eigenvectors)
pca.components_
```

```py
array([[-0.38830581, -0.824242  , -0.412121  ],
       [-0.92153057,  0.34731128,  0.17365564],
       [ 0\.        , -0.4472136 ,  0.89442719]])
```

我们还可以通过`explained_variance_`字段查看对应的特征值。记住，数据点在特征向量***u***ᵢ上的标量投影的方差等于其对应的特征值。

```py
pca.explained_variance_
```

```py
array([3.64826952e+00, 5.13762062e-01, 3.20547162e-32])
```

请注意，特征值是按降序排列的。因此，`pca.components_`的第一行给出第一个主成分。清单 3 绘制了主成分与数据点的图（见图 15）。

```py
# Listing 3
v1 = pca.components_[0]
v2 = pca.components_[1]
v3 = pca.components_[2]

fig = plt.figure(figsize=(10, 10))
ax1 = fig.add_subplot(111, projection='3d')

ax1.scatter(x, y, z, color = 'blue', alpha= 0.1)
ax1.plot([0, v1[0]], [0, v1[1]], [0, v1[2]],
         color="black", zorder=6)
ax1.plot([0, v2[0]], [0, v2[1]], [0, v2[2]],
         color="black", zorder=6)
ax1.plot([0, v3[0]], [0, v3[1]], [0, v3[2]],
         color="black", zorder=6)

ax1.scatter(x, y, z, color = 'blue', alpha= 0.1)
ax1.plot([0, 7*v1[0]], [0, 7*v1[1]], [0, 7*v1[2]],
         color="gray", zorder=5)
ax1.plot([0, 5*v2[0]], [0, 5*v2[1]], [0, 5*v2[2]],
         color="gray", zorder=5)
ax1.plot([0, 3*v3[0]], [0, 3*v3[1]], [0, 3*v3[2]],
         color="gray", zorder=5)

ax1.text(v1[0], v1[1]-0.2, v1[2], "$\mathregular{v}_1$",
         fontsize=20, color='red', weight="bold",
         style="italic", zorder=9)
ax1.text(v2[0], v2[1]+1.3, v2[2], "$\mathregular{v}_2$",
         fontsize=20, color='red', weight="bold",
         style="italic", zorder=9)
ax1.text(v3[0], v3[1], v3[2], "$\mathregular{v}_3$", fontsize=20,
         color='red', weight="bold", style="italic", zorder=9)

ax1.view_init(20, 185)
ax1.set_xlabel("$x_1$", fontsize=20, zorder=2)
ax1.set_ylabel("$x_2$", fontsize=20)
ax1.set_zlabel("$x_3$", fontsize=20)
ax1.set_xlim([-5, 5])
ax1.set_ylim([-7, 7])
ax1.set_zlim([-4, 4])

plt.show()
```

![](img/35703b15aeb523219b54ca9ca4d2dd3d.png)

图 15

请注意，第 3 个特征值几乎为零。这是因为数据集位于二维平面上（*x*₃= *x*₁/2），如图 15 所示，它在***v***₃方向上没有方差。我们可以使用`transform()`方法获取每个数据点相对于主成分定义的新坐标系统的坐标。`transform()`返回的数组的每一行给出一个数据点的坐标。

```py
# Listing 4

# Z* = UΣ
pca.transform(X)
```

```py
([[ 3.09698570e+00, -3.75386182e-01, -2.06378618e-17],
       [-9.49162774e-01, -7.96300950e-01, -5.13280752e-18],
       [ 1.79290419e+00, -1.62352748e+00,  2.41135694e-18],
       ...,
       [ 2.14708946e+00, -6.35303400e-01,  4.34271577e-17],
       [ 1.25724271e+00,  1.76475781e+00, -1.18976523e-17],
       [ 1.64921984e+00, -3.71612351e-02, -5.03148111e-17]])
```

现在我们可以选择前 2 个主成分，并将原始数据点投影到它们所张成的子空间上。因此，我们将原始数据点（具有 *3* 个特征）转换为这些投影数据点，这些数据点属于二维子空间。为了做到这一点，我们只需删除由 `pca.transform(X)` 返回的数组的第 3 列。这意味着我们将原始数据集的维度从 3 降到 2，同时最大化投影数据的方差。列表 5 绘制了这个二维数据集，结果如图 16 所示。

```py
# Listing 5

fig = plt.figure(figsize=(8, 6))
plt.scatter(pca.transform(X)[:,0], pca.transform(X)[:,1])
plt.axis('equal')
plt.axhline(y=0, color='gray')
plt.axvline(x=0, color='gray')
plt.xlabel("$v_1$", fontsize=20)
plt.ylabel("$v_2$", fontsize=20)
plt.xlim([-8.5, 8.5])
plt.ylim([-4, 4])
plt.show()
```

![](img/c01c51bb119d4559c5986b30bad9652a.png)

图 16

我们也可以使用 SVD 获得相同的结果。列表 6 使用 `numpy` 中的 `svd()` 函数对 `X` 进行奇异值分解。

```py
# Listing 6

U, s, VT = LA.svd(X)
print("U=", np.round(U, 4))
print("Diagonal of elements of Σ=", np.round(s, 4))
print("V^T=", np.round(VT, 4))
```

```py
U= [[ 1.620e-02 -5.200e-03  1.130e-02 ... -2.800e-03 -2.100e-02 -6.200e-03]
 [-5.000e-03 -1.110e-02  9.895e-01 ...  1.500e-03 -3.000e-04  1.100e-03]
 [ 9.400e-03 -2.270e-02  5.000e-04 ... -1.570e-02  1.510e-02 -7.100e-03]
 ...
 [ 1.120e-02 -8.900e-03 -1.800e-03 ...  9.998e-01  2.000e-04 -1.000e-04]
 [ 6.600e-03  2.460e-02  1.100e-03 ...  1.000e-04  9.993e-01 -0.000e+00]
 [ 8.600e-03 -5.000e-04 -1.100e-03 ... -1.000e-04 -0.000e+00  9.999e-01]]
Diagonal of elements of Σ= [190.9949  71.6736   0\.    ]
V^T= [[-0.3883 -0.8242 -0.4121]
 [-0.9215  0.3473  0.1737]
 [ 0\.     -0.4472  0.8944]]
```

这个函数返回矩阵 ***U*** 和 ***V****ᵀ* 以及 ***Σ*** 的对角元素（记住 ***Σ*** 的其他元素为零）。请注意，***V****ᵀ* 的行给出了与 `pca.components_` 返回的主成分相同的结果。

现在要获得 ***X****ₖ*，我们只保留 ***U*** 和 ***V*** 的前 2 列，以及 ***Σ*** 的前 2 行和列（方程 14）。如果我们将它们相乘，我们得到：

![](img/f12fe9fc2937245f6bbb379a1046fba5.png)

列表 7 计算了这个矩阵：

```py
# Listing 7

k = 2
Sigma = np.zeros((X.shape[0], X.shape[1]))
Sigma[:min(X.shape[0], X.shape[1]),
      :min(X.shape[0], X.shape[1])] = np.diag(s)

X2 = U[:, :k] @ Sigma[:k, :k]  @ VT[:k, :]
X2
```

```py
array([[-0.85665, -2.68304, -1.34152],
       [ 1.10238,  0.50578,  0.25289],
       [ 0.79994, -2.04166, -1.02083],
       ...,
       [-0.24828, -1.99037, -0.99518],
       [-2.11447, -0.42335, -0.21168],
       [-0.60616, -1.37226, -0.68613]])
```

***Z****=***U***₂***Σ***₂ 的每一行给出了相对于前 2 个主成分形成的基的投影数据点的坐标。列表 8 计算了 ***Z****=***U***₂***Σ***₂。请注意，它给出了列表 4 中的 `pca.transform(X)` 的前两列。因此，PCA 和 SVD 都找到了相同的子空间和相同的投影数据点。

```py
# Listing 8

# each row of Z*=U_k Σ_k gives the coordinate of projection of the 
# same row of X onto a rank-k subspace
U[:, :k] @ Sigma[:k, :k] 
```

```py
array([[ 3.0969857 , -0.37538618],
       [-0.94916277, -0.79630095],
       [ 1.79290419, -1.62352748],
       ...,
       [ 2.14708946, -0.6353034 ],
       [ 1.25724271,  1.76475781],
       [ 1.64921984, -0.03716124]])
```

现在我们创建一个自编码器，并用这个数据集进行训练，以便后来与 PCA 进行比较。图 17 显示了网络架构。瓶颈层有两个神经元，因为我们想将数据点投影到二维子空间上。

![](img/39c23c90c07acfa7a7f19f410b812e9e.png)

图 17（作者提供的图片）

列表 9 在 Pytorch 中定义了这个架构。所有层中的神经元都有线性激活函数和零偏置。

```py
# Listing 9

seed = 9 
np.random.seed(seed)
torch.manual_seed(seed)
np.random.seed(seed)

class Autoencoder(nn.Module):
    def __init__(self):
        super(Autoencoder, self).__init__()
        ## encoder 
        self.encoder = nn.Linear(3, 2, bias=False)

        ## decoder 
        self.decoder = nn.Linear(2, 3, bias=False)

    def forward(self, x):
        encoded = self.encoder(x)
        decoded = self.decoder(encoded)
        return encoded, decoded

# initialize the NN
model1 = Autoencoder().double()
print(model1)
```

我们使用 MSE 成本函数和 Adam 优化器。

```py
# Listing 10

# specify the quadratic loss function
loss_func = nn.MSELoss()

# Define the optimizer
optimizer = torch.optim.Adam(model1.parameters(), lr=0.001)
```

我们使用在列表 1 中定义的设计矩阵来训练这个模型。

```py
X_train = torch.from_numpy(X) 
```

然后我们训练了 3000 轮：

```py
# Listing 11

def train(model, loss_func, optimizer, n_epochs, X_train):
    model.train()
    for epoch in range(1, n_epochs + 1):
        optimizer.zero_grad()
        encoded, decoded = model(X_train)
        loss = loss_func(decoded, X_train)
        loss.backward()
        optimizer.step()

        if epoch % int(0.1*n_epochs) == 0:
            print(f'epoch {epoch} \t Loss: {loss.item():.4g}')
    return encoded, decoded

encoded, decoded = train(model1, loss_func, optimizer, 3000, X_train)
```

```py
epoch 300   Loss: 0.4452
epoch 600   Loss: 0.1401
epoch 900   Loss: 0.05161
epoch 1200   Loss: 0.01191
epoch 1500   Loss: 0.003353
epoch 1800   Loss: 0.0009412
epoch 2100   Loss: 0.0002304
epoch 2400   Loss: 4.509e-05
epoch 2700   Loss: 6.658e-06
epoch 3000   Loss: 7.02e-07
```

Pytorch 张量 `encoded` 存储了隐藏层的输出（*z*₁, *z*₂），张量 `decoded` 存储了自编码器的输出（*x^*₁, *x^*₂, *x^*₃）。我们首先将它们转换为 `numpy` 数组。

```py
encoded = encoded.detach().numpy()
decoded = decoded.detach().numpy()
```

如前所述，具有中心化数据集和 MSE 成本函数的线性自编码器解决了以下最小化问题：

![](img/ccd9f3f42aede7a1eb82f57de3006ff2.png)

其中

![](img/b59158741e371db665d931bf3cff17cc.png)

***Z*** 包含了训练数据集中所有样本在瓶颈层的输出。我们还看到，这个最小化问题的解由方程 23 给出。因此，在这种情况下，我们有：

![](img/1363e42e9de39bbe00d2eb313140c105.png)

一旦训练了自编码器，我们可以检索矩阵 ***Z**** 和 ***W****。数组 `encoded` 给出矩阵 ***Z****：

```py
# Z* values. Each row gives the coordinates of one of the 
# projected data points
Zstar = encoded
Zstar
```

```py
array([[ 2.57510917, -3.13073321],
       [-0.20285442,  1.38040138],
       [ 2.39553775, -1.16300036],
       ...,
       [ 2.0265917 , -1.99727172],
       [-0.18811382, -2.15635479],
       [ 1.26660007, -1.74235118]])
```

列表 12 检索矩阵 ***W^***[2]：

```py
# Listing 12

# Each row of W^[2] gives the wights of one of the neurons in the
# output layer
W2 = model1.decoder.weight
W2 = W2.detach().numpy()
W2
```

```py
array([[ 0.77703505,  0.91276084],
       [-0.72734132,  0.25882988],
       [-0.36143178,  0.13109568]])
```

要得到 ***W****，我们可以写出：

```py
# Each row of Pstar (or column of W2) is one of the basis vectors
Wstar = W2.T
Wstar
```

```py
array([[ 0.77703505, -0.72734132, -0.36143178],
       [ 0.91276084,  0.25882988,  0.13109568]])
```

***W**** 的每一行表示一个基向量 (***w****ᵢ*)，由于瓶颈层有两个神经元，我们最终得到两个基向量 (***w***₁, ***w***₂)。我们可以很容易地看到 ***w***₁ 和 ***w***₂ 不形成正交基，因为它们的内积不为零：

```py
w1 = Wstar[0]
w2 = Wstar[1]

# p1 and p2 are not orthogonal since thier inner product is not zero
np.dot(w1, w2)
```

```py
0.47360735759
```

现在我们可以使用公式 25 轻松计算 ***X***₂：

```py
# X2 = Zstar @ Pstar
Zstar @ Wstar
```

```py
array([[-0.8566606 , -2.68331059, -1.34115189],
       [ 1.10235133,  0.50483352,  0.25428269],
       [ 0.7998756 , -2.04339283, -1.0182878 ],
       ...,
       [-0.24829863, -1.99097748, -0.99430834],
       [-2.11440724, -0.42130609, -0.21469848],
       [-0.60615728, -1.37222311, -0.68620423]])
```

请注意，这个数组和在列表 7 中使用 SVD 计算得到的数组 `X2` 是相同的（由于数值误差，它们之间有一个小的差异）。如前所述，***Z**** 的每一行给出了相对于由向量 ***w***₁ 和 ***w***₂ 形成的基的投影数据点 (***x̃****ᵢ*) 的坐标。

列表 13 绘制数据集、其主成分 ***v***₁ 和 ***v***₂ 以及新的基向量 ***w***₁ 和 ***w***₂ 的两个不同视图。结果如图 18 所示。请注意，数据点和基向量都位于同一平面上。请注意，训练自编码器从权重的随机初始化开始，因此如果我们在列表 9 中不使用随机种子，向量 ***w***₁ 和 ***w***₂ 将会不同，但它们始终位于主成分的同一平面上。

```py
# Listing 13

fig = plt.figure(figsize=(18, 14))
plt.subplots_adjust(wspace = 0.01)
origin = [0], [0], [0] 

ax1 = fig.add_subplot(121, projection='3d')
ax2 = fig.add_subplot(122, projection='3d')
ax1.set_aspect('auto')
ax2.set_aspect('auto')
def plot_view(ax, view1, view2):
    ax.scatter(x, y, z, color = 'blue', alpha= 0.1)
    # Principal components 
    ax.plot([0, pca.components_[0,0]], [0, pca.components_[0,1]],
            [0, pca.components_[0,2]],
            color="black", zorder=5)
    ax.plot([0, pca.components_[1,0]], [0, pca.components_[1,1]],
            [0, pca.components_[1,2]],
            color="black", zorder=5)

    ax.text(pca.components_[0,0], pca.components_[0,1],
            pca.components_[0,2]-0.5, "$\mathregular{v}_1$",
            fontsize=18, color='black', weight="bold",
            style="italic")
    ax.text(pca.components_[1,0], pca.components_[1,1]+0.7,
            pca.components_[1,2], "$\mathregular{v}_2$",
            fontsize=18, color='black', weight="bold",
            style="italic")

    # New basis found by autoencoder
    ax.plot([0, w1[0]], [0, w1[1]], [0, w1[2]],
             color="darkred", zorder=5)
    ax.plot([0, w2[0]], [0, w2[1]], [0, w2[2]],
             color="darkred", zorder=5)

    ax.text(w1[0], w1[1]-0.2, w1[2]+0.1,
            "$\mathregular{w}_1$", fontsize=18, color='darkred',
            weight="bold", style="italic")
    ax.text(w2[0], w2[1], w2[2]+0.3,
            "$\mathregular{w}_2$", fontsize=18, color='darkred',
            weight="bold", style="italic")

    ax.view_init(view1, view2)
    ax.set_xlabel("$x_1$", fontsize=20, zorder=2)
    ax.set_ylabel("$x_2$", fontsize=20)
    ax.set_zlabel("$x_3$", fontsize=20)
    ax.set_xlim([-3, 5])
    ax.set_ylim([-5, 5])
    ax.set_zlim([-4, 4])
plot_view(ax1, 25, 195)
plot_view(ax2, 0, 180)
plt.show()
```

![](img/8a6d11464ab866194541fb7bba9f5f90.png)

图 18

列表 14 绘制了 ***Z**** 的行，结果如图 19 所示。这些行表示编码后的数据点。需要注意的是，如果我们将这个图与图 16 进行比较，它们看起来不同。我们知道自编码器和 PCA 给出的投影数据点（相同的 ***X***₂）是相同的，但当我们在二维空间中绘制这些投影数据点时，它们看起来不同。为什么？

```py
# Listing 14

# This is not the right way to plot the projected data points in
# a 2d space since {w1, w2} is not an orthogonal basis

fig = plt.figure(figsize=(8, 8))
plt.scatter(Zstar[:, 0], Zstar[:, 1])
i= 6452
plt.scatter(Zstar[i, 0], Zstar[i, 1], color='red', s=60)
plt.axis('equal')
plt.axhline(y=0, color='gray')
plt.axvline(x=0, color='gray')
plt.xlabel("$z_1$", fontsize=20)
plt.ylabel("$z_2$", fontsize=20)
plt.xlim([-9,9])
plt.ylim([-9,9])
plt.show()
```

![](img/1a770b9390508713ff7e9ca6228fc3a9.png)

图 19

原因是我们为每个图有不同的基。在图 16 中，我们有相对于由 ***v***₁ 和 ***v***₂ 形成的正交基的投影数据点的坐标。然而，在图 19 中，投影数据点的坐标是相对于 ***w***₁ 和 ***w***₂ 的，它们不是正交的。因此，如果我们尝试使用正交坐标系统（如图 19 的坐标系统）来绘制它们，我们会得到一个失真的图。这一点在图 20 中也得到了演示。

![](img/c3b87b6cb798566db5bc8e33f7989684.png)

图 20（图像由作者提供）

要正确绘制 ***Z**** 的行，我们首先需要找到向量 ***w***₁ 和 ***w***₂ 相对于由 *V*={***v***₁, ***v***₂} 形成的正交基的坐标。

![](img/aa056c59839d139f9b4ba80e437de752.png)

我们知道，***Z*** 的每一行的转置给出了相对于由 *W*={***w***₁, ***w***₂} 形成的基的投影数据点的坐标。因此，我们可以使用方程 1 来获取相对于正交基 *V*={***v***₁, ***v***₂} 的相同数据点的坐标。

![](img/110809ceba18fe4ae932cbb1ff41e2ee.png)

其中

![](img/09cf621b1347be64ff1914f7ffeff0f1.png)

是坐标变换矩阵。列表 15 使用这些方程将 ***Z*** 的行相对于正交基 *V*={***v***₁, ***v***₂} 绘制出来。结果如图 21 所示，现在它与图 15 使用 SVD 生成的图形完全一致。

```py
# Listing 15

w1_V = np.array([np.dot(w1, v1), np.dot(w1, v2)])
w2_V = np.array([np.dot(w2, v1), np.dot(w2, v2)])
P_W = np.array([w1_V, w2_V]).T

Zstar_V = np.zeros((Zstar.shape[0], Zstar.shape[1]))

for i in range(len(Zstar_B)):
    Zstar_V[i] = P_W @ Zstar[i]

fig = plt.figure(figsize=(8, 6))
plt.scatter(Zstar_V[:, 0], Zstar_V[:, 1])
plt.axis('equal')
plt.axhline(y=0, color='gray')
plt.axvline(x=0, color='gray')
plt.scatter(Zstar_V[i, 0], Zstar_V[i, 1], color='red', s=60)
plt.quiver(0, 0, w1_V[0], w1_V[1], color=['black'], width=0.007,
           angles='xy', scale_units='xy', scale=1)
plt.quiver(0, 0, w2_V[0], w2_V[1], color=['black'], width=0.007,
           angles='xy', scale_units='xy', scale=1)
plt.text(w1_V[0]+0.1, w2_V[1]-0.2, "$[\mathregular{w}_1]_V$",
         weight="bold", style="italic", color='black',
         fontsize=20)
plt.text(w2_V[0]-2.25, w2_V[1]+0.1, "$[\mathregular{w}_2]_V$",
         weight="bold", style="italic", color='black',
         fontsize=20)

plt.xlim([-8.5, 8.5])
plt.xlabel("$v_1$", fontsize=20)
plt.ylabel("$v_2$", fontsize=20)
plt.show()
```

![](img/bac576768ae1cf36b53e8f563fa5ae06.png)

图 21

图 22 展示了在此案例研究中创建的线性自编码器的不同组件及其值的几何解释。

![](img/7ef3b78d6db73668380803a24adb3e72.png)

图 22（作者提供的图片）

**非线性自编码器**

尽管自编码器不能找到数据集的主成分，但它仍然是比 PCA 更强大的降维工具。在本节中，我们将讨论非线性自编码器，并举一个 PCA 失败而非线性自编码器仍能进行降维的例子。PCA 的一个问题是它假设投影数据点的最大方差沿主成分方向。换句话说，它假设这些方差都在直线上，而在许多实际应用中，这种假设并不成立。

让我们来看一个例子。列表 16 生成了一个名为`X_circ`的随机圆形数据集，并在图 23 中绘制了它。数据集包含 70 个数据点。`X_circ`是一个 2d 数组，每一行代表一个数据点（观察值）。我们还为每个数据点分配了一个颜色。这个颜色在建模中并没有使用，我们只是为了保持数据点的顺序。

```py
# listing 16

np.random.seed(0)
n = 90
theta = np.sort(np.random.uniform(0, 2*np.pi, n))
colors = np.linspace(1, 15, num=n)

x1 = np.sqrt(2) * np.cos(theta)
x2 = np.sqrt(2) * np.sin(theta)
X_circ = np.array([x1, x2]).T

fig = plt.figure(figsize=(8, 6))
plt.axis('equal')
plt.scatter(X_circ[:,0], X_circ[:,1], c=colors, cmap=plt.cm.jet)

plt.xlabel("$x_1$", fontsize= 18)
plt.ylabel("$x_2$", fontsize= 18)

plt.show()
```

![](img/219dc78e8859d1e9871c74043d178f09.png)

图 23

接下来，我们使用 PCA 查找该数据集的主成分。列表 17 找到主成分并在图 24 中绘制它们。

```py
# Listing 17

pca = PCA(n_components=2, random_state = 1)
pca.fit(X_circ)

fig = plt.figure(figsize=(8, 6))
plt.axis('equal')
plt.scatter(X_circ[:,0], X_circ[:,1], c=colors,
            cmap=plt.cm.jet)
plt.quiver(0, 0, pca.components_[0,0], pca.components_[0,1],
           color=['black'], width=0.01, angles='xy',
           scale_units='xy', scale=1.5)
plt.quiver(0, 0, pca.components_[1,0], pca.components_[1,1],
           color=['black'], width=0.01, angles='xy',
           scale_units='xy', scale=1.5)
plt.plot([-2*pca.components_[0,0], 2*pca.components_[0,0]],
         [-2*pca.components_[0,1], 2*pca.components_[0,1]],
         color='gray')
plt.text(0.5*pca.components_[0,0], 0.8*pca.components_[0,1], 
         "$\mathregular{v}_1$", color='black', fontsize=20)
plt.text(0.8*pca.components_[1,0], 0.8*pca.components_[1,1],
         "$\mathregular{v}_2$", color='black', fontsize=20)
plt.show()
```

![](img/39d64ba714ef6579fb254914868573f7.png)

图 24

在这个数据集中，最大方差沿着圆圈而不是直线。然而，PCA 仍然假设投影数据点的最大方差沿向量 ***v***₁（第一个主成分）。列表 18 计算了投影数据点到 ***v***₁ 的坐标，并在图 25 中绘制了它们。

```py
# Listing 18

projected_points = pca.transform(X_circ)[:,0]
fig = plt.figure(figsize=(16, 2))
frame = plt.gca()
plt.scatter(projected_points, [0]*len(projected_points),
            c=colors, cmap=plt.cm.jet, alpha =0.7)
plt.axhline(y=0, color='grey')
plt.xlabel("$v_1$", fontsize=18)
#plt.xlim([-1.6, 1.7])
frame.axes.get_yaxis().set_visible(False)
plt.show()
```

![](img/cd0aac564886e2796b0a02bc789183d9.png)

图 25

正如你所见，投影数据点已经丧失了顺序，颜色也混杂在一起。现在我们在这个数据集上训练一个非线性自编码器。图 26 展示了它的架构。网络有两个输入特征和两个神经元的输出层。共有 5 个隐藏层，隐藏层中的神经元数量分别为 64、32、1、32 和 64。因此，瓶颈层只有一个神经元，这意味着我们希望将训练数据集的维度从 2 降至 1。

![](img/6bf3d46caca9032700030cf969bfff87.png)

图 26（作者提供的图片）

你可能注意到的一点是，第一个隐藏层中的神经元数量在增加。因此，只有隐藏层具有双面漏斗形状。这是因为我们只有两个输入特征，所以需要在第一个隐藏层中增加更多神经元，以便为训练网络提供足够的神经元。列表 19 定义了 Pytorch 中的自编码器网络。

```py
# Listing 19

seed = 3
np.random.seed(seed)
torch.manual_seed(seed)
np.random.seed(seed)

class Autoencoder(nn.Module):
    def __init__(self, in_shape, enc_shape):
        super(Autoencoder, self).__init__()
        # Encoder
        self.encoder = nn.Sequential(
            nn.Linear(in_shape, 64),
            nn.ReLU(True),
            nn.Dropout(0.1),
            nn.Linear(64, 32),
            nn.ReLU(True),
            nn.Dropout(0.1),
            nn.Linear(32, enc_shape),
        )

        #Decoder
        self.decoder = nn.Sequential(
            nn.BatchNorm1d(enc_shape),
            nn.Linear(enc_shape, 32),
            nn.ReLU(True),
            nn.Dropout(0.1),
            nn.Linear(32, 64),
            nn.ReLU(True),
            nn.Dropout(0.1),
            nn.Linear(64, in_shape)
        )

    def forward(self, x):
        encoded = self.encoder(x)
        decoded = self.decoder(encoded)
        return encoded, decoded

model2 = Autoencoder(in_shape=2, enc_shape=1).double()
print(model2)
```

正如你所见，现在所有隐藏层都有一个非线性 RELU 激活函数。我们仍然使用 MSE 损失函数和 Adam 优化器。

```py
loss_func = nn.MSELoss()
optimizer = torch.optim.Adam(model2.parameters())
```

我们使用 `X_circ` 作为训练数据集，但我们使用 `MinMaxScaler()` 将所有特征缩放到 [0,1] 的范围内。

```py
X_circ_scaled = MinMaxScaler().fit_transform(X_circ)
X_circ_train = torch.from_numpy(X_circ_scaled)
```

接下来，我们用 5000 个周期训练模型。

```py
# Listing 20

def train(model, loss_func, optimizer, n_epochs, X_train):
    model.train()
    for epoch in range(1, n_epochs + 1):
        optimizer.zero_grad()
        encoded, decoded = model(X_train)
        loss = loss_func(decoded, X_train)
        loss.backward()
        optimizer.step()

        if epoch % int(0.1*n_epochs) == 0:
            print(f'epoch {epoch} \t Loss: {loss.item():.4g}')
    return encoded, decoded

encoded, decoded = train(model2, loss_func, optimizer, 5000, X_circ_train)
```

```py
epoch 500   Loss: 0.01391
epoch 1000   Loss: 0.005599
epoch 1500   Loss: 0.007459
epoch 2000   Loss: 0.005192
epoch 2500   Loss: 0.005775
epoch 3000   Loss: 0.005295
epoch 3500   Loss: 0.005112
epoch 4000   Loss: 0.004366
epoch 4500   Loss: 0.003526
epoch 5000   Loss: 0.003085
```

最后，我们绘制了瓶颈层（编码数据）中单个神经元的值，用于训练数据集中的所有观测点。请记住，我们为训练数据集中的每个数据点分配了颜色。现在我们使用相同的颜色表示编码数据点。这个图在图 27 中显示，现在与 PCA 生成的投影数据点（图 25）相比，大多数投影数据点的顺序是正确的。

```py
encoded = encoded.detach().numpy()

fig = plt.figure(figsize=(16, 2))
frame = plt.gca()
plt.scatter(encoded.flatten(), [0]*len(encoded.flatten()),
            c=colors, cmap=plt.cm.jet, alpha =0.7)
plt.axhline(y=0, color='grey')
plt.xlabel("$z_1$", fontsize=18)
frame.axes.get_yaxis().set_visible(False)
plt.show()
```

![](img/6611990f4a83bffc3c87bad4886073bf.png)

图 27

这是因为非线性自编码器不再将原始数据点投影到一条直线上。自编码器试图找到一条曲线（也称为非线性流形），沿着这条曲线，投影的数据点具有最大的方差，并将输入数据点投影到这些曲线上（图 28）。这个例子清楚地展示了自编码器相对于 PCA 的优势。PCA 是线性变换，因此不适用于具有非线性相关的数据集。另一方面，我们可以在自编码器中使用非线性激活函数。这使我们能够使用自编码器进行非线性降维。

![](img/34a292b1dc7cad5b90725425df4b7fea.png)

图 28（作者提供的图片）

在本文中，我们讨论了 PCA、SVD 和自编码器背后的数学。PCA 为数据集找到一个新的正交坐标系统。这个坐标系统的每个轴称为主成分。选择主成分的方式是使数据点在每个坐标轴上的方差在所有可能的方向上最大化，这些方向都与已经考虑过的主成分正交。

线性自编码器和 PCA 有一些相似之处。我们看到一个具有中心化输入特征、线性激活和 MSE 成本函数的自编码器可以找到由主成分张成的相同子空间。我们也得到 PCA 的相同投影数据点。然而，它不能找到主成分本身。实际上，它返回的基底甚至不一定是正交的。

另一方面，自编码器比 PCA 更灵活。PCA 的一个问题是，它假设投影数据点的最大方差沿着由主成分表示的直线。因此，如果最大方差沿着非线性曲线，PCA 无法找到它。然而，一个具有非线性激活函数的自编码器可以找到一个非线性流形，并将数据点投影到上面。

我希望你喜欢阅读这篇文章。如果你有任何问题或建议，请告诉我。文章中的所有代码清单都可以从 GitHub 下载为 Jupyter Notebook，链接为：

[`github.com/reza-bagheri/autoencoders_pca`](https://github.com/reza-bagheri/autoencoders_pca)

**附录**

**寻找主成分的方程：**

对于第二主成分，我们需要找到最大化的向量***u***₂

![](img/bab1a7eedb631b5c728c8f54fca3ef1c.png)

在这些约束下：

![](img/a4032df1626261724645ff5f3e98e062.png)![](img/d3039d4fa2822fd4d3b7b1bb0c8c3e00.png)

这意味着我们需要最大化

![](img/5ce90bfaf336958a06c89c60d7ec6975.png)

关于***u***₂。为了找到最大点，我们写道：

![](img/c4069bb91b9700131e7ef10ff11f7316.png)

通过将这个方程乘以***u***₁*ᵀ*，我们得到：

![](img/bf7b3ce6aa46e648b541157d5e8ae603.png)

现在使用方程 7 和协方差矩阵是对称矩阵的事实，我们可以写：

![](img/ce7f051bcc787a15d2e924b4085e3a05.png)

通过将这个方程代入前面的方程，我们得到：

![](img/149daa3593107f1e2d1894bf0a6e9bc2.png)

现在使用正交性约束和***u***₁被标准化的事实，我们得到：

![](img/56ee3a3ac5e248a3a55b66373d5c45ad.png)

使用这个方程和方程 A.1，可以得出

![](img/85b5ee0e10bcb9284087af49b7f8baab.png)

所以，我们有

![](img/e587a287535f41ea34777ccdfd6430a4.png)

我们还需要找到最大化的向量***u***ᵢ*

![](img/278b15c978542f6ee4700b08847045c1.png)

在这些约束下：

![](img/c53a1113e808d0bd90ad22f90a4bece1.png)![](img/6560b5efee8d1c3c3d95b35f0eef3164.png)

这等同于最大化

![](img/d465b68f027230d8ba3bbd270cc579d0.png)

关于***u***ᵢ*。为了找到最大点，我们将这个项对***u***ᵢ*的导数设为零：

![](img/7b327a4913bcf8541bb87d7566c6fd09.png)

通过将这个方程乘以***u***ₖᵀ*（其中 1≤*k*≤*i*-1），我们得到：

![](img/99172139f7ce3363576a68b7c3c95ce3.png)

我们知道之前的主成分是 ***S*** 的特征向量，并且 ***S*** 是一个对称矩阵。因此，我们有：

![](img/7766332b79d69f429dfe68da240817e9.png)

并且利用正交约束，我们得出：

![](img/75ba0689243cb07b3282018b7cba0d33.png)

将此方程代入方程 A.2 中，可以得出

![](img/5c031dc5dbae280e6fb5027aca6393b5.png)

因此 ***u****ᵢ* 是 ***S*** 的特征向量，而 *λᵢ* 是其对应的特征值。

**方程 17 的证明**：

假设我们有一个秩为 *r* 的 *m*×*n* 矩阵 ***X***，并且 ***X*** 的奇异值已排序，因此我们有：

![](img/a3ec7a72e61d91be3bbfd91796432947.png)

我们想要证明的是

![](img/141434731cbea5b51d5c2fa4dd2e1c74.png)

其中 ***A*** 是一个秩为 *k* 的 *m*×*n* 矩阵，***Z*** 和 ***W*** 分别是 *m*×*k* 和 *k*×*n* 的矩阵。我们知道 *k*<*n*。此外，在设计矩阵中，我们通常有 *m*>*n*。因此 *k* 小于 *m* 和 *n*。

我们知道矩阵的秩不能超过其行数或列数。因此，我们有：

![](img/057732fb756e5c0ea14bb9c5d4984eaa.png)

还可以证明

![](img/7d8e56808a99aa467c0cd40f65493371.png)

因此，有

![](img/d2fb6cefcc9abf66e11e920f1058e25a.png)

因此，***ZW*** 的秩不能超过 *k*。此外，我们将展示，最小化 ||***X***-***ZW***||_*F* 的矩阵 ***ZW*** 不能有低于 *k* 的秩。假设 ***Z*** 和 ***W*** 是两个矩阵，使得 ***Z*** ***W*** 是一个秩为 *k* 的矩阵，并且在所有秩为 *k* 的矩阵中最小化 ||***X***-***ZW***||_*F*。根据方程 16：

![](img/dc21d93ce1cf6104db6fb24e64a47a77.png)

同样地，假设 ***Z*** 和 ***W*** 是两个矩阵，使得 ***Z*** ***W*** 是一个秩为 *m* 的矩阵 (*m*<*k*)，在所有秩为 *m* 的矩阵中最小化 ||***X***-***Z*** ***W***||_*F*。我们有：

![](img/09c08fc7e8fc229f0784382661d6190a.png)

显然

![](img/7515e451ad7c54685ae9724368b24839.png)

因此

![](img/29ab8bb917b89d0ee0a94ceb6eea4ccd.png)

这意味着秩为 *k* 的矩阵总是给出 ||***X***-***Z W***||_*F* 的较低值。因此，我们得出结论，***ZW*** 的秩不能少于 *k*。因此，最小化 ||***X***-***Z W***||_*F* 的 ***ZW*** 的最佳值应为秩为 *k* 的矩阵。

**线性自编码器的成本函数**：

我们想要证明的是

![](img/63545cbdfcbc8291f5d52a47c4b7a9c3.png)

为了证明这一点，我们首先计算矩阵 ***X***-***Z*(*W^***[2])*ᵀ*：

![](img/45f7ee04a58b8a4302f9fb4f1b11355a.png)

但根据方程 22，我们有：

![](img/b765e48d83add0e0232f67d5f36f8473.png)

因此对于第 *k* 个观察值，我们可以写成：

![](img/937757446daa5f2aca063b6141dcb6cf.png)

将此方程代入方程 A.4 中，我们得到：

![](img/9522bcc5dbae280e6fb5027aca6393b5.png)

最终使用方程 15，我们得到：

![](img/c2565eae516b2383d79b259ec15d42f0.png)
