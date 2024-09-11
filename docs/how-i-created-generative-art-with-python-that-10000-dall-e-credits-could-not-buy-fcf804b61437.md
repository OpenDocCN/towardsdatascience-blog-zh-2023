# 如何使用 Python 创造 DALL-E 10000 份积分无法购买的生成艺术

> 原文：[https://towardsdatascience.com/how-i-created-generative-art-with-python-that-10000-dall-e-credits-could-not-buy-fcf804b61437?source=collection_archive---------1-----------------------#2023-07-19](https://towardsdatascience.com/how-i-created-generative-art-with-python-that-10000-dall-e-credits-could-not-buy-fcf804b61437?source=collection_archive---------1-----------------------#2023-07-19)

[](https://borach.medium.com/?source=post_page-----fcf804b61437--------------------------------)[![Borach Jansema](../Images/02280890ed87239c75cbcbfa7c5d686c.png)](https://borach.medium.com/?source=post_page-----fcf804b61437--------------------------------)[](https://towardsdatascience.com/?source=post_page-----fcf804b61437--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----fcf804b61437--------------------------------) [Borach Jansema](https://borach.medium.com/?source=post_page-----fcf804b61437--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F26c634bbd08&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-created-generative-art-with-python-that-10000-dall-e-credits-could-not-buy-fcf804b61437&user=Borach+Jansema&userId=26c634bbd08&source=post_page-26c634bbd08----fcf804b61437---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----fcf804b61437--------------------------------) · 11 分钟阅读 · 2023年7月19日 [](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Ffcf804b61437&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-created-generative-art-with-python-that-10000-dall-e-credits-could-not-buy-fcf804b61437&user=Borach+Jansema&userId=26c634bbd08&source=-----fcf804b61437---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Ffcf804b61437&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fhow-i-created-generative-art-with-python-that-10000-dall-e-credits-could-not-buy-fcf804b61437&source=-----fcf804b61437---------------------bookmark_footer-----------)

Python 和 Pillow: 如何编写 DALL-E 无法做到的代码

在这篇博客文章中，我将展示一些我使用 Python 编程语言创建的生成艺术作品，具体利用了 Pillow 和 Torch 库。我的艺术创作灵感来源于奥地利音乐作曲家和视觉艺术家罗曼·豪本斯托克-拉马蒂的视觉构图。

在 2021 年初，我经常浏览 Catawiki，因为我想购买一些艺术品来装饰我的家庭办公室。当我在 2021 年初在 Catawiki 上发现 Haubenstock-Ramati 的作品时，我立刻被他复杂而美丽的参数艺术所吸引。我一直想用我的编程技能做一些创意性的工作，因此受到启发开发了可以生成类似输出的代码。下面的图像是激发我灵感的其中一幅图像，由 Roman Haubenstock-Ramati 创作。

![](../Images/f0d763fe94d9d99a05ffa139f53c29b8.png)

[Konstellationen](https://www.mutualart.com/Artwork/-Konstellationen-/9D3C9F0000BAC080)，1970/1971年，由 Roman Haubenstock-Ramati 创作

在 2022 年 4 月 Dall-E 2 发布后，我尝试使用该模型生成应该类似于 Haubenstock-Ramati 作品的艺术作品。要求模型这样做是一个有争议的话题，因为关于 AI 模型能否生成如此类似于艺术家作品的输出，以至于这些输出可能被视为对原作的版权侵权，存在有效的担忧。这一讨论超出了这篇博客的范围，但我想澄清的是，我输入到 Dall-E 的提示并不是为了生成 Haubenstock-Ramati 作品的精确复制品或贬低他的作品。我编写的代码也是如此，它们并不是为了分发他的作品的副本，而仅仅是演示如何使用 Python 创建视觉几何构图。

DALL-E 的输出很有趣，但并没有完全捕捉到他原作的精髓。输出缺乏 Haubenstock-Ramati 艺术作品中存在的精确约束和复杂性。我尝试了许多不同的提示，但始终无法接近我想要的效果。

![](../Images/cf1d05296b01dbbdc4e8e52b47dad8a3.png)![](../Images/db2e5d309d0197d7951a6b0786d86f2a.png)

Dall-E 根据我的提示生成的一些输出：“创建一幅罗曼·豪本斯托克-拉马蒂风格的绘画作品，融入图形符号和实验音乐构图的元素。画作应以黑白色为主，具有大胆的线条和几何形状，并应包含一个中心主题，代表作品的主题。”

为了简化过程，我向 Dall-E 提出了一个更简单的请求：“画一条垂直线连接到一个矩形，再连接一个正方形到这条线，然后用另一条垂直线将正方形连接到另一个矩形，最后用另一条垂直线将矩形连接到一个圆。”出乎意料的是，结果并不如我所愿。尽管提示很简单，Dall-E 仍然难以理解形状之间的预期关系，产生了意外的结果。

![](../Images/5dd1084d4055b0477b078be052210bf0.png)![](../Images/1823a60e74cfde60397f932c321880ce.png)

Dall-E 根据提示生成的图像：“绘制一个垂直线连接一个矩形，将一个方块连接到这条线，再用另一条垂直线连接方块到另一个矩形，最后用另一条垂直线将矩形连接到一个圆形。”

我清楚地意识到 Dall-E 无法处理几何约束的提示，我尝试了一个更简单的提示：“创建一个仅显示两条正交线的图”。这也被证明过于困难。

![](../Images/e15c859898a59c0013db60450f0a5031.png)![](../Images/7df3731a890e4d049fb72a539fa977b7.png)

Dall-E 生成的图像根据提示：“创建一个仅显示两条正交线的图”

Dall-E 不能完成这一任务让我感到惊讶，但考虑到像 Dall-E 这样的模型如何工作，这并不令人意外。它基于潜在扩散，这本质上是一个噪声过程，不适合基于约束的精确提示。

接下来，我将展示我生成的图像，并详细讨论如何编写类似的代码。

![](../Images/51e8c59fd457abd3d68e79bd9add5169.png)

使用我的代码生成的图像的单一示例。

![](../Images/58e5a69a4e133c2e19b5e292681dd20c.png)

一个展示我代码生成不同图像的 gif，显示了相同参数下图像的多样性。

我使用 Python 和 Pillow 制作了这些图像，不涉及任何机器学习。我的代码生成的图像通过 Torch 引入了随机性，Torch 是一个多功能的软件包，我因为其熟悉和方便而使用。它通常用于机器学习（ML）。但这些图像并不是通过机器学习（ML）生成的。

你可能会想知道图像的多样性来源于哪里，我个人喜欢我的代码能够生成给人类似感觉但仔细看又各不相同的图像。输出的多样性是一个至关重要的特性。我的代码生成的图像的变化源自于对随机变量的复杂使用。在概率论和统计学中，随机变量是其可能值是随机现象的结果的变量。

现在我将描述我代码生成图像的过程，并展示一些 Python 示例，展示这个生成过程从高层次的视角看起来是怎样的。

我们可以将生成过程分为 3 步。

+   步骤 1：生成中心图形。这是通过采样一个矩形、一条线、一种矩形、一个方块、一条线和一个圆形来完成的。这些图形被放置在固定的位置，形状的大小由随机变量确定。

+   步骤 2：从三个不同的分布中采样三个具有线条和相邻的簇。在每个簇中，放置了若干条具有不同起始和结束点的垂直线。

+   步骤 3：在线条的簇中采样并绘制圆形和矩形。

![](../Images/8ca93965e4c1b962b68be02f9a5e2595.png)

动图显示了单个图像的逐步生成过程。

**步骤 1**

为了理解随机变量在我的代码中的作用，请考虑我们图像创建过程中的第一步：形成一个纵向矩形，其特点是高度大于宽度。这个矩形，虽然看似简单，却体现了随机变量的作用。

矩形可以分解为四个主要元素：起始的 x 和 y 坐标，以及结束的 x 和 y 坐标。现在，这些点在从特定分布中选择时，转变为随机变量。但我们如何决定这些点的范围，或更具体地说，它们来自的分布呢？答案在于统计学中最常见且关键的分布之一：正态分布。

正态分布由两个参数定义——均值 (μ) 和标准差 (σ)，在我们的图像生成过程中发挥着关键作用。均值 μ 表示分布的中心，因此充当随机变量值围绕的点。标准差 σ 量化了分布中的离散程度。它决定了随机变量可能取的值范围。本质上，较大的标准差会导致生成图像的多样性更大。

```py
import torch
canvas_height = 1000
canvas_width = 1500

#loop to show different values
for i in range(5):
    #create normal distribution to sample from
    start_y_dist = torch.distributions.Normal(canvas_height * 0.8, canvas_height * 0.05)
    #sample from distribution
    start_y = int(start_y_dist.sample())

    #create normal distribution to sample height from
    height_dist = torch.distributions.Normal(canvas_height * 0.2, canvas_height * 0.05)

    height = int(height_dist.sample())
    end_y = start_y + height

    #start_x is fixed because of this being centered
    start_x = canvas_width // 2
    width_dist = torch.distributions.Normal(height * 0.5, height * 0.1)

    width = int(width_dist.sample())
    end_x = start_x + width

    print(f"start_x: {start_x}, end_x: {end_x}, start_y: {start_y}, end_y: {end_y}, width: {width}, height: {height}")
```

```py
start_x: 750, end_x: 942, start_y: 795, end_y: 1101, width: 192, height: 306
start_x: 750, end_x: 835, start_y: 838, end_y: 1023, width: 85, height: 185
start_x: 750, end_x: 871, start_y: 861, end_y: 1061, width: 121, height: 200
start_x: 750, end_x: 863, start_y: 728, end_y: 962, width: 113, height: 234
start_x: 750, end_x: 853, start_y: 812, end_y: 986, width: 103, height: 174
```

采样一个正方形看起来非常相似，我们只需采样高度或宽度，因为它们是一样的。采样一个圆形则更简单，因为我们只需采样半径。

在 Python 中绘制矩形是一个简单的过程，特别是当使用 Pillow 库时。以下是如何做到这一点：

```py
from PIL import Image, ImageDraw

# Create a new image with white background

# Loop to draw rectangles
for i in range(5):
    img = Image.new('RGB', (canvas_width, canvas_height), 'white')

    draw = ImageDraw.Draw(img)

    # Creating normal distributions to sample from
    start_y_dist = torch.distributions.Normal(canvas_height * 0.8, canvas_height * 0.05)
    start_y = int(start_y_dist.sample())

    height_dist = torch.distributions.Normal(canvas_height * 0.2, canvas_height * 0.05)
    height = int(height_dist.sample())
    end_y = start_y + height

    start_x = canvas_width // 2
    width_dist = torch.distributions.Normal(height * 0.5, height * 0.1)
    width = int(width_dist.sample())
    end_x = start_x + width

    # Drawing the rectangle
    draw.rectangle([(start_x, start_y), (end_x, end_y)], outline='black')

    img.show()
```

**步骤 2**

在这些图像中的垂直线的背景下，我们考虑了三个随机变量，即：

1.  线的起始 y 坐标（y_start）

1.  线的结束 y 坐标（y_end）

1.  线的 x 坐标（x）

由于我们处理的是垂直线，每条线只需要采样一个 x 坐标。线的宽度是恒定的，由画布的大小控制。

需要一些额外的逻辑来确保线条不交叉。为此，我们基本上需要将图像视为一个网格，并跟踪被占用的位置。为了简化问题，我们暂时忽略这一点。

这是一个在 Python 中的示例。

```py
import torch
from PIL import Image, ImageDraw

# Setting the size of the canvas
canvas_size = 1000
# Number of lines
num_lines = 10
# Create distributions for start and end y-coordinates and x-coordinate
y_start_distribution = torch.distributions.Normal(canvas_size / 2, canvas_size / 4)
y_end_distribution = torch.distributions.Normal(canvas_size / 2, canvas_size / 4)
x_distribution = torch.distributions.Normal(canvas_size / 2, canvas_size / 4)
# Sample from the distributions for each line
y_start_points = y_start_distribution.sample((num_lines,))
y_end_points = y_end_distribution.sample((num_lines,))
x_points = x_distribution.sample((num_lines,))
# Create a white canvas
image = Image.new('RGB', (canvas_size, canvas_size), 'white')
draw = ImageDraw.Draw(image)
# Draw the lines
for i in range(num_lines):
    draw.line([(x_points[i], y_start_points[i]), (x_points[i], y_end_points[i])], fill='black')
# Display the image
image.show()
```

然而，这样做只会得到直线。另一部分是线末端的圆形，我称这些为相邻圆。随机变量也决定它们的过程。首先，是否会有一个相邻圆是从伯努利分布中采样的，而形状的位置（左、中、右）是从均匀分布中采样的。

圆形可以完全由一个参数定义：它的半径。我们可以将线段长度视为影响圆形半径的条件。这形成了一个条件概率模型，其中圆形的半径（R）依赖于线段长度（L）。我们使用条件高斯分布。该分布的均值（μ）是线段长度平方根的函数，而标准差（σ）是一个常数。

我们最初建议给定线段长度 L 的半径 R 服从正态分布。这被表示为 R | L ~ N(μ(L), σ²)，其中 N 是正态（高斯）分布，σ 是标准差。

然而，这存在一个小问题：正态分布包括了采样负值的可能性。在我们的场景中，这个结果是不可能的，因为半径不能为负。

为了规避这个问题，我们可以使用半正态分布。这个分布与正态分布类似，由一个尺度参数 σ 定义，但关键的是，它被限制为非负值。给定线段长度的半径服从半正态分布：R | L ~ HN(σ)，其中 HN 表示半正态分布。这样，σ 由所需均值确定，即 σ = √(2L) / √(2/π)，确保所有采样的半径都是非负的，并且分布的均值是 √(2L)。

```py
from PIL import Image, ImageDraw
import numpy as np
import torch
# Define your line length
L = 3000

# Calculate the desired mean for the half-normal distribution
mu = np.sqrt(L * 2)

# Calculate the scale parameter that gives the desired mean
scale = mu / np.sqrt(2 / np.pi)

# Create a half-normal distribution with the calculated scale parameter
dist = torch.distributions.HalfNormal(scale / 3)

# Sample and draw multiple circles
for _ in range(10):
    # Create a new image with white background
    img_size = (2000, 2000)
    img = Image.new('RGB', img_size, (255, 255, 255))
    draw = ImageDraw.Draw(img)

    # Define the center of the circles
    start_x = img_size[0] // 2
    start_y = img_size[1] // 2
    # Sample a radius from the distribution
    r = int(dist.sample())

    print(f"Sampled radius: {r}")

    # Define the bounding box for the circle
    bbox = [start_x - r, start_y - r, start_x + r, start_y + r]

    # Draw the circle onto the image
    draw.ellipse(bbox, outline ='black',fill=(0, 0, 0))

    # Display the image
    img.show()
```

**第3步**

我们过程中的第3步是第1步和第2步元素的结合。在第1步中，我们处理了在设定位置采样和绘制矩形的任务。在第2步中，我们学会了如何使用正态分布在画布的一部分上绘制线条。此外，我们还掌握了如何采样和绘制圆形的知识。

当我们过渡到第3步时，我们将重新利用前面步骤中的技巧。我们的目标是将方形和圆形和谐地分布在我们之前采样的线条周围。正态分布将在这个任务中再次派上用场。

我们将重用用于创建线条集群的参数。然而，为了增强视觉效果并避免重叠，我们对均值（μ）和标准差值引入了一些噪声。

在这一步中，我们的任务不是定位线条，而是放置采样的矩形和圆形。我鼓励你尝试这些技巧，并看看是否能将圆形和矩形添加到你的线条集群中。

在这篇博客文章中，我已经解剖并简化了支撑我的代码的过程，以便更深入地理解其操作方式。我展示了像 Dall-E 这样的生成性 AI 模型在遵循精确约束方面的困难。

编写产生这些图像的代码对我来说是一次极好的体验。看到每写一行代码，图像逐渐变化，真的很酷。我希望这篇博客文章激发了你对艺术与编程交汇点的兴趣。我鼓励你利用你的编程技能，通过代码让你的想象力变为现实。无需耗尽你的 Dall-E 额度，创造的力量就在你的指尖。
