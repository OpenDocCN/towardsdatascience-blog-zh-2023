# NumPy 广播

> 原文：[https://towardsdatascience.com/numpy-broadcasting-4c4cb9dff1e7?source=collection_archive---------20-----------------------#2023-01-04](https://towardsdatascience.com/numpy-broadcasting-4c4cb9dff1e7?source=collection_archive---------20-----------------------#2023-01-04)

## 定义、规则和示例

[](https://medium.com/@cretanpan?source=post_page-----4c4cb9dff1e7--------------------------------)[![Pan Cretan](../Images/8b3fbab70c0e61f7ca516d2f54b646e5.png)](https://medium.com/@cretanpan?source=post_page-----4c4cb9dff1e7--------------------------------)[](https://towardsdatascience.com/?source=post_page-----4c4cb9dff1e7--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----4c4cb9dff1e7--------------------------------) [Pan Cretan](https://medium.com/@cretanpan?source=post_page-----4c4cb9dff1e7--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2Fff990ba57425&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnumpy-broadcasting-4c4cb9dff1e7&user=Pan+Cretan&userId=ff990ba57425&source=post_page-ff990ba57425----4c4cb9dff1e7---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----4c4cb9dff1e7--------------------------------) ·9分钟阅读·2023年1月4日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2F4c4cb9dff1e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnumpy-broadcasting-4c4cb9dff1e7&user=Pan+Cretan&userId=ff990ba57425&source=-----4c4cb9dff1e7---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2F4c4cb9dff1e7&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fnumpy-broadcasting-4c4cb9dff1e7&source=-----4c4cb9dff1e7---------------------bookmark_footer-----------)![](../Images/c72e9c7fc065552e52bfb5c5c995bb30.png)

摄影：[Jean-Guy Nakars](https://unsplash.com/@jgnak?utm_source=medium&utm_medium=referral) 摄于 [Unsplash](https://unsplash.com/?utm_source=medium&utm_medium=referral)

# 介绍

[NumPy](https://numpy.org/) 提供了通过矢量化进行快速计算的方法，这避免了使用较慢的 Python 循环。矢量化在使用二元 ufuncs（如加法或乘法）时也可用，附加的好处是数组不需要具有相同的形状。具有不同形状的数组之间的操作称为 [广播](https://numpy.org/doc/stable/user/basics.broadcasting.html#general-broadcasting-rules)，这可能会特别令人困惑，特别是对于多维数组，或当两个数组都需要扩展时。

有许多示例和教程，但我发现通过思考并实际记住广播规则来处理问题最有用。这样更容易考虑任何给定的使用案例，并编写代码，而无需依赖试错法。

# 广播规则

我强烈推荐两本数据分析和数据科学的书籍。它们都有关于广播的小节。

[数据分析的 Python](https://wesmckinney.com/book/advanced-numpy.html#numpy_broadcasting) 由 Wes McKinney 编写，包含以下广播规则：

> 如果对于每个*尾部维度*（即从末尾开始）轴长度匹配，或者任一长度为1，则两个数组可以进行广播。广播将对缺失或长度为1 的维度进行。

[Python 数据科学手册](https://jakevdp.github.io/PythonDataScienceHandbook/02.05-computation-on-arrays-broadcasting.html) 由 Jake VanderPlas 编写，包含更详细的广播规则：

> 规则1：如果两个数组在维度数量上不同，维度较少的那个数组的形状会在其前面（左侧）用1进行*填充*。
> 
> 规则2：如果两个数组的形状在任何维度上不匹配，则形状为1的数组会被扩展以匹配另一个形状。
> 
> 规则3：如果在任何维度上大小不一致且都不等于1，将会引发错误。

我发现第二组规则更容易遵循。下面的示例将使用这些规则。

# 示例

可能最简单的广播示例之一，以及一个典型模式，是从每一列中减去列均值。进行此操作后，列均值将变为数值上等于零。

```py
a = np.arange(12).reshape(4, 3)
means_columns = a.mean(axis=0)
res = a - means_columns
print('original array', a, sep='\n')
print('.. column means', a.mean(axis=0), sep='\n')
print('demeaned array', res, sep='\n')
print('.. column means', res.mean(axis=0), sep='\n')
```

这会打印

```py
original array
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]
.. column means
[4.5 5.5 6.5]
demeaned array
[[-4.5 -4.5 -4.5]
 [-1.5 -1.5 -1.5]
 [ 1.5  1.5  1.5]
 [ 4.5  4.5  4.5]]
.. column means
[0\. 0\. 0.]
```

让我们看看这里发生了什么。计算 `a.mean(axis=0)` 的列均值会生成一个形状为 (3,) 的一维数组。涉及减法的两个数组在形状上有所不同，因此 `means_columns` 会根据规则1在左侧填充1以匹配形状。因此，在幕后，`means_columns` 会被重塑为 (1, 3)。然后根据规则2，`means_columns` 会沿着轴0扩展，使其形状变为 (3, 3) 以匹配 `a` 的形状。

除了使用规则预测维度较少的数组如何被扩展外，我们还可以使用 `[np.broadcast_to](https://numpy.org/doc/stable/reference/generated/numpy.broadcast_to.html)`，它返回一个只读视图，该视图具有给定的形状。该视图可能是不连续的，且不同元素可能指向相同的内存地址。

```py
means_columns_bc = np.broadcast_to(means_columns, a.shape)
print(means_columns_bc)
print('base', means_columns_bc.base, sep='\n')
print('strides', means_columns_bc.strides, sep='\n')
```

这会打印

```py
[[4.5 5.5 6.5]
 [4.5 5.5 6.5]
 [4.5 5.5 6.5]
 [4.5 5.5 6.5]]
base
[4.5 5.5 6.5]
strides
(0, 8)
```

我们可以看到基础是原始的均值数组（因此它是一个视图），而沿第一个轴的步幅是0，这意味着同一列的不同元素指向相同的内存位置（有关NumPy内部的介绍，请参见[这里](https://medium.com/towards-data-science/numpy-internals-an-introduction-bcaafa1a68a2)）。NumPy确实在尽可能优化内存使用！

如果我们想对行进行去均值操作怎么办？可以通过`a.mean(axis=1)`快速计算行的均值，该操作将返回一个形状为（4，）的数组。将其形状左侧填充1，意味着数组将变成（1，4）。根据规则3，这两个数组的最后维度不一致且都不是1。这意味着广播不会发生。我们也可以预见到这一点，因为`np.broadcast_to(a.mean(axis=1), a.shape)`引发异常，告知我们广播无法生成请求的形状（4, 3）。形状的不兼容性还可以通过执行`np.broadcast_shapes(a.shape, a.mean(axis=1).shape)`来观察，这也引发异常，解释了形状不匹配。通过将行均值重塑为（4，1）数组来进行行去均值操作，可以使用`a.mean(axis=1).reshape(-1, 1)`或`a.mean(axis=1)[:, np.newaxis]`

```py
means_rows = a.mean(axis=1)
res = a - means_rows.reshape(-1, 1) # or res = a - means_rows[:, np.newaxis]
print('original array', a, sep='\n')
print('.. row means', a.mean(axis=1), sep='\n')
print('demeaned array', res, sep='\n')
print('.. row means', res.mean(axis=1), sep='\n')
```

这将打印

```py
original array
[[ 0  1  2]
 [ 3  4  5]
 [ 6  7  8]
 [ 9 10 11]]
.. row means
[ 1\.  4\.  7\. 10.]
demeaned array
[[-1\.  0\.  1.]
 [-1\.  0\.  1.]
 [-1\.  0\.  1.]
 [-1\.  0\.  1.]]
.. row means
[0\. 0\. 0\. 0.]
```

规则2清楚地解释了为什么这有效，因为形状为（4，1）的数组可以在列中扩展，使其形状变成（4，3）。

在第三个示例中，我们将演示广播如何在ufunc二元函数中扩展两个数组

```py
a = np.arange(4)
b = np.arange(3)
res = a[:, np.newaxis] + b[np.newaxis, :]
print('result array', res, sep='\n')
```

这将得到

```py
result array
[[0 1 2]
 [1 2 3]
 [2 3 4]
 [3 4 5]]
```

严格来说，重塑`b`并不是必要的，但它使事情更清晰。我们还可以使用`[np.broadcast_arrays](https://numpy.org/doc/stable/reference/generated/numpy.broadcast_arrays.html)`或相关且更灵活的`[np.broadcast](https://numpy.org/doc/stable/reference/generated/numpy.broadcast.html#numpy.broadcast)`来广播两个数组，而不应用ufunc。为了完整起见，还有其他方法可以实现与广播相同的结果，一个例子是`np.add.outer(a, b)`，它产生与

```py
np.array_equal(np.add.outer(a, b), a[:, np.newaxis] + b)
```

返回True。

对任何轴上的高维数组进行去均值操作可以被推广为

```py
def demean_axis(arr, axis=1):
    means = arr.mean(axis)
    indexer = [slice(None)]*arr.ndim
    indexer[axis] = np.newaxis
    return arr - means[tuple(indexer)]

arr = np.linspace(1, 12, 24*3).reshape(6,4,3)
res = demean_axis(arr, axis=1)
```

我们可以确认`np.abs(res.mean(axis=1)).max()`在数值上等于零。上述函数取自Wes McKinney的书籍，但需要稍微修改以适配本文使用的NumPy版本（1.23.4）。

作为一个更实际的例子，我们可以使用广播将彩色图像转换为灰度图像。广播部分已用注释标出：

```py
import matplotlib
matplotlib.use("TkAgg")
import matplotlib.pyplot as plt
import io
import numpy as np
from PIL import Image
# read in the original image (png)
with open('landscape_water_lake_nature_trees.png', mode='rb') as f:
    image_orig = f.read()
f = io.BytesIO(image_orig)
im = Image.open(f)
image_orig = np.array(im)/255.
print('shape of original image', image_orig.shape, sep='\n')

# convert RGBA to RGB (pillow could be used for this)
background = (1., 1., 1.)
row = image_orig.shape[0]
col = image_orig.shape[1]
image_color = np.zeros( (row, col, 3), dtype='float32' )
r, g, b, a = image_orig[:,:,0], image_orig[:,:,1], image_orig[:,:,2], image_orig[:,:,3]
a = np.asarray( a, dtype='float32' )
R, G, B = background
image_color[:,:,0] = r * a + (1.0 - a) * R
image_color[:,:,1] = g * a + (1.0 - a) * G
image_color[:,:,2] = b * a + (1.0 - a) * B
print('shape of image after RGBA to RGB conversion', image_color.shape, sep='\n')

# convert to greyscale
conv = np.array([0.2126, 0.7152, 0.0722])
# --- broadcasting !!! ---
image_grey = (image_color[:,:,:3]*conv).sum(axis=2)
# --- broadcasting !!! ---
print('shape of image after conversion to greyscale', image_grey.shape, sep='\n')

# plot the image
fig = plt.figure(figsize=(8, 4))
axs = fig.subplots(1, 2)
axs[0].axis('off')
axs[0].set_title('RGB image')
axs[0].imshow(image_color)
axs[1].axis('off')
axs[1].set_title('greyscale image')
axs[1].imshow(image_grey, cmap='gray')
axs[0].annotate('',xy=(0.52,0.5),xytext=(0.50,0.5),arrowprops=dict(facecolor='black'),
             xycoords='figure fraction', textcoords='figure fraction')
fig.savefig('RGBA_to_greyscale.png')
```

上述代码生成

![](../Images/3aee3670314a7f03619d446c12e9fc02.png)

将透明PNG图像转换为灰度图像（照片由[nextvoyage](https://pixabay.com/users/nextvoyage-5275305/)在[pixabay](https://pixabay.com/)提供）

也许值得关注的是，原图是一个 RGBA 图像（红色、绿色、蓝色、透明度），即它还包含第四个 alpha 通道来指示每个像素的透明度。我们通过使用白色背景将 RGBA 图像转换为 RGB。这以及示例中的几乎所有内容，都可以使用[Pillow](https://pillow.readthedocs.io/en/stable/)来完成，但我们故意尽可能使用 NumPy（例如，Pillow 可以[转换为灰度图像](https://stackoverflow.com/questions/12201577/how-can-i-convert-an-rgb-image-into-grayscale-in-python)以保持透明度）。代码的最后部分是一些[Matplotlib](https://matplotlib.org/)的花招，用于比较彩色图像和灰度图像。

转换为灰度图像是通过以下公式完成的。

![](../Images/55253152222c572e69b7f1c72fe5af93.png)

来自维基百科的[this page](https://en.wikipedia.org/wiki/Grayscale)。执行乘法和求和的代码部分，将图像数组的形状从（1080，1920，3）更改为（1080，1920）的部分已被注释掉。你可能会想，为了一行广播代码展示如此长的示例是否值得。这正是要点！广播是简洁的，如果没有它，代码会更长且更慢。通常，算法背后的魔法是几行 NumPy 操作，通常包括广播。

## 设置值

大多数 NumPy 用户将广播与数组加法或乘法相关联。然而，当使用索引设置值时，广播同样适用。下面你可以找到一组示例，我们将不对其详细评论。

```py
# set one row, same value to all columns
a = np.ones((4,3))
a[0] = -1
# array([[-1., -1., -1.],
#        [ 1.,  1.,  1.],
#        [ 1.,  1.,  1.],
#        [ 1.,  1.,  1.]])

# set one column, same value to all rows
a = np.ones((4,3))
a[:, 0] = -1
# array([[-1.,  1.,  1.],
#        [-1.,  1.,  1.],
#        [-1.,  1.,  1.],
#        [-1.,  1.,  1.]])

# set all rows, same value to all elements
a = np.ones((4,3))
a[:] = -1
# array([[-1., -1., -1.],
#        [-1., -1., -1.],
#        [-1., -1., -1.],
#        [-1., -1., -1.]])

# set all rows, different value to each column
a = np.ones((4,3))
b = np.array([-1, -2, -3])
a[:] = b
# array([[-1., -2., -3.],
#        [-1., -2., -3.],
#        [-1., -2., -3.],
#        [-1., -2., -3.]])

# set all rows, different value to each row
a = np.ones((4,3))
b = np.array([-1, -2, -3, -4])
b = b[:, np.newaxis]
a[:] = b
# array([[-1., -1., -1.],
#        [-2., -2., -2.],
#        [-3., -3., -3.],
#        [-4., -4., -4.]])

# set some rows, different value to each column
a = np.ones((4,3))
b = np.array([-1, -2, -3])
a[:3] = b
# array([[-1., -2., -3.],
#        [-1., -2., -3.],
#        [-1., -2., -3.],
#        [ 1.,  1.,  1.]])

# set some rows, different value to each row
a = np.ones((4,3))
b = np.array([-1, -2, -3])
a[:3] = b[:, np.newaxis]
# array([[-1., -1., -1.],
#        [-2., -2., -2.],
#        [-3., -3., -3.],
#        [ 1.,  1.,  1.]])

# set some columns, different value to each column
a = np.ones((4,3))
b = np.array([-1, -2])
a[:,:2] = b
# array([[-1., -2.,  1.],
#        [-1., -2.,  1.],
#        [-1., -2.,  1.],
#        [-1., -2.,  1.]])

# set some columns, different value to each row
a = np.ones((4,3))
b = np.array([-1, -2, -3, -4])
a[:,:2] = b[:, np.newaxis]
# array([[-1., -1.,  1.],
#        [-2., -2.,  1.],
#        [-3., -3.,  1.],
#        [-4., -4.,  1.]])
```

# 结论

广播可能看起来很复杂，但如果记住几个关键原则，它可以很容易掌握。最重要的原则是数组的形状从右开始对齐。缺失的维度用一填充，始终在左侧。通过扩展维度为一的形状，这两个形状变得相同。在两个（或更多！）数组可以进行广播之前，可能需要使用`np.newaxis`进行一些重新形状调整。广播不仅用于计算整个数组，还用于设置数组中的某些值。这就是要点。通过一些练习，NumPy 的广播可以导致令人惊讶的简洁和高效的代码。充分利用它的潜力！
