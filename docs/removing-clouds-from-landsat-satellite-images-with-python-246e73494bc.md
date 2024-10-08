# 用 Python 去除 Landsat 卫星图像中的云层

> 原文：[`towardsdatascience.com/removing-clouds-from-landsat-satellite-images-with-python-246e73494bc`](https://towardsdatascience.com/removing-clouds-from-landsat-satellite-images-with-python-246e73494bc)

## 计算你感兴趣区域的云量，去除云层并使用另一张卫星图像修补它们

[](https://conorosullyds.medium.com/?source=post_page-----246e73494bc--------------------------------)![Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----246e73494bc--------------------------------)[](https://towardsdatascience.com/?source=post_page-----246e73494bc--------------------------------)![Towards Data Science](https://towardsdatascience.com/?source=post_page-----246e73494bc--------------------------------) [Conor O'Sullivan](https://conorosullyds.medium.com/?source=post_page-----246e73494bc--------------------------------)

·发布于[Towards Data Science](https://towardsdatascience.com/?source=post_page-----246e73494bc--------------------------------) ·14 分钟阅读·2023 年 5 月 29 日

--

![](img/139abceec02d150bde92512b5d84aa21.png)

（来源：作者）

我们的技术已经发展到征服浩瀚的太空。我们已经发射了配备最先进传感器的卫星，用于监测我们变化的星球。然而，一个对手却让这些先进系统感到困扰——云层。

无论你是想比较光谱指数的值还是在卫星图像上训练机器学习模型，云层都是一个问题。因此，我们将探索如何使用云掩膜来：

+   计算你感兴趣区域的**云量百分比**

+   **从图像中去除**云层

+   **用另一张日期不同的图像修补**云像素

关于实际的掩膜，我们将探索两个选项：

+   在**Landsat QA 文件**（QA_PIXEL.tif）中提供的方法

+   一种替代的**机器学习方法**

我们将解释用于完成此任务的 Python 代码，你可以在[GitHub](https://github.com/conorosully/medium-articles/blob/master/src/remote%20sensing/landsat_clouds.ipynb)上找到完整的项目。让我们清除这些天空吧！

# 下载卫星图像

我们讨论的方法适用于任何栅格数据。在这篇文章中，我们将专门处理 Landsat 卫星图像。你可以使用[EarthExplorer](https://earthexplorer.usgs.gov/)门户下载 Landsat 场景。或者，如果你想使用 Python，下面的文章将带你完成这一过程：

[](/downloading-landsat-satellite-images-with-python-a2d2b5183fb7?source=post_page-----246e73494bc--------------------------------) ## 使用 Python 下载 Landsat 卫星图像

### 使用 landsatxplore Python 包简化 Landsat 场景下载

towardsdatascience.com

最终，你应该会有一个像**图 1**一样的文件夹。这些都是[ Landsat 2 级科学产品](https://www.usgs.gov/landsat-missions/landsat-collection-2-level-2-science-products)中可用的文件。我们将处理高亮显示的文件。这些是 3 个可见光波段和 QA_PIXEL 文件。

![](img/b845ab3da664b3198809738ea990de2d.png)

图 1：Landsat 二级科学产品文件（来源：作者）

这个特定场景拍摄于南非开普敦上空。为了查看这一点，我们使用**get_RGB**函数可视化可见光波段。这个函数以文件名/ID 作为参数。然后它会加载波段（第 7–9 行），将它们堆叠（第 12 行），[缩放](https://www.usgs.gov/faqs/how-do-i-use-scale-factor-landsat-level-2-science-products)（第 13 行）和剪裁（第 16 行）。

```py
import tifffile as tiff
import numpy as np

def get_RGB(ID):

    # Load Blue (B2), Green (B3) and Red (B4) bands
    B2 = tiff.imread('./data/{}/{}_SR_B2.TIF'.format(ID, ID))
    B3 = tiff.imread('./data/{}/{}_SR_B3.TIF'.format(ID, ID))
    B4 = tiff.imread('./data/{}/{}_SR_B4.TIF'.format(ID, ID))

    # Stack and scale bands
    RGB = np.dstack((B4, B3, B2))
    RGB = np.clip(RGB*0.0000275-0.2, 0, 1)

    # Clip to enhance contrast
    RGB = np.clip(RGB,0,0.3)/0.3

    return RGB
```

我们使用这个功能来获取**图 1**（第 3–4 行）中下载场景的 RGB 可视化。你可以在**图 2**中看到结果图像。注意所有的云！让我们看看能否利用 Landsat 云掩膜来处理这些云。

```py
import matplotlib.pyplot as plt

ID = 'LC09_L2SP_175083_20230410_20230412_02_T1'
RGB = get_RGB(ID)

# Plot the RGB image
fig, ax = plt.subplots(figsize=(20, 10))
ax.imshow(RGB)
```

![](img/89908ec1b1269cf93dc7accc1c6ca975.png)

图 2：Landsat 场景的 RGB 可视化（来源：作者）

# Landsat 云掩膜

## 什么是云掩膜？

云掩膜是一种分割图，将云与地球表面的其他特征分开。该图将图像中的每个像素分类为云或非云。它还可以包括其他类似的特征，如云影。它们在遥感中很重要，因为隔离云像素能更准确地测量和解释土地覆盖分析。

用于分类云像素的方法有很多种。Landsat 使用一种叫做[CFMask](https://www.usgs.gov/landsat-missions/cfmask-algorithm)的算法。该算法使用基于光谱指数的各种统计数据的组合。它在云像素的分类准确率为 89%，在云影的分类准确率为 96% [1]。稍后，我们将讨论一种替代的机器学习方法来创建云掩膜。

## 了解质量保证（QA）波段

CFMask 算法在每个 Landsat 场景上运行，结果可以在 QA 波段中找到。该文件是一个与太阳反射波段（例如可见光波段）具有相同高度和宽度的数组。波段中的每个像素都是一个 16 位整数，包含该像素的分类。你可以在[数据格式控制手册](https://www.usgs.gov/media/files/landsat-8-9-olitirs-collection-2-level-2-data-format-control-book)中找到有关这种编码的更多信息。

例如，假设 QA 文件中的一个像素值为**22280**。为了理解这个整数的含义，我们首先将其转换为 16 位二进制数——**0101011100001000**。这个数字中的每一位映射到**图 3**中看到的标志。第 3 位的值为 1，表示该像素被分类为云。接下来，我们将考虑云掩码中位 1 到 4 的覆盖类型。

![](img/bc422ce67a78418a67fa3204c49036b9.png)

图 3：Landsat QA 波段位描述和示例（来源：作者）

我们使用这些信息来创建**get_mask**函数。它将返回 4 种覆盖类型中每个像素的分类——云、阴影、扩张云和卷云。如果一个像素被分类为给定的覆盖类型，它的值将为 0，否则为 1。稍后，我们将看到这使我们能够通过将卫星图像与掩码相乘来去除不需要的像素。

```py
def get_mask(val,type='cloud'):

    """Get mask for a specific cover type"""

    # convert to binary
    bin_ = '{0:016b}'.format(val)

    # reverse string
    str_bin = str(bin_)[::-1]

    # get bit for cover type
    bits = {'cloud':3,'shadow':4,'dilated_cloud':1,'cirrus':2}
    bit = str_bin[bits[type]]

    if bit == '1':
        return 0 # cover
    else:
        return 1 # no cover
```

为了应用此功能，我们首先加载 QA 波段（第 2 行）。我们对波段中的每个像素应用**get_mask**，以获得 4 种覆盖类型中的每一种（第 6–9 行）。我们使用**np.vectorize()**来加速，而不是对每个像素进行循环处理。每个掩码将是一个与场景具有相同维度的数组，每个元素的值为 0 或 1。

```py
# QA band
QA = tiff.imread('./data/{}/{}_QA_PIXEL.TIF'.format(ID, ID))
QA = np.array(QA)

# Get masks
cloud_mask = np.vectorize(get_mask)(QA,type='cloud')
shadow_mask = np.vectorize(get_mask)(QA,type='shadow')
dilated_cloud_mask = np.vectorize(get_mask)(QA,type='dilated_cloud')
cirrus_mask = np.vectorize(get_mask)(QA,type='cirrus')
```

我们可以在**图 4**中看到这些掩码的实际效果。这里我们将看到**图 2**中看到的图像与 4 种掩码的分类结果以不同颜色叠加。放大一个区域，我们可以更好地理解每种覆盖类型：

+   **阴影**是云下的较暗区域

+   **扩张云**是被分类为云的像素周围的边缘

+   **卷云**是“细丝状”的云

如果一个像素被分类为卷云，它也会被分类为云。

![](img/339af0757a3c4b210063d4c002ba2122.png)

图 4：Landsat 分割掩码包含 4 种覆盖类型——云、阴影、扩张云和卷云（来源：作者）

以下代码用于创建**图 4**。我们将每个掩码的图层添加到原始 RGB 图像中（第 15–22 行）。这通过使用**cv2.addWeighted()**函数（第 22 行）来完成。每个掩码图层都有不同的颜色（第 8–11 行），透明度为 50%。通过提取像素范围来裁剪感兴趣区域（AOI）（第 43 行）。接下来，我们将对该区域进行更详细的查看。

```py
import cv2
import matplotlib as mpl

# segmentation image
seg = RGB.copy()

# color for each cover type
colors = np.array([[247, 2, 7],
                    [201, 116, 247],
                    [0, 234, 255],
                    [3, 252, 53]])/255

masks = [cloud_mask, shadow_mask, dilated_cloud_mask, cirrus_mask]

for i,mask in enumerate(masks):

    # color for cover type
    temp = seg.copy()
    temp[mask == 0] = colors[i]

    # add to segmentation
    seg = cv2.addWeighted(seg, 0.5, temp, 0.5, 0)

fig, ax = plt.subplots(1,2,figsize=(20, 10))

ax[0].imshow(seg)

# add legend with colors for each cover type
legend_elements = [mpl.patches.Patch(facecolor=colors[0], label='Cloud'),
                     mpl.patches.Patch(facecolor=colors[1], label='Shadow'),
                        mpl.patches.Patch(facecolor=colors[2], label='Dilated Cloud'),
                        mpl.patches.Patch(facecolor=colors[3], label='Cirrus')]

ax[0].legend(handles=legend_elements, loc='upper right')

# draw white rectangle around area of interest
h = 300
x,y = 4500,4500
rect = mpl.patches.Rectangle((x-h,y-h),h*2,h*2,linewidth=2,edgecolor='w',facecolor='none')
ax[0].add_patch(rect)

# crop area of interest
crop_seg =  seg[y-h:y+h,x-h:x+h,:]

ax[1].imshow(crop_seg)

ax[0].set_axis_off()
ax[1].set_axis_off()
```

## 评估感兴趣区域

整个场景的云覆盖百分比作为场景的元数据的一部分提供。在我们的案例中，这个值是**59%**。通过云掩码，我们可以计算特定 AOI 的这一数据。这很有用，因为它允许我们以编程方式搜索那些 AOI 无云的场景。即使场景的其他部分有云也无妨。

为了查看这一点，我们选择与**图 4**（第 2 行）相同的感兴趣区域。请记住，云掩码的值为 0 表示没有云，否则为 1。我们将其取反并计算平均值（第 5 行）。这给我们一个**69%**的值。不幸的是，我们的 AOI 比场景总体更有云！

```py
# crop area of interest
cloud_aoi = cloud_mask[y-h:y+h,x-h:x+h]

# calculate percentage of cloud cover
per_cloud = np.average(1-cloud_aoi)*100
print('Percentage of clouds: {:.2f}%'.format(per_cloud))
```

## 去除云层

一旦我们有了掩膜，去除不需要的像素就很简单。我们只需将 RGB 图像与掩膜相乘。下面的代码将去除被标记为云层的像素（第 2 行）。请记住，RGB 图像是一个 3D 数组。相比之下，**cloud_mask**是一个 2D 数组。这就是为什么我们使用**np.newaxis**来添加额外的维度。

```py
# Remove clouds
rm_clouds = RGB*cloud_mask[:, :, np.newaxis]
```

你可以在**图 5**中看到结果。云层像素现在全部被值[0,0,0]替代。这与用于边界框的值相同。这意味着这些像素“没有数据”，可以在进一步分析中忽略。最终，这些分析的结果依赖于云掩膜的准确性。这就是为什么研究人员不断寻求改进这些掩膜。

![](img/9173baae6d2167f736c2041563575f9b.png)

图 5：使用 Landsat 云掩膜从卫星图像中去除云层（来源：作者）

# 云掩膜的机器学习方法

我们提到 Landsat 使用一种称为[CFMask](https://www.usgs.gov/landsat-missions/cfmask-algorithm)的算法。该算法基于遥感和气象知识，了解云层像素在卫星图像中的表现。另一种方法是使用机器学习。这需要训练图像，其中所有云层像素都被手动标记。然后，机器学习算法学习光谱波段和标签之间的关系。

[一种方法](https://medium.com/sentinel-hub/clouds-segmentation-in-landsat-8-images-da370815235)使用在海岸气溶胶（B1）、红色（B4）和短波红外 2（B7）波段上训练的遗传算法。得到的算法在**cloud_pred**函数中给出。**pred**公式（第 4-10 行）给出了三个波段的加权。如果加权值大于 B7 波段，则像素被标记为云层（第 12 行）。

```py
def cloud_pred(B1,B4,B7):
    """Cloud prediction model"""

    pred = 2.16246741593412 -0.796409165054949*B4 + \
    0.971776520302587*np.sqrt(abs(0.028702220187686*B7*B1 + \
    0.971297779812314*np.sin(B1))) + \
    0.0235599298084993*np.floor(0.995223926146334*np.sqrt(abs(0.028702220187686*B7*B1 + 0.971297779812314*np.sin(B1)))+ \
    0.00477607385366598*abs(0.028702220187686*B7*B1 + \
    0.971297779812314*np.sin(B1))) - 0.180030905136552*np.cos(B4) + \
    0.0046635498889134*abs(0.028702220187686*B7*B1 + 0.971297779812314*np.sin(B1))

    cloud = np.where(pred > B7,0,1)

    return cloud
```

要使用此功能，我们首先加载三个必要的波段（第 4-6 行）。通过将这些波段传递给**cloud_pred**函数（第 9 行），我们获得云掩膜。然后，我们可以像以前一样使用这个掩膜去除云层（第 12 行）。你可以在**图 6**中看到结果图像。

```py
ID = "LC09_L2SP_175083_20230410_20230412_02_T1"

# Get Coastal Aerosol (B1), Red (B4) and Shortwave Infrared 2 (B7) bands
B1 = tiff.imread('./data/{}/{}_SR_B1.TIF'.format(ID, ID))
B4 = tiff.imread('./data/{}/{}_SR_B4.TIF'.format(ID, ID))
B7 = tiff.imread('./data/{}/{}_SR_B7.TIF'.format(ID, ID))

# Get cloud mask
cloud_mask_2 = cloud_pred(B1,B4,B7)

# Remove clouds
rm_clouds = RGB*cloud_mask_2[:, :, np.newaxis]
```

![](img/6d63b900d94333d487b6dbdf8a96167e.png)

图 6：使用机器学习方法从卫星图像中去除云层（来源：作者）

研究人员表示上述方法的准确度为**89%**——与 CFMask 相似。如前所述，提升算法的准确度是一个活跃的研究领域。最先进的方法使用深度学习算法进行图像分割，如 U-Net。这些方法的缺点是需要准确标记的训练数据。

# 云层修复

对于某些应用，仅仅从图像中去除云层可能就足够了。对于其他应用，我们希望替换或“修复”云层。例如，为了创建没有云层的景观可视化。

在**图 7**中，我们可以看到我们一直在处理的有云图像。这张图像拍摄于 2023 年 10 月 4 日。幸运的是，8 天后在同一区域拍摄了一张清晰图像。我们将探讨如何用清晰图像中的像素（第 7 行）替换有云图像中的像素（第 6 行）。

```py
# IDs for cloudy and non-cloudy images
IDs = ['LC09_L2SP_175083_20230410_20230412_02_T1',
       'LC08_L2SP_175083_20230418_20230429_02_T1']

# Get RGB
cloudy = get_RGB(IDs[0])
clear = get_RGB(IDs[1])
```

![](img/ab15da8c8131f201d2cd2ecbbed7d753.png)

图 7：拍摄于 8 天前的有云图像和清晰图像（来源：作者）。

我们将对**关注区域**和**整个场景**执行此操作。在这两种情况下，考虑像素的*地理位置*非常重要。即使有云和清晰图像拍摄于相同区域，像素也不会完全对齐。换句话说，数组位置 [x,y] 的像素的 UTM 坐标在每个图像中都会有所不同。因此，如果仅依赖数组位置，我们将替换错误区域的像素。为了解决这个问题，我们基于本文中使用的方法：

如何在 Landsat 卫星图像上用 Python 绘制坐标

### 使用 Landsat 元数据和 Rasterio 将像素位置映射到地理坐标

towardsdatascience.com

## 关注区域

首先，我们将学习如何修复一个**关注区域**（AOI）。我们从创建一个包含所有 4 种覆盖类型的掩码开始（第 2 行），并从有云图像中移除不需要的像素（第 5 行）。然后，我们围绕我们的**关注区域**裁剪得到的图像（第 8-9 行）。我们将 (x,y) 视为有云图像中**关注区域**的中心。

```py
# get mask for cloudy image
mask = cloud_mask*shadow_mask*dilated_cloud_mask*cirrus_mask

# remove cloudy pixels and fill with adjusted clear pixels
rm_mask = cloudy*mask[:, :, np.newaxis]

#crop area of interest
x,y,h = 4500,4500,300
crop_rm_mask = rm_mask[y-h:y+h,x-h:x+h]
```

如前所述，我们需要考虑像素的地理位置。我们使用 rasterio 包来帮助实现这一点。我们加载有云图像（第 4 行）和清晰图像（第 5 行）的红色波段。

```py
import rasterio as rio

# Open red band with rasterio for geolocation
geo_cloudy = rio.open('./data/{}/{}_SR_B4.TIF'.format(IDs[0], IDs[0]))
geo_clear = rio.open('./data/{}/{}_SR_B4.TIF'.format(IDs[1], IDs[1]))
```

我们在**有云图像**中找到**关注区域**中心的 UTM 坐标（第 2 行）。然后，我们在**清晰图像**中找到这些 UTM 坐标的数组位置（第 5 行）。打印这些坐标得到**(4430, 4510)**。你可以看到这与原始中心**(4500,4500)**不同。

```py
# get UTM coordinates from cloudy image
utmx, utmy = geo_cloudy.xy(y,x)

# get pixels from clear image
y_,x_ = geo_clear.index(utmx, utmy)
print(x_,y_)
```

我们使用新中心从**清晰图像**（第 2 行）裁剪**关注区域**。然后，我们使用原始中心（第 5 行）裁剪有云图像的云掩码。**crop_clear**和**crop_mask**的 UTM 坐标将完美对齐。因此，我们通过将清晰图像与掩码的反向相乘（第 6 行）来创建**填充**。最后一步是将**填充**添加到无云的有云图像中（第 9 行）。

```py
# crop clear image 
crop_clear = clear[y_-h:y_+h,x_-h:x_+h]

#get fill from clear image
crop_mask = mask[y-h:y+h,x-h:x+h]
fill = crop_clear*(1-crop_mask[:, :, np.newaxis])

#inpaint area of interest 
inpaint = crop_rm_mask + fill
```

你可以在**图 8**中看到结果。效果不错！大部分景观看起来是连贯的。不过，也有一些问题，我们将在最后讨论。

![](img/53f10e3ef571aedc757d7e62ab9a29e6.png)

图 8：使用清晰图像修复的关注区域（来源：作者）

## 整个图像

对整个场景进行修复更加复杂。为此，我们需要对齐多云和晴朗场景的边界框。边界框是场景的外部黑色边框。我们通过从晴朗图像中添加和移除一些像素来对齐这些框的 UTM 坐标。

![](img/efe597bd3cedb66ab980f4d360a7f699.png)

图 9：添加和移除像素以对齐边界框

我们首先计算从晴朗图像的顶部、底部、左侧和右侧添加/移除的像素数量。我们获取**晴朗图像**的左上角（UL）（第 3 行）和右下角（LR）（第 4 行）的位置。这些位置基于晴朗图像的尺寸（第 2 行）。

```py
# get pixel coordinates of clear image corners
y,x = geo_clear.read(1).shape
clear_ul = (0,0) # upper left
clear_lr = (y,x) # lower right
```

接下来，我们从**多云图像**中获取边界（第 2 行）。这些边界提供了多云图像边界框的 UTM 坐标。然后，我们从**晴朗图像**中获取这些坐标的数组位置（第 3–4 行）。请注意，这些位置可能会超出晴朗图像的尺寸。

```py
# get pixel coordinates of cloudy image corners
cloudy_bounds = geo_cloudy.bounds
new_clear_ul = geo_clear.index(cloudy_bounds.left,cloudy_bounds.top)
new_clear_lr = geo_clear.index(cloudy_bounds.right,cloudy_bounds.bottom)
```

原始和新角落位置之间的差异将告诉我们如何调整晴朗图像以对齐 UTM 坐标。打印调整量（第 8 行）给出 -10 0 70 -70。这意味着我们必须将晴朗图像调整为：

+   从顶部移除 10 像素

+   底部无调整

+   向左添加 70 像素

+   从右侧移除 70 像素

```py
# calculate pixel adjustment 
top_adj = clear_ul[0] - new_clear_ul[0]
bottom_adj = new_clear_lr[0] - clear_lr[0]

left_adj = clear_ul[1] -  new_clear_ul[1]
right_adj = new_clear_lr[1] - clear_lr[1]

print(top_adj, bottom_adj, left_adj, right_adj)
```

在计算调整量后，我们可以使用**adjust_rgb**函数。这会在正调整情况下添加黑色像素（第 6–17 行）。然后，它会在负调整情况下裁剪像素（第 20–27 行）。

```py
def adjust_rgb(rgb,top_adj, bottom_adj, left_adj, right_adj):

    adj_rgb = rgb.copy()

    #Adding black pixels 
    if top_adj > 0:
        add_top = np.zeros((top_adj,rgb.shape[1],3))
        adj_rgb = np.vstack((add_top,adj_rgb))
    if bottom_adj > 0:
        add_bottom = np.zeros((bottom_adj,rgb.shape[1],3))
        adj_rgb = np.vstack((adj_rgb,add_bottom))
    if left_adj > 0:
        add_left = np.zeros((rgb.shape[0],left_adj,3))
        adj_rgb = np.hstack((add_left,adj_rgb))
    if right_adj > 0:
        add_right = np.zeros((rgb.shape[0],right_adj,3))
        adj_rgb = np.hstack((adj_rgb,add_right))

    #Removing pixels
    if top_adj < 0:
        adj_rgb = adj_rgb[-top_adj:,:,:]
    if bottom_adj < 0:
        adj_rgb = adj_rgb[:bottom_adj,:,:]
    if left_adj < 0:
        adj_rgb = adj_rgb[:,-left_adj:,:]
    if right_adj < 0:
        adj_rgb = adj_rgb[:,:right_adj,:]

    return adj_rgb
```

我们使用此函数来调整我们的晴朗图像（第 6 行）。我们从多云图像中移除不需要的像素（第 12 行），然后从调整后的晴朗图像中获取替换像素（第 13 行）。由于它们已经对齐，我们可以简单地将它们叠加在一起（第 14 行）。

```py
# Get RGB images
cloudy_RGB = get_RGB(IDs[0])
clear_RGB = get_RGB(IDs[1])

# Adjust clear RGB image
clear_RGB_adj = adjust_rgb(clear_RGB,top_adj, bottom_adj, left_adj, right_adj)

# get mask for cloudy image
mask = cloud_mask*shadow_mask*dilated_cloud_mask*cirrus_mask

# remove cloudy pixels and fill with adjusted clear pixels
rm_mask = cloudy_RGB*mask[:, :, np.newaxis]
fill_mask = clear_RGB_adj*(1-mask[:, :, np.newaxis])
inpaint = rm_mask+fill_mask
```

你可以在**图 10**中查看结果。总体来看，景观效果不错。如果我们放大关注区域，你会发现它与**图 8**完全相同。然而，如前所述，还是存在一些问题。

![](img/ae783822cc05e00bd08c1cd1b6d35dbc.png)

图 10：使用晴朗图像修复的整个场景（来源：作者）

如果我们在另一个兴趣区域放大，我们可以看到这一点。透明的白色像素是被晴朗图像中的像素替换的。注意，一些阴影像素尚未被替换。这可能会导致景观中亮度变化剧烈的地方出现不连续性。

![](img/27bcbd3ed109499fbdb85ac11834e684.png)

图 11：未成功修复的云影（来源：作者）

问题在于云掩模并不完美。如果我们无法以 100%的准确度识别所有云层，就无法完全替换所有云层。景观随时间变化也可能导致不连续性。河流会移动，潮汐会上涨。为了创建最佳的可视化效果，选择拍摄时间接近你的多云图像的替换图像。

希望你喜欢这篇文章！你可以在[Mastodon](https://sigmoid.social/@conorosully) | [Twitter](https://twitter.com/conorosullyDS) | [YouTube](https://www.youtube.com/channel/UChsoWqJbEjBwrn00Zvghi4w) | [Newsletter](https://mailchi.mp/aa82a5ce1dc0/signup) 上找到我 — 注册即可免费获取[Python SHAP 课程](https://adataodyssey.com/courses/shap-with-python/)

[](https://conorosullyds.medium.com/membership?source=post_page-----246e73494bc--------------------------------) [## 使用我的推荐链接加入 Medium — Conor O’Sullivan

### 作为 Medium 会员，你的一部分会费将用于支持你阅读的作者，你将能完全访问所有故事……

[conorosullyds.medium.com](https://conorosullyds.medium.com/membership?source=post_page-----246e73494bc--------------------------------)

## 参考文献

[1] Steve Foga 等，**云检测算法比较与验证，用于操作 Landsat 数据产品**，[`www.sciencedirect.com/science/article/pii/S0034425717301293?via%3Dihub`](https://www.sciencedirect.com/science/article/pii/S0034425717301293?via%3Dihub=)

**Landsat 8–9 OLI/TIRS 集合 2 级 2 数据格式控制手册** [`www.usgs.gov/media/files/landsat-8-9-olitirs-collection-2-level-2-data-format-control-book`](https://www.usgs.gov/media/files/landsat-8-9-olitirs-collection-2-level-2-data-format-control-book)

Leah Wasser **第 3 课：在 Python 中清理遥感数据 — 云、阴影与云掩模** [`www.earthdatascience.org/courses/use-data-open-source-python/multispectral-remote-sensing/landsat-in-Python/remove-clouds-from-landsat-data/`](https://www.earthdatascience.org/courses/use-data-open-source-python/multispectral-remote-sensing/landsat-in-Python/remove-clouds-from-landsat-data/)

Leah Wasser **第 4 课：如何在 Python 中用不同的栅格数据集的值替换栅格单元值** [`www.earthdatascience.org/courses/use-data-open-source-python/multispectral-remote-sensing/landsat-in-Python/replace-raster-cell-values-in-remote-sensing-images-in-python/`](https://www.earthdatascience.org/courses/use-data-open-source-python/multispectral-remote-sensing/landsat-in-Python/replace-raster-cell-values-in-remote-sensing-images-in-python/)

Landsat 任务 **CFMask 算法** [`www.usgs.gov/landsat-missions/cfmask-algorithm`](https://www.usgs.gov/landsat-missions/cfmask-algorithm)

[Ing Grenet](https://medium.com/u/1f2c139115b6?source=post_page-----246e73494bc--------------------------------) **Landsat-8 图像中的云分割** [`medium.com/sentinel-hub/clouds-segmentation-in-landsat-8-images-da370815235`](https://medium.com/sentinel-hub/clouds-segmentation-in-landsat-8-images-da370815235)

SentinelHub **Landsat 8 云分割脚本** [`custom-scripts.sentinel-hub.com/custom-scripts/landsat-8/clouds_segmentation/`](https://custom-scripts.sentinel-hub.com/custom-scripts/landsat-8/clouds_segmentation/)
