# 在Python中创建分形

> 原文：[https://towardsdatascience.com/creating-fractals-in-python-a502e5fc2094?source=collection_archive---------3-----------------------#2023-03-24](https://towardsdatascience.com/creating-fractals-in-python-a502e5fc2094?source=collection_archive---------3-----------------------#2023-03-24)

## **深入几何学、递归算法和三角形……很多很多的！**

[](https://medium.com/@bobby.elmes?source=post_page-----a502e5fc2094--------------------------------)[![Robert Elmes](../Images/a18f0f3f6edfdb7e4ec23712f3620ab7.png)](https://medium.com/@bobby.elmes?source=post_page-----a502e5fc2094--------------------------------)[](https://towardsdatascience.com/?source=post_page-----a502e5fc2094--------------------------------)[![Towards Data Science](../Images/a6ff2676ffcc0c7aad8aaf1d79379785.png)](https://towardsdatascience.com/?source=post_page-----a502e5fc2094--------------------------------) [Robert Elmes](https://medium.com/@bobby.elmes?source=post_page-----a502e5fc2094--------------------------------)

·

[关注](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fsubscribe%2Fuser%2F60c271ca8c1f&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-fractals-in-python-a502e5fc2094&user=Robert+Elmes&userId=60c271ca8c1f&source=post_page-60c271ca8c1f----a502e5fc2094---------------------post_header-----------) 发表在 [Towards Data Science](https://towardsdatascience.com/?source=post_page-----a502e5fc2094--------------------------------) ·8 分钟阅读·2023年3月24日[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fvote%2Ftowards-data-science%2Fa502e5fc2094&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-fractals-in-python-a502e5fc2094&user=Robert+Elmes&userId=60c271ca8c1f&source=-----a502e5fc2094---------------------clap_footer-----------)

--

[](https://medium.com/m/signin?actionUrl=https%3A%2F%2Fmedium.com%2F_%2Fbookmark%2Fp%2Fa502e5fc2094&operation=register&redirect=https%3A%2F%2Ftowardsdatascience.com%2Fcreating-fractals-in-python-a502e5fc2094&source=-----a502e5fc2094---------------------bookmark_footer-----------)![](../Images/2559843ce7d15598d62ec3de02d518d7.png)

这是我在今年早些时候拍的一张照片，那天特别阴沉，即使在英格兰也是如此。

**分形** 是在不同尺度上自相似的无限复杂的图案。例如，树干分裂成更小的枝条。这些枝条进一步分裂成更小的枝条，依此类推。

通过程序生成分形，我们可以将简单的形状转化为复杂的重复图案。

在这篇文章中，我将探讨如何使用一些基本的A-Level几何知识和一点编程技巧，在Python中构建令人印象深刻的分形。

分形在数据科学中扮演着重要角色。例如，在分形分析中，会评估数据集的分形特征，以帮助理解潜在过程的结构。此外，分形生成中心的递归算法可以应用于广泛的数据问题，从二分查找算法到递归神经网络。

# **想法**

我想写一个可以绘制等边三角形的程序。在三角形的每一边上，它还必须能够绘制一个稍小的外向三角形。它应该能够根据我的需要重复这个过程，希望能创造出一些有趣的图案。

![](../Images/b85843885804ec5f00e910a76c929f64.png)

这个粗略的草图展示了我想生成的图案类型。

# 表示图像

我将把图像表示为一个二维像素数组。像素数组中的每个单元格将表示该像素的颜色（RGB）。

为了实现这一点，我们可以使用库[**NumPy**](https://numpy.org/install/)生成像素数组，并使用[**Pillow**](https://pillow.readthedocs.io/en/latest/installation.html)将其转换为可以保存的图像。

![](../Images/eee75796763fdfb6ff12d0994022901b.png)

蓝色像素的 x 值为 3，y 值为 4，可以通过二维数组 **pixels[4][3]** 进行访问

# 绘制直线

现在是时候开始编码了！

首先，我需要一个函数，可以接受两个坐标集并在它们之间绘制一条直线。

以下代码通过在两个点之间进行插值，每一步都向像素数组中添加新的像素。你可以把这个过程想象成逐像素地为一条线着色。

*我在每个代码片段中使用了续行符 ‘\’，以便将一些较长的代码行适配进来。*

```py
import numpy as np
from PIL import Image
import math

def plot_line(from_coordinates, to_coordinates, thickness, colour, pixels):

    # Figure out the boundaries of our pixel array
    max_x_coordinate = len(pixels[0])
    max_y_coordinate = len(pixels)

    # The distances along the x and y axis between the 2 points
    horizontal_distance = to_coordinates[1] - from_coordinates[1]
    vertical_distance = to_coordinates[0] - from_coordinates[0]

    # The total distance between the two points
    distance =  math.sqrt((to_coordinates[1] - from_coordinates[1])**2 \
                + (to_coordinates[0] - from_coordinates[0])**2)

    # How far we will step forwards each time we colour in a new pixel
    horizontal_step = horizontal_distance/distance
    vertical_step = vertical_distance/distance

    # At this point, we enter the loop to draw the line in our pixel array
    # Each iteration of the loop will add a new point along our line
    for i in range(round(distance)):

        # These 2 coordinates are the ones at the center of our line
        current_x_coordinate = round(from_coordinates[1] + (horizontal_step*i))
        current_y_coordinate = round(from_coordinates[0] + (vertical_step*i))

        # Once we have the coordinates of our point, 
        # we draw around the coordinates of size 'thickness'
        for x in range (-thickness, thickness):
            for y in range (-thickness, thickness):
                x_value = current_x_coordinate + x
                y_value = current_y_coordinate + y

                if (x_value > 0 and x_value < max_x_coordinate and \
                    y_value > 0 and y_value < max_y_coordinate):
                    pixels[y_value][x_value] = colour

# Define the size of our image
pixels = np.zeros( (500,500,3), dtype=np.uint8 )

# Draw a line
plot_line([0,0], [499,499], 1, [255,200,0], pixels)

# Turn our pixel array into a real picture
img = Image.fromarray(pixels)

# Show our picture, and save it
img.show()
img.save('Line.png')
```

![](../Images/447cf94ec4ce65aaaba8829fe64a9545.png)

当我让这个函数在像素数组的每个角落之间绘制一条黄色直线时的结果

# 绘制三角形

现在我有了一个可以在两个点之间绘制直线的函数，是时候绘制第一个等边三角形了。

给定三角形的中心点和边长，我们可以使用方便的公式计算高度：**h = ½(√3a)**。

现在，利用这个高度、中心点和边长，我可以确定三角形的每个角落应该在哪里。使用我之前制作的*plot_line*函数，我可以在每个角落之间绘制一条直线。

```py
def draw_triangle(center, side_length, thickness, colour, pixels):

    # The height of an equilateral triangle is, h = ½(√3a)
    # where 'a' is the side length
    triangle_height = round(side_length * math.sqrt(3)/2)

    # The top corner
    top = [center[0] - triangle_height/2, center[1]]

    # Bottom left corner
    bottom_left = [center[0] + triangle_height/2, center[1] - side_length/2]

    # Bottom right corner
    bottom_right = [center[0] + triangle_height/2, center[1] + side_length/2]

    # Draw a line between each corner to complete the triangle
    plot_line(top, bottom_left, thickness, colour, pixels)
    plot_line(top, bottom_right, thickness, colour, pixels)
    plot_line(bottom_left, bottom_right, thickness, colour, pixels)
```

![](../Images/bd2db1465beb955f579cce25c68219ac.png)

在 500x500 像素的 PNG 图像中心绘制一个三角形的结果

# 生成分形

一切就绪。几乎所有需要的条件都已经准备好，可以在 Python 中创建我的第一个分形了。真令人兴奋！

然而，这最后一步可以说是最棘手的。我希望我们的三角形函数能够对每条边调用自身。为了使其有效，我需要能够计算每个新小三角形的中心点，并正确地旋转它们，使其与附加的边垂直。

通过从我希望旋转的坐标中减去中心点的偏移量，然后应用旋转坐标对的公式，我们可以使用此函数旋转三角形的每个角。

```py
def rotate(coordinate, center_point, degrees):
    # Subtract the point we are rotating around from our coordinate
    x = (coordinate[0] - center_point[0])
    y = (coordinate[1] - center_point[1])

    # Python's cos and sin functions take radians instead of degrees
    radians = math.radians(degrees)

    # Calculate our rotated points 
    new_x = (x * math.cos(radians)) - (y * math.sin(radians))
    new_y = (y * math.cos(radians)) + (x * math.sin(radians))

    # Add back our offset we subtracted at the beginning to our rotated points
    return [new_x + center_point[0], new_y + center_point[1]]
```

![](../Images/ba950b6f2ac329e4149fd6af97c8e53e.png)

一个我们将每个坐标旋转了35度的三角形

现在我可以旋转三角形，我必须将注意力转向在第一个三角形的每一边上绘制一个新的、更小的三角形。

为了实现这一点，我扩展了*draw_triangle*函数，以计算每条边的新三角形的旋转和中心点，新三角形的边长由参数*shrink_side_by*减少。

一旦计算出新三角形的中心点和旋转角度，它会调用*draw_triangle*（它自身）来从当前线的中心绘制新的、更小的三角形。这将再次触发相同的代码块，计算另一个更小三角形的中心点和旋转角度。

这称为递归算法，因为我们的*draw_triangle*函数现在会调用自身，直到达到我们希望绘制的三角形的*max_depth*。重要的是要有这个逃逸条件，否则函数理论上会无限递归（但实际上调用堆栈会变得过大，导致栈溢出错误）！

```py
def draw_triangle(center, side_length, degrees_rotate, thickness, colour, \
                  pixels, shrink_side_by, iteration, max_depth):

    # The height of an equilateral triangle is, h = ½(√3a) 
    # where 'a' is the side length
    triangle_height = side_length * math.sqrt(3)/2

    # The top corner
    top = [center[0] - triangle_height/2, center[1]]

    # Bottom left corner
    bottom_left = [center[0] + triangle_height/2, center[1] - side_length/2]

    # Bottom right corner
    bottom_right = [center[0] + triangle_height/2, center[1] + side_length/2]

    if (degrees_rotate != 0):
        top = rotate(top, center, degrees_rotate)
        bottom_left = rotate(bottom_left, center, degrees_rotate)
        bottom_right = rotate(bottom_right, center, degrees_rotate)

    # Coordinates between each edge of the triangle
    lines = [[top, bottom_left],[top, bottom_right],[bottom_left, bottom_right]]

    line_number = 0

    # Draw a line between each corner to complete the triangle
    for line in lines:
        line_number += 1

        plot_line(line[0], line[1], thickness, colour, pixels)

        # If we haven't reached max_depth, draw some new triangles
        if (iteration < max_depth and (iteration < 1 or line_number < 3)):
            gradient = (line[1][0] - line[0][0]) / (line[1][1] - line[0][1])

            new_side_length = side_length*shrink_side_by

            # Center of the line of the traingle we are drawing
            center_of_line = [(line[0][0] + line[1][0]) / 2, \
                              (line[0][1] + line[1][1]) / 2]

            new_center = []
            new_rotation = degrees_rotate

            # Amount we need to rotate the traingle by
            if (line_number == 1):
                new_rotation += 60
            elif (line_number == 2):
                new_rotation -= 60
            else:
                new_rotation += 180

            # In an ideal world this would be gradient == 0,
            # but due to floating point division we cannot
            # ensure that this will always be the case
            if (gradient < 0.0001 and gradient > -0.0001):
                if (center_of_line[0] - center[0] > 0):
                    new_center = [center_of_line[0] + triangle_height * \
                                 (shrink_side_by/2), center_of_line[1]]
                else:
                    new_center = [center_of_line[0] - triangle_height * \
                                  (shrink_side_by/2), center_of_line[1]]

            else:

                # Calculate the normal to the gradient of the line
                difference_from_center = -1/gradient

                # Calculate the distance from the center of the line
                # to the center of our new traingle
                distance_from_center = triangle_height * (shrink_side_by/2)

                # Calculate the length in the x direction, 
                # from the center of our line to the center of our new triangle
                x_length = math.sqrt((distance_from_center**2)/ \
                                     (1 + difference_from_center**2))

                # Figure out which way around the x direction needs to go
                if (center_of_line[1] < center[1] and x_length > 0):
                    x_length *= -1

                # Now calculate the length in the y direction
                y_length = x_length * difference_from_center

                # Offset the center of the line with our new x and y values
                new_center = [center_of_line[0] + y_length, \
                              center_of_line[1] + x_length]

            draw_triangle(new_center, new_side_length, new_rotation, \
                          thickness, colour, pixels, shrink_side_by, \
                          iteration+1, max_depth)
```

![](../Images/6f19399b68b1763a7be024446e414a19.png)

三角形分形，*shrink_side_by* = 1/2 和 *max_depth* = 2

# 结果

以下是一些通过修改*shrink_side_by*和*max_depth*值输入到我们的*draw_triangle*函数中可以生成的不同图像示例。

有趣的是，这些大型重复模式通常会创造出更复杂的形状，如六边形，但具有迷人的对称性。

![](../Images/59173552d3ab9bd0be4790d692fce68b.png)

在重复三角形的对称性中，开始出现越来越复杂的形状。

![](../Images/33ccff8b7816f00bb7c42646b9fb0dd3.png)

一种类似雪花的分形，使用了修改版的`draw_triangle`函数，该函数还绘制了一个朝内的三角形。

![](../Images/24051971e50a4ea2ca719724ece759f6.png)

另一种创作，使用每次迭代减少的更小的尺寸

*除非另有说明，否则所有图片均由作者提供。*

# 结论

分形非常有趣，可以创造出美丽的图案。使用一些简单的概念和一丝创造力，我们可以生成非常令人印象深刻的结构。

在理解分形的核心属性并应用递归算法时，我们创建了一个坚实的基础，这有助于我们理解数据科学中更复杂的分形问题。

随时阅读和下载完整代码[**这里**](https://github.com/BobbyElmes/Triangular-Fractal-Generator---Python-)。如果你发现改进或扩展的方法，请告诉我！

*我很好奇你能用不同的形状创造出什么？*
